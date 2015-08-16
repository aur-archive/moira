# Maintainer: Steven Allen <steven@stebalien.com>
pkgbase='moira'
pkgname=$pkgbase
true && pkgname=('moira-clients' 'libmoira')
_srcpkgname='debathena-moira'
pkgver=4.0.0_r4149
_srcpkgver=${pkgver/_/-}
pkgrel=1
url="https://debathena.mit.edu/"
arch=('x86_64' 'i686')
license=('custom')
groups=('athena')
makedepends=('krb5' 'ncurses' 'termcap' 'ncurses' 'e2fsprogs')
provides=('libmoira' 'moira-clients')
options=('!libtool' '!buildflags')
pkgdesc="Athena Service Management system"
source=("https://debathena.mit.edu/apt/pool/debathena/d/${_srcpkgname}/${_srcpkgname}_${_srcpkgver}.orig.tar.gz")
sha256sums=('85c906ca030ccced1a54add0f45ffddb5db61b44fc689be60328226e9090c775')

build() {
  cd "$srcdir/$_srcpkgname-$_srcpkgver"
  unset LDFLAGS CXXFLAGS CFLAGS
  (
    cd 'util/et'
    ./configure
    make
  )

  ./configure --sysconfdir=/etc --prefix=/usr

  (
    cd lib
    make LDFLAGS="-fPIC"
  )
  (
    cd clients
    make LDFLAGS="-lkrb5"
  )
}

package_libmoira() {
  true && depends=('ncurses' 'e2fsprogs')
  true && provides=()
  true && pkgdesc="Athena Service Management system (libraries)."

  cd "$srcdir/$_srcpkgname-$_srcpkgver/lib"
  install -d -m755 "$pkgdir/usr/lib/"
  make DESTDIR="$pkgdir" install
  install -D -m644 "../include/mit-copyright.h" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
package_moira-clients() {
  true && depends=('krb5' 'ncurses' 'termcap' 'libmoira')
  true && provides=()
  true && pkgdesc="Athena Service Management system (clients)."

  cd "$srcdir/$_srcpkgname-$_srcpkgver/clients"
  
  install -d -m755 "$pkgdir/usr/bin/" "$pkgdir/usr/lib/"
  make DESTDIR="$pkgdir" install
  mv "$pkgdir"/usr/bin/{chfn,moira-chfn}
  mv "$pkgdir"/usr/bin/{chsh,moira-chsh}
  install -D -m644 "../include/mit-copyright.h" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
