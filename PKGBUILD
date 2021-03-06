# Maintainer: sulaweyo <sledge.sulaweyo@gmail.com>
# Contributor: dequis <dx@dxzone.com.ar>
# Contributor: Jan Was <janek.jan+arch@gmail.com>
# Great Contributor: mainiak <mainiak@gmail.com> (first maintainer)
# Contributor: spooky <spookyh+arch@gmail.com>
# Contributor: Hyacinthe Cartiaux <hyacinthe.cartiaux@free.fr>
# Contributor: Florent Peterschmitt <florent@peterschmitt.fr>
# Contributor: Christian Schwarz <me et cschwarz punkt com>

pkgname=apt-cacher-ng
pkgver=3
pkgrel=3.1
pkgdesc="A caching proxy specialized for package files."
url="http://www.unix-ag.uni-kl.de/~bloch/acng/"
arch=('i686' 'x86_64' 'armv7h' 'armv6h')
license=('custom')
depends=('zlib' 'bzip2' 'fuse' 'xz' 'openssl' 'libwrap')
makedepends=('cmake')
source=("http://ftp.debian.org/debian/pool/main/a/apt-cacher-ng/apt-cacher-ng_${pkgver}.${pkgrel}.orig.tar.xz"
        'acng.conf.patch'
        'apt-cacher-ng.service.patch'
        'apt-cacher-ng.tmpfile'
)

backup=('etc/apt-cacher-ng/acng.conf')
md5sums=('cc673b249a72562969089512e85396d6'
         '605df6eb185f688fc992f61fee05a4a1'
         'eb2ab7d23012507a590db35154b9667e'
         '29979b8064ff52aa24017b42c37e6bfb')

install=apt-cacher-ng.install


build() {
  cd ${srcdir}/${pkgname}-${pkgver}.${pkgrel}
  rm -rf builddir
  mkdir -p builddir
  src=$PWD

  cd builddir

  cmake $src -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr -DEXTRA_LIBS_ACNG=pthread || return 1

  make all || return 1

  patch -Np0 -i "${srcdir}/acng.conf.patch"
  patch -Np0 -i "${srcdir}/apt-cacher-ng.service.patch"
}

package() {
  cd ${srcdir}/${pkgname}-${pkgver}.${pkgrel}

  make -C builddir install DESTDIR=${pkgdir}

  mv ${pkgdir}/usr/sbin/ ${pkgdir}/usr/bin/
  mv ${pkgdir}/usr/lib/apt-cacher-ng/{acngfs,acngtool,in.acng} ${pkgdir}/usr/bin

  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  install -D -m644 ${srcdir}/${pkgname}-${pkgver}.${pkgrel}/builddir/systemd/apt-cacher-ng.service ${pkgdir}/usr/lib/systemd/system/apt-cacher-ng.service
  install -D -m644 ${srcdir}/${pkgname}-${pkgver}.${pkgrel}/builddir/systemd/apt-cacher-ng.conf ${pkgdir}/usr/lib/tmpfiles.d/apt-cacher-ng.conf
  mkdir -p ${pkgdir}/var/log/apt-cacher-ng
  mkdir -p ${pkgdir}/var/cache/apt-cacher-ng

}


# vim:set ts=2 sw=2 et:
