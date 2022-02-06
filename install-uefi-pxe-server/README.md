# Install UEFI PXE server

The setup of PXE boot for UEFI computers is slightly different from the setup supporting the BIOS computers. I will not cover it here, because the topic is very big for one article. In some rare cases you will only need to find grubx64.efi file and replace `pxelinux.0` with `grubx64.efi`. Take it from the system that matches your client computers, for example, take it from official [Ubuntu 20.04 (Focal) distribution](http://archive.ubuntu.com/ubuntu/dists/focal/main/uefi/grub2-amd64/current/). Then put `grubx64.efi` into `/srv/tftpboot/`. But usually it's not that simple.

When I first time configured Raspberry Pi like this I've got this error

```
Start PXE over IPv4.
Station IP address is 192.168.0.7
PXE-E21: Remote boot cancelled
```

{% content-ref url="../troubleshooting/pxe-e21-remote-boot-cancelled.md" %}
[pxe-e21-remote-boot-cancelled.md](../troubleshooting/pxe-e21-remote-boot-cancelled.md)
{% endcontent-ref %}

This error is very cruel, because it's not giving you any idea of what can cause it. I started thinking about driver updates, missing configuration lines, not suitable efi files and even about running another DHCP server.
