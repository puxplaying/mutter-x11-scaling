# Maintainer: Georg Wagner <puxplaying_at_gmail_dot_com>
# Contributor: @xabbu <https://github.com/xabbu>
# Contributor: Stefano Capitani <stefano_at_manjaro_dot_org>
# Contributor: Mark Wagie <mark_at_manjaro_dot_org>

# Archlinux credits:
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

# Ubuntu credits:
# Marco Trevisan: <https://salsa.debian.org/gnome-team/mutter/-/blob/ubuntu/master/debian/patches/x11-Add-support-for-fractional-scaling-using-Randr.patch>

pkgname=mutter-x11-scaling
_pkgname=mutter
pkgver=42.0
pkgrel=2
pkgdesc="A window manager for GNOME with X11 fractional scaling patch"
url="https://gitlab.gnome.org/GNOME/mutter"
arch=(x86_64)
license=(GPL)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas
         libcanberra startup-notification zenity libsm gnome-desktop 
         libxkbcommon-x11 gnome-settings-daemon libgudev libinput pipewire
         xorg-xwayland graphene libxkbfile libsysprof-capture)
makedepends=(gobject-introspection git egl-wayland meson xorg-server
             wayland-protocols sysprof gi-docgen)
checkdepends=(xorg-server-xvfb wireplumber python-dbusmock)
conflicts=($_pkgname)
options=(debug)
_commit=9249aba72a5c4454894c08735a4963ca1665e34d  # tags/42.0^0
source=("git+https://gitlab.gnome.org/GNOME/mutter.git#commit=$_commit"
	"Revert-mutter-commit-ef0f7084.patch"
	"x11-Add-support-for-fractional-scaling-using-Randr.patch")
sha256sums=('SKIP'
            '0450d2fabbba731178ba08cb4f99725d8993b1715ed95e3a8fceec3f1e08bdd8'
            '1678c3375d8877b3686c7dd3fd1356bf949a9ab5b40ee070b239be3ef1db2f82')

pkgver() {
  cd $_pkgname
  git describe --tags | sed 's/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd $_pkgname

  # Fix Dash-to-dock not autohiding
  git cherry-pick -n 2aad56b949b8 0280b0aaa563

  # https://bugs.archlinux.org/task/74360
  git cherry-pick -n f9857cb8bd7af20e819283917ae165fa40c19f07

  # Ubuntu Patch for X11 fractional scaling
  # patch -p1 -i "${srcdir}/Revert-mutter-commit-ef0f7084.patch"
  patch -p1 -i "${srcdir}/x11-Add-support-for-fractional-scaling-using-Randr.patch"

  # Make tests run
  sed -i '/catchsegv/d' meson.build
}

build() {
  CFLAGS="${CFLAGS/-O2/-O3} -fno-semantic-interposition"
  LDFLAGS+=" -Wl,-Bsymbolic-functions"
  arch-meson $_pkgname build \
    -D egl_device=true \
    -D wayland_eglstream=true \
    -D installed_tests=false
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

  #meson test -C build --print-errorlogs
)

check() {
  dbus-run-session xvfb-run -s '-nolisten local +iglx -noreset' \
    bash -c "$(declare -f _check); _check"
}

package() {
  provides=($_pkgname libmutter-10.so)
  groups=(gnome)

  meson install -C build --destdir "$pkgdir"
}
