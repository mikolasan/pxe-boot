---
description: 'Spoiler: very bad combination'
---

# DHCP + TFTP on dnsmasq

dnsmasq config is very simple. Just check it here [https://yulistic.gitlab.io/2018/03/configuring-dnsmasq-only-for-dhcp-server-in-ubuntu-pc/](https://yulistic.gitlab.io/2018/03/configuring-dnsmasq-only-for-dhcp-server-in-ubuntu-pc/) or here [https://wiki.archlinux.org/title/dnsmasq](https://wiki.archlinux.org/title/dnsmasq)

Here's my example:

```
#Disable DNS Server
port=0
server=192.168.0.1

#Enable DHCP logging
log-dhcp

#Respond to PXE request for the specified network;
#Run as DHCP proxy
dhcp-range=192.168.0.0,proxy,255.255.255.0

dhcp-boot=pxelinux.0

#Flag forces "simple and safe" behaviour
dhcp-no-override

#Provide network boot option called "Network Boot"
pxe-service=x86PC,"Network Boot",pxelinux

enable-tftp
tftp-root=/srv/tftpboot
```

It can support both BIOS and UEFI clients by serving them specific files depending on their architecture.



But proxy DHCP and TFTP worked fine till the known bug in Grub 2.06, but with DHCP and TFTP enabled in dnsmasq you can get

```
pxe e16 error no offer were received
```

{% content-ref url="troubleshooting/pxe-e16-no-offer-received.md" %}
[pxe-e16-no-offer-received.md](troubleshooting/pxe-e16-no-offer-received.md)
{% endcontent-ref %}

To debug this situation I started collecting TCP packets on Raspberry Pi with

```
sudo tcpdump -i eth0 udp port 67 and port 68 -e -n -vv
```

So really, less packet, no offer.

{% file src=".gitbook/assets/proxyDHCP.log" %}
proxy DHCP and TFTP
{% endfile %}

{% file src=".gitbook/assets/dnsmasqDHCP.log" %}
DHCP and TFTP
{% endfile %}

I assume there is bug in dnsmasq. And because I already disabled DHCP server on my router nothing can stop me to test another standalone DHCP and TFTP server.
