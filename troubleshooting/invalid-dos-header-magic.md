# Invalid DOS header magic

Happens when you try to sign not EFI file. For example doing this

```
sbsign --key MOK.key --cert MOK.crt --output bzImage.efi.signed bzImage
```

To sign a kernel you need the kernel to be build with a special option `CONFIG_EFI_STUB` [https://www.kernel.org/doc/html/latest/admin-guide/efi-stub.html](https://www.kernel.org/doc/html/latest/admin-guide/efi-stub.html). As a result kernel is built with different name: `bzImage.efi`.

## Reference

* [https://unix.stackexchange.com/questions/596455/cryptic-error-when-attempted-to-sign-kernel-modules](https://unix.stackexchange.com/questions/596455/cryptic-error-when-attempted-to-sign-kernel-modules)
* [https://bentley.link/secureboot/](https://bentley.link/secureboot/)
* [https://www.wzdftpd.net/blog/tag/uefi.html](https://www.wzdftpd.net/blog/tag/uefi.html)

Now I have an answer [https://github.com/rhboot/pesign/issues/64](https://github.com/rhboot/pesign/issues/64)
