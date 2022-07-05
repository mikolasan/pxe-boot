---
description: I have very strange issue with my Raspberry Pi 4.
---

# Raspberry Pi 4 not booting

Once I installed LibreELEC to use my Pie as a media player (to display a fireplace video on 4K TV). Intallation is a one-liner command on Linux, boot screen is beautiful with no kernel prints flickering on the screen, and yes, HDMI just works.

But when I install Raspberry OS, or Ubuntu, or Ubuntu Server then it displays first kernel prints then goes black, or it displays four pixels enlarged to the whole screen (looks like a spectrum or a rainbow) and nothing.&#x20;

![Rainbow screen](../.gitbook/assets/IMG\_3269.JPG)

I thought that I bricked my Pie somehow.&#x20;

But then I install LibreELEC and it works!

I rebooted Raspbian many times and once in a while it actually boots. That time I checked my IP address and configured SSH server. Then I disconnected HDMI cable and now it boots every time.

## Get more intel

Insert SD card into your computer and on the boot partition you can find `cmdline.txt` file where you can remove `quiet` and `splash` options to get more prints.

### error -110

During boot I see this error many times

```
mmc0 error "-110" whilst initialising SD card
```

There is one explanation that makes sense

> Error -110 is a timeout. If the card works in your PC but not on your Pi, then the only thing that (really) changes when you move it between the two is the connector in which it sits.
>
> The bootloader in the Pi is a very simple ROM that will read bootcode.bin from the SD card and then run it. bootcode then loads the GPU firmware blob and the linux kernel image. This 2-stage approach allows the GPU firmware and kernel to be arbitrary since they live on the SD card. My bet is that the first stage does this in a very basic SD card mode (that all SD cards must employ when they power up) but when the kernel copied from the SD card tries to use the SD card with its all-singing all-dancing driver it fails.

Source: [M33P](https://forums.raspberrypi.com/memberlist.php?mode=viewprofile\&u=33446) from [https://forums.raspberrypi.com/viewtopic.php?t=27470](https://forums.raspberrypi.com/viewtopic.php?t=27470)

> Error -110 means timeout. Basically, your MMC controller is not able to talk correctly with your SD card. It usually happens when your card is not correctly inserted in the slot (for example the spring is pushing back your card too far) or maybe you are using a micro SD card and it is not correctly inserted in the SD card adapter. What can also happen is that your SD card adapter is slightly broken and some connections are not correctly made (e.g. your card has negotiated 4 bits mode and some lines are in fact not connected).

Source: [https://unix.stackexchange.com/questions/344083/mmc0-error-110-whilst-initialising-sd-card](https://unix.stackexchange.com/questions/344083/mmc0-error-110-whilst-initialising-sd-card)

## Try another SD card

There is a list ([https://elinux.org/RPi\_SD\_cards](https://elinux.org/RPi\_SD\_cards)) of "supported" and not good SD cards tested by the public with different Raspberry Pi versions, different images, and with different ways to install.

So don't take that information for granted. Because "bad" SD card might work for another OS (see next paragraph). Or you should use special software to fix your SD card.

For example did you know that

> In general, formatting tools provided with operating systems can format various storage media including SD/SDHC/SDXC Cards, but it may not be optimized for SD/SDHC/SDXC Cards and it may result in lower performance.

Try **SD Memory Card Formatter** from SD Association [https://www.sdcard.org/downloads/formatter/](https://www.sdcard.org/downloads/formatter/)

![](<../.gitbook/assets/2022-07-03 12\_30\_44-.png>)

Try **SmartDisk FAT32 Format Utility** (desription [https://www.easeus.com/partition-master/smartdisk-fat32-format-utility-tool.html](https://www.easeus.com/partition-master/smartdisk-fat32-format-utility-tool.html), download from Verbatim [https://www.verbatim.com/index/search.php?words=fat32+tool](https://www.verbatim.com/index/search.php?words=fat32+tool)). Though I think it doesn't work anymore

![Not recognizing SD card on Windows 10](<../.gitbook/assets/2022-07-03 12\_52\_03-SmartDisk FAT32 Format Utility.png>)

Use NOOBS ([https://github.com/raspberrypi/noobs](https://github.com/raspberrypi/noobs)), that used to be the official way of installing an OS onto SD card ([https://www.raspberrypi.com/news/introducing-noobs/](https://www.raspberrypi.com/news/introducing-noobs/)), but currently is replaced by Raspberry Pi Imager.

The latest official release of NOOBS can be downloaded from [https://downloads.raspberrypi.org/NOOBS\_latest](https://downloads.raspberrypi.org/NOOBS\_latest)

But be careful with manual steps, it may lead to new errors. Like `Unable to read partition as FAT`:

![](<../.gitbook/assets/2022-07-03 14-50-50.JPG>)

## Try another OS

That is why, just in case, I have a list of different OSes that can be loaded to Raspberry Pi 4 sometimes just for test.
