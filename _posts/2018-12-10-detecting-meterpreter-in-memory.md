---
title:  "Detecting Meterpreter Malware In Memory With Yara"
categories: 
  - antivirus
tags:
  - blue team
  - antivirus
  - meterpreter
toc: false
---

My plan is fairly simple: read the memory of each process and scan it for static indicators of meterpreter.

First, I needed to find the "signatures" to look for, so I created a standard https meterpreter payload and executed it. Then I attached ollydbg and opened the Memory map to look around for distinct indicators of meterpreter. I noticed there were some memory locations with MZ headers, which indicates that it may be a DLL that it has reflectively injected.
<figure>
	<a href="/assets/images/B5J3UcY.png"><img src="/assets/images/B5J3UcY.png"></a>
</figure>

Looking through that, I found some interesting strings, such as "ReflectiveLoader", "metsrv.dll", and various strings starting with "stdapi_" and "priv_fs". Therefore, I created a yara rule to detect those strings. After I created the yara rule, I used a simple powershell command to run it against every process synchronously:

```
ps | %{ "trying "+$_.id; & ".\yara64.exe" .\meterpreter.yara $_.id}
```

<figure>
	<a href="/assets/images/fyywfYD.png"><img src="/assets/images/fyywfYD.png"></a> 
</figure>

Based on my experimentation, the yara rule works accurately, and the only false positives were from my chrome browser because I was researching those strings. It took around 4 minutes to run for 16 GB of RAM, and there was no noticeable performance hit either. Therefore, I consider this an effective way to detect meterpreter in memory, even though the performance could have been improved with parallelism. Bypassing it is logically simple, but would require rebuilding metasploit. This can be further expanded to include other malware, such as Pupy.  

The yara file is available here: [https://gist.github.com/TheKevinWang/1af5f64fece548b01c0267ecf06444e7](https://gist.github.com/TheKevinWang/1af5f64fece548b01c0267ecf06444e7)