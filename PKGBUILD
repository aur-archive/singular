# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Rémy Oudompheng <oudomphe@clipper.ens.fr>

pkgname=singular
pkgver=3.1.7
_majver=3-1-7
pkgrel=2
pkgdesc="Computer Algebra System for polynomial computations"
arch=('i686' 'x86_64')
url="http://www.singular.uni-kl.de/"
license=('GPL')
depends=('flint' 'ntl') #polymake
source=("http://www.mathematik.uni-kl.de/ftp/pub/Math/Singular/src/$_majver/Singular-$_majver.tar.gz" 'templates.patch')
md5sums=('b28c1b406a4203369ea484d87ffe113c'
         'a267423f3b25f0b91853f9cf607974a7')

prepare() {
  cd Singular-$_majver
  patch -p1 -i ../templates.patch
}

build() {
  cd Singular-$_majver

  export CPP=/usr/bin/cpp
  export CXXCPP=/usr/bin/cpp
  export CFLAGS="$CFLAGS -fPIC"
  export CXXFLAGS="$CXXFLAGS -fPIC"

# use system ntl
  rm -r ntl

  mkdir -p build

  ./configure --prefix=$PWD/build/usr/lib/Singular \
     --bindir=$PWD/build/usr/lib/Singular --libdir=$PWD/build/usr/lib/Singular --includedir=$PWD/build/usr/include \
     --with-apint=gmp --with-gmp=/usr --with-malloc=system --with-ntl=/usr --with-flint=/usr --disable-doc --with-NTL --without-MP \
     --enable-Singular --enable-factory --enable-libfac --enable-IntegerProgramming --disable-doc
  make install-nolns

  export CFLAGS="$CFLAGS -DPIC -DLIBSINGULAR"
  export CXXFLAGS="$CXXFLAGS -DPIC -DLIBSINGULAR"

  ./configure --prefix=$PWD/build/usr/lib/Singular \
     --bindir=$PWD/build/usr/lib/Singular --libdir=$PWD/build/usr/lib/Singular --includedir=$PWD/build/usr/include \
     --with-apint=gmp --with-gmp=/usr --with-malloc=system --with-ntl=/usr --with-flint=/usr --disable-doc --with-NTL --without-MP \
     --enable-Singular --enable-factory --enable-libfac --enable-IntegerProgramming --disable-doc
  make clean
  make install-libsingular

# needed by Sage, not installed by default
  cp Singular/sing_dbm.h build/usr/include/singular/
}

package() {
  cd Singular-$_majver

  cp -r build/* "$pkgdir"/ 

  mkdir -p "$pkgdir"/usr/bin
  ln -s /usr/lib/Singular/Singular "$pkgdir"/usr/bin/
  ln -s /usr/lib/Singular/libsingular.so "$pkgdir"/usr/lib/
}

