# Maintainer: Your Name <youremail@domain.com>
pkgname=zbectl
pkgver=0.1
pkgrel=1
pkgdesc="script for managing boot environments using zfs"
arch=(any)
url=""
license=('GPL')
groups=()
depends=(zfs bash)
makedepends=()
optdepends=()
provides=()
conflicts=()
replaces=()
backup=()
options=()
install=
changelog=
source=(git+file:///madnas/nicolai/dev/scripts/zbectl)
noextract=()
md5sums=('SKIP')

package() {
	cd "$pkgname"
	install -D -m 755 zbectl "$pkgdir/usr/bin/zbectl"
	install -D -m 644 99-zbectl-upgrade.hook "$pkgdir/usr/share/libalpm/hooks/99-zbectl-upgrade.hook"
	install -D -m 644 99-zbectl-remove.hook "$pkgdir/usr/share/libalpm/hooks/99-zbectl-remove.hook"
	install -D -m 644 10-zbectl-pre.hook "$pkgdir/usr/share/libalpm/hooks/10-zbectl-pre.hook"
}

