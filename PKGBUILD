# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak-bin
# renovate: datasource=github-releases depName=yaakapp/app
pkgver=2024.10.1
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
	yaak-app-beta
	yaak-appimage
	yaak-git
)
replaces=(yaak-app)
options=(
	!strip     # Stripping symbols would break the binary
	!emptydirs # Remove empty directories from package because why not
)
source_x86_64=(
	"${pkgname}-${pkgver}.deb::https://github.com/yaakapp/app/releases/download/v${pkgver}/yaak_${pkgver}_amd64.deb"
	"${pkgname}-${pkgver}.LICENSE::https://raw.githubusercontent.com/yaakapp/app/refs/tags/v${pkgver}/LICENSE"
)
b2sums_x86_64=('791e7e4ccb73ab6673c45f352ccc874149af9654356272ba2446f74b649940e0a1cff3a3b0fab964ce742559da0f2c50dad4ae24942b8691c62380109df65ed6'
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
}
