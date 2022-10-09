
# [__Arch Linux Installation x86_64 EFI__](https://wiki.archlinux.org/title/Installation_guide)

## hard disk drive preparation
format the HDD you want to install the Linux System on and put a `gpt` partition label on it

```sh
gparted
```
-> /dev/nvme0n1 -> delete partitions  
-> drive -> create partitioning-table -> gpt  
-> execute  
or  
```sh
parted /dev/nvme0p1
# delete all partitions
(parted) print
(parted) rm NUMBER 
# make gpt label
(parted) mklabel gpt
(parted) quit
```
___

## booting

* press F1 while system boot -> BIOS/EFI
  * disable secure boot
* press F12 to select the boot device
  * select option *Arch Linux Install medium (x86_64, EFI)*

#  Installation

## initial config

set keyboard-layout
```sh
loadkeys de-latin1
```
check for network connection
```sh
ip link
ping lukesmith.xyz
```
to connect via Wi-Fi try:
```
iwctl
[iwd]# device list
[iwd]# station <wlan0> get-networks
[iwd]# station <wlan0> connect <SSID>
[iwd]# //enter passphrase
[iwd]# station <wlan0> show
[iwd]# exit

ping lukesmith.xyz
```
___
#### __NOTE__
In the installation image, `systemd-networkd`, `systemd-resolved`, `iwd` and `ModemManager` are preconfigured and enabled by default. That will **not** be the case for the newly installed system.
___
update the system clock
```sh
timedatectl set-ntp true
```

## partitioning
___
|#|description|usage|
|---|---|---|
|1.|__EFI__|`bootloader`|
|2.|__SWAP__|`(optional) RAM for hibernation / paging file`|
|3.|__/__|`root / LinuxFilesystem`|
|4.|__/home__|`(optional) seperate home partition`|
___
| directory				      | usage             | memory    | type  |device-directory|mount-directory|
| --------------------- | :-------:         | :------:  | :---: |---|---|
| /boot/efi				      | EFI-partition     | 960M      | FAT32 |/dev/nvme0n1p1|/mnt/boot/efi|
| 		                  | swap-Partition    | 24G       |       |/dev/nvme0n1p2||
| /                     | systempartition   | 88GB      | ext4  |/dev/nvme0n1p3|/mnt|
| /home                 | home partition    | 364GB     | ext4  |/dev/nvme0n1p4| /mnt/home |
___

```
960M    Linux efi           /dev/nvme0n1p1    /mnt/boot/efi
476G    Linux filesystem    /dev/nvme0n1p2    /mnt
```
___
list devices
```sh
lsblk
```
create partitions
```sh
cfdisk /dev/nvme0n1
```
check partitions
```sh
lsblk
```
make/format filesystems
```sh
mkfs.fat -F32 /dev/nvme0n1p1
mkfs.ext4 /dev/nvme0n1p3
mkfs.ext4 /dev/nvme0n1p4
mkswap /dev/nvme0n1p2
```
mount partitions
```sh
mount /dev/nvme0n1p3 /mnt
mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p1 /mnt/boot/efi
mkdir /mnt/home
mount /dev/nvme0n1p4 /mnt/home
swapon /dev/nvme0n1p2
```
check mounts
```sh
lsblk
```
___
## (optional) optimize mirrorlist for installation
backup mirrorlist
```sh
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bkp
```
install rankmirrors tool
```sh
pacman --noconfirm -S pacman-contrib 
```
optimize mirrorlist
```sh
rankmirrors -n 6 /etc/pacman.d/mirrorlist.bkp > /etc/pacman.d/mirrorlist
```
___
## (optional) enable multilib for Steam :)
edit `/etc/pacman.conf` and uncomment the `[multilib]` section:
```
/etc/pacman.conf

[multilib]
Include = /etc/pacman.d/mirrorlist
```

## installing linux core

```sh
pacstrap -i /mnt linux linux-firmware base base-devel vim
```
___
## basic configuring
fstab -U = UUID, -L = Label
```sh
genfstab -U /mnt >> /mnt/etc/fstab
```
change root (chroot) into system
```sh
arch-chroot /mnt
```
set time-zone
```sh
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
```
syn hardware clock
```sh
hwclock --systohc
```
localization  
edit `/etc/locale.gen` and uncomment `de_DE.UTF-8 UTF-8` and other needed locales. generate the locales by running:
```sh
/etc/locale.gen
locale-gen
```
create the `locale.conf` file, and set the `LANG` variable accordingly:
```sh
/etc/locale.conf

LANG=de_DE.UTF-8
LANG=en_US.UTF-8
```
set keyboard layout
```sh
/etc/vconsole.conf
KEYMAP=de-latin1
```
set hostname
```sh
/etc/hostname
hostname
```
configure hosts file
```sh
/etc/hosts
127.0.0.1   localhost
::1         localhost
127.0.1.1   hostname.localdomain hostname
```
installing packages
```sh
pacman -S zsh dash man-db man-pages git zsh-completions bash-completion
```
set root password
```sh
passwd
```
## grub bootloader
install grub bootloader
```sh    
pacman -S grub efibootmgr

grub-install --target=x86_64-efi --bootloader=GRUB --efi-directory=/boot/efi --removable
```
generate grub config
```sh
grub-mkconfig -o /boot/grub/grub.cfg
```
change bootloader resolution of grub.cfg
```sh
vim /etc/default/grub

# GRUB_GFXMODE=AUTO
GRUB_GFXMODE=1920x1080
```
regenerate grub config
```sh
grub-mkconfig -o /boot/grub/grub.cfg
```

## Network Configuration
complete the network configuration for the newly installed environment, that may include installing suitable network management software.

[__NetworkManager__](https://wiki.archlinux.org/title/NetworkManager)
```sh
pacman -S networkmanager
systemctl enable NetworkManager
```
___
```sh
exit
umount -R /mnt
reboot
shutdown -h now
```
___
NetworkManager comes with __nmcli__ and __nmtui__.

### __nmcli__
List nearby Wi-Fi networks:
```sh
nmcli device wifi list
```
Connect to a Wi-Fi network:
```sh
nmcli device wifi connect <SSID_or_BSSID> password <password>
```
Connect to a Wi-Fi on the wlan1 interface:
```sh
nmcli device wifi connect <SSID_or_BSSID> password <password> ifname wlan1 <profile_name>
```
Disconnect an Interface:
```sh
nmcli device disconnect ifname eth0
```
Get a list of connections with their names, UUIDs, types and backing devices:
```sh
nmcli connection show
```
Activate a connection (i.e. connect to a network with an existing profile):
```sh
nmcli connection up <name_or_uuid>
```
Delete a connection:
```sh
nmcli connection delete <name_or_uuid>
```
See a list of network devices and their state:
```sh
nmcli device
```
Turn off Wi-Fi:
```sh
nmcli radio wifi off
```
Get list of connections:
```sh
nmcli connection
```
### __nmtui__
```
nmtui - Text User Interface for controlling NetworkManager
```

### __NetworkManager configuration file:__
```
/etc/NetworkManager/NetworkManager.conf
```
Usually no configuration needs to be done to the global defaults.
After editing a configuration file, the changes can be applied by running:
```
nmcli general reload
```

# post-installation config

add a user
```sh
useradd -m <user> -s /bin/zsh
```
add user to wheel group
```sh
usermod -aG wheel <user>
```
set user password
```sh
passwd <user>
```
modify user shell
```sh
usermod <user> -s /bin/<shell>
```
uncomment wheel group to allow members to execute any command
```sh
EDITOR=vim visudo

# %wheel ALL=(ALL) ALL
```
uncomment to allow members of the wheel group to execute any command without having to provide a password
```sh
<user> ALL=(ALL) NOPASSWD: ALL
```
reboot into the installed ArchLinux
```sh
poweroff
```
___

symlink /bin/sh to /bin/dash
```sh
#/bin/sh -> /bin/dash
ln -sf /bin/dash /bin/sh
ls -al /bin/sh
```

## post installation recommendations
+ [general recommendations](https://wiki.archlinux.org/title/General_recommendations)
+ [disable the traumatizing "BEEP!" noise](./notes-n-errors.md)
+ [Microcode + LTS-Kernel](./Microcode%2BLTS-Kernel.md)
+ [AUR Helper - yay](./yay.md)
+ [window manager](./suckless.md)
+ [enable audio](./audio.md)
