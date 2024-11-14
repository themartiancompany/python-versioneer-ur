# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=versioneer
pkgname="${_py}-${_pkg}"
pkgver=0.29
pkgrel=2
_pkgdesc=(
  'A tool for managing a recorded version'
  'number in setuptools-based python projects'
)
arch=(
  'any'
)
_http="https://github.com"
_ns="python-${_pkg}"
url="${_http}/${_ns}/${_ns}"
license=(
  'custom:Unlicense'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-setuptools"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-installer"
  "${_py}-wheel"
)
_commit='28c613dbef5fce09dc3ba6b1baa811c2d76b2245'
source=(
  "${pkgname}::git+${url}#commit=${_commit}"
)
b2sums=(
  'SKIP'
)

pkgver() {
  cd \
    "${pkgname}"
  git \
    describe \
    --tags | \
    sed \
      's/^v//'
}

build() {
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  cd \
    "${pkgname}"
  "${_py}" \
    setup.py \
      make_versioneer
  "${_py}" \
    -m \
      unittest \
      discover \
        test
}

package() {
  local \
    site_packages
  site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # symlink license file
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/${pkgname#python-}-$pkgver.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
