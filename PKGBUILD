# Maintainer: Jason R. McNeil <jason@jasonrm.net>

_pkgname=predictionio
_gitname=PredictionIO
pkgname=${_pkgname}-git
pkgver=v0.9.3.r23.gb399cc6
pkgrel=1
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
  '0865ed8b9c13c61ff01616ec43f056c2aac0de041f79aeaf5145748f5ceecf38'
  '9fd91b3d3e9a769d3e864dfeaf999627aee690468d132707b3d19a3ddc8e7efc'
)
backup=()

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

  install -d "$pkgdir/opt/"

  cp -r "${srcdir}/${_gitname}/${TARDIR}" "$pkgdir/opt/predictionio"

  install -Dm644 "${srcdir}/predictionio-eventserver.service" "${pkgdir}/usr/lib/systemd/system/predictionio-eventserver.service"
  install -Dm644 "${srcdir}/pio-env.sh" "${pkgdir}/etc/predictionio/pio-env.sh"

  cd "$pkgdir/opt/predictionio/conf"
  ln -sf "/etc/predictionio/spark-env.sh" .
}
