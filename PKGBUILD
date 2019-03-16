# Maintainer: Vladislav Semyonoff <vsemyonoff@gmail.com>
# Contributor: Vladislav Semyonoff <vsemyonoff@gmail.com>

_module=i915
_module_path=drivers/gpu/drm/${_module}
_pkgname=${_module}-patched
pkgname=${_pkgname}-dkms

pkgver=0.0.1
pkgrel=1

arch=('any')
pkgdesc="Kernel 5.0.2 patched i915 module, Dell XPS 9570 black screen fix"
url="https://github.com/vsemyonoff/${_pkgname}"
license=('GPL2')

conflicts=("${_pkgname}")
depends=('dkms')
makedepends=('git')

install=package.install

source=("git+https://github.com/vsemyonoff/${_pkgname}#branch=master" "dkms.conf")

sha256sums=('SKIP' 'SKIP')

package() {
  # Copy dkms.conf
  install -Dm644 dkms.conf "${pkgdir}"/usr/src/${_pkgname}-${pkgver}/dkms.conf

  # Set name and version
  sed -e "s/@MODULE@/${_module}/" \
      -e "s|@MODULE_PATH@|${_module_path}|" \
      -e "s/@PKGNAME@/${_pkgname}/" \
      -e "s/@PKGVER@/${pkgver}/" \
      -i "${pkgdir}"/usr/src/${_pkgname}-${pkgver}/dkms.conf

  # Copy sources (including Makefile)
  cp -dr --no-preserve='ownership' ${_pkgname}/* "${pkgdir}/usr/src/${_pkgname}-${pkgver}/"

  # Override module path
  echo "override ${_module} 5.0.* updates/${_module_path}" |
      install -Dm644 /dev/stdin "${pkgdir}/etc/depmod.d/${_pkgname}-5.0.conf"
}
