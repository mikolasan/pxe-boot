---
description: Take it from your favorite distribution
---

# Where to get grubx64.efi?

## shimx64.efi

Part of **shim-signed** package in Ubuntu

Package info [https://packages.ubuntu.com/source/bionic/shim-signed](https://packages.ubuntu.com/source/bionic/shim-signed)

Direct link [http://archive.ubuntu.com/ubuntu/pool/main/s/shim-signed/shim-signed\_1.34.9.tar.xz](http://archive.ubuntu.com/ubuntu/pool/main/s/shim-signed/shim-signed\_1.34.9.tar.xz)&#x20;

```
tar xf shim-signed_1.34.9.tar.xz
```

shim is the first of Secure Boot process that is signed by Microsoft. shim pretends to be legit for Secure Boot checks and then loads grubx64.efi. That's all it does. Proof:

```
hexdump -C shimx64.efi.signed.orig | grep "g.r.u.b"
000afeb0  67 00 72 00 75 00 62 00  78 00 36 00 34 00 2e 00  |g.r.u.b.x.6.4...|
```

Read more here [https://blog.learningtree.com/how-does-linux-boot-part-3-uefi-to-shim-to-the-next-link-in-the-chain/](https://blog.learningtree.com/how-does-linux-boot-part-3-uefi-to-shim-to-the-next-link-in-the-chain/)

## grubx64.efi

Part of **grub-efi-amd64-signed** package

Package info [https://packages.ubuntu.com/bionic/amd64/grub-efi-amd64-signed/download](https://packages.ubuntu.com/bionic/amd64/grub-efi-amd64-signed/download)&#x20;

Direct link [http://security.ubuntu.com/ubuntu/pool/main/g/grub2-signed/grub-efi-amd64-signed\_1.167\~18.04.5+2.04-1ubuntu44.1.2\_amd64.deb](http://security.ubuntu.com/ubuntu/pool/main/g/grub2-signed/grub-efi-amd64-signed\_1.167\~18.04.5+2.04-1ubuntu44.1.2\_amd64.deb)

Unzip deb package on Manjaro is tricky. Just use GUI archive manager it can do the trick. CLI unzip didn't work for me.

```
cp shim-signed-deb/data/usr/lib/shim/shimx64.efi.signed .
sha256sum shimx64.efi.signed
8728305fc438a5b230f90056606aaf98669ffee946765b3b373eeabd1aa8b88d
cp grub2-signed-deb/data/usr/lib/grub/x86_64-efi-signed/grubnetx64.efi.signed .
sha256sum grubnetx64.efi.signed
3ebbd970bcb0386d485506285ac6d2c59bd4d5267e7b46caf90c8f9e2668ba86
cp shimx64.efi.signed bootx64.efi
cp grubnetx64.efi.signed grubx64.efi
cp ubuntu-18.04.5-desktop-amd64.iso/boot/grub/{grub.cfg,font.pf2} .
```

## Reference

All this steps with pictures on [https://c-nergy.be/blog/?p=13334](https://c-nergy.be/blog/?p=13334)

