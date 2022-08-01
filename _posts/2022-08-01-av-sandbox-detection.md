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

 If you ever had a live implant uploaded to VirusTotal you will notice many callbacks from sandboxes. A common defense faced by attackers is the use of sandboxes to test and analyze the implant, leading to far greater detection rates than static methods alone. Since red team payloads are at high risk of being uploaded to VirusTotal by targets, I wanted to find ways to distinguish between a sandbox and real computer so that the malicious payload does not detonate in the sandbox. Many of the common techniques rely on VM detection, such as GetTickCount. However, a VM does not necessarily imply that it's a sandbox. Thus, a more robust anti-sandbox method would rely on distinctions between sandboxes and user devices. While environmental keying is a effective method, it often involves non-public knowledge of the target environment, which may not always be consistant, such as the computer domain name or installed apps.

Probing sandbox environments:

I created a .doc macro payload that will use powershell to run various commands, such as ls $env:userprofile\Desktop, netstat, to gather information about the host and upload it to Dropbox. This avoids the need to use a C2 server. I found the .doc macro payload to be particularly effective in generating sandbox executions. I then uploaded the payload to virustotal, resulting in myrids of antiviruses running the payload in their sandboxes. 
After analyzing the data, I found numerous anamolies, such as the presence of obvious "honey" files, connections to IPs associated with antiviruses, lack of connections to Microsoft IPs, used storage space under 35 GB, a single low resolution display, etc. 

Many data points, such as the lack of files in a user's Download directory may strongly suggest, but not implicate the computer as a sandbox. 
Thus, these data points can be combined to determine the risk of the computer being a sandbox.
