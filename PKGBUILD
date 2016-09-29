# Maintainer: lsfxz <lsfxz at pfho dot net>
pkgname=qabel-desktop-git
_pkgname=qabel-desktop
pkgver=0.9.1.beta.3.r24.ga12edcc
pkgrel=1
pkgdesc="Secure communication and cloud storage application"
arch=(x86_64)
url="https://www.qabel.de"
license=('custom:QaPLv0.2')
depends=('java-runtime>=8' 'sh' 'java-openjfx>=8' 'java-environment')
makedepends=('maven' 'unzip')
provides=('qabel-desktop')
source=("git+https://github.com/Qabel/qabel-desktop.git"
        "qabel-desktop-ex"
        "qabel-openfx-fix")
sha256sums=('SKIP'
            'eb9bcb3ec084542889c704069d235152eb9bfac44d93bbdff7ca97e12912a145'
            '6e138f90f09d0a70ce04b1894c4c0d9ca40fd5cb682159aecf1bae85b99cc9c4')
pkgver() {
  cd "$_pkgname"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
        cd "${srcdir}/${_pkgname}"
        patch -R -Np1 -i "${srcdir}/qabel-openfx-fix"
        ./gradlew distZip -Dhttp.proxyHost=localhost -Dhttp.proxyPort=3128 -Dhttps.proxyHost=localhost -Dhttps.proxyPort=3128
        unzip build/distributions/qabel-desktop-linux_amd64-dev.zip -d "${srcdir}"
        cd "${srcdir}/qabel-desktop-linux_amd64-dev"
}

package() {
        cd "${srcdir}/qabel-desktop-linux_amd64-dev/lib/"
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
