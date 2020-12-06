# mutter-x11-scaling
Mutter build with Ubuntu patch for Xorg fractional scaling on Manjaro / Arch Linux.

All Credits belong to [Arch Linux](https://www.archlinux.org/packages/extra/x86_64/mutter/) and [Ubuntu](https://salsa.debian.org/gnome-team/mutter/-/blob/ubuntu/master/debian/patches/x11-Add-support-for-fractional-scaling-using-Randr.patch)

On [Manjaro](https://manjaro.org/) ```mutter-x11-scaling``` and ```gnome-control-center-x11-scaling``` packages can be installed from the repository.

---
To enable fractional scaling after installation run:
- ```gsettings set org.gnome.mutter experimental-features "['x11-randr-fractional-scaling']"```

To disable fractional scaling run:
- ```gsettings reset org.gnome.mutter experimental-features```


Or build the patched [gnome-control-center-x11-scaling](https://github.com/puxplaying/gnome-control-center-x11-scaling) package for proper multi-monitor management and the toggle option from Ubuntu.

---

Build instructions for Manjaro / Arch Linux:
```
sudo pacman -Syu base-devel git
git clone https://github.com/puxplaying/mutter-x11-scaling.git
cd mutter-x11-scaling
makepkg -srci
```
---

![alt text](https://github.com/puxplaying/mutter-x11-scaling/blob/master/123.png)
