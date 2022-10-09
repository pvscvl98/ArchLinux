# LTS-Kernel Installation / GRUB Configuration / Microcode

Install LTS-Kernel 
```
sudo pacman -S linux-lts
```
make grub config
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
reboot system
```
sudo reboot
```

---

## configure grub to use normal kernel  

note that any changes directly written to _/boot/grub/grub.cfg_  
will be overwritten by the content of _/etc/default/grub_ on _grub-mkconfig_.  
therefore make sure to set your _/etc/default/grub_ settings (color, background image ...)  
__before__ making any changes to _/boot/grub/grub.cfg_.  
the following configuration will be __appended__ to the _/boot/grub/grub_.cfg made by grub-mkconfig

check your /boot directory for Linux LTS Kernel
```
ls -l /boot

efi/
grub/
initramfs-linux-fallback.img
initramfs-linux.img
initramfs-linux-lts-fallback.img
initramfs-linux-lts.img
vmlinuz-linux
vmlinuz-linux-lts
```

edit the first menuentry
```
/boot/grub/grub.cfg

menuentry 'Arch Linux' ... {
    ...
    echo    'Loading Linux linux ...'
    linux   '/boot/vmlinuz-linux root=UUID='
    initrd  '/boot/initramfs-linux.img'
}
```

---

## GRUB menuentries

append the menuentries to /boot/grub/grub.cfg in the 40_custom entry:
```
### BEGIN /etc/grub.d/40_custom ###

menuentry "entry name" {
    echo "entry message"
        command
}

### END /etc/grub.d/40_custom ###
```

system restart
```
menuentry "System restart" {
    echo "System rebooting ..."
        reboot
}
```

system shutdown
```
menuentry "System shutdown" {
    echo "System shutting down ..."
        halt
}
```

https://unix.stackexchange.com/questions/8690/what-is-the-difference-between-halt-and-shutdown-commands/196471#196471
https://unix.stackexchange.com/questions/205464/whats-the-difference-between-poweroff-and-halt
---



# Microcode

install the appropriate microcode package
```
sudo pacman -S amd-ucode
```
manually add the microcode update to the /boot/grub/grub.cfg file

repeat this for each entry
```
vim /boot/grub/grub.cfg

...
echo 'Loading initial ramdisk'
initrd	/boot/amd-ucode.img /boot/initramfs-linux.img
...
```
make grub config
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
---

## Sources
+ [Arch Wiki - GRUB](https://wiki.archlinux.org/title/GRUB)
+ [Arch Wiki - Kernel Parameters#GRUB](https://wiki.archlinux.org/title/Kernel_parameters#GRUB)
---
+ [Arch Wiki - Microcode](https://wiki.archlinux.org/title/Microcode#Configuration)