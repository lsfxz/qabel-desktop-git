# Maintainer : Christian Hofmann <chof@pfho.net>
pkgname=qabel-desktop-git
_pkgname=qabel-desktop
pkgver=0.11.3.beta.3.r0.g6975588
pkgrel=1
pkgdesc="Secure communication and cloud storage application"
arch=(x86_64)
url="https://www.qabel.de"
license=('custom:QaPLv0.2')
depends=('java-runtime>=8' 'sh' 'java-openjfx>=8' 'java-environment')
makedepends=('maven' 'unzip')
provides=('qabel-desktop')
source=("git+https://github.com/Qabel/qabel-desktop.git"
        "qabel-desktop-ex")
sha256sums=('SKIP'
            'eb9bcb3ec084542889c704069d235152eb9bfac44d93bbdff7ca97e12912a145')
pkgver() {
  cd "$_pkgname"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
        cd "${srcdir}/${_pkgname}"
        # fixed upstream
        # patch -R -Np1 -i "${srcdir}/qabel-openfx-fix"
        ./gradlew distZip
        unzip client/build/distributions/client-linux_amd64-dev.zip -d "${srcdir}"
        cd "${srcdir}/client-linux_amd64-dev"
}

package() {
        cd "${srcdir}/client-linux_amd64-dev/lib/"
        rm -r dev
        for libfile in *
        do
          install -Dm755 "${libfile}" "${pkgdir}/usr/share/java/${_pkgname}/${libfile}"
        done
        mkdir -p "${pkgdir}/usr/lib"
        ln -s "/usr/share/java/${_pkgname}/libcurve25519.so" "${pkgdir}/usr/lib/libcurve25519.so"

        cd ${srcdir}
        install -Dm755 "${srcdir}/${_pkgname}-ex" "${pkgdir}/usr/bin/${_pkgname}"
        install -Dm755 "${srcdir}/${_pkgname}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
