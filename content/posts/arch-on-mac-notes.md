+++
title = "'Reviving' an old MacBook with Arch Linux"
date = 2023-09-01
draft = false

[taxonomies]
tags = ["Linux","MacBook"]

[extra]
lang = "en"
toc = true
comment = true
outdate_alert = false
display_tags = true
featured = false
+++

This entire debacle began when I received an (almost) decade old MacBook Pro. To be precise, one from Mid 2014 with a 4th gen i5 processor (the ultra-low-power one at that), 8 GB DDR3 RAM, and a 128 GB SSD.

<!--more-->

Apart from a degenerating battery and faulty speakers, it was quite functional. The only problem? Factory resetting and losing my Big Sur install. Thereafter, regardless of what I tried, it refused to update. The archaic UI and lack of software support was jarring, but bearable.

The straw that broke the camel's back was when Apple's email, almost with a tinge of schadenfreude, announced I had to wait 15 days to... change my password? Befuddled and peeved, I decided to slap Linux on this trusty old war machine and be done with Apple's tomfoolery.

Over time, I like to think that I've perfected the experience of Linux on Mac, at least, as much as I possibly can.

From my initial appreciation of its elegance to my blossoming hatred for Apple to eventual acceptance, my MacBook's toughed through Fanta spills, bootleg keyboard replacements, bricked Arch install and everything else I've thrown at it.

The rubber feet have all but eroded away, the battery capacity has fallen beneath 65%, the keys have been physically swapped, drawn and taped over, but it's withered the storms and emerged as an indispensable companion.

{% note(header="Note") %}
I've condensed most of the contents of this script into a convenient bootstrap script for my own uses, but you are advised to go through the contents of this article anyway.
{% end %}

## Installing Arch

Not much to say here, just follow a standard Arch Linux installation guide from literally anywhere. Regarding internet availability, I can vouch that it indeed \*can\* work from just the installer TTY, it's just quite finicky and admittedly a pain in the ass to set up.

You could connect an ethernet cable via a hub if one's at hand or activate USB tethering through an Android phone for a seamless experience.

## Input

### Keyboard

Add the following lines

```
options hid-apple swap_fn_leftctrl=1
options hid_apple swap_opt_cmd=1
```

in `/etc/modprobe.d/hid_apple.conf`, regenerate your initramfs\* w/ `mkinitcpio -p Linux`, then reboot your Mac for a traditional keyboard experience.

\* _If, for whatever reason, you're unable to run mkinitcpio, just re-download the Linux kernel_ - ***sudo pacman -S linux***.

### Touchpad

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
    Option "DisableWhileTyping" "1"
    Option "TransformationMatrix" "1 0 0 0 1 0 0 0 0.3"
EndSection
```

For natural scrolling, tap-to-click and two-finger-right-click.

## Scaling

Add `Xft.dpi: 192` in `~/.Xresources` (to ensure that it's run on startup, add `xrdb -merge ~/.Xresources` in your `~/.xinitrc`), and

```.bash_profile
export QT_AUTO_SCREEN_SCALE_FACTOR=1
export GDK_SCALE=2
export GDK_DPI_SCALE=0.5
```

in `~/.bash_profile` for GTK + QT app scaling. Touch any of the aforementioned files if they don't exist already.


## Webcam

Make sure you have `kmod` and the `linux-headers` packages installed. Then trivially grab the `bcwc-pcie-git` package from the AUR. Upon installing, run ***modprobe facetimehd*** to load the camera's kernel module, and it *should* just work out of the box.

## Packages

Since most of this guide holds true for setting up an X11 window manager, you'll need to install certain packages for software that you'd expect to be provided in a full-fledged desktop. Some examples include `rofi` for launching apps, `dunst` for notifications, `feh` for viewing images and setting wallpapers, and so on and so forth.

## Other

- For internet (at least on my Macbook Pro 2014), you need the `broadcom-wl` and `b43-firmware` packages. The latter is available in the AUR.
- Additionally, `cpupower-gui`* & `mbpfan-git` (fan daemon) are essential for Linux on Mac.

*_Might not be required. I like to underclock when I'm not doing something intensive to keep the battery cool and the fans quiet. This might just be my little machine squealing on its last legs however..._

## Addendum

I have since shifted to a nifty little Wayland compositor called [River](https://codeberg.org/river/river/), and if you decided to do so too, the _scaling_ and _touchpad_ sections of this post should be considered deprecated, as these are configured on a compositor-level in Wayland.

However, if you want to stick with an X11 window manager, they're still perfectly valid.
