---
description: First you need an operating system on the server
---

# Install Raspberry Pi OS

Just follow the official tutorial: [https://www.raspberrypi.org/documentation/installation/installing-images/linux.md](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)

It's simple. Here is my one-liner:

```bash
unzip -p 2021-05-07-raspios-buster-armhf.zip \
  | sudo dd of=/dev/sdX bs=4M conv=fsync status=progress
```

