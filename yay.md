# yay
```sh
sudo pacman -S git
```
```sh
cd /opt
sudo git clone https://aur.archlinux.org/yay.git
```
change the owner of the source directory. replace “username” with your own user name.
```sh
sudo chown -R <username>:wheel ./yay
```
go to the directory and compile.
```sh
makepkg -si
```
# using yay to install packages
```sh
yay -S <package>
```
# yay tips
refresh the system packages and upgrade:
```sh
yay -Syyu
```
purge package:
```sh
yay -Rns <package>
```
get system status:
```sh
yay -Ps
```