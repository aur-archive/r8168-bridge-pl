# Maintainer: Piotr Górski <sir_lucjan@openlinux.pl>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Contributor:Bob Fanger < bfanger(at)gmail >
# Contributor: Filip <fila pruda com>, Det < nimetonmaili(at)gmail >

 
pkgname=r8168-bridge-pl
_pkgname=r8168
pkgver=8.039.00
pkgrel=2
pkgdesc="A kernel module for Realtek 8168 network cards"
url="http://www.realtek.com.tw"
license=("GPL")
arch=('i686' 'x86_64')
depends=('glibc' 'linux-bridge-pl')
makedepends=('linux-bridge-pl-headers')
source=(https://r8168dl.appspot.com/files/$_pkgname-$pkgver.tar.bz2)
install=$_pkgname.install

build() {
    _kernver=$(pacman -Q linux | cut -d . -f 2 | cut -f 1 -d -)
    KERNEL_RELEASE=$(cat /usr/lib/modules/extramodules-3.18-bridge-pl/version)

    cd "$_pkgname-$pkgver"

    # avoid using the Makefile directly -- it doesn't understand
    # any kernel but the current.
    make -C /usr/lib/modules/$KERNEL_RELEASE/build \
            SUBDIRS="$srcdir/$_pkgname-$pkgver/src" \
            EXTRA_CFLAGS="-DCONFIG_R8168_NAPI -DCONFIG_R8168_VLAN" \
            modules
}

package() {
    _kernver=$(pacman -Q linux | cut -d . -f 2 | cut -f 1 -d -)
    depends=("linux-bridge-pl>=3.18" "linux-bridge-pl<3.19")
    KERNEL_VERSION=$(cat /usr/lib/modules/extramodules-3.18-bridge-pl/version)
    msg "Kernel = $KERNEL_VERSION"

    cd "$_pkgname-$pkgver"
    install -Dm644 src/$_pkgname.ko "$pkgdir/usr/lib/modules/extramodules-3.18-bridge-pl/$_pkgname.ko"
    find "$pkgdir" -name '*.ko' -exec gzip -9 {} +

    sed -i "s|extramodules-.*-ARCH|extramodules-3.18-bridge-pl|" "$startdir/$_pkgname.install"
    }
    
    
sha256sums=('767d922270274e781d8d42493a0021db1cafcb0388ac62564d0c0c3d82703edd')
