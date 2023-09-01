+++
author = "Mujtaba Asim"
title = "'Reviving' an old Macbook with Arch Linux"
date = "2023-09-1"
description = "Notes on setting up Linux on a Macbook (without a desktop environment)."
tags = [
    "Linux", 
    "Mac"
]
+++

This entire debacle began when I recieved an almost decade old Macbook Pro. To be precise, one from Mid 2014 with a 4th gen i5 processor, 8 GB DDR3 RAM, and a 128 GB SSD.

<!--more-->

Apart from a degenerating battery and faulty speakers, it was quite functional. The only problem? Factory restarting and losing my Big Sur install. After that, regardless of what I tried, it refused to update. The archaic UI and lack of software support was jarring, but bearable. 

The straw that broke the camel's back was when Apple's email, almost with a tinge of schadenfreude, announced I had to wait 15 days to... reset my password? Befuddled and peeved, I decided to slap Linux on this trusty old war machine and be done with Apple's tomfoolery.

Over time, I like to think that I've perfected the experience of Linux on Mac, at least, as much as is technologically possible...

From my inital appreciation of its elegance to my blossoming hatred for Apple to eventual acceptance, my Macbook's toughed through Fanta spills, bootleg keyboard replacements, bricked Arch install and everything else I've thrown at it.

The feet have eroded away, the battery life is garbage, the keys have been physically swapped, drawn and taped over but it's withered the storms and emerged as an indispensable companion.

![keyboard](https://raw.githubusercontent.com/masroof-maindak/masroof-maindak.github.io/master/content/assets/keyboard.png)

## Keyboard

Add the following lines

```
options hid-apple swap_fn_leftctrl=1
options hid_apple swap_opt_cmd=1
```
in `/etc/modprobe.d/hid_apple.conf`, regenerate your initramfs* w/ `mkinitcpio -p Linux`, then reboot your Mac for a traditional keyboard experience.

*_If, for whatever reason, you're unable to run mkinitcpio, just redownload the Linux kernel - `sudo pacman -S linux`._

## Scaling

Add `Xft.dpi: 192` in `~/.Xresources` (to ensure that it's run on startup, add `xrdb -merge ~/.Xresources` in your `~/.xinitrc`), and

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

1. `sudo pacman -S`
```
reflector broadcom-wl wmctrl playerctl pulseaudio pulseaudio-alsa qbittorrent polybar alsa-utils brightnessctl networkmanager xorg-xinit ranger htop neovim firefox dunst thunar feh gthumb nitrogen maim shotgun firefox rofi wezterm lxappearance-gtk3 mpv imagemagick ffmpeg zathura zathura-pdf-mupdf upower unzip jq
```
2. `paru -S`
```
b43-firmware mbpfan-git cpupower-gui visual-studio-code-bin dragon-drop lutgen-git spotify_player
```
3. Enable services for `NetworkManager` + `mbpfan`.
4. Compile `eww` and `berry` from source.
5. Copy rice files from [dots](https://github.com/masroof-maindak/dots/) repo.

_The up-to-date version of the packages in the aforementioned steps will be in `MacbookPro2014/$HOME/packageList.md` in my dots repo._

## Other

- For internet (at least on my Macbook Pro 2014), you need the `broadcom-wl` and `b43-firmware` packages. The latter is available in the AUR. You can download them via USB tethering (Android phone required) or an ethernet adapter.
- Note: `cpupower`* (Set to the 'powersave' governor (underclocks processor to lowest frequency))  & `mbpfan-git` (fan daemon) are essential for Linux on Mac.

*_Might not be required. Essential in my case because otherwise, my battery barely lasts me a whole 90 minutes and the laptop gets unbearably hot._