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
pkgver=40.1
pkgrel=2
pkgdesc="A window manager for GNOME"
url="https://gitlab.gnome.org/GNOME/mutter"
arch=(x86_64)
license=(GPL)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas
         libcanberra startup-notification zenity libsm gnome-desktop upower
         libxkbcommon-x11 gnome-settings-daemon libgudev libinput pipewire
         xorg-xwayland graphene libxkbfile)
makedepends=(gobject-introspection git egl-wayland meson xorg-server)
checkdepends=(xorg-server-xvfb pipewire-media-session)
conflicts=($pkgbase)
provides=(libmutter-8.so $pkgbase)
groups=(gnome)
install=mutter.install
_commit=faf4240c74024d04f88986f95f65364ca8121ba4  # tags/40.1^0
_scaling_commit=91d9bdafd5d624fe1f40f4be48663014830eee78 # Commit 91d9bdaf
source=("git+https://gitlab.gnome.org/GNOME/mutter.git#commit=$_commit"
	"x11-Add-support-for-fractional-scaling-using-Randr.patch::https://salsa.debian.org/gnome-team/mutter/-/raw/$_scaling_commit/debian/patches/x11-Add-support-for-fractional-scaling-using-Randr.patch")
sha256sums=('SKIP'
            '19de314590e3311563b11da3305d8e9c8ba1f859fe65db668ccd0457250a9ca5')

pkgver() {
  cd $pkgbase
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgbase

  # https://bugs.archlinux.org/task/71027
  git cherry-pick -n f954ff84b8e98be7017bd0ac0521f568d93f62e2

  # Ubuntu Patch for X11 fractional scaling
  patch -p1 -i "${srcdir}/x11-Add-support-for-fractional-scaling-using-Randr.patch"
}

build() {
  CFLAGS="${CFLAGS/-O2/-O3} -fno-semantic-interposition"
  LDFLAGS+=" -Wl,-Bsymbolic-functions"
  arch-meson $pkgbase build \
    -D egl_device=true \
    -D wayland_eglstream=true \
    -D installed_tests=false \
    -D profiler=false
  meson compile -C build
}

_check() (
  mkdir -p -m 700 "${XDG_RUNTIME_DIR:=$PWD/runtime-dir}"
  glib-compile-schemas "${GSETTINGS_SCHEMA_DIR:=$PWD/build/data}"
  export XDG_RUNTIME_DIR GSETTINGS_SCHEMA_DIR

  pipewire &
  _p1=$!

  pipewire-media-session &
  _p2=$!

  trap "kill $_p1 $_p2; wait" EXIT

  meson test -C build --print-errorlogs
)

check() {
  dbus-run-session xvfb-run \
    -s '-screen 0 1920x1080x24 -nolisten local +iglx -noreset' \
    bash -c "$(declare -f _check); _check"
}

package() {
  meson install -C build --destdir "$pkgdir"
}
