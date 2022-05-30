# mutter-x11-scaling
Mutter build with Ubuntu patch for Xorg fractional scaling on Manjaro / Arch Linux.

All Credits belong to [Arch Linux](https://www.archlinux.org/packages/extra/x86_64/mutter/) and [Ubuntu](https://salsa.debian.org/gnome-team/mutter/-/blob/ubuntu/master/debian/patches/ubuntu/x11-Add-support-for-fractional-scaling-using-Randr.patch)

This package is available on [Manjaro](https://manjaro.org/) and the [AUR](https://aur.archlinux.org/packages/mutter-x11-scaling).

---
To enable fractional scaling after installation run:
- ```gsettings set org.gnome.mutter experimental-features "['x11-randr-fractional-scaling']"```

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

![1](https://user-images.githubusercontent.com/28549766/170991844-d06a1f0c-8b2c-4af6-b092-55acb090e284.png)
