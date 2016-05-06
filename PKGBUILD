# Maintainer: Aleksey Filippov <sarum9in@gmail.com>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Thomas S Hatch <thatch45@gmail.com>
# Contributor: Geoffroy Carrier <geoffroy@archlinux.org>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

_pkgbase="protobuf"
pkgname=(
  'protobuf3'
  'python2-protobuf3'
  'python-protobuf3'

  'protobuf3-slot'
  'python2-protobuf3-slot'
  'python-protobuf3-slot'
)
pkgver=3.0.0_beta_2
_pkgver=$(echo $pkgver | tr _ -)
pkgrel=2
pkgdesc="Protocol Buffers - Google's data interchange format"
arch=('i686' 'x86_64')
url='https://developers.google.com/protocol-buffers/'
license=('BSD')
depends=('gcc-libs' 'zlib')
makedepends=('unzip' 'python-setuptools' 'python2-setuptools')
source=("https://github.com/google/${_pkgbase}/archive/v${_pkgver}.tar.gz")
md5sums=('e7f2602baffcbc27fb607de659cfbab6')

prepare() {
  mkdir -p "$_pkgbase-$_pkgver-slot"
  tar --strip-components=1 -xf "v${_pkgver}.tar.gz" -C "$_pkgbase-$_pkgver-slot"

  cd "$_pkgbase-$_pkgver-slot"/python
  sed -r "s|^__version__ = '(.*)(-slot)?'$|__version__ = '\1-slot'|" -i google/protobuf/__init__.py
}

build() {
  cd $_pkgbase-$_pkgver
  ./autogen.sh
  ./configure --prefix=/usr
  make $MAKEFLAGS

  cd ../$_pkgbase-$_pkgver-slot
  ./autogen.sh
  ./configure --prefix=/usr --program-prefix=protobuf3- --libdir=/usr/lib/protobuf3 --includedir=/usr/include/protobuf3
  make $MAKEFLAGS
}

check() {
  make -C $_pkgbase-$_pkgver check
  make -C $_pkgbase-$_pkgver-slot check
}

_protobuf3() {
  make DESTDIR="$pkgdir" install
  install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

package_protobuf3() {
  conflicts=('protobuf' 'protobuf-cpp')
  provides=('protobuf' 'protobuf-cpp')
  replaces=('protobuf-cpp')

  cd $_pkgbase-$_pkgver
  _protobuf3
  install -Dm644 editors/protobuf-mode.el \
    "$pkgdir"/usr/share/emacs/site-lisp/protobuf-mode.el
}

package_protobuf3-slot() {
  cd $_pkgbase-$_pkgver-slot
  _protobuf3
  install -d "$pkgdir"/usr/lib/pkgconfig
  mv "$pkgdir"/usr/lib/protobuf3/pkgconfig/protobuf.pc "$pkgdir"/usr/lib/pkgconfig/protobuf3.pc
  mv "$pkgdir"/usr/lib/protobuf3/pkgconfig/protobuf-lite.pc "$pkgdir"/usr/lib/pkgconfig/protobuf3-lite.pc
  rmdir "$pkgdir"/usr/lib/protobuf3/pkgconfig
}

_python2-protobuf3() {
  python2 setup.py install --root="$pkgdir"

  install -d "$pkgdir"/usr/share/licenses/$pkgname
  ln -s /usr/share/licenses/$_pkgbase/LICENSE "$pkgdir"/usr/share/licenses/$pkgname/
}

_python_postpatch() {
  cd $pkgdir/usr/lib/python*/site-packages
  mv google/protobuf google/protobuf3
  cd google/protobuf3
  find . -type f -name "*.py" -exec sed -i 's/google.protobuf/google.protobuf3/g' {} +
}

package_python2-protobuf3() {
  depends=('python2' 'python2-six' "protobuf3=${pkgver}")
  conflicts=('python2-protobuf')
  provides=('python2-protobuf')

  cd $_pkgbase-$_pkgver/python
  _python2-protobuf3
}

package_python2-protobuf3-slot() {
  pkgdesc='Python 2 bindings for Google Protocol Buffers'
  depends=('python2' 'python2-six' "protobuf3-slot=${pkgver}")

  cd $_pkgbase-$_pkgver-slot/python
  _python2-protobuf3
  _python_postpatch
}

_python-protobuf3() {
  python3 setup.py install --root="$pkgdir"

  install -d "$pkgdir"/usr/share/licenses/$pkgname
  ln -s /usr/share/licenses/$_pkgbase/LICENSE "$pkgdir"/usr/share/licenses/$pkgname/
}

package_python-protobuf3() {
  pkgdesc='Python 3 bindings for Google Protocol Buffers'
  depends=('python' 'python-six' "protobuf3=${pkgver}")
  conflicts=('python-protobuf')
  provides=('python-protobuf')

  cd $_pkgbase-$_pkgver/python
  _python-protobuf3
}

package_python-protobuf3-slot() {
  pkgdesc='Python 3 bindings for Google Protocol Buffers'
  depends=('python' 'python-six' "protobuf3-slot=${pkgver}")

  cd $_pkgbase-$_pkgver-slot/python
  _python-protobuf3
  _python_postpatch
}
