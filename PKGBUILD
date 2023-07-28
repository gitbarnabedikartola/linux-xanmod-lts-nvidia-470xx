# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Arch credits:
# Maintainer : Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux-xanmod-lts
_extramodules=$(find /usr/lib/modules -type d -iname 6.1.42*xanmod* | rev | cut -d "/" -f1 | rev)

pkgname=$_linuxprefix-nvidia-470xx
pkgdesc="NVIDIA drivers for linux"
pkgver=470.199.02
pkgrel=61421
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("$_linuxprefix-extramodules")
depends=("$_linuxprefix" "nvidia-utils=$pkgver")
makedepends=("$_linuxprefix-headers")
provides=("nvidia=$pkgver" 'NVIDIA-MODULE')
options=(!strip)
install=nvidia.install
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
sha256sums=('9c86f9ef6aceaf2b292407aa161b98d817b2eb10a615f971d29a20c2a748ad09')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}"
    # patches here
}

build() {
    _kernver=$(find /usr/lib/modules -type d -iname 6.1.42*xanmod* | rev | cut -d "/" -f1 | rev)

    cd "${_pkg}"
    make -C kernel SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    cd "${_pkg}"
    install -Dm644 kernel/*.ko -t "${pkgdir}/usr/lib/modules/${_extramodules}/"

    # compress each module individually
    find "${pkgdir}" -name '*.ko' -exec xz -T1 {} +

    install -Dm644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}
