# Notes and Errors
___
## Notes
___
switch tty using `alt` + `F2, F3,...`
___
### clipboards and selections (copy paste stuff)
linux has 3 "(clipboards/selections)?" which contents can be pasted/accessed by:
    1. middle mouse button
    2. ctrl + shift + v
    3. shift + insert
you can copy text by using:
    1. rightclick
    2. ctrl + shift +c
    3. application copy (rightclick selection menu)
___
### sysctl commands without password
```sh
echo "%wheel ALL=(ALL) NOPASSWD: /usr/bin/poweroff,/usr/bin/shutdown,/usr/bin/reboot,/usr/bin/mount,/usr/bin/umount,/usr/bin/pacman -Syu,/usr/bin/pacman -Syyu,/usr/bin/pacman -Syyu --noconfirm,/usr/bin/pacman -Syyuw --noconfirm" > /etc/sudoers.d/sysctl
```
___
### Disable the traumatizing BEEP noise once and for all!

https://wiki.archlinux.org/title/PC_speaker

The PC speaker can be disabled by unloading the pcspkr kernel module:
```sh
rmmod pcspkr
```

Blacklisting the pcspkr module will prevent udev from loading it at boot. Create a file `/etc/modprobe.d/nobeep.conf`:
```txt
blacklist pcspkr
```

```sh
sudo echo 'blacklist pcspkr' > /etc/modprobe.d/nobeep.conf
```

Blacklisting it on the kernel command line is yet another way. Simply add `modprobe.blacklist=pcspkr` to your bootloader's kernel line.
___
## Errors
___
error:
```log
:: proceed with installation? [Y/n] y
(110/110) checking keys iin keyring
(110/110) checking package integrity
error: package: signature from "user <user@email.org>" is unknown trust
:: File /var/cache/pacman/pkg/package.tar.zst is corrupted (invalid or corrupted package (PGP signature)).
Do you want to delete it? [Y/n] n
error: failed to commit transaction (invalid or corrupted package)
```

fix:
```sh
sudo pacman -S archlinux-keyring
```

source: https://www.youtube.com/watch?v=1WHVIYXXOgQ
___


