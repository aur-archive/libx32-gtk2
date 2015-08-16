# $Id: PKGBUILD 76653 2012-09-25 20:30:51Z bluewind $
# Maintainer:  Ionut Biru <ibiru@archlinux.org
# Contributor: Pierre Schmitz <pierre@archlinux.de>
# Contributor: Mikko Seppälä <t-r-a-y@mbnet.fi>

_pkgbasename=gtk2
pkgname=libx32-$_pkgbasename
pkgver=2.24.13
pkgrel=1.1
pkgdesc="The GTK+ Toolkit (v2) (x32 ABI)"
arch=('x86_64')
url="http://www.gtk.org/"
install=gtk2.install
depends=('libx32-atk>=1.30.0' 'libx32-pango>=1.28.0' 'libx32-cairo>=1.10.0' 'libx32-gdk-pixbuf2>=2.22.1' 
         'libx32-libcups>=1.4.4' 'libx32-libxcursor' 'libx32-libxrandr>=1.3' 'libx32-libxi>=1.3' 'libx32-libxinerama' 'libx32-libxcomposite' 'libx32-libxdamage' 
         $_pkgbasename)
makedepends=('pkgconfig' 'gcc-multilib-x32')
options=('!libtool' '!docs')
license=('LGPL')
source=(http://ftp.gnome.org/pub/gnome/sources/gtk+/2.24/gtk+-${pkgver}.tar.xz
        xid-collision-debug.patch
        gtk-modules-x32.patch)
sha256sums=('35e1a01e46b02970b02ee9f299390d0aa57c1215ad2667bcd584b72f4ea6513d'
            'd758bb93e59df15a4ea7732cf984d1c3c19dff67c94b957575efea132b8fe558'
            '66691475c0300055e5c6a739cf07b211cfc8739e8a23f7b13c78b574da29c9bb')

build() {
  export CC="gcc -mx32"
  export CXX="g++ -mx32"
  export PKG_CONFIG_PATH="/usr/libx32/pkgconfig"

  cd "${srcdir}/gtk+-${pkgver}"
  patch -Np1 -i "${srcdir}/xid-collision-debug.patch"
  patch -p1 -i ${srcdir}/gtk-modules-x32.patch

  CXX=/bin/false ./configure --prefix=/usr \
      --sysconfdir=/etc \
      --localstatedir=/var \
      --libdir=/usr/libx32 \
      --with-xinput=yes

  #https://bugzilla.gnome.org/show_bug.cgi?id=655517
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make
}

package() {
  cd "${srcdir}/gtk+-${pkgver}"
  make DESTDIR="${pkgdir}" install
  rm -rf "${pkgdir}"/etc
  rm -rf "${pkgdir}"/usr/{include,share}

  cd "${pkgdir}"/usr/bin
  mv gtk-query-immodules-2.0 gtk-query-immodules-2.0-x32
  rm -f gtk-builder-convert gtk-demo gtk-update-icon-cache
}
