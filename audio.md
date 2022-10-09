# Audio
## basic audio for conzoomers, using ALSA
usually the following will do the trick and you will have basic audio by ALSA
```sh
pacman -S alsa-utils sof-firmware alsa-ucm-conf
```

## [pipewire](https://wiki.archlinux.org/title/PipeWire)
```sh
pacman -S pipewire pipewire-alsa pipewire-pulse pipewire-jack pipewire-docs wireplumber wireplumber-docs pulsemixer
```

```sh
systemctl --user enable pipewire pipewire-pulse wireplumber
```

on error:
```sh
systemctl --user restart pipewire pipewire-pulse wireplumber
```

sources:

https://www.reddit.com/r/archlinux/comments/m7yc6j/pipewire_0324_no_audio_devices_found/