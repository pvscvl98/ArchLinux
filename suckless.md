# suckless
```sh
sudo pacman -Syu
```
make parent directory:
```
mkdir ~/suckless && cd ~/suckless
```
[**dwm**](https://dwm.suckless.org/)
```
git clone https://git.suckless.org/dwm
```
[**dmenu**](https://tools.suckless.org/dmenu/)
```
git clone https://git.suckless.org/dmenu
```
[**st**](https://st.suckless.org/)
```
git clone https://git.suckless.org/st
```

packages needed to compile:
```sh
sudo pacman -S xorg-server xorg-xinit xorg-xsetroot xorg-xrandr xorg-xrdb libxinerama libxft
```

compile and install:
```sh
cd dwm && sudo make clean install && cd - && \
cd dmenu && sudo make clean install && cd - && \
cd st && sudo make clean install && cd -
```

```sh
echo "exec dwm" > ~/.xinitrc
```

```sh
startx
```
