# Maintainer: Caesim404 <caesim404 at gmail dot com>
pkgname="sikulix-git"
pkgver="1.1.1"
pkgrel="1"
pkgdesc="Automates GUI tasks using image recognition"
arch=("x86_64" "i686") # contains binary blobs
url="http://www.sikulix.com/"
license=("MIT")
depends=("java-environment-jdk>=7"
         "opencv2"
         "tesseract>=3.0.2"
         "wmctrl"
         "lsb-release"
         "xdotool")
makedepends=("git" "maven>=3")
provides=("sikulix")
conflicts=("sikulix")
source=("${pkgname}::git+https://github.com/RaiMan/SikuliX-2014.git"
        "LICENSE" # http://www.sikulix.com/disclaimer.html
        "sikulix.desktop"
        "sikulix"
        "sikulix.png")
md5sums=("SKIP"
         "abb66c31bad0efba32a2a0a2982db023"
         "9fbf1a9299fcf9334bf362fc6344fde2"
         "d7185558ab460eb2f4980bc9ee086c21"
         "27aafa7f5ee889ff51da6db4e62da5d4")

prepare() {
    cd "${srcdir}/${pkgname}"
    mvn "-Duser.home=${srcdir}" clean install
}

build() {
    cd "${srcdir}/${pkgname}"
    
    sed -i "/-jar/i\           <argument>-Duser.home=${srcdir}</argument>" Setup/pom.xml
    sed -i "/-forsetup\.jar/a\ <argument>options</argument> \
                               <argument>1.1</argument> \
                               <argument>3</argument>" Setup/pom.xml
    
    mvn "-Duser.home=${srcdir}" -pl Setup exec:exec
}

package() {
    local sikulix="${srcdir}/.Sikulix/SikulixSetup"
    
    install -Dm755 "${srcdir}/sikulix"         "${pkgdir}/usr/bin/sikulix"
    install -Dm755 "${sikulix}/runsikulix"     "${pkgdir}/usr/share/sikulix/runsikulix"
    install -Dm644 "${sikulix}/sikulix.jar"    "${pkgdir}/usr/share/sikulix/sikulix.jar"
    
    install -Dm644 "${srcdir}/sikulix.desktop" "${pkgdir}/usr/share/applications/sikulix.desktop"
    install -Dm644 "${srcdir}/sikulix.png"     "${pkgdir}/usr/share/pixmaps/sikulix.png"
    install -Dm644 "${srcdir}/LICENSE"         "${pkgdir}/usr/share/licenses/sikulix/LICENSE"
}
