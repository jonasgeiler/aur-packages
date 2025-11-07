# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=drsprinto-appimage
# renovate: datasource=drsprinto
pkgver=4.0.8
pkgrel=1
pkgdesc='Reports device health and compliance status to the Sprinto employee portal (AppImage version)'
arch=(x86_64)
url='https://sprinto.com/'
license=(LicenseRef-unknown)
depends=(
	# As reported by namcap
	hicolor-icon-theme
	zlib
)
provides=(drsprinto)
conflicts=(
	drsprinto
	drsprinto-bin
	drsprinto-git
)
options=(
	!strip     # Stripping symbols would break the AppImage
	!emptydirs # Remove empty directories from package some icon dirs are empty
)
source_x86_64=("${pkgname}-${pkgver}.AppImage::https://static.sprinto.com/drsprinto/DrSprinto-${pkgver}.AppImage")
b2sums_x86_64=('23ccb3dd57ed7a3cff179f2f8f5af17685c2cc791f3f0ec7d57732c93df92c7c7e037d5cbec67fa6ee5d5716a922489a49fe6fef310081ffd7fd4b816874f414')

prepare() {
	cd "${srcdir}"
	chmod +x "${srcdir}/${pkgname}-${pkgver}.AppImage"
	"${srcdir}/${pkgname}-${pkgver}.AppImage" --appimage-extract > /dev/null
	chmod -R a-x+rX "${srcdir}/squashfs-root/usr/"
}

build() {
	sed -e 's|Exec=.*|Exec=env DESKTOPINTEGRATION=0 APPIMAGELAUNCHER_DISABLE=1 /usr/bin/drsprinto|' \
		-e '$aPath=/usr/bin' \
		-e '$aGenericName=MDM Client' \
		-i "${srcdir}/squashfs-root/drsprinto.desktop"
}

package() {
	install -Dm755 \
		"${srcdir}/${pkgname}-${pkgver}.AppImage" \
		"${pkgdir}/usr/bin/drsprinto"
	install -Dm644 \
		"${srcdir}/squashfs-root/drsprinto.desktop" \
		"${pkgdir}/usr/share/applications/drsprinto.desktop"

	install -dm755 "${pkgdir}/usr/share/"
	cp -a \
		"${srcdir}/squashfs-root/usr/share/icons" \
		"${pkgdir}/usr/share/icons"
}
