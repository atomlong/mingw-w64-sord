# Maintainer: Alexandros Theodotou <alex@zrythm.org>
# `sord` Maintainer: David Runge <dave@sleepmap.de>
# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: speps <speps at aur dot archlinux dot org>

_pkgbase=sord
pkgname=mingw-w64-sord
pkgver=0.16.2
pkgrel=1
pkgdesc="A lightweight C library for storing RDF data in memory"
arch=('any')
url="https://drobilla.net/software/sord/"
license=('custom:ISC')
depends=('mingw-w64-serd' 'mingw-w64-pcre')
makedepends=('mingw-w64-python')
source=("https://download.drobilla.net/${_pkgbase}-${pkgver}.tar.bz2"{,.sig})
sha512sums=('fe143c07ed18c15ffaf2461ca587df76e365a075f5d93f5eaa4c26196e0b1dd59d24d16e176de3664a2658377a2934083af5742c80884e8e7dd201dcaccb9698'
            'SKIP')
validpgpkeys=('907D226E7E13FA337F014A083672782A9BF368F3') # David Robillard

_architectures=('i686-w64-mingw32' 'x86_64-w64-mingw32')

prepare() {
  cd "${_pkgbase}-${pkgver}"
  # remove local call to ldconfig
  sed -i "/ldconfig/d" wscript
}

build() {
  cd "${_pkgbase}-${pkgver}"

  for _arch in "${_architectures[@]}"; do
    python waf configure --prefix=/usr/"$_arch" \
                         --test
    python waf build
  done
}

check() {
  cd "${_pkgbase}-${pkgver}"
  python waf test --verbose-tests
}

package() {
  cd "${_pkgbase}-${pkgver}"
  python waf install --destdir="$pkgdir"
  # license
  install -vDm 644 COPYING \
    "$pkgdir/usr/share/licenses/$_pkgbase/LICENSE"
  # docs
  install -t "$pkgdir/usr/share/doc/${_pkgbase}" \
    -vDm 644 {AUTHORS,NEWS,README}
}

# vim:set ts=2 sw=2 et: