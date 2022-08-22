---
layout: post
title:  "Switching from ArrowOS to Xiaomi EU custom ROM on Poco X3 NFC"
date:   2022-08-22
categories: rom xiaomi android arrowos custom poco x3 nfc phone flash
---

After I bought my Xiaomi POCO X3 NFC last year, I flashed the latest ArrowOS. I wanted a clean android with more control and an open source, and ArrowOS delivered that. However, after one year of using ArrowOS, I noticed some strange behavior. My network usage went up almost exponentially without me changing my behavior or the settings. The statistics showed that "System Updates" was constantly downloading data in the background for no apparent reason and I couldn't get it to stop, so eventually, I decided the best way to handle this was to reinstall the entire OS. A friend recommended to flash Xiaomi.EU, and so I did. As there is a lot of information out there on how to flash your android phone and some of it is very confusing (at least I felt thst way). I decided to share my experience/documentation on the matter.

**Heres a quick summary of the most important things:**

|  |  |
| -- | -- |
| Phone | Xiaomi POCO X3 NFC Global 2021 (surya) |
| OS | From ArrowOS to Xiaomi.EU |
| Recovery | Custom TWRP to custom TWRP |

I went with a *very clean* install and also used to chance to switch to a different build of TWRP, so we'll start with that.

***Please note** that this guide assumes your device is already unlocked and developer mode as well as USB debugging is enabled.*

1. I used the TWRP custom recovery by *birgudav* from XDA which you can download [here](https://androidfilehost.com/?fid=15664248565197184079).



```pwsh
 ./adb devices

 ./adb shell getprop ro.build.ab_update
./adb shell getprop

./adb reboot bootloader

./fastboot flash recovery twrp.img
./fastboot reboot

 ./adb reboot recovery

Get-FileHash .\xiaomi.eu_multi_POCOX3NFC_V13.0.1.0.SJGMIXM_v13-12.zip -Algorithm MD5
"BBCE24C17F8A1BE0F8A99B30508D4357" -eq "bbce24c17f8a1be0f8a99b30508d4357"


./adb push .\xiaomi.eu_multi_POCOX3NFC_V13.0.1.0.SJGMIXM_v13-12.zip /sdcard/xiaomi.eu_multi_POCOX3NFC_V13.0.1.0.SJGMIXM_v13-12.zip
```