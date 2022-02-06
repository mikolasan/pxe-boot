# Update option ROM on the NIC

I thought that my error PXE-E21 caused by old Intel Gigabit network card. That's why I assumed that I can update its firmware and make UEFI PXE fixed. But this was not the case for me.&#x20;

```
Option ROM area in the flash is not supported for this device on port 1
```

I have it solved in

{% content-ref url="the-case-without-proxy-dhcp.md" %}
[the-case-without-proxy-dhcp.md](the-case-without-proxy-dhcp.md)
{% endcontent-ref %}

But I still want to leave my notes about firmware update procedure, maybe I will need it at some point in the future.

{% hint style="info" %}
There is a way to update firmware on an Intel NIC from UEFI. Here is a guide [https://calvin.me/how-to-update-intel-nic-firmware](https://calvin.me/how-to-update-intel-nic-firmware)
{% endhint %}

{% embed url="https://superuser.com/questions/1219822/computer-refuses-to-boot-in-uefi-mode-gives-error-about-intel-gigabit-network" %}

You download driver from [https://www.intel.com/content/www/us/en/download/15755/29137/intel-ethernet-connections-boot-utility-preboot-images-and-efi-drivers.html](https://www.intel.com/content/www/us/en/download/15755/29137/intel-ethernet-connections-boot-utility-preboot-images-and-efi-drivers.html)

Manjaro users download kernel package [https://gitlab.manjaro.org/packages/core/linux510](https://gitlab.manjaro.org/packages/core/linux510), run it

```
makepkg
```

and go sip some tea.

Building `iqvlinux` module you probably will get **No rule to make target 'scripts/module.lds'** error

```
Compiling the driver...Error: make: Entering directory '/scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver'
common.mk:139: Missing System.map file - depmod will not check for missing symbols
make  ccflags-y=" -I/scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/inc -I/scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/inc/linux -DNAL_LINUX -DNAL_DRIVER -DNAL_DRIVER_UNSECURE -DLINUX -D__KERNEL__ -DMODULE -O2 -pipe -Wall" -C "/usr/src/linux-5.10" CONFIG_IQVLINUX=m CONFIG_MODULE_SIG=n CONFIG_MODULE_SIG_ALL= M="/scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver"    modules
make[1]: Entering directory '/usr/src/linux-5.10'
  CC [M]  /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/nalioctldrv.o
  CC [M]  /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/linuxnaldriver.o
/scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/linuxnaldriver.c: In function ‘NalDeviceControl’:
/scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/linuxnaldriver.c:197:1: warning: the frame size of 4112 bytes is larger than 2048 bytes [-Wframe-larger-than=]
  197 | }
      | ^
  CC [M]  /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/linuxdriveros_i.o
  CC [M]  /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/linuxdriverpci_i.o
  CC [M]  /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/linuxdriverdevice_i.o
  CC [M]  /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/linuxdrivermemory_i.o
  LD [M]  /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/iqvlinux.o
WARNING: Symbol version dump "Module.symvers" is missing.
         Modules may not have dependencies or modversions.
  MODPOST /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/Module.symvers
WARNING: modpost: Symbol info of vmlinux is missing. Unresolved symbol check will be entirely skipped.
  CC [M]  /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/iqvlinux.mod.o
make[3]: *** No rule to make target 'scripts/module.lds', needed by '/scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver/iqvlinux.ko'.  Stop.
make[2]: *** [scripts/Makefile.modpost:117: __modpost] Error 2
make[1]: *** [Makefile:1709: modules] Error 2
make[1]: Leaving directory '/usr/src/linux-5.10'
make: *** [Makefile:60: all] Error 2
make: Leaving directory '/scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver'
Error: unable to find driver file (iqvlinux.ko) in /scgames/installer/intel-preboot/APPS/BootUtil/Linux_x64/DRIVER/iqvlinux/src/linux/driver
```

Here is an idea: [https://github.com/Mange/rtl8192eu-linux-driver/issues/205#issuecomment-736366347](https://github.com/Mange/rtl8192eu-linux-driver/issues/205#issuecomment-736366347). But copy from your sources, not from master, and remove the last include line (that's sed command). I had many includes in the file from master, so `sed` command didn't work for me.

Then when you install the module be sure that kernel from which you build your kernel module and to which you are inserting module should be of same version.

```
sudo insmod iqvlinux.ko
insmod: ERROR: could not insert module iqvlinux.ko: Invalid module format
```
