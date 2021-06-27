---
layout: default
title: "Boot Archcraft with GRUB"
parent: Boot Archcraft
nav_order: 2
---

# Boot Archcraft ISO with grub2 bootloader

If you already have Linux installed on your system and want to try Archcraft without making a bootable USB drive, you can do so with the GRUB2 bootloader.
Follow the steps below to boot Archcraft ISO with GRUB2 :

- Open terminal and edit **/etc/grub.d/40_custom** with your favorite text editor.

```bash
$ sudo vim /etc/grub.d/40_custom
```

- Add the following entry in the file, replace **(hd0,X)** with your root partition, e.g. (hd0,2) and **/path/to/archcraft.iso** with your Archcraft ISO path.

```bash
menuentry "Archcraft Live ISO" --class archcraft {
    set root='(hd0,X)'
    set isofile="/path/to/archcraft.iso"
    set dri="free"
    search --no-floppy -f --set=root $isofile
    probe -u $root --set=abc
    set pqr="/dev/disk/by-uuid/$abc"
    loopback loop $isofile
    linux  (loop)/arch/boot/x86_64/vmlinuz-linux img_dev=$pqr img_loop=$isofile driver=$dri quiet splash vt.global_cursor_default=0 loglevel=2 rd.systemd.show_status=false rd.udev.log-priority=3 sysrq_always_enabled=1 cow_spacesize=2G
    initrd  (loop)/arch/boot/intel-ucode.img (loop)/arch/boot/amd-ucode.img (loop)/arch/boot/x86_64/initramfs-linux.img
}
```

- Save the file and update **grub.cfg**, the GRUB configuration file.

```bash
# On Arch Linux
$ sudo grub-mkconfig -o /boot/grub/grub.cfg
						
# On Ubuntu & its derivatives
$ sudo update-grub
```

- After updating the config file, Reboot the system and boot into Archcraft Live. Try it out, or maybe install it on a USB or SDcard.
