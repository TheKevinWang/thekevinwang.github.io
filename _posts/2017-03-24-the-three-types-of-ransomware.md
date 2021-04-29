---
title: "The Three Types of Ransomware"
categories: 
  - malware
tags:
  - ransomware
  - petya
toc: false
---

Ransomware encrypts data and demands ransom money for the decryption key. The ransomware threat is growing, and it is increasingly targeting businesses. While there are hundreds of different strains of ransomware, they generally fall under three different types: file encryption, full disk encryption, and Master File Table (MFT) encryption. All of these methods accomplish the same goal, but through very different means.

1) File encryption

The vast majority of ransomware are file encryption ransomware, which encrypts each individual file. Generally, only certain filetypes are targetted so that the computer does not become unusable. Many schemes exist, such as using symmetric encryption for the files, and encrypting the passwords using asymmetric encryption. File encryption ransomware is apt for targeting personal computers, as the victim can still use the computer to buy bitcoins. However, it is suboptimal for enterprise environments because the process is often slow and breaks services, alerting the administrators. Many will also fail to encrypt files with locks, which are common in Windows servers. There are exceptions, of course. Samsam ransomware targets enterprise environments. Examples include CryptoLocker, CryptoWall, and Hidden Tear.

2) Full Disk Encryption

This type of ransomware encrypts the entire hard-drive (or at least the used portion). This usually happens in the background without interrupting services. However, the encryption still takes a long time, and users will notice high disk usage. The system is then restarted to remove the decryption key from memory and present the user with the ransom screen. This is often superior to file ransomware in enterprise environments. However, damage can be prevented if it is detected before the shutdown. Examples include Mamba (used in the San Francisco Metro attack), HDD Cryptor, and Bitlocker ([theoretical](http://www.blackhillsinfosec.com/?p=5023)).

3) Master File Table Encryption

Master file table (MFT) ransomware encrypts the MFT, rather than the files. The master file table keeps track of the disk locations of every file. Because the contents of the files are scattered across the disk, without the MFT, finding the files on a disk is near impossible without paying an expert. MFT ransomware writes a malicious bootloader to disk (stage 1), and then forces a restart, which runs the bootloader and encrypts the MFT (stage 2). This greatly speeds up the process and effectively prevents access to the entire hard drive. Petya and it's close cousin GoldenEye are the only known MFT ransomware. However, MFT ransomware is only dangerous during the second stage. The weakness is when the malicious bootloader is written, but not yet executed. During that time, antivirus can detect and remove the malicious bootloader before it does any damage. Clearly, this must have many benefits, as Petya is dead, yet unaffiliated hackers have taken the effort to "[patch](https://securelist.com/blog/research/77762/petrwrap-the-new-petya-based-ransomware-used-in-targeted-attacks/)" petya for their own use in in corporate environments. Secure boot can prevent such attacks. 

As hackers increasingly target enterprise environments, the use of full disk encryption and MFT encryption will likely increase. Attackers are increasingly targetting the entire domain, rather than single computers. Furthermore, cloud and mobile devices will likely to be targeted as well. It is important to understand the methods used by attackers to better defend against them.