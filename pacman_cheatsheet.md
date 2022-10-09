# Pacman CheatSheet

https://archlinux.org/pacman/pacman.8.html

## SystemUpgrade
synchronize with remote repository (full update & upgrade of system + packages)
```sh
pacman -Syu
```
## Search
search for a package in remote repository
```sh
pacman -Ss package
```
search for a package in local repository
```sh
pacman -Qs package
```
search for packages owning certain files
```sh
pacman -F file
```
display files owned by certain packages
```sh
pacman -Fl package
```
## Install & Remove
install a package
```sh
pacman -S package
```
completely remove a package, including dependencies and files
```sh
pacman -Rns package
```
remove obsolete (old version) packages
```sh
pacman -Sc
```
## List
list installed packages
```sh
pacman -Q
```
list installed packages you explicitly installed
```sh
pacman -Qe
```
list installed packages from main repositories
```sh
pacman -Qn
```
list installed packages from the Arch User Repository
```sh
pacman -Qm
```
list unneeded/obsolete dependencies 
```sh
pacman -Qdt
```
## Configuring pacman
the pacman configuration file is located in __/etc/pacman.conf__ 
```
vim /etc/pacman.conf
```
```
Color
VerbosePkgLists
ILoveCandy
```
the pacman mirrorlist is loacted in __/etc/pacman.d/mirrorlist__
```
vim /etc/pacman.d/mirrorlist
```