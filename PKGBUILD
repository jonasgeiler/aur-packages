# Maintainer: Jonas Geiler <aur@jonasgeiler.com>

_name=yaak

pkgname="${_name}"-appimage
pkgver=2024.7.0
pkgrel=1
pkgdesc="Interact with REST, GraphQL and gRPC in a simple yet powerful desktop application (AppImage version)"
arch=('x86_64')
url="https://yaak.app/"
license=('unknown')
depends=('glibc' 'zlib' 'hicolor-icon-theme')
provides=("${_name}")
options=(!strip !debug) # necessary, otherwise the AppImage file in the package gets truncated
_appimage="${_name}_${pkgver}_amd64.AppImage"
source_x86_64=("https://releases.yaak.app/releases/${pkgver}/${_appimage}.tar.gz")
sha256sums_x86_64=('6f6350b5656700f7aa9e73918ed5a4038d2732ef06f0a2d88f11048257efd61c')

prepare() {
    # Extract AppImage
    chmod +x "${_appimage}"
    ./"${_appimage}" --appimage-extract > /dev/null
}

build() {
    # Adjust .desktop so it will work outside of AppImage container
    sed -i -E "s|Exec=.*|Exec=env DESKTOPINTEGRATION=0 APPIMAGELAUNCHER_DISABLE=1 /usr/bin/${_name}|" \
        "squashfs-root/${_name}-app.desktop"

    # Fix permissions; .AppImage permissions are 700 for all directories
    chmod -R a-x+rX squashfs-root/usr
}

package() {
    # AppImage
    install -Dm755 "${srcdir}/${_appimage}" \
        "${pkgdir}/opt/appimages/${_name}.AppImage"

    # Desktop file
    install -Dm644 "${srcdir}/squashfs-root/${_name}-app.desktop" \
        "${pkgdir}/usr/share/applications/${_name}-app.desktop"

    # Icon images
    install -dm755 "${pkgdir}/usr/share/"
    cp -a "${srcdir}/squashfs-root/usr/share/icons" "${pkgdir}/usr/share/"

    # Symlink executable
    install -dm755 "${pkgdir}/usr/bin/"
    ln -s "/opt/appimages/${_name}.AppImage" "${pkgdir}/usr/bin/${_name}"
}
