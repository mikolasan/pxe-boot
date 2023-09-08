# Install UEFI PXE server

The setup of PXE boot for UEFI computers is slightly different from the setup supporting the BIOS computers. You will only need to find grubx64.efi file and replace `pxelinux.0` with `grubx64.efi`. Take it from the system that matches your client computers, for example, take it from official [Ubuntu 20.04 (Focal) distribution](http://archive.ubuntu.com/ubuntu/dists/focal/main/uefi/grub2-amd64/current/). Then put `grubx64.efi` into `/srv/tftpboot/`.&#x20;

And dnsmasq configuration should be updated accordingly

{% code title="/etc/dnsmasq.conf" %}
```
#Disable DNS Server
port=0

#Respond to PXE request for the specified network;
#Run as DHCP proxy
dhcp-range=192.168.0.0,proxy,255.255.255.0

# The boot filename, Server name, Server Ip Address
dhcp-boot=efi64/bootx64.efi

# PXE menu.  The first part is the text displayed to the user.  The second is the timeout, in seconds.
# pxe-prompt="Booting PXE Client", 1

# The known types are x86PC, PC98, IA64_EFI, Alpha, Arc_x86,
# Intel_Lean_Client, IA32_EFI, ARM_EFI, BC_EFI, Xscale_EFI and X86-64_EFI
# This option is first and will be the default if there is no input from the user.

# PXEClient:Arch:00000
pxe-service=X86PC, "Boot BIOS PXE", pxelinux.0

# PXEClient:Arch:00007
pxe-service=BC_EFI, "Boot UEFI PXE-BC", efi64/bootx64.efi

# PXEClient:Arch:00009
pxe-service=X86-64_EFI, "Boot UEFI PXE-64", efi64/bootx64.efi

#dhcp-boot=pxelinux.0

#Provide network boot option called "Network Boot"
#pxe-service=x86PC,"Network Boot",pxelinux

#Flag forces "simple and safe" behaviour
dhcp-no-override

enable-tftp
tftp-root=/srv/tftpboot

# Logging.
log-facility=/var/log/dnsmasq.log   # logfile path.
log-async
log-queries # log queries.
log-dhcp    # log dhcp related messages.
```
{% endcode %}

But usually it's not that simple.

When I first time configured Raspberry Pi like this I've got this error

```
Start PXE over IPv4.
Station IP address is 192.168.0.7
PXE-E21: Remote boot cancelled
```

{% content-ref url="../troubleshooting/pxe-e21-remote-boot-cancelled/" %}
[pxe-e21-remote-boot-cancelled](../troubleshooting/pxe-e21-remote-boot-cancelled/)
{% endcontent-ref %}

This error is very cruel, because it's not giving you any idea of what can cause it. I started thinking about driver updates, missing configuration lines, not suitable efi files and even about running another DHCP server.
