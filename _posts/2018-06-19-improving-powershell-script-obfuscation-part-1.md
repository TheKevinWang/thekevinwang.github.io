---
title:  "Improving PowerShell Script Obfuscation Part 1"
categories: 
  - obfuscation
  - av-bypass
tags:
  - red team
  - antivirus
  - powershell
toc: false
---

Public PowerShell obfuscators like Invoke-Obfuscation and ISE Steroids do not actually rename the function and variable names, which makes easier to use because it is used the same way as the original. Furthermore, Endgame has released a [deobfuscator](https://www.endgame.com/blog/technical-blog/deobfuscating-powershell-putting-toothpaste-back-tube) for Invoke-Obfuscation. Hiding the original names is a basic and effective anti-analysis method. Variable and function names tell a lot about the how the script works. As part of my effort to create a better obfuscator, I decided to modify the Invoke-Obfuscation.ps1 script by Daniel Bohannon to properly obfuscate variable and function names by renaming them. Instead of giving it random variable names, I decided to use a dictionary word list. In the future I plan to build a list of the most common real world variable and function names. This blends in more closely with normal code, so it doesn't get detected as obfuscated or suspicious, while still satisfying the purpose of removing the original names. The downside is that because the names are changed, all references must be changed as well, so it won't be as easy to use. I learned a lot about script obfuscation and reversing obfuscation from this project.

Here is part of my Get-ClickScreenshot script, which takes a screenshot after every click:
```
 Function Get-Screenshot ($FilePath) {
 $ScreenBounds = [Windows.Forms.SystemInformation]::VirtualScreen
 $Width = $ScreenBounds.Width
 $Height = $ScreenBounds.Height
 $Size = New-Object System.Drawing.Size($Width, $Height)
 $Point = New-Object System.Drawing.Point(0, 0)
 $ScreenshotObject = New-Object Drawing.Bitmap $Width, $Height
 $DrawingGraphics = [Drawing.Graphics]::FromImage($ScreenshotObject)
 $DrawingGraphics.CopyFromScreen($ScreenBounds.Left, $ScreenBounds.Top, 0, 0, $Size);
 $DrawingGraphics.Dispose()
 $ScreenshotObject.Save($FilePath)
 $ScreenshotObject.Dispose()
}
```
Here is the obfuscated version:
```
Function nothing ($mother) {
 $master = [Windows.Forms.SystemInformation]::VirtualScreen
 $ads = $master.Width
 $light = $master.Height
 $ads = New-Object System.Drawing.Size($ads, $light)
 $running = New-Object System.Drawing.Point(0, 0)
 $subjects = New-Object Drawing.Bitmap $ads, $light
 $popular = [Drawing.Graphics]::FromImage($subjects)
 $popular.CopyFromScreen($master.Left, $master.Top, 0, 0, $ads);
 $popular.Dispose()
 $subjects.Save($mother)
 $subjects.Dispose()
}
```  