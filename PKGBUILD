# Maintainer: trya <tryagainprod@gmail.com>

pkgname=gens-gs
pkgver=r7
pkgrel=5
pkgdesc="An emulator of Sega Genesis, Sega CD and 32X, combining features from various forks of Gens"
url="http://segaretro.org/Gens/GS"
arch=('i686' 'x86_64')
license=('GPL')
if [[ $CARCH == "x86_64" ]]; then
  depends=('lib32-gtk2' 'lib32-sdl' 'lib32-libgl')
  makedepends=('nasm' 'gcc-multilib')
  optdepends=('lib32-alsa-plugins: sound with Gens/GS for Pulseaudio users'
              'lib32-libpulse: sound with Gens/GS for Pulseaudio users')
else
  depends=('gtk2' 'sdl' 'libgl')
  makedepends=('nasm' 'gcc')
fi
replaces=('bin32-gens-gs')
conflicts=('gens' 'gens-gs-git' 'bin32-gens-gs' 'bin32-gens')
options=('!libtool')
source=("http://segaretro.org/images/6/6d/Gens-gs-r7.tar.gz"
        gens-gtk.patch)
md5sums=('bcb17b49774aa318a224c741028aabc3'
         '94a8ea744dee8caea73db1223ac67dcd')

build() {
  if [ $CARCH == "x86_64" ]; then
    export CC="gcc -m32"
    export CXX="g++ -m32"
    export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"
  fi

  cd "$srcdir/$pkgname-$pkgver"
  
  patch -Np1 < ../gens-gtk.patch
  if [ $CARCH == "x86_64" ]; then
    i386 ./configure --prefix=/usr
  else
    ./configure --prefix=/usr LIBS="-ldl -lX11"
  fi
  make
}

package() {
  cd "$srcdir/$pkgname-$pkgver"
  make DESTDIR="$pkgdir" install
  rm -f "$pkgdir/usr/lib/mdp/*.a"
}
