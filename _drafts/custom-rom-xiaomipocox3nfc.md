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

***Please note** that this guide assumes your device is already unlocked and developer mode, as well as USB debugging is enabled. Further, you'll need Android Debug Bridge (ADB). The website for unlocking your Xiaomi device can be found [here](https://en.miui.com/unlock/index.html) and a guide on how to install ADB [here](https://www.xda-developers.com/install-adb-windows-macos-linux/).*

1. I used the TWRP custom recovery by *birgudav* from XDA which you can download [here](https://androidfilehost.com/?fid=15664248565197184079).

2. Once downloaded you should rename it to `twrp.img` and move it into the ADB folder. With a terminal from there and your phone connected via USB you should execute `./adb devices` to see if your device is recognized.

3. Now flash the custom recovery with the following set of ADB commands. Once your device reboots you should see the TWRP screen.

```pwsh
./adb reboot bootloader
./fastboot flash recovery twrp.img
./adb reboot recovery
```

4. Download and 

**Command summary:**

```pwsh
# List connected devices
./adb devices

# Check whether your device has a/b partitioning or just a
./adb shell getprop ro.build.ab_update
./adb shell getprop # List all device properties

# Install custom recovery
./adb reboot bootloader
./fastboot flash recovery twrp.img
./adb reboot recovery

# Get and compare file hash
Get-FileHash <filepath> -Algorithm MD5
"<filehash>" -eq "<filehash>"

# Copy files to device
./adb push <filepath> /sdcard/<filename>

# Reboot system
./fastboot reboot
```