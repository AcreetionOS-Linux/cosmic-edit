# Maintainer: Mark Wagie <mark at proton dot me>
# Co-Maintainer: soloturn <soloturn@gmail.com>
#
# some rando on the internet: Liv Rickey <rickeyollie@gmail.com>
pkgname=cosmic-edit-git
pkgver=1.0.0.alpha.3.r0.g3d92409
pkgrel=1
pkgdesc="Text editor for the COSMIC desktop"
arch=('x86_64')
url="https://github.com/pop-os/cosmic-edit"
license=('GPL-3.0-or-later')
depends=(
  'cosmic-icons-git'
  'libxkbcommon'
  'wayland'
)
makedepends=(
  'cargo'
  'git'
  'git-lfs'
  'just'
  'mold'
)
provides=("${pkgname%-git}" 'cosmic-text-editor')
conflicts=("${pkgname%-git}" 'cosmic-text-editor')
source=('git+https://github.com/AcreetionOS-Linux/cosmic-edit.git')
#sha256sums=('SKIP'
#            'a6bd11d790dbdc9eaa3741990eaa4d7ac1e123f8b30ac62cd9303670be98fbfb')

#skip this for now
sha256sums=('SKIP')

#comment out do to it erroring
#pkgver() {
#  cd "${pkgname%-git}"
#  git describe --long --tags --abbrev=7 | sed 's/^epoch-//;s/\([^-]*-g\)/r\1/;s/-/./g'
#}

prepare() {
  cd "${pkgname%-git}"

  # Use thin LTO, we already patched this in the repo. no need to do it again
  #patch -Np1 -i ../lto.patch

  export RUSTUP_TOOLCHAIN=stable
  cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"

  git lfs install --local
  git remote add network-origin https://github.com/pop-os/cosmic-edit
  git lfs fetch network-origin
  git lfs checkout
}

build() {
  cd "${pkgname%-git}"
  export RUSTUP_TOOLCHAIN=stable

  # use mold instead of lld to speed up build
  RUSTFLAGS+=" -C link-arg=-fuse-ld=mold"

  # use nice to build with lower priority
  nice just build-release --frozen
}

package() {
  cd "${pkgname%-git}"
  just rootdir="$pkgdir" install
  }
