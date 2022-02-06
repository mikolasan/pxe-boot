---
description: Impossible
---

# Test UEFI PXE in Virtual Box

After this article [https://c-nergy.be/blog/?p=13334](https://c-nergy.be/blog/?p=13334) I wanted to setup PXE in Virtual Box, because VMWare is not free. I even do not know if they have a trial version - their website is confusing me. They have cloud-cloud-cloud solutions, but I just need very simple local virtual machine. That's it.

So.

I do not know how about you, but I was not able to set it up following this answer [https://unix.stackexchange.com/questions/369603/virtualbox-uefi-pxeboot](https://unix.stackexchange.com/questions/369603/virtualbox-uefi-pxeboot)

But BIOS PXE is working.

## BIOS PXE in Virtual Box

```
mkdir -p /home/nikolay/.config/VirtualBox/TFTP
sudo pacman -S syslinux
cp /usr/lib/syslinux/bios/{pxelinux.0,ldlinux.c32,libcom32.c32,libutil.c32,vesamenu.c32} /home/nikolay/.config/VirtualBox/TFTP

mkdir -p /home/nikolay/.config/VirtualBox/TFTP/pxelinux.cfg
nano /home/nikolay/.config/VirtualBox/TFTP/pxelinux.cfg/default
```

Read more here [https://serverfault.com/questions/617352/pxe-boot-with-virtualbox](https://serverfault.com/questions/617352/pxe-boot-with-virtualbox) and here [https://gist.github.com/sickbock/6b6a5df9a9d2286ebc1a](https://gist.github.com/sickbock/6b6a5df9a9d2286ebc1a)
