# Created by: spcmd [https://github.com/spcmd]

pkgname=spotymenu
pkgver=1.0
pkgrel=1
arch=('any')
url='https://github.com/spcmd/spotymenu'
source=("spotymenu" "spotymenurc")
sha1sums=('SKIP' 'SKIP')
pkgdesc='Use Spotfiy via dmenu'
depends=('spotify' 'dmenu' 'jq')

package() {
    install -Dm755 "$srcdir"/spotymenu "$pkgdir"/usr/bin/spotymenu
    install -Dm644 "$srcdir"/spotymenurc "$pkgdir"/etc/spotymenu/spotymenurc
}
