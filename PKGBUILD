# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

_linuxprefix=linux61-rt
_extramodules=extramodules-6.1-rt-MANJARO

_module=bbswitch
pkgname="${_linuxprefix}-${_module}"
pkgver=0.8
pkgrel=35
pkgdesc="Kernel module allowing to switch dedicated graphics card on Optimus laptops"
arch=('x86_64')
url="https://github.com/Bumblebee-Project/bbswitch"
license=('GPL-2.0-or-later')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}")
makedepends=("${_linuxprefix}-headers")
provides=("${_module}")
replaces=("linux515-rt-${_module}" "linux60-rt-${_module}")
source=("$pkgname-$pkgver.tar.gz::https://github.com/Bumblebee-Project/bbswitch/archive/v${pkgver}.tar.gz"
        'kernel57.patch'
        'kernel518.patch')
sha256sums=('76cabd3f734fb4fe6ebfe3ec9814138d0d6f47d47238521ecbd6a986b60d1477'
            '3b8039f3cd32d2aa8ad0b2426f28faac218eacd134c1e39454c9feca9d612789'
            '04061ecbee433de137d8e68cd42271a30c172bb87829cf350d50df1b24414139')

prepare() {
  cd "${_module}-${pkgver}"
  patch -Np1 < ../kernel57.patch
  patch -Np1 < ../kernel518.patch
}

build() {
  _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"

  cd "${_module}-${pkgver}"
  make KDIR="/usr/lib/modules/${_kernver}/build"
}

package() {
  cd "${_module}-${pkgver}"
  _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"

  install -Dm644 *.ko -t "$pkgdir/usr/lib/modules/${_kernver}/extramodules/"
  find "${pkgdir}" -name '*.ko' -exec zstd --rm -19 {} +
}
