---
description: In case proxy DHCP doesn't work for you configuration
---

# The case without proxy DHCP

In case proxy DHCP doesn't work for you configuration, then you should install DHCP server on Raspberry Pi.

This happened to me with UEFI clients. First you are fighting with Secure Boot and you find shim that is signed by Microsoft. Then shim loads grub and grub tries to load some stuff from the main DHCP server, but not from your proxy DHCP. This is the point where you cannot fight further with just your grandma's router. Not to every router you can add DHCP option 66. But your Pie can server as DHCP server. Hoorah!

{% hint style="warning" %}
Do not forget to stop dnsmasq service before starting isc-dhcp-server [https://raspberrypi.stackexchange.com/questions/101038/isc-dhcp-server-cant-bind-to-dhcp-address](https://raspberrypi.stackexchange.com/questions/101038/isc-dhcp-server-cant-bind-to-dhcp-address)
{% endhint %}

Install real cool DHCP server and TFTP server

```bash
apt install -y isc-dhcp-server tftpd-hpa
```

Update configuration

**/etc/dhcp/dhcpd.conf**

```
default-lease-time 600;
max-lease-time 7200;

allow booting;

option space pxelinux;
option pxelinux.magic code 208 = string;
option pxelinux.configfile code 209 = text;
option pxelinux.pathprefix code 210 = text;
option pxelinux.reboottime code 211 = unsigned integer 32;
option architecture-type code 93 = unsigned integer 16;

# in this example, we serve DHCP requests from 192.168.0.(20 to 50)
# and we have a router at 192.168.0.1
subnet 192.168.0.0 netmask 255.255.255.0 {
  option routers 192.168.0.1;             # our router
  range 192.168.0.20 192.168.0.50;

  option broadcast-address 192.168.0.255;
  option domain-name-servers 192.168.0.1; # our router, again
  class "pxeclients" {
    match if substring (option vendor-class-identifier, 0, 9) = "PXEClient";
    # bootp style option
    next-server 192.168.0.11;
    # DHCP style option
    option tftp-server-name "192.168.0.11";
    if option architecture-type = 00:10 {
      filename "grub/http.efi";
    } elsif option architecture-type = 00:0f {
      filename "grub/http32.efi";
    } elsif option architecture-type = 00:09 {
      filename "grub/bootx64.efi";
      option boot-size 2344;
    } elsif option architecture-type = 00:07 {
      filename "grub/bootx64.efi";
      option boot-size 2344;
    } elsif option architecture-type = 00:06 {
      filename "grub/bootx32.efi";
      option boot-size 2344;
    } else {
      filename "pxelinux.0";
    }
  }
}
```

Set your interface name in **/etc/default/isc-dhcp-server**

```
INTERFACESv4="eth0"
```

And setup TFTP in **/etc/default/tftpd-hpa**

{% code title="/etc/default/tftpd-hpa" %}
```
TFTP_USERNAME="pi"
TFTP_DIRECTORY="/srv/tftpboot"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="-l -a 0.0.0.0:69 -4 -v --secure"
```
{% endcode %}

{% hint style="warning" %}
Do not remove `--secure` flag! It is not about security, without it PXE boot just breaks.
{% endhint %}
