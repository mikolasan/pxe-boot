# Unable to fetch TFTP image: Invalid Parameter

When you use dnsmasq in proxy mode and you need PXE Boot in UEFI mode on machine with Secure Boot enabled, then PXE will work, TFTP will work,&#x20;

![](<../.gitbook/assets/2021-08-05 13-58-47.PNG>)

shim will work, but next step apparently failed. Check your logs:

```
Jul 28 18:27:16 dnsmasq-tftp[28673]: sent /srv/tftpboot/efi64/bootx64.efi to 192.168.0.161
Jul 28 18:27:16 dnsmasq-tftp[28673]: file /srv/tftpboot/efi64/grubx64.efi not found
```

(where bootx64.efi is jusr a renamed shimx64.efi)

Also grubnetx64.efi might not work

Grub 2.06 cannot work with DHCP proxy. It will try to use master DHCP server.

It's a known bug [https://bugzilla.redhat.com/show\_bug.cgi?id=1809246](https://bugzilla.redhat.com/show\_bug.cgi?id=1809246)

As [Dan Berkowitz](https://serverfault.com/users/496361/dan-berkowitz) says here [https://github.com/rhboot/shim/issues/111#issuecomment-438790478](https://github.com/rhboot/shim/issues/111#issuecomment-438790478) and here [https://serverfault.com/questions/907298/dnsmasq-proxydhcp-pxe-and-efi-start-image-returned-invalid-parameter](https://serverfault.com/questions/907298/dnsmasq-proxydhcp-pxe-and-efi-start-image-returned-invalid-parameter) it is possible to fix with additional setting on master DHCP server.

They have tried to fix it in Grub in Fedora [https://github.com/rhboot/grub2/pull/82](https://github.com/rhboot/grub2/pull/82)
