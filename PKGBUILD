# Maintainer: Alexandros Theodotou <alex@zrythm.org>
# `sord` Maintainer: David Runge <dave@sleepmap.de>
# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: speps <speps at aur dot archlinux dot org>

_pkgbase=sord
pkgname=mingw-w64-sord
pkgver=0.16.8
pkgrel=1
pkgdesc="A lightweight C library for storing RDF data in memory"
arch=('any')
url="https://drobilla.net/software/sord/"
license=('custom:ISC')
depends=('mingw-w64-serd' 'mingw-w64-pcre')
makedepends=('mingw-w64-gcc' 'python')
options=('!libtool' '!strip' '!buildflags' '!makeflags')
source=("https://download.drobilla.net/${_pkgbase}-${pkgver}.tar.bz2"{,.sig})
sha512sums=('24ed50de8e5bb321e557bac6d3e441b2ed49adabf828bf0e1b33a080c89306dde80443dc8b563098fcc184c4d6e53b7e716b523ddccdf56d08301d1b0120f2b2'
            'SKIP')
validpgpkeys=('907D226E7E13FA337F014A083672782A9BF368F3') # David Robillard

_architectures=('i686-w64-mingw32' 'x86_64-w64-mingw32')

prepare() {
  cd "${_pkgbase}-${pkgver}"
  # remove local call to ldconfig
  sed -i "/ldconfig/d" wscript
}

build() {
  cd "${srcdir}"

  for _arch in "${_architectures[@]}"; do
    rm -rf build-${_arch}
    cp -r "${_pkgbase}-${pkgver}" build-${_arch}
    pushd build-${_arch}

    export PKG_CONFIG_PATH="/usr/${_arch}/lib/pkgconfig"
    CC="$_arch-gcc" python waf configure --prefix=/usr/"$_arch" #\
                         #--test
    python waf build

    popd
  done
}

check() {
  cd "${_pkgbase}-${pkgver}"
  #python waf test --verbose-tests
}

package() {
  cd "${srcdir}"

  for _arch in "${_architectures[@]}"; do
    pushd "build-${_arch}"

    python waf install --destdir="$pkgdir"
    # license
    install -vDm 644 COPYING \
      "$pkgdir/usr/$_arch/share/licenses/$_pkgbase/LICENSE"
    # docs
    install -t "$pkgdir/usr/$_arch/share/doc/${_pkgbase}" \
      -vDm 644 {AUTHORS,NEWS,README.md}

    # move DLL to bin directory
    install -d $pkgdir/usr/${_arch}/bin
    mv "$pkgdir"/usr/${_arch}/lib/*.dll "$pkgdir"/usr/${_arch}/bin

    # strip
    ${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "$pkgdir"/usr/${_arch}/lib/*.a

    popd
  done
}

# vim:set ts=2 sw=2 et:
