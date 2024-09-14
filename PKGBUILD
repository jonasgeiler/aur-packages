# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=yaak-bin
# renovate: datasource=github-releases depName=yaakapp/app
pkgver=2024.8.2
pkgrel=1
pkgdesc='Simple and intuitive API client for calling REST, GraphQL, and gRPC APIs (Pre-compiled version)'
arch=(x86_64)
url='https://yaak.app/'
license=(MIT)
depends=(
	# As reported by namcap
	cairo
	gcc-libs
	gdk-pixbuf2
	glib2
	glibc
	gtk3
	hicolor-icon-theme
	libsoup3
	pango
	webkit2gtk-4.1
)
provides=(yaak yaak-app)
conflicts=(
	yaak
	yaak-app
	yaak-app-beta
	yaak-appimage
	yaak-git
)
options=(
	!strip     # Stripping symbols would break the binary
	!emptydirs # Remove empty directories from package because why not
)
source_x86_64=(
	"${pkgname}-${pkgver}.deb::https://github.com/yaakapp/app/releases/download/v${pkgver}/yaak_${pkgver}_amd64.deb"
	# TODO: Use "https://raw.githubusercontent.com/yaakapp/app/v${pkgver}/LICENSE" after next release
	"${pkgname}-${pkgver}.LICENSE::https://raw.githubusercontent.com/yaakapp/app/b616c5d78f5ac9aa721b847e755ada2f87353f4d/LICENSE"
)
b2sums_x86_64=('2a6325b121932f66a6d84c3e3f56caeae1418f2fc53ff9e8925917925922d26e33de2f716b2ec3693372032f74ad6ecb199bca8964e05551c5ef927639f85614'
	'011fb406bfe4a8944efbae1f9cfa420fe421f1de3ae628802548676a1fe1318850a5f98c60cd29899efe3946dec329b6607f04917e966808f62f9e4ecaaea13b')

prepare() {
	bsdtar -xf "${srcdir}/data.tar.gz" -C "${srcdir}/"
}

build() {
	sed -e 's|Name=yaak|Name=Yaak|' \
		-e '$aGenericName=API Client' \
		-i "${srcdir}/usr/share/applications/yaak-app.desktop"
}

package() {
	cp -a \
		"${srcdir}/usr/" \
		"${pkgdir}/usr/"
	install -Dm644 \
		"${srcdir}/${pkgname}-${pkgver}.LICENSE" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
