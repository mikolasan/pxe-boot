# Secure Boot and custom certificates

Let's say you have a laptop with Windows installed on it by its manufacturer (so called OEM version). You want to keep Secure Boot enabled, but you also want to boot some custom Linux from USB drive or through the network via PXE.

![Secure Boot Enabled](<../.gitbook/assets/2021-08-05 12-49-45.JPG>)

You need to sign a kernel, otherwise you get this error&#x20;

{% content-ref url="bzimage-has-invalid-signature.md" %}
[bzimage-has-invalid-signature.md](bzimage-has-invalid-signature.md)
{% endcontent-ref %}

You normally would follow instructions like this [https://ubuntu.com/blog/how-to-sign-things-for-secure-boot](https://ubuntu.com/blog/how-to-sign-things-for-secure-boot), where new signature is installed by mokutil, but you are on Windows right now.

According to Eclypsium in [https://eclypsium.com/2020/07/29/theres-a-hole-in-the-boot/](https://eclypsium.com/2020/07/29/theres-a-hole-in-the-boot/) there is Kaspersky Rescue Disk 18 [https://support.kaspersky.com/krd18](https://support.kaspersky.com/krd18) that bypasses Secure Boot. Official build is hardened with signatures, but hacky version still can be found online ([https://usbtor.ru/viewtopic.php?p=65909](https://usbtor.ru/viewtopic.php?p=65909)). It will work only if you don't update your Windows (since somewhere like 2020), because UEFI Forum already have this bootloader in the revocation list ([https://uefi.org/revocationlistfile](https://uefi.org/revocationlistfile))

```
30: {microsoft} {sha256} 81d8fb4c9e2e7a8225656b4b8273b7cba4b03ef2e9eb20e0a0291624eca1ba86
```



Probably a great article [http://www.rodsbooks.com/efi-bootloaders/secureboot.html](http://www.rodsbooks.com/efi-bootloaders/secureboot.html), but good God Roderick W. Smith, I fall asleep after each paragraph, I don't know which note is important, I read information and don't know how to apply it.

## Packages to your system

* sbsigntools
* pesign

## Add to ESP partition

* public key (for MOK),
* shimx64.efi (Secure boot solution from Matthew J. Garrett),
* mmx64.efi (mm stands for MOK Manager),
* grubx64.efi (GRUB 2)

Where to get this files?

```
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.key -out MOK.crt \\
  -nodes -days 3650 -subj "/CN=Your Name/"
openssl x509 -in MOK.crt -out MOK.cer -outform DER
```

```bash
git clone https://github.com/rhboot/shim.git
cd shim
git submodule update
make
```

Following tutorial from [https://doc.opensuse.org/documentation/leap/reference/html/book-reference/cha-uefi.html](https://doc.opensuse.org/documentation/leap/reference/html/book-reference/cha-uefi.html)

```
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.key -out MOK.crt -nodes -days 3650 -subj "/CN=Ethogaming/"
openssl x509 -in MOK.crt -out MOK.cer -outform DER

openssl pkcs12 -export -inkey MOK.key -in MOK.crt \
-name kernel_cert -out cert.p12

certutil -d . -N

pk12util -d . -i cert.p12
pesign -n . -c kernel_cert -i bzImage -o vmlinuz.signed -s
```

Getting an error

```
pesign: could not parse signature list in EFI binary
```

Later I realized ([https://github.com/rhboot/pesign/issues/64](https://github.com/rhboot/pesign/issues/64)) that my kernel should have `CONFIG_EFI_STUB` enabled [https://www.kernel.org/doc/html/latest/admin-guide/efi-stub.html](https://www.kernel.org/doc/html/latest/admin-guide/efi-stub.html)

Finding another tutorials

* [https://en.opensuse.org/openSUSE:UEFI\_Image\_File\_Sign\_Tools](https://en.opensuse.org/openSUSE:UEFI\_Image\_File\_Sign\_Tools)
* [https://media.defense.gov/2020/Sep/15/2002497594/-1/-1/0/CTR-UEFI-Secure-Boot-Customization-UOO168873-20.PDF](https://media.defense.gov/2020/Sep/15/2002497594/-1/-1/0/CTR-UEFI-Secure-Boot-Customization-UOO168873-20.PDF)
* [https://ubuntu.com/blog/how-to-sign-things-for-secure-boot](https://ubuntu.com/blog/how-to-sign-things-for-secure-boot)

And trying

```
openssl x509 -in MOK.cer -inform DER -outform PEM -out MOK.pem
sbsign --key MOK.key --cert MOK.pem --output bzImage.signed bzImage
```

And getting an error

{% content-ref url="../troubleshooting/invalid-dos-header-magic.md" %}
[invalid-dos-header-magic.md](../troubleshooting/invalid-dos-header-magic.md)
{% endcontent-ref %}

### Sign the kernel

```
sbsign --key ~/efitools/MOK.key --cert ~/efitools/MOK.crt \\
         --output vmlinuz-signed.efi vmlinuz.efi
warning: file-aligned section .text extends beyond end of file
warning: checksum areas are greater than image size. Invalid section table?
```

### How to compile grubx64.efi

bug [https://savannah.gnu.org/bugs/index.php?55636](https://savannah.gnu.org/bugs/index.php?55636)

[https://github.com/rhboot/grub2/pull/82](https://github.com/rhboot/grub2/pull/82)

[https://bugzilla.redhat.com/show\_bug.cgi?id=1809246](https://bugzilla.redhat.com/show\_bug.cgi?id=1809246)

[https://www.gnu.org/software/grub/grub-download.html](https://www.gnu.org/software/grub/grub-download.html)

git clone [https://git.savannah.gnu.org/git/grub.git](https://git.savannah.gnu.org/git/grub.git)

patch

[https://lists.gnu.org/archive/html/grub-devel/2014-04/msg00091.html](https://lists.gnu.org/archive/html/grub-devel/2014-04/msg00091.html)
