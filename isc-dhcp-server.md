# ISC DHCP Server

If you copy-paste configuration for isc-dhcp-server from [https://wiki.debian.org/PXEBootInstall](https://wiki.debian.org/PXEBootInstall), then you will get several issues immediately.&#x20;

```
default-lease-time 600;
max-lease-time 7200;

allow booting;

# in this example, we serve DHCP requests from 192.168.0.(3 to 253)
# and we have a router at 192.168.0.1
subnet 192.168.0.0 netmask 255.255.255.0 {
  range 192.168.0.3 192.168.0.253;
  option broadcast-address 192.168.0.255;
  option routers 192.168.0.1;             # our router
  option domain-name-servers 192.168.0.1; # our router, again
  filename "pxelinux.0"; # (this we will provide later)
}

group {
  next-server 192.168.0.2;                # our Server
  host tftpclient {
    if option architecture-type = 00:07 {
      filename "debian-installer/amd64/bootnetx64.efi";
    } else {
      filename "pxelinux.0";
    }
  }
}
```

So better adapt from a better tutorial from Red Hat [https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/7/html/installation\_guide/chap-installation-server-setup](https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/7/html/installation\_guide/chap-installation-server-setup)

### Unknown option architecture-type

To use an option you need to define that option. See the same if-else block but with option that has another name [https://serverfault.com/questions/926886/failing-to-pxe-boot-uefi-server](https://serverfault.com/questions/926886/failing-to-pxe-boot-uefi-server)

### Check for PXEClient

Here is an example config from Red Hat docs

```
option space pxelinux;
option pxelinux.magic code 208 = string;
option pxelinux.configfile code 209 = text;
option pxelinux.pathprefix code 210 = text;
option pxelinux.reboottime code 211 = unsigned integer 32;
option architecture-type code 93 = unsigned integer 16;

subnet 10.0.0.0 netmask 255.255.255.0 {
	option routers 10.0.0.254;
	range 10.0.0.2 10.0.0.253;

	class "pxeclients" {
	  match if substring (option vendor-class-identifier, 0, 9) = "PXEClient";
	  next-server 10.0.0.1;

	  if option architecture-type = 00:07 {
	    filename "uefi/shim.efi";
	    } else {
	    filename "pxelinux/pxelinux.0";
	  }
	}
}
```

By the way that simple block doesn't cover all 4 possible architecture types: [https://superuser.com/questions/1182862/uefi-mode-pxe-booting-doesnt-work](https://superuser.com/questions/1182862/uefi-mode-pxe-booting-doesnt-work)&#x20;

{% hint style="warning" %}
not "else if", not elseif, not elif, but **elsif**
{% endhint %}

More DHCP configs: [https://wiki.syslinux.org/wiki/index.php?title=PXELINUX#DHCP\_config\_-\_Simple](https://wiki.syslinux.org/wiki/index.php?title=PXELINUX#DHCP\_config\_-\_Simple)

[https://docs.centos.org/en-US/centos/install-guide/pxe-server/](https://docs.centos.org/en-US/centos/install-guide/pxe-server/)
