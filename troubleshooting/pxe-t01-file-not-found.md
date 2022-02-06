# PXE-T01: File not found

![](../.gitbook/assets/20210909\_102611.jpg)

### Check configuration

{% tabs %}
{% tab title="DHCP" %}
{% code title="/etc/dhcp/dhcpd.conf" %}
```
default-lease-time 3600;
max-lease-time 7200;

allow booting;
allow bootp;

option space pxelinux;
option pxelinux.magic code 208 = string;
option pxelinux.configfile code 209 = text;
option pxelinux.pathprefix code 210 = text;
option pxelinux.reboottime code 211 = unsigned integer 32;
option architecture-type code 93 = unsigned integer 16;

subnet 192.168.3.0 netmask 255.255.255.0 {
  option routers 192.168.3.1;             # our router
  range 192.168.3.25 192.168.3.50;

  option broadcast-address 192.168.3.255;
  option domain-name-servers 192.168.3.1; # our router, again
  filename "pxelinux.0";

  class "pxeclients" {
    match if substring (option vendor-class-identifier, 0, 9) = "PXEClient";
    # bootp style option
    next-server 192.168.3.13;
    # DHCP style option
    option tftp-server-name "192.168.3.13";
    if option architecture-type = 00:07 {
      filename "grub/grubx64.efi";
    } else {
      filename "pxelinux.0";
    }
  }

}
```
{% endcode %}
{% endtab %}

{% tab title="TFTP" %}
{% code title="/etc/default/tftpd-hpa" %}
```
TFTP_USERNAME="pi"
TFTP_DIRECTORY="/srv/tftpboot"
TFTP_ADDRESS=":69"
TFTP_OPTIONS="-l -a 0.0.0.0:69 -4 -v --secure"
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Get logs

`sudo journalctl -u tftpd-hpa`

`sudo less /var/log/syslog`

`sudo journalctl -u isc-dhcp-server`&#x20;

### Possible reasons

* missing files
* wrong ownership
* symlinks
