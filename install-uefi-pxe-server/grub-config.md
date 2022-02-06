# GRUB config

{% code title="grub.cfg" %}
```
if loadfont /srv/tftpboot/grub/font.pf2 ; then
        set gfxmode=auto
        insmod efi_gop
        insmod efi_uga
        insmod gfxterm
        terminal_output gfxterm
fi

set menu_color_normal=white/black
set menu_color_highlight=black/light-gray

set timeout=15

menuentry "Our system" {
        set gfxpayload=keep
        linux   /pxe_system/vmlinuz root=/dev/nfs ip=dhcp nfsroot=192.168.0.11:/srv/tftpboot/pxe_system,tcp,v3 rootfstype=ext4 rw --
        initrd  /pxe_system/initrd.img
}

menuentry "Kali" {
        set gfxpayload=keep
        linux   /kali/live/vmlinuz root=/dev/nfs ip=dhcp nfsroot=192.168.0.11:/srv/tftpboot/kali,tcp,v3 rootfstype=ext4 rw --
        initrd  /kali/live/initrd.img
}
```
{% endcode %}
