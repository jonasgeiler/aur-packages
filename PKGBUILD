# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak-appimage
# renovate: datasource=github-releases depName=getyaak/app
pkgver=2025.1.1
pkgrel=1
pkgdesc='Offline and Git friendly API client for HTTP, GraphQL, WebSockets, SSE, and gRPC (AppImage version)'
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
	yaak-beta-bin
	yaak-bin
	yaak-git
)
options=(
	!strip     # Stripping symbols would break the AppImage
	!emptydirs # Remove empty directories from package some icon dirs are empty
)
source_x86_64=(
	"${pkgname}-${pkgver}.AppImage::https://github.com/mountain-loop/yaak/releases/download/v${pkgver}/yaak_${pkgver}_amd64.AppImage"
	"${pkgname}-${pkgver}.LICENSE::https://raw.githubusercontent.com/mountain-loop/yaak/refs/tags/v${pkgver}/LICENSE"
)
b2sums_x86_64=('aff41b070507eacce046f8cdd83d414715740e39c64360b42e4014669f85356aef5995c84a4f3e2ba69780e625f691b59300daae4dbab78807f195fe1a676846'
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
