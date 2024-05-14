# mutter-x11-scaling
Mutter build with Ubuntu patch for Xorg fractional scaling on Manjaro / Arch Linux.

All credits belong to [Arch Linux](https://www.archlinux.org/packages/extra/x86_64/mutter/) and [Ubuntu](https://salsa.debian.org/gnome-team/mutter/-/blob/ubuntu/master/debian/patches/ubuntu/x11-Add-support-for-fractional-scaling-using-Randr.patch), this package is available on [Manjaro](https://manjaro.org/) and the [AUR](https://aur.archlinux.org/packages/mutter-x11-scaling).

---
To enable fractional scaling after installation run:
- ```gsettings set org.gnome.mutter experimental-features "['x11-randr-fractional-scaling']"```
- Then open Settings > Displays to set the scale.

To disable fractional scaling run:
- ```gsettings reset org.gnome.mutter experimental-features```

---

Build instructions for Manjaro / Arch Linux:

```
sudo pacman -Syu base-devel git
git clone https://github.com/puxplaying/mutter-x11-scaling.git
cd mutter-x11-scaling
makepkg -srci
```
---

**Note:**

For proper multi-monitor fractional scaling, install also the [gnome-control-center-x11-scaling](https://github.com/puxplaying/gnome-control-center-x11-scaling) package.

---
