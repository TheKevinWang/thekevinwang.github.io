Some notes after performing red teaming of the Mac environment of an F500 company. 

Not all ideas are mine. just some notes. 

# detection

crowdstrike detection is very spotty. some things like JXA malware (apfell) gets detected and is hard to bypass, while others like unmodified Mythic Poseidon payloads are 
undetected even from a live phishing email. this will evolve over time, but overall binary malware detection is very behind compared to windows.
However, as with windows, sometimes attacks that don't get detected in the lab get detected on a real target, such as the use of the Poseidon "shell" command, 
this was bypassed by directly executing binaries with the `run` command and using `osascript` with applescript if shell functionality was needed. 
Some other interesting bypasses were the mounting of DMGs with executable binaries to bypass the creation of executable files being detected, and the use 
of `dlopen` to run dylibs and executables without making them an executable file. In memory execution of post exploitation tools would also be interesting to test, but 
the public tooling at the time was not really there for ARM.  

# initial access

macOS security constantly evolves, and macOS 15.? made it significantly more difficult for unsigned payloads, such as unsigned an .app in a DMG, to be executed via the GUI by making it so that one has 
to click through in the settings app. Some ways around this include clickfix style attacks, an dmg where the user is instructed to drag a script file into terminal and hit enter (thus running the script), 
apple automation payloads, vs code payloads, etc. In terms of email, impersonation of apps of internal services was the most successful.

# post exploitation

macos local security depends on controlling fine grain permissions, such as the ability to keylog, such that it requires
user permission via the gui, or is restricted entirely by mdm, such as only to certain apps. While it makes it harder to use legitamately,
it does indeed make post exploitation much more difficult compared to windows. Therefore, attacking macos is also more a matter of social engineering
the user, such as with fake password prompts, [TCC clickjacking](https://github.com/breakpointHQ/TCC-ClickJacking) (very underestimated), "permission needed" prompts masqurading as chrome or company update policy, etc. 
There are 2 kinds of TCC permissions, ones that can be granted with a popup and clicking accept, such as finder automation permission, which are trivial to clickjack for indirect full disk access, and 
other more powerful ones that require the user to go into the settings page and toggle a switch. 
An interesting aspect is that clipboard read/write does not require special permissions and is silent to the user. Access to the user's ~/ dir is also available. 
It is also possible to modify the user's Dock by writing to a json file, such as to "backdoor" an app.

Of course, exploits like [dylib hijacking](https://objective-see.org/products/dhs.html) and [electroniz3r](https://github.com/r3ggi/electroniz3r) exist, but are not widespread. 

Sometimes, persistent reprompts are needed, as users will naturally resist doing work, especially if it seems phishy.

## accessibility permission

The same method to that is "exploited" by legit software like [Rectangle](https://github.com/rxhanson/Rectangle/blob/e8d9f5974516553cb9d77fbc5a644f961af5206b/Rectangle/AccessibilityAuthorization/AccessibilityAuthorization.swift#L17), can be exploited 
by malware to trick the user into providing the more powerful permissions. The trick for the accessibility permission in particular involves checking the permission with `AXIsProcessTrusted` so that the app appears in the Accessibilty view of the Settings app, and then popping this view directly
The accessibility permission is special because it undermines this security model by allowing the 
malware to move the mouse and provide itself any permission or any action like exporting the user's corporate login certificate private keys, 
installing malicious browser extensions, grant itself more permissions, make banking or crypto transactions, etc.
, provided there is nothing like biometric authentication. However, because the user can see this action on the screen, one has to find a time when the user is away. There is a
way to black out the screen by setting the brightness to 0, but this did not work when a second monitor was plugged in and set as the main monitor. However, one can check the idle
time with `CGEventSourceSecondsSinceLastEventType` and whether the screen is locked with `IORegistryGetRootEntry`, and use `caffeinate` so that the mac does not sleep. 
A locked state with high idle time combined with knowledge about the user can be used to determine when the user is away. When this happens, 
the malware can use the accessibility permission to unlock the computer with the user's password, perform the malicious actions,
close everything out, and then lock the computer as if nothing happened. Without apple enterprise and MDM, there is no other way to 
set such permissions programatically. 

Moreover, the accessibility permission grants keylogging of non-passwords, and the ability to screenshot via keyboard shortcut. An interesting vector is that the cmd+shift+3 shortcut
can be controlled with the settings `defaults write com.apple.screencapture show-thumbnail -bool FALSE;defaults write com.apple.screencapture location /exfil_path` 
to screenshot to a exfiltration dir and remove the lingering screenshot preview popup. 


# watchouts

## sound

Some actions, such as copying something with finder automation, or screenshotting via keyboard automation, will make a sound. This can be mitigated by muting the volume, but one must be 
careful to first check if the user is playing sound is with the `pmset -g` command or using the `IOPMCopyAssertionsStatus` API, or else the user may notice the muting.

## tcc context

It's possible to use `launchctl` and `open` to break the tcc context, such as when the user is tricked into providing a permission to an app masqurading as an company updater tool, and one needed to run within that context.

# conclusion

I think there is a lot for Windows to learn from Mac security. When it comes to permissions and freedom to do post exploitation actions, 
it is harder for hackers, qut when it comes to detection, it is much easier. There is also not as much public tooling and research.
