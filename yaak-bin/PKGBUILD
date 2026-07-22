# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak-bin
# renovate: datasource=github-releases depName=getyaak/app
pkgver=2026.5.0
pkgrel=2
pkgdesc='Fast, offline and Git-friendly API client for HTTP, GraphQL, WebSockets, SSE, and gRPC (Pre-compiled version)'
arch=(x86_64)
url='https://yaak.app/'
license=(MIT)
depends=(
	# As reported by namcap
	cairo
	dbus
	fontconfig
	gdk-pixbuf2
	glib2
	glibc
	gtk3
	hicolor-icon-theme
	libgcc
	libsoup3
	libstdc++
	webkit2gtk-4.1
	zlib
)
provides=(yaak yaak-app yaak-app-client)
conflicts=(
	yaak
	yaak-app-beta
	yaak-appimage
	yaak-beta-bin
	yaak-git
)
replaces=(yaak-app)
options=(
	!strip     # Stripping symbols would break the binary
	!emptydirs # Remove empty directories from package because why not
)
source_x86_64=(
	"${pkgname}-${pkgver}.deb::https://github.com/mountain-loop/yaak/releases/download/v${pkgver}/yaak_${pkgver}_amd64.deb"
	"${pkgname}-${pkgver}.LICENSE::https://raw.githubusercontent.com/mountain-loop/yaak/refs/tags/v${pkgver}/LICENSE"
)
b2sums_x86_64=('89cd95e5cb2d7f984e7449ecbd22061e4e4bc46ba3894aa8fc8d49a5c06f85e5752a2f141be39185510296c2a5fa55b5f349c716b362f987f3fefdbabd3fdaf0'
               '011fb406bfe4a8944efbae1f9cfa420fe421f1de3ae628802548676a1fe1318850a5f98c60cd29899efe3946dec329b6607f04917e966808f62f9e4ecaaea13b')

prepare() {
	bsdtar -xf "${srcdir}/data.tar.gz" -C "${srcdir}/"
}

build() {
	sed -e 's|Name=yaak|Name=Yaak|' \
		-e '$aGenericName=API Client' \
		-i "${srcdir}/usr/share/applications/yaak.desktop"
}

package() {
	cp -a \
		"${srcdir}/usr/" \
		"${pkgdir}/usr/"
	install -Dm644 \
		"${srcdir}/${pkgname}-${pkgver}.LICENSE" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

	# Alias old binary name (yaak-app)
	ln -sr \
		"${pkgdir}/usr/bin/yaak-app-client" \
		"${pkgdir}/usr/bin/yaak-app"
}
