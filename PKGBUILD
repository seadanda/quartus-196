# Maintainer: Dónal Murray <donal.murray@cern.ch>

pkgname=quartus-196
pkgver=16.1.0.196
pkgrel=0
pkgdesc="Quartus Prime Standard Edition 16.1 build 196 with Modelsim simulation tool" # with arria 10 FPGAs when I find a dl source
arch=('x86_64')
url="http://dl.altera.com/?edition=standard"
license=('custom')

_alteradir="/opt/altera/16.1"

depends=('desktop-file-utils' 'expat' 'fontconfig' 'freetype2' 'glibc'
         'gtk2' 'libcanberra' 'libpng' 'libpng12' 'libice' 'libsm'
         'util-linux' 'ncurses' 'tcl' 'tcllib' 'zlib' 'libx11' 'libxau'
         'libxdmcp' 'libxext' 'libxft' 'libxrender' 'libxt' 'libxtst'
         'lib32-expat' 'lib32-fontconfig' 'lib32-freetype2' 'lib32-glibc'
         'lib32-gtk2' 'lib32-libcanberra' 'lib32-libpng' 'lib32-libpng12'
         'lib32-libice' 'lib32-libsm' 'lib32-ncurses5-compat-libs'
         'lib32-util-linux' 'lib32-ncurses' 'lib32-zlib' 'lib32-libx11'
         'lib32-libxau' 'lib32-libxdmcp' 'lib32-libxext' 'lib32-libxft'
         'lib32-libxrender' 'lib32-libxt' 'lib32-libxtst')


source=("http://download.altera.com/akdlm/software/acdsinst/16.1/${pkgver##*.}/ib_installers/QuartusSetup-${pkgver}-linux.run"
        "http://download.altera.com/akdlm/software/acdsinst/16.1/${pkgver##*.}/ib_installers/ModelSimSetup-${pkgver}-linux.run"
	      "quartus" "quartus.desktop" "quartus.install" "quartus.sh" "51-usbblaster.rules")
md5sums=('d8a1730c18f2d79eb080786fffe2e203'
         'f665d7016ff793e64f57b08b37487d0e'
         '4d5a388355a2a62c658709e0ba63edea'
         '9e5eff4435bb7b6de0f0014c598167c1'
         'a331a81c44aed062a7af6d28542c3d82'
         '737d51fcc74c8d6d2114c8f4ba79e4de'
         '9a053f97c2ca41ef30d60ec2ab8ecd55')

options=(!strip !upx)
install='quartus.install'
PKGEXT=".pkg.tar" # Do not compress

package() {
    cd "${srcdir}"

    chmod a+x "QuartusSetup-${pkgver}-linux.run"

    echo "Badly made installer never properly returns. Also doesn't work without GUI."
    echo "Click finish then \"ps -e | grep Quartus\" and kill the pid."
    echo "installing Quartus Prime Standard ${pkgver}"
    ./"QuartusSetup-${pkgver}-linux.run" --installdir "${pkgdir}/${_alteradir}" &

    read -p "Press enter after successfully installing to continue"

    # Remove uninstaller and install logs since we have a working package management
    rm -r "${pkgdir}/${_alteradir}/uninstall"
    rm -r "${pkgdir}/${_alteradir}/logs"

    # Fix broken vsim deps
    echo "sed -i \"s,linux_rh60,linux,q\" \"${pkgdir}/${_alteradir}/modelsim\_ase/bin/vsim\""

    # Replace altera directory in integration files
    sed -i.bak "s,_alteradir,${_alteradir},g" quartus.sh

    # Copy license file
    install -D -m644 "${pkgdir}/${_alteradir}/licenses/license.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

    # Install integration files
    install -D -m755 quartus.sh "${pkgdir}/etc/profile.d/quartus.sh"
    install -D -m755 quartus "${pkgdir}/usr/bin/quartus"
    install -D -m644 51-usbblaster.rules "${pkgdir}/etc/udev/rules.d/51-usbblaster.rules"
    install -D -m644 quartus.desktop "${pkgdir}/usr/share/applications/quartus.desktop"
}

# vim:set ts=2 sw=2 et:
