---
title:  "Bypassing Kaspersky Antivirus with python.exe and Pupy"
categories: 
  - implant
  - av-bypass
tags:
  - red team
  - antivirus
  - pupy
  - python
toc: false
---
<figure>
	<a href="/assets/images/kymeba4ksodyixcorkjh.webp"><img src="/assets/images/kymeba4ksodyixcorkjh.webp"></a>
</figure>   
Kaspersky antivirus is notorious for being difficult to bypass. Black Hills did an interesting blog post where they experienced difficulty bypassing Kaspersky, but found a way to bypass it using an obscure RAT called "TrevorC2." Although they used Kaspersky Internet Security, which is designed for personal use, I chose to use the business version, Kaspersky Endpoint Security, as this version is more likely to be encountered in the enterprise environment. Furthermore, to be more realistic, I used a command and control server hosted on the internet rather than the local network. I found a way to use a popular RAT called [Pupy](https://github.com/n1nj4sec/pupy) to bypass Kaspersky in a fairly easy way by installing python with user privileges and using the python interpreter to run the Pupy python payload.

I tried some of the methods Black Hills mentioned was blocked and that found they were truely blocked. Therefore, I decided to try something different. I like scripting languages because they're easy to obfuscate and antiviruses tend to have a hard time with them. First, I tried [Koadic](https://github.com/zerosum0x0/koadic), a JScript based RAT. While this does bypass most antiviruses, Kaspersky detected it.
<figure>
	<a href="/assets/images/abWUVIA.png"><img src="/assets/images/abWUVIA.png"></a>
</figure> 
Then, I decided to use python instead. I wrote a PowerShell script to download and install python, which does not require admin privileges. Then, it installs pip and uses it to install dependencies, all with user privileges. Finally, I generated a Pupy python payload, ran it with python, and voila!     
<figure>
	<a href="/assets/images/HvZM0a7.png"><img src="/assets/images/HvZM0a7.png"></a>
</figure> 
<figure>
	<a href="/assets/images/q3MGwMd.png"><img src="/assets/images/q3MGwMd.png"></a>
</figure> 
Not only did I get a shell, I was also able to run LaZagne, my favorite general purpose credential dumping tool. I could have improved this method by using the python -c flag to retrieve and run the payload in memory, but it did not even detect it on disk. Even the best antiviruses can by bypassed. However, application whitelisting would have prevented this. Thus, it's important to have defense in depth. 