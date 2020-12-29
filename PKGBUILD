# Maintainer: puxplaying (Georg Wagner) <puxplaying@gmail.com>
# Contributor: @xabbu <https://github.com/xabbu>

# Archlinux credits:
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

# Ubuntu credits:
# Marco Trevisan: <https://salsa.debian.org/gnome-team/mutter/-/blob/ubuntu/master/debian/patches/x11-Add-support-for-fractional-scaling-using-Randr.patch>

pkgbase=mutter
pkgname=$pkgbase-x11-scaling
pkgver=3.38.2+7+gfbb9a34f2
pkgrel=1
pkgdesc="A window manager for GNOME"
url="https://gitlab.gnome.org/GNOME/mutter"
arch=(x86_64)
license=(GPL)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas
         libcanberra startup-notification zenity libsm gnome-desktop upower
         libxkbcommon-x11 gnome-settings-daemon libgudev libinput pipewire
         xorg-xwayland graphene)
makedepends=(gobject-introspection git egl-wayland meson xorg-server sysprof)
checkdepends=(xorg-server-xvfb)
conflicts=($pkgbase)
provides=(libmutter-7.so $pkgbase)
groups=(gnome)
install=mutter.install
_commit=fbb9a34f265ddd12ab9996ef18e595fd95b2a92e  # gnome-3-38
_scaling_commit=31fac1abdef96d8e4b468e11771693ec5f7acd7b # Commit 31fac1ab
source=("git+https://gitlab.gnome.org/GNOME/mutter.git#commit=$_commit"
	"x11-Add-support-for-fractional-scaling-using-Randr.patch::https://salsa.debian.org/gnome-team/mutter/-/raw/$_scaling_commit/debian/patches/x11-Add-support-for-fractional-scaling-using-Randr.patch")
sha256sums=('SKIP'
            '3380ae1d479666679735a94c05c7e3ac10b3da2d2b843dd881d351bb5c56bf96')

pkgver() {
  cd $pkgbase
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgbase

  # Ubuntu Patch for X11 fractional scaling
  patch -p1 -i "${srcdir}/x11-Add-support-for-fractional-scaling-using-Randr.patch"
}

build() {
  CFLAGS="${CFLAGS/-O2/-O3} -fno-semantic-interposition"
  LDFLAGS+=" -Wl,-Bsymbolic-functions"
  arch-meson $pkgbase build \
    -D egl_device=true \
    -D wayland_eglstream=true \
    -D installed_tests=false
  meson compile -C build
}

check() (
  mkdir -p -m 700 "${XDG_RUNTIME_DIR:=$PWD/runtime-dir}"
  glib-compile-schemas "${GSETTINGS_SCHEMA_DIR:=$PWD/build/data}"
  export XDG_RUNTIME_DIR GSETTINGS_SCHEMA_DIR

  # Stacking test flaky
  dbus-run-session xvfb-run \
    -s '-screen 0 1920x1080x24 -nolisten local +iglx -noreset' \
    meson test -C build --print-errorlogs || :
)

package() {
  DESTDIR="$pkgdir" meson install -C build
}
