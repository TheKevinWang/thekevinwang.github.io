---
title:  "Probing antivirus sandboxes for custom detection markers."
categories: 
  - av-bypass
tags:
  - sandbox
  - antivirus
  - implant
toc: false
---

 If you ever uploaded a live implant to VirusTotal you will notice many callbacks from sandboxes. A common defense faced by attackers is the use of EDRs that use sandboxes to test and analyze the implant, leading to far greater detection rates than static methods alone. Since red team payloads are at risk of being uploaded to VirusTotal by targets, I wanted to find a way to prevent my payload from being executed. While excellent tools like [al-khaser](https://github.com/LordNoteworthy/al-khaser) exist, I wanted to specifically target the currently active sandboxes on VirusTotal. 

Probing sandbox environments:

I created a .doc macro payload that will use powershell to run various commands, such as `ls $env:userprofile\Desktop`, `netstat`, `ps`, wmi, etc to gather information about the host and upload it to Dropbox. This avoids the need to burn a C2 server. I found the .doc macro payload to be particularly effective in generating sandbox executions. I then uploaded the payload to virustotal, resulting in myrids of antiviruses running the payload in their sandboxes. 
After analyzing the data, I found numerous anamolies, such as: 
* the presence of multiple obvious "honey" files, such as "passwords.txt" 
* lack of network activity, such as absence of connections with TIME_WAIT, CLOSE_WAIT, FIN_WAIT state.
* network connections to ips associated with AV/cybersecurity companies
* used storage space under 35 GB
* a single low resolution display
* physical memory ≤ 2GB
* lack of items in taskbar and start menu
* processes using unrealistically low memory, like bitcoin-qt and steam using 3 MB of memory.
* presence of high entropy randomized file names.
* lack of media PnP devices

I have written a tool to detect the sandboxes uses by VT, and have confirmed it no longer runs my payload after doing the check. I released the tool on GitHub [https://github.com/TheKevinWang/Check-Sandbox](https://github.com/TheKevinWang/Check-Sandbox). Furthermore, I implemented a limit for honey file matches, since a single "passwords.txt" could be legit, but not both "passwords.txt" and "accounts.xlsx". Because malware sandboxes can vary greatly, no single check is quite effective against all VMs, so a mix of checks works better. I could not find a single sandbox that could bypass all the checks, but they will eventually adapt to these checks. I'm glad I looked into it myself because al-khaser checks if the memory is under 1GB instead of 2GB, and misses most of the honey files. However, the malware sandboxes will continue to adapt. 
