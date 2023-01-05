---
description: >-
  This approach works fine for Legacy (BIOS) PXE Boot. But UEFI PXE Boot
  appeared to be an advanced topic. Get ready to dive deep.
---

# Install PXE server on Raspberry Pi 4

To setup a PXE Server we will need the following dependencies:

* dnsmasq
* NFS

Network boot is possible due to [TFTP](https://en.wikipedia.org/wiki/Trivial\_File\_Transfer\_Protocol). Usually TFTP server has the same IP address as DHCP server, but we will use _dnsmasq_ to configure some kind of relay for our goal. It is called proxy DHCP to be more precise. This way our setup will work in any local network even with grandma's router.

![](<../.gitbook/assets/diagram (1).png>)

We will setup a NFS (Network File System) server, which will allow computers to access files on PXE server over the network. Using TFTP the client only receives crucial parts of the system important to load, like kernel and initramfs, but the rest files like other additional software and user files will stay on the server, though the client will see these files locally, but all magic is done by NFS.&#x20;

_Note: This tutorial assumes you are the **root** user, if not, please add `sudo` for all the commands._

## Install and configure

The following command will install required packages on Raspberry OS:

```bash
apt install -y dnsmasq pxelinux syslinux-common nfs-kernel-server
```

Once downloading is complete, stop the _dnsmasq_ for now

```bash
systemctl stop dnsmasq
```

### PXELINUX

Create directory where all transferable operating system files will reside

```bash
mkdir /srv/tftpboot
```

Take important boot modules (pxelinux.0, ldlinux.c32, libutil.c32, menu.c32, poweroff.c32, reboot.c32 and vesamenu.c32) and place them into **/srv/tftpboot**

```bash
cp /usr/lib/PXELINUX/pxelinux.0 \
    /usr/lib/syslinux/modules/bios/{ldlinux.c32,libcom32.c32,libutil.c32,menu.c32,poweroff.c32,reboot.c32,vesamenu.c32} \
    /srv/tftpboot/
```

Create directory for the PXE configuration file.

{% hint style="warning" %}
**Important**: this directory must be called **pxelinux.cfg**
{% endhint %}

```bash
mkdir -p /srv/tftpboot/pxelinux.cfg
```

Prepare boot menu design

```bash
nano /srv/tftpboot/pxelinux.cfg/default
```

with this content

{% code title="/srv/tftpboot/pxelinux.cfg/default" %}
```
MENU TITLE Network Boot Menu
UI vesamenu.c32
MENU INCLUDE pxelinux.cfg/pxe.conf

LABEL next
    MENU LABEL Load local bootloader
    MENU DEFAULT
    localboot

# Separator
MENU SEPARATOR

LABEL reboot
    MENU LABEL Reboot
    COM32 reboot.c32

LABEL poweroff
    MENU LABEL Power Off
    COM32 poweroff.c32
```
{% endcode %}

And styles separately

```bash
nano /srv/tftpboot/pxelinux.cfg/pxe.conf
```

{% code title="/srv/tftpboot/pxelinux.cfg/pxe.conf" %}
```
MENU BACKGROUND pxelinux.cfg/logo.png
MENU RESOLUTION 1024 768
NOESCAPE 1
ALLOWOPTIONS 0
PROMPT 0
menu width 32
menu rows 5
MENU MARGIN 0
MENU VSHIFT 10
MENU HSHIFT 46
menu color title                1;37;40    #ffffffff #00000000 std
menu color border               36;40      #c00090f0 #00000000 std
menu color sel                  30;47      #00000000 #ffffffff all
menu color unsel                37;40      #ffffffff #00000000 std
```
{% endcode %}

And the background **/srv/tftpboot/pxelinux.cfg/logo.png**

![logo.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627461597527/6EueRlS2J.png)

Check PXELINUX configuration syntax here: [https://wiki.syslinux.org/wiki/index.php?title=Config](https://wiki.syslinux.org/wiki/index.php?title=Config)

We will add real entries in the Test section.

### dnsmasq

Update the content of **/etc/dnsmasq.conf**. By default this file full of comments above each option. You can read through and tweak settings to your liking ([online manpages](https://thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html)), but I already know what I put into:

```bash
mv /etc/dnsmasq.conf /etc/dnsmasq.conf.example
nano /etc/dnsmasq.conf
```

{% code title="/etc/dnsmasq.conf" %}
```
#Disable DNS Server
port=0

#Enable DHCP logging
log-dhcp

#Respond to PXE request for the specified network;
#Run as DHCP proxy
dhcp-range=192.168.0.0,proxy,255.255.255.0

dhcp-boot=pxelinux.0

#Flag forces "simple and safe" behavior
dhcp-no-override

#Provide network boot option called "Network Boot"
pxe-service=x86PC,"Network Boot", pxelinux

#Turn tftp ON
enable-tftp

#Set tftp root
tftp-root=/srv/tftpboot
```
{% endcode %}

Add the following line to the **/etc/default/dnsmasq** file

{% code title="/etc/default/dnsmasq" %}
```
DNSMASQ_EXCEPT=lo
```
{% endcode %}

### NFS

In order to give access to specific files to NFS clients add the following line to the **/etc/exports** file

{% code title="/etc/exports" %}
```
/srv/tftpboot 192.168.0.0/24(rw,sync,no_root_squash,no_subtree_check)
```
{% endcode %}

{% hint style="info" %}
In case you add Linux systems to **/srv/tftpboot** using symlinks, you need to add its real path to **/etc/exports** as well. For example you have symlink `/srv/tftpboot/kali -> /media/external_hdd/kali`, then add&#x20;

{% code title="/etc/exports" %}
```
/media/external_hdd/kali 192.168.0.0/24(rw,sync,no_root_squash,no_subtree_check)
```
{% endcode %}
{% endhint %}

Then make this change live:

```bash
exportfs -a
```

## Test

For tests we are going to run Kali Linux and custom Buildroot build (here can be another article about it, but here we'll assume that you already have **output/images/rootfs.tar.gz**)

### Kali

Download an ISO image from [official website](https://www.kali.org/get-kali/#kali-live) and transfer it to Raspberry Pi (when I was writing this tutorial the last version was **2021.2** and ISO name correspondingly **kali-linux-2021.2-live-amd64.iso**)

```bash
scp kali-linux-2021.2-live-amd64.iso pi@192.168.0.11:~
```

Then extract it to **/srv/tftpboot/kali**

```bash
mkdir -p /srv/tftpboot/kali
mount kali-linux-2021.2-live-amd64.iso kali
```

Add to boot menu an entry

{% code title="/srv/tftpboot/pxelinux.cfg/default" %}
```
LABEL kali
    MENU vesamenu.c32
    MENU LABEL Kali Linux Live
    KERNEL /kali/live/vmlinuz
    APPEND root=/dev/nfs rw nfsroot=192.168.0.11:/srv/tftpboot/kali,tcp,vers=4 ip=dhcp rootfstype=ext4 --
```
{% endcode %}

### Buildroot

Do not forget to enable in kernel configuration

* CONFIG\_IP\_PNP\_DHCP=y
* NFS filesystem support (CONFIG\_NFS\_FS).
* Root file system on NFS (CONFIG\_ROOT\_NFS).

Transfer rootfs archive to Raspberry Pi

```bash
scp rootfs.tar.gz pi@192.168.0.11:~
```

Then extract it to **/srv/tftpboot/buildroot**

```bash
mkdir -p /srv/tftpboot/buildroot
tar xvpf ~/rootfs.tar.gz -C /srv/tftpboot/buildroot
```

Add to boot menu an entry

{% code title="/srv/tftpboot/pxelinux.cfg/default" %}
```
LABEL buildroot
    MENU vesamenu.c32
    MENU LABEL Custom Buildroot build
    KERNEL /buildroot/boot/bzImage
    APPEND root=/dev/nfs rw nfsroot=192.168.0.11:/srv/tftpboot/buildroot,tcp,vers=4 ip=dhcp rootfstype=ext4 --
```
{% endcode %}
