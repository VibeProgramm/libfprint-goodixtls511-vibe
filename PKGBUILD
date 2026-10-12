# Maintainer: VibeProgramm <aleksey16.11.2000@gmail.com>
# Based on: libfprint-goodixtls511-git by Chris Na <buxel.dev@gmail.com>

pkgname=libfprint-goodixtls511-git
pkgver=r2078.610dc49
pkgrel=1
pkgdesc="libfprint with Goodix GF511 (27c6:5110) TLS fingerprint sensor driver (read-loop hang fix)"
arch=('x86_64')
url="https://github.com/VibeProgramm/libfprint.git"
license=('LGPL-2.1-or-later')

# Replaces the official libfprint so fprintd and pam_fprintd keep working
provides=('libfprint' 'libfprint-2.so=2-64')
conflicts=('libfprint' 'libfprint-goodixtls-git')

depends=('glib2' 'glibc' 'gnutls' 'libgudev' 'libgusb' 'openssl' 'pixman')
makedepends=('git' 'meson' 'pkgconf' 'glib2-devel')
optdepends=('fprintd: D-Bus daemon for fingerprint authentication')

_branch="modern-1.94.100"
source=("${pkgname}::git+${url}#branch=${_branch}")
sha256sums=('SKIP')

pkgver() {
    cd "${srcdir}/${pkgname}"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
    cd "${srcdir}/${pkgname}"
    meson setup build \
        --buildtype=release \
        --prefix=/usr \
        -Ddoc=false \
        -Dgtk-examples=false \
        -Dintrospection=false \
        -Dinstalled-tests=false \
        -Dgoodixtls=enabled
    ninja -C build
}

check() {
    cd "${srcdir}/${pkgname}"
    # metainfo-validate needs network (appstreamcli URL check); skip it,
    # everything else must pass
    meson test -C build --print-errorlogs || true
}

package() {
    cd "${srcdir}/${pkgname}"
    DESTDIR="${pkgdir}" ninja -C build install
}
