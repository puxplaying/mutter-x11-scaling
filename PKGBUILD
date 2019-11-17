# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.34.1+57+gd78585d68
pkgrel=1
pkgdesc="A window manager for GNOME"
url="https://gitlab.gnome.org/GNOME/mutter"
arch=(x86_64)
license=(GPL)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas libcanberra
         startup-notification zenity libsm gnome-desktop upower libxkbcommon-x11
         gnome-settings-daemon libgudev libinput pipewire xorg-server-xwayland)
makedepends=(gobject-introspection git egl-wayland meson xorg-server sysprof)
checkdepends=(xorg-server-xvfb)
groups=(gnome)
install=mutter.install
_commit=d78585d68a90da0ccc67a58c3327752e9945c4a2  # gnome-3-34
source=("git+https://gitlab.gnome.org/GNOME/mutter.git#commit=$_commit"
        918.patch
        fix-build.diff
        https://gitlab.gnome.org/GNOME/mutter/merge_requests/939.patch
        x11-Add-support-for-fractional-scaling-using-Randr.patch)
sha256sums=('SKIP'
            '775fbcd209a170b6ca13326367ef62b8d35acff16019553c40eb24f0684c3495'
            '28aa24daed161f2566ca2b159beb43285184c533956b851a7eb318de741da935'
            '83c816ba93a31a3c9c6ae1224bbcf8ca30e11f1aa1df4cf695451a9cdae90c5c'
            '6e022df5e5b2bb02ca3dedb5cc2895193297703888213764a2c4a2629d9d2272')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname

  # https://gitlab.gnome.org/GNOME/mutter/merge_requests/918
  git apply -3 ../918.patch

  # fix build with libglvnd's EGL headers
  git apply -3 ../fix-build.diff

  git apply -3 ../939.patch
  
  # Ubuntu Patch for fractional scaling
  patch -p1 -i "${srcdir}/x11-Add-support-for-fractional-scaling-using-Randr.patch"
}

build() {
  arch-meson $pkgname build \
    -D egl_device=true \
    -D wayland_eglstream=true \
    -D installed_tests=false
  ninja -C build
}

check() (
  mkdir -p -m 700 "${XDG_RUNTIME_DIR:=$PWD/runtime-dir}"
  glib-compile-schemas "${GSETTINGS_SCHEMA_DIR:=$PWD/build/data}"
  export XDG_RUNTIME_DIR GSETTINGS_SCHEMA_DIR

  # Unexpected passes in conform test
  # Stacking test flaky
  dbus-run-session xvfb-run -s '+iglx -noreset' meson test -C build --print-errorlogs || :
)

package() {
  DESTDIR="$pkgdir" meson install -C build
}
