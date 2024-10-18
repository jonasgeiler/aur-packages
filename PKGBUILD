# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak-appimage
# renovate: datasource=github-releases depName=yaakapp/app
pkgver=2024.11.3
pkgrel=1
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
provides=(yaak yaak-app)
conflicts=(
	yaak
	yaak-app-beta
	yaak-bin
	yaak-git
)
options=(
	!strip     # Stripping symbols would break the AppImage
	!emptydirs # Remove empty directories from package some icon dirs are empty
)
source_x86_64=(
	"${pkgname}-${pkgver}.AppImage::https://github.com/yaakapp/app/releases/download/v${pkgver}/yaak_${pkgver}_amd64.AppImage"
	"${pkgname}-${pkgver}.LICENSE::https://raw.githubusercontent.com/yaakapp/app/refs/tags/v${pkgver}/LICENSE"
)
b2sums_x86_64=('09bedfe5635bdc20d1478df06403b107f9d8eddd356c892b907abfa9b8412b7f4d549915d168f10bf45e8ce652f666f95cd76123e5385fbf1135603e14ed49e1'
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
		-i "${srcdir}/squashfs-root/yaak.desktop"
}

package() {
	install -Dm755 \
		"${srcdir}/${pkgname}-${pkgver}.AppImage" \
		"${pkgdir}/usr/bin/yaak-app"
	install -Dm644 \
		"${srcdir}/squashfs-root/yaak.desktop" \
		"${pkgdir}/usr/share/applications/yaak.desktop"
	install -Dm644 \
		"${srcdir}/${pkgname}-${pkgver}.LICENSE" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

	install -dm755 "${pkgdir}/usr/share/"
	cp -a \
		"${srcdir}/squashfs-root/usr/share/icons" \
		"${pkgdir}/usr/share/icons"
}
