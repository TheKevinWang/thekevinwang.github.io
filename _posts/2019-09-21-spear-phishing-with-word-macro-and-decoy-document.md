---
title:  "Spear Phishing with Word Macro and Decoy Document"
categories: 
  - web
tags:
  - open redirect
  - phishing
  - google
toc: false
---
Microsoft Word macros are often utilized in spear phishing attacks. These documents typically include a lure to trick the user into clicking "Enable Content." However, after executing the payload, the user may become suspicious of the lack of effect. As users are increasingly aware of cyber threats and the importance of reporting such attacks, the use of a "decoy" document becomes necessary for a successful penetration test.

<figure>
	<a href="/assets/images/r7yrzafbzorbglbp8gcr.webp"><img src="/assets/images/r7yrzafbzorbglbp8gcr.webp"></a>
    <figcaption>Sophisticated lure used by BlackEnergy APT</figcaption>
</figure> 

Previously, only advanced persistent threats used decoy documents. However, other attackers are catching on, and the ease of producing such decoys means that not doing so is just lazy penetration testing. A phishing report can mean the difference between a successful penetration test and getting detected and kicked out of the network. Unfortunately, popular penetration testing tools, such as PowerShell Empire and Nishang, do not include an option to deploy decoy documents. Most decoy documents are deployed by using a PowerShell script to download and open a new document. Unfortunately, this action can be suspicious, and PowerShell can be blocked or restricted. 

After some research and testing, I found a simple yet effective method to remove the initial lure and display the decoy document in a smooth and unsuspicious manner. My method uses hidden text and does not download and open a new document. However, this relies on the victim having hidden text enabled. Fortunately, this is enabled by default. 
<figure>
	<a href="/assets/images/263257f06cbb481943679060048d03f5.png"><img src="/assets/images/263257f06cbb481943679060048d03f5.png"></a>
</figure>
My technique involves using the first page as the lure, and then appending the decoy document after the first page. The decoy component is then given the "hidden" font property to hide it. This also hides the presence of the additional pages so the user only sees the initial lure. The macro will execute the payload, and then delete the first page (containing the lure) and display all hidden text (displaying the decoy). The user will think clicking on "enable content" has actually enabled the "content," unsuspecting of the malicious payload executed in the background.

The problem is that it changes the document, so Word will prompt the user to save the document when closed. However, with some researching and hacking, I was able to bypass this dialog. After browsing Microsoft's Word VBA reference, I found the "Application.DocumentBeforeClose" event along with some example code. Using thier template, I replaced thier prompt with "Application.Quit SaveChanges:=wdDoNotSaveChanges" to close the document without saving. However, I found out that there will be an error because the "save changes" dialog prevents the document from closing. By first cancelling the close operation, and then closing the document without saving, I was able to overcome this obstacle.

<figure>
	<a href="/assets/images/05928bad692fdbc2e94f0be0318d409d.png"><img src="/assets/images/05928bad692fdbc2e94f0be0318d409d.png"></a>
</figure>
I included a phishing lure and uploaded it to Github:
[https://github.com/TheKevinWang/MacroPhishing](https://github.com/TheKevinWang/MacroPhishing)
<figure>
	<a href="https://raw.githubusercontent.com/TheKevinWang/MacroPhishing/master/WordMacroDemo.gif"><img src="https://raw.githubusercontent.com/TheKevinWang/MacroPhishing/master/WordMacroDemo.gif"></a>
</figure>
