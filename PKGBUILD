# Maintainer: Jason R. McNeil <jason@jasonrm.net>

_pkgname=predictionio
_gitname=PredictionIO
pkgname=${_pkgname}-git
pkgver=v0.9.3.r23.gb399cc6
pkgrel=2
pkgdesc="An open-source machine learning server"
arch=('any')
url="https://prediction.io"
license=('APACHE')
provides=('predictionio')
depends=('java-environment>=7', 'apache-spark')
makedepends=('git')
optdepends=(
  'elasticsearch: default metadata store'
  'apache-hbase: default event data store'
  'postgresql: alternative metadata and event data store'
)
install='predictionio.install'
source=(
    'git+https://github.com/PredictionIO/PredictionIO.git'
    'predictionio-eventserver.service'
    'pio-env.sh'
)
sha256sums=(
  'SKIP'
  '06f97eab783d2f9a07324a55bf0605f3b7cde656b1002e710c81182967c8cc33'
  '9fd91b3d3e9a769d3e864dfeaf999627aee690468d132707b3d19a3ddc8e7efc'
)
backup=(
  'etc/predictionio/pio-env.sh'
)

pkgver() {
  cd "${srcdir}/${_gitname}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  cd "${srcdir}/${_gitname}"
  JAVA_HOME=/usr/lib/jvm/default-runtime ./make-distribution.sh
}

package() {
  cd "${srcdir}/${_gitname}"

  VERSION=$(grep version build.sbt | grep ThisBuild | grep -o '".*"' | sed 's/"//g')
  TARNAME="PredictionIO-$VERSION.tar.gz"
  TARDIR="PredictionIO-$VERSION"

  tar -xf ${TARNAME}

  install -d "${pkgdir}/usr/bin/" "${pkgdir}/usr/share/"

  cp -r "${srcdir}/${_gitname}/${TARDIR}" "${pkgdir}/usr/share/predictionio"

  install -Dm644 "${srcdir}/predictionio-eventserver.service" "${pkgdir}/usr/lib/systemd/system/predictionio-eventserver.service"
  install -Dm644 "${srcdir}/pio-env.sh" "${pkgdir}/etc/predictionio/pio-env.sh"

  cd "$pkgdir/usr/bin"
  for binary in pio pio-class pio-shell; do
    binpath="/usr/share/predictionio/bin/$binary"
    ln -s "$binpath" $binary
    # Because we'll be symlinking we need to override the installation directory detection
    sed -i 's|^export FWDIR=.*$|export FWDIR=/usr/share/predictionio|' "$pkgdir/$binpath"
  done

  cd "${pkgdir}/usr/share/predictionio/conf"
  ln -sf "/etc/predictionio/pio-env.sh" .
}
