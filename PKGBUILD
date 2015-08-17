# This is a fork of Samsagax's nvidia-bumblebee, supporting PAE kernels instead of normal kernels
# Maintainer: wyy1326 <wyy1326[at]gmail[dot]com>
# Contributor: Thomas Baechler <thomas[at]archlinux[dot]org>

pkgname=nvidia-bumblebee-pae
pkgver=313.26
_extramodules="extramodules-`uname -r | cut -c1-3`-pae"
pkgrel=1
pkgdesc="NVIDIA drivers for linux. Packaged for Bumblebee. PAE version"
arch=('i686')
url="http://www.nvidia.com/"
depends=('linux-pae' "nvidia-utils-bumblebee=${pkgver}")
provides=("nvidia=${pkgver}")
makedepends=('linux-pae-headers')
conflicts=('nvidia' 'nvidia-96xx' 'nvidia-173xx' 'dkms-nvidia')
license=('custom')
install=nvidia.install
options=(!strip)

_arch='x86'
_pkg="NVIDIA-Linux-${_arch}-${pkgver}"
source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
md5sums=('3c2f5138d0fec58b27e26c5b37d845b8')

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version || true)"
    cd "${srcdir}"
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}/kernel"
    make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
    install -d -m755 "${pkgdir}/usr/lib/modprobe.d"
    echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nouveau_blacklist.conf"
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
}
