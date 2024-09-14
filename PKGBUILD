# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=yaak-appimage
pkgver=2024.8.2
pkgrel=4
pkgdesc='Simple and intuitive API client for calling REST, GraphQL, and gRPC APIs (AppImage version)'
arch=(x86_64)
url='https://yaak.app/'
license=(MIT)
depends=(
    # As reported by namcap
    glibc
    hicolor-icon-theme
    zlib
)
# TODO: Add all provides and conflicts
provides=(yaak)
conflicts=(yaak)
options=(
    !strip # Stripping symbols would break the appimage
    !emptydirs # Remove empty directories from package some icon dirs are empty
)
source_x86_64=(
    "${pkgname}-${pkgver}.AppImage::https://github.com/yaakapp/app/releases/download/v${pkgver}/yaak_${pkgver}_amd64.AppImage"
    # TODO: Use "https://raw.githubusercontent.com/yaakapp/app/v${pkgver}/LICENSE" after next release
    "${pkgname}-${pkgver}.LICENSE::https://raw.githubusercontent.com/yaakapp/app/b616c5d78f5ac9aa721b847e755ada2f87353f4d/LICENSE"
)
b2sums_x86_64=('4f69a9ace92b1844744b9a38c0c02577f466cc83fe8493ea7d6d416473279dec94f5e2a2849a9ab4c6d8352e035053f3eda337f3c8347feef15c940e85f3a8b0'
               '011fb406bfe4a8944efbae1f9cfa420fe421f1de3ae628802548676a1fe1318850a5f98c60cd29899efe3946dec329b6607f04917e966808f62f9e4ecaaea13b')

prepare() {
    cd "${srcdir}"
    chmod +x "${srcdir}/${pkgname}-${pkgver}.AppImage"
    "${srcdir}/${pkgname}-${pkgver}.AppImage" --appimage-extract > /dev/null
    chmod -R a-x+rX "${srcdir}/squashfs-root/usr/"
}

build() {
    sed -e 's|Exec=.*|Exec=env DESKTOPINTEGRATION=0 APPIMAGELAUNCHER_DISABLE=1 /usr/bin/yaak-app|' \
        -e '$aPath=/usr/bin' \
        -e 's|Name=yaak|Name=Yaak|' \
        -e '$aGenericName=API Client' \
        -i "${srcdir}/squashfs-root/yaak-app.desktop"
}

package() {
    install -Dm755 \
        "${srcdir}/${pkgname}-${pkgver}.AppImage" \
        "${pkgdir}/usr/bin/yaak-app"
    install -Dm644 \
        "${srcdir}/squashfs-root/yaak-app.desktop" \
        "${pkgdir}/usr/share/applications/yaak-app.desktop"
    install -Dm644 \
        "${srcdir}/${pkgname}-${pkgver}.LICENSE" \
        "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

    install -dm755 "${pkgdir}/usr/share/"
    cp -a \
        "${srcdir}/squashfs-root/usr/share/icons" \
        "${pkgdir}/usr/share/icons"
}
