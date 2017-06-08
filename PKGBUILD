# Maintainer: Your Name <youremail@domain.com>
pkgname=zbectl-git
pkgver=0.1
pkgrel=1
pkgdesc="script for managing boot environments using zfs"
arch=(any)
url=""
license=('GPL')
groups=()
depends=(zfs bash)
makedepends=(git)
optdepends=()
provides=()
conflicts=()
replaces=()
backup=()
options=()
install=
changelog=
source=($pkgname::git+git://git.nicolai.tech/zbectl.git)
noextract=()
md5sums=('SKIP')

pkgver() {
	cd "$pkgnamej"
	git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

package() {
	cd "$pkgname"
	install -D -m 755 zbectl "$pkgdir/usr/bin/zbectl"
	install -D -m 644 99-zbectl-upgrade.hook "$pkgdir/usr/share/libalpm/hooks/99-zbectl-upgrade.hook"
	install -D -m 644 99-zbectl-remove.hook "$pkgdir/usr/share/libalpm/hooks/99-zbectl-remove.hook"
	install -D -m 644 10-zbectl-pre.hook "$pkgdir/usr/share/libalpm/hooks/10-zbectl-pre.hook"
	install -D -m 644 zbectl.8 "$pkgdir/usr/share/man/man8/zbectl.8"
}

