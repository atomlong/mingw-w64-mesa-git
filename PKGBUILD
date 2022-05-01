pkgname=mingw-w64-mesa-git
pkgver=22.2.0_devel.153080.53fe6f10846
pkgrel=1
pkgdesc="An open-source implementation of the OpenGL specification (mingw-w64)"
arch=('any')
url="https://www.mesa3d.org/"
license=("custom")
makedepends=('mingw-w64-meson' 'mingw-w64-cmake' 'python-mako')
depends=('mingw-w64-llvm' 'mingw-w64-vulkan-icd-loader' 'mingw-w64-dlfcn')
provides=('mingw-w64-mesa')
conflicts=('mingw-w64-mesa')
options=('staticlibs' '!strip' '!buildflags')
source=("git+git://anongit.freedesktop.org/mesa/mesa")
sha256sums=('SKIP')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

pkgver() {
  cd mesa
  read -r _ver <VERSION
  echo ${_ver/-/_}.$(git rev-list --count HEAD).$(git rev-parse --short HEAD)
}

build() {
  cd "${srcdir}"/mesa
  for _arch in ${_architectures}; do
    ${_arch}-meson build-${_arch} -Db_lto=false -Dgallium-drivers=swrast,zink -Dvulkan-drivers=swrast -Dvulkan-icd-dir=bin
    ninja -C build-${_arch} ${MAKEFLAGS}
  done
}

package() {
  cd "${srcdir}"/mesa
  for _arch in ${_architectures}; do
    DESTDIR="$pkgdir" ninja -C build-${_arch} install
    rm -r "$pkgdir"/usr/${_arch}/{lib,include}
    ${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
  done
}
