# Maintainer: Jason R. McNeil <jason@jasonrm.net>

_pkgname=predictionio
_gitname=PredictionIO
pkgname=${_pkgname}-git
pkgver=v0.9.3.r23.gb399cc6
pkgrel=1
pkgdesc="An open-source machine learning server"
arch=('any')
url="http://spark.apache.org"
license=('APACHE')
depends=('java-environment>=7')
makedepends=('git')
optdepends=(
  'elasticsearch: default metadata store'
  'apache-hbase: default event data store'
  'apache-spark: default processing engine' # might be required on each node
  'postgresql: alternative metadata and event data store'
)
install='predictionio.install'
source=(
    'git+https://github.com/PredictionIO/PredictionIO.git'
    'predictionio.service'
    'pio-env.sh'
)
sha256sums=('SKIP'
            '4656c089aa5b20d417fdd65284e42a35c22dfae29c0318f0c404b5643c5e3016'
            '2716fb2c72583d1737e9f635bbd4ca56c86e4dca47df03624eb40441570b9e3c')
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

  install -Dm644 "${srcdir}/predictionio.service" "${pkgdir}/usr/lib/systemd/system/predictionio.service"
  install -Dm644 "${srcdir}/pio-env.sh" "${pkgdir}/etc/predictionio/pio-env.sh"

  # cd "$pkgdir/usr/share/predictionio/conf"
  # ln -sf "/etc/predictionio/spark-env.sh" .
}
