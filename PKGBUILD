# Maintainer: Jonas Geiler <aur@jonasgeiler.com>

_basename=yaak
_basedesc='The most intuitive desktop API client to organize and execute REST, GraphQL, and gRPC requests'

pkgname="${_basename}-appimage"
pkgver=2024.8.2
pkgrel=2
pkgdesc="${_basedesc} (AppImage download)"
url='https://yaak.app/'
license=(MIT)
provides=("${_basename}")

depends=(glibc
         hicolor-icon-theme
         zlib)

_appimage="${pkgname}-${pkgver}.AppImage"
_license="${pkgname}-${pkgver}.LICENSE"
arch=(x86_64)
# TODO: Use "https://raw.githubusercontent.com/yaakapp/app/v${pkgver}/LICENSE" after next release
source_x86_64=("${_appimage}::https://github.com/yaakapp/app/releases/download/v${pkgver}/yaak_${pkgver}_amd64.AppImage"
               "${_license}::https://raw.githubusercontent.com/yaakapp/app/master/LICENSE")
b2sums_x86_64=('4f69a9ace92b1844744b9a38c0c02577f466cc83fe8493ea7d6d416473279dec94f5e2a2849a9ab4c6d8352e035053f3eda337f3c8347feef15c940e85f3a8b0'
               '011fb406bfe4a8944efbae1f9cfa420fe421f1de3ae628802548676a1fe1318850a5f98c60cd29899efe3946dec329b6607f04917e966808f62f9e4ecaaea13b')
options=('!strip')

prepare() {
    # Extract AppImage
    chmod +x "${_appimage}"
    ./"${_appimage}" --appimage-extract > /dev/null
}

build() {
    # Adjust executable in .desktop so it will work outside of AppImage container
    sed -i -E "s|Exec=.*|Exec=env DESKTOPINTEGRATION=0 APPIMAGELAUNCHER_DISABLE=1 /usr/bin/${_basename}|" \
        squashfs-root/yaak-app.desktop
    echo 'Path=/usr/bin' >> squashfs-root/yaak-app.desktop

    # Adjust name in .desktop to match website
    sed -i -E 's|Name=yaak|Name=Yaak|' \
        squashfs-root/yaak-app.desktop

    # Add generic name to .desktop
    echo 'GenericName=API Client' >> squashfs-root/yaak-app.desktop

    # Fix permissions (AppImage extract permissions are 700 for all directories)
    chmod -R a-x+rX squashfs-root/usr

    # Remove empty directories in icons directory
    find squashfs-root/usr/share/icons -type d -empty -delete
}

package() {
    # Add AppImage
    install -Dm755 \
        "${srcdir}/${_appimage}" \
        "${pkgdir}/opt/${pkgname}/${_basename}.AppImage"

    # Add license
    install -Dm644 \
        "${srcdir}/${_license}" \
        "${pkgdir}/opt/${pkgname}/LICENSE"

    # Add .desktop file
    install -Dm644 \
        "${srcdir}/squashfs-root/yaak-app.desktop" \
        "${pkgdir}/opt/${pkgname}/${_basename}.desktop"

    # Add icon images (directly into /usr/share/icons)
    install -dm755 "${pkgdir}/usr/share/"
    cp -a \
        "${srcdir}/squashfs-root/usr/share/icons" \
        "${pkgdir}/usr/share/icons"

    # Symlink AppImage
    install -dm755 "${pkgdir}/usr/bin/"
    ln -s \
        "/opt/${pkgname}/${_basename}.AppImage" \
        "${pkgdir}/usr/bin/${_basename}"

    # Symlink license
    install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}/"
    ln -s \
        "/opt/${pkgname}/LICENSE" \
        "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

    # Symlink .desktop file
    install -dm755 "${pkgdir}/usr/share/applications/"
    ln -s \
        "/opt/${pkgname}/${_basename}.desktop" \
        "${pkgdir}/usr/share/applications/${_basename}.desktop"
}
