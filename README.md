# mutter-x11-scaling
Mutter build with Ubuntu patch for Xorg fractional scaling on Manjaro / Arch Linux.

The binary package from the release page was build on the stable branch.

All Credits belong to [Arch Linux](https://www.archlinux.org/packages/extra/x86_64/mutter/) and [Ubuntu](https://git.launchpad.net/ubuntu/+source/mutter/tree/debian/patches/x11-Add-support-for-fractional-scaling-using-Randr.patch)

---
To enable fractional scaling after installation run:
- ```gsettings set org.gnome.mutter experimental-features "['x11-randr-fractional-scaling']"```

To disable fractional scaling run:
- ```gsettings reset org.gnome.mutter experimental-features```
---

Build instructions for Manjaro Linux:
```
sudo pacman -Syu base-devel git
git clone https://github.com/puxplaying/mutter-x11-scaling.git
cd mutter-x11-scaling
makepkg -srci
```
---

![alt text](https://github.com/puxplaying/mutter-x11-scaling/blob/master/123.png)
