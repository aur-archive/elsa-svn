# Maintainer: Samsagax <samsagax at gmail dot com>
# Contributor: Swift Geek <swiftgeek+spam@gmail.com>

pkgname=elsa-svn
pkgver=74294
pkgrel=1
pkgdesc="Successor of the e17 display manager - Entrance"
url="http://trac.enlightenment.org/e"
license=("GPL")
arch=("i686" "x86_64")
provides=('elsa')
conflicts=('entrance')
depends=('elementary')
makedepends=('subversion')
source=('elsa-pam'
	'elsa.service')
md5sums=('9a76cae5b3a0fcbb6116fa08c7a587b5'
	 '9a8f77b632507f77a8bfa45967e18d6c')

_svntrunk="http://svn.enlightenment.org/svn/e/trunk/PROTO/elsa/"
_svnmod="elsa"

build ()
{
	cd $srcdir

	msg "Connecting to $_svntrunk SVN server...."
	if [ -d $_svnmod/.svn ]; then
		(cd $_svnmod && svn up -r $pkgver)
	else
		svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
	fi

	msg "SVN checkout done or server timeout"
	msg "Starting make..."

	rm -rf $_svnmod-build
	cp -r $_svnmod $_svnmod-build
	cd $_svnmod-build
	
    ./autogen.sh \
        --prefix=/usr \
        --sysconfdir=/etc
	
    make

}

package() {
    cd $srcdir/$_svnmod-build

    make DESTDIR=$pkgdir install

    # install license files
    install -Dm644 $srcdir/$_svnmod-build/COPYING \
        $pkgdir/usr/share/licenses/$pkgname/COPYING

    # install pam file
    install -Dm644 $srcdir/elsa-pam \
        $pkgdir/etc/pam.d/elsa

    # install systemd files
    install -Dm644 $srcdir/elsa.service \
        $pkgdir/usr/lib/systemd/system/elsa.service
}
