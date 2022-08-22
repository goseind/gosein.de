---
layout: post
title:  "Installing Xiaomi EU custom ROM on Poco X3 NFC"
date:   2022-08-22
categories: rom xiaomi android
---

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