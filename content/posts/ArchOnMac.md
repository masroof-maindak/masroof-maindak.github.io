---
title: "'Reviving' an old Macbook Pro with Arch Linux"
date: 2023-08-25T02:09:13+05:00
draft: true
---

<br>This entire debacle began when I recieved an almost decade old Macbook pro. Apart from a degenerating battery, it was quite functional. The only problem? Factory restarting it and losing my Big Sur install.


## Keyboard
Add the following lines

```
options hid-apple swap_fn_leftctrl=1
options hid_apple swap_opt_cmd=1
```
in `/etc/modprobe.d/hid_apple.conf`, regenerate your initramfs* w/ `mkinitcpio -p Linux`, then reboot your Mac for a traditional keyboard experience.

*_If, for whatever reason, you're unable to run mkinitcpio, just redownload the Linux kernel - `sudo pacman -S linux`._

## Scaling

Add `Xft.dpi: 192` in `~/.Xresources` (make sure to merge with xrdb in your `.xinitrc`), and

```
export QT_AUTO_SCREEN_SCALE_FACTOR=1
export GDK_SCALE=2
export GDK_DPI_SCALE=0.5
```
in `~/.bash_profile` for GTK + QT apps.

## Touchpad

Add the following to `/etc/X11/xorg.conf.d/30-touchpad.conf`:
```
Section "InputClass"
    Identifier "touchpad"
    Driver "libinput"
    MatchIsTouchpad "on"
    Option "Tapping" "on"
    Option "NaturalScrolling" "true"
    Option "ClickMethod" "clickfinger"
    Option "AccelProfile" "flat"
    Option "DisableWhileTyping" "0"
    Option "TransformationMatrix" "1 0 0 0 1 0 0 0 0.3"
EndSection
```
For natural scrolling, tap-to-click and two-finger-right-click.

## Install
Do note that this a highly opinionated step that exists moreso for my own convenience and logging purposes than an actual guide.

1. ```sudo pacman -S reflector broadcom-wl wmctrl playerctl pulseaudio pulseaudio-alsa qbittorrent polybar alsa-utils brightnessctl networkmanager xorg-xinit ranger htop neovim firefox dunst thunar feh gthumb nitrogen maim shotgun firefox rofi wezterm lxappearance-gtk3 mpv imagemagick ffmpeg zathura zathura-pdf-mupdf upower unzip jq```
2. ```paru -S b43-firmware mbpfan-git cpupower-gui visual-studio-code-bin dragon-drop lutgen-git spotify_player```.
3. Enable services for `NetworkManager` + `mbpfan`.
4. Compile `eww` and `berry` from source.
5. Copy rice files from dots repo.

## Other

- For internet (at least on my Macbook Pro 2014), you need the `broadcom-wl` and `b43-firmware` packages. The latter is available in the AUR.
- Note: `cpupower` (Set to the 'powersave' governor (underclocks processor to lowest frequency))  & `mbpfan-git` (fan daemon) are essential for Linux on Mac. Regarding the former -
- Also check out [scripts](Scripts/) and move the contents to your `.local/bin` (or any other directory in your path).