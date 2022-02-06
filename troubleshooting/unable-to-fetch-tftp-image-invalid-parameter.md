# Unable to fetch TFTP image: Invalid Parameter

When you use dnsmasq in proxy mode and you need PXE Boot in UEFI mode on machine with Secure Boot enabled, then PXE will work, TFTP will work, shim will work, but grubnetx64.efi will not.

![](<../.gitbook/assets/2021-08-05 13-58-47.PNG>)

Grub 2.06 cannot work with DHCP proxy. It will try to use master DHCP server.

It's a known bug [https://bugzilla.redhat.com/show\_bug.cgi?id=1809246](https://bugzilla.redhat.com/show\_bug.cgi?id=1809246)

As [Dan Berkowitz](https://serverfault.com/users/496361/dan-berkowitz) says here [https://github.com/rhboot/shim/issues/111#issuecomment-438790478](https://github.com/rhboot/shim/issues/111#issuecomment-438790478) and here [https://serverfault.com/questions/907298/dnsmasq-proxydhcp-pxe-and-efi-start-image-returned-invalid-parameter](https://serverfault.com/questions/907298/dnsmasq-proxydhcp-pxe-and-efi-start-image-returned-invalid-parameter) it is possible to fix with additional setting on master DHCP server.

They have tried to fix it in Grub in Fedora [https://github.com/rhboot/grub2/pull/82](https://github.com/rhboot/grub2/pull/82)
