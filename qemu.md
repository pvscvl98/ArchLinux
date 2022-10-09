# [QEMU](https://wiki.archlinux.org/title/QEMU)
https://www.qemu.org/
___

## Details on packages available in Arch Linux:
+ The `qemu-desktop` package provides the `x86_64 architecture emulators for full-system emulation` `(qemu-system-x86_64)`.
+ The `qemu-emulators-full package` provides the `x86_64 usermode` variant `(qemu-x86_64)` and also for the `rest of supported architectures` it includes `both full-system and usermode variants` (e.g. qemu-system-arm and qemu-arm).
+ The headless versions of these packages (only applicable to full-system emulation) are `qemu-base (x86_64-only)` and `qemu-emulators-full (rest of architectures)`.

```sh
sudo pacman -S qemu-base qemu-emulators-full
```

```sh
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
```


# [KVM](https://wiki.archlinux.org/title/KVM)

# [libvirt](https://wiki.archlinux.org/title/libvirt)
https://libvirt.org/
___
Note: If you are using `firewalld`, as of `libvirt 5.1.0` and `firewalld 0.7.0` you no longer need to change the firewall backend to iptables. libvirt now installs a `zone` called `'libvirt'` in firewalld and manages its required network rules there. [Firewall and network filtering in libvirt](https://libvirt.org/firewall.html)

```sh
sudo pacman -S libvirt iptables-nft dnsmasq dmidecode bridge-utils openbsd-netcat
```
Optional: `libguestfs` 
Note: for TPMv2 emulation install: `swtpm`

```sh
sudo usermod -aG libvirt void
sudo usermod -aG libvirt-qemu void
```

### Authenticate with file-based permissions
To define file-based permissions for users in the libvirt group to manage virtual machines, uncomment and define:

`/etc/libvirt/libvirtd.conf`
```txt
#unix_sock_group = "libvirt"
#unix_sock_ro_perms = "0777"  # set to 0770 to deny non-group libvirt users
#unix_sock_rw_perms = "0770"
#auth_unix_ro = "none"
#auth_unix_rw = "none"
```

enable and start the vmnet
```sh
sudo virsh net-start default
sudo virsh net-autostart default

sudo virsh net-list --all
```

configure vmnet:

### Domains
Virtual machines are called domains. If working from the command line, use virsh to list, create, pause, shutdown domains, etc. virt-viewer can be used to view domains started with virsh. Creation of domains is typically done either graphically with virt-manager or with virt-install (a command line program installed as part of the virt-install package).

Creating a new domain typically involves using some installation media, such as an .iso from the storage pool or an optical drive.

Print active and inactive domains:
```sh
virsh list --all
```

# [virt-manager](https://wiki.archlinux.org/title/Virt-Manager)
https://virt-manager.org/
___

```sh
sudo pacman -S virt-manager virt-viewer
```
___

shared clipboard on debian based distros:
on the guest install `spice-vdagent` and restart the machine.

___

sources:

https://www.youtube.com/watch?v=wxxP39cNJOs