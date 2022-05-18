---
description: PXE config for adding initrd and having access to NFS
---

# Kernel w/ initramfs

Just one additional line, very similar to grub config

```
LABEL buildroot
    MENU vesamenu.c32
    MENU LABEL Custom Buildroot build
    KERNEL /buildroot/boot/bzImage
    INITRD /buildroot/boot/rootfs.cpio
    APPEND root=/dev/nfs rw nfsroot=192.168.0.11:/srv/tftpboot/buildroot,tcp,vers=4 ip=dhcp rootfstype=ext4 --
```

It depends if use two step process with initial small linux in rootfs.cpio (as initramfs) or rootfs.cpio already has everything and do not jump to new root.

### One system

The system is loaded, but NFS is not taking part to this moment, because neither kernel nor ramfs do not need it. The last line in PXE config is ignored, because chroot procedure is not happening.

### Chroot to NFS root

It's achieved by proper buildroot config and NFS kernel options that I will cover in the next chapter.
