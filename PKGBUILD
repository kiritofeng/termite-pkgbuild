pkgname=(termite-git termite-terminfo-git)
pkgver=15.r5.g9565563
pkgrel=1

pkgdesc="A simple VTE-based terminal"
url="https://github.com/thestinger/termite/"
arch=('i686' 'x86_64')
license=('LGPL')

makedepends=('git' 'vte3-ng' 'ncurses')

source=(git://github.com/thestinger/termite
        git+https://github.com/thestinger/util
        static-vte.patch
        term-variable.patch)

md5sums=('SKIP'
         'SKIP'
         'bb3cf918ed5a7401b2c77dbfbbfe8d37'
         '8f38b98ce6b351cf58e7c0a69498b0fc')

pkgver() {
  cd termite
  git describe --long --always | sed 's/^v//; s/-/.r/; s/-/./'
}

prepare() {
  cd termite
  git submodule init
  git config submodule.util.url "$srcdir"/util
  git submodule update
  patch --forward --strip=1 --input="${srcdir}/static-vte.patch"
  patch --forward --strip=1 --input="${srcdir}/term-variable.patch"
}

build() {
  cd termite
  make
}

package_termite-git() {
  pkgdesc="A simple VTE-based terminal"
  depends=('vte3-ng' 'termite-terminfo-git')
  conflicts=('termite')
  backup=(etc/xdg/termite/config)

  cd termite
  make PREFIX=/usr DESTDIR="$pkgdir" install
  rm -rf "$pkgdir"/usr/share/terminfo
  install -Dm644 config "$pkgdir"/etc/xdg/termite/config
}

package_termite-terminfo-git() {
  pkgdesc='Terminfo for Termite, a simple VTE-based terminal'
  conflicts=('termite-terminfo')

  cd termite
  install -d "$pkgdir"/usr/share/terminfo
  tic -x termite.terminfo -o "$pkgdir"/usr/share/terminfo
}
