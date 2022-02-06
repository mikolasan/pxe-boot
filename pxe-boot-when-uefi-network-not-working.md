# PXE Boot when UEFI Network not working

### Idea

> My solution was to build a customized iPXE 3 bootloader image with an embed script that fetches the actual boot instructions. This iPXE image was then signed and placed in the EFI system partition on the emmc as ESP/EFI/BOOT/BOOTX64.EFI. I may make a more detailed guide to this at some point in the near future.
>
> [https://forum.radxa.com/t/cant-seem-to-pxe-boot-uefi-network-stack-not-working/4571/18](https://forum.radxa.com/t/cant-seem-to-pxe-boot-uefi-network-stack-not-working/4571/18)

### Resources

[https://ipxe.org/download](https://ipxe.org/download)

{% embed url="https://gist.github.com/AdrianKoshka/5b6f8b6803092d8b108cda2f8034539a" %}

Why `efi/boot/bootx64.efi` ? Matthew Garrett explained it here [https://mjg59.livejournal.com/138188.html](https://mjg59.livejournal.com/138188.html)

### Troubleshooting

* same problem on forum [https://forum.ipxe.org/showthread.php?tid=13245](https://forum.ipxe.org/showthread.php?tid=13245)
* bootx64 is bad
* ipxe options should be added to nvram [https://wiki.gentoo.org/wiki/Efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr)

```
sudo efibootmgr -c -d /dev/sdb -p 1 -l '\EFI\ipxe.efi' -L "iPXE"
```
