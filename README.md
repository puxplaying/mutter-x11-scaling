# mutter-x11-scaling
Mutter build with Ubuntu patch for Xorg fractional scaling on Manjaro / Arch Linux.

All Credits belong to [Arch Linux](https://www.archlinux.org/packages/extra/x86_64/mutter/) and [Ubuntu](https://salsa.debian.org/gnome-team/mutter/-/blob/ubuntu/master/debian/patches/x11-Add-support-for-fractional-scaling-using-Randr.patch)

---
To enable fractional scaling after installation run:
- ```gsettings set org.gnome.mutter experimental-features "['x11-randr-fractional-scaling']"```

To disable fractional scaling run:
- ```gsettings reset org.gnome.mutter experimental-features```


Or install the patched [gnome-control-center-x11-scaling](https://www.archlinux.org/packages/extra/x86_64/mutter/) package for proper multi-display management and the toggle option from Ubuntu.

---

Build instructions for Manjaro / Arch Linux:
```
sudo pacman -Syu base-devel git
git clone https://github.com/puxplaying/mutter-x11-scaling.git
cd mutter-x11-scaling
makepkg -srci
```
---

![alt text](https://github.com/puxplaying/mutter-x11-scaling/blob/3.38.2/123.png)
