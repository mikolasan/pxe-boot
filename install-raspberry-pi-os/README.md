---
description: First you need an operating system on the server
---

# Install Raspberry Pi OS

Just follow the official tutorial: [https://www.raspberrypi.org/documentation/installation/installing-images/linux.md](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)

## Linux

It's simple on Linux. Here is my one-liner:

```bash
unzip -p 2022-01-28-raspios-bullseye-arm64.zip \
  | sudo dd of=/dev/sdX bs=4M conv=fsync status=progress
```

Or if you install something like LibreELEC

```bash
gzip -d -c LibreELEC-RPi4.arm-10.0.1.img.gz \
  | sudo dd of=/dev/sdX bs=4M conv=fsync status=progress
```

Or if you install something like Manjaro

```bash
xz -d -c Manjaro-ARM-xfce-rpi4-21.12.img.xz \
  | sudo dd of=/dev/sdX bs=4M conv=fsync status=progress
```

\
Source: [https://raspberrypi.stackexchange.com/questions/32200/how-do-i-write-the-retropie-img-file-to-my-sd-card](https://raspberrypi.stackexchange.com/questions/32200/how-do-i-write-the-retropie-img-file-to-my-sd-card)

## Windows

It's simple on Windows.

![Raspberry Pi Imager](<../.gitbook/assets/2022-07-02 22\_37\_47-Raspberry Pi Imager v1.4.png>)
