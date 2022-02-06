# NFS kernel options

This line

```
root=/dev/nfs rw nfsroot=192.168.0.11:/srv/tftpboot/buildroot,tcp,vers=4 ip=dhcp rootfstype=ext4
```

* **root=/dev/nfs** - specify location of root filesystem. For local systems it usually looks like root=/dev/sda1 or root=UUID=80efa619-1b3a-4a17-92a3-e60334584d9f
* **rw** - root filesystem will mounted in read-write mode
* **nfsroot=192.168.0.11:/srv/tftpboot/buildroot,tcp,vers=4** - using NFS client it will access remote location and specify /dev/nfs
* **ip=dhcp** - this will configure network interface and 192.168.0.11will become reachable
* **rootfstype=ext4** - remote NFS filesystem mounted on local machine as ext4

Still a question

```
tcp,vers=4
```

or

```
tcp,v4
```

### PXELINUX syntax

```
LABEL buildroot
    MENU vesamenu.c32
    MENU LABEL Custom Buildroot build
    KERNEL /buildroot/boot/bzImage
    APPEND root=/dev/nfs rw nfsroot=192.168.0.11:/srv/tftpboot/buildroot,tcp,vers=4 ip=dhcp rootfstype=ext4 --
```

### GRUB syntax

```
menuentry "Custom Buildroot build" {
        set gfxpayload=keep
        linux   /buildroot/boot/bzImage root=/dev/nfs ip=dhcp nfsroot=192.168.0.11:/srv/tftpboot/pxe_system,tcp,v3 rootfstype=ext4 rw --
        initrd  /buildroot/boot/initrd.img
}
```
