# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak
# renovate: datasource=github-releases depName=getyaak/app
pkgver=2024.12.1
# should be the same commit hash used in the GitHub Actions run for the current release tag
# (check the step where it checks out the plugins repository)
_plugins_commit=ab48f118af3988e70859fdb37afa69c9cbe509d1
pkgrel=1
pkgdesc='Simple and intuitive API client for calling REST, GraphQL, and gRPC APIs'
arch=(aarch64 armv7h i686 pentium4 x86_64)
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
makedepends=(
	appmenu-gtk-module   # Tauri
	cargo                # Rust dependencies & Tauri build
	git                  # Sources
	libappindicator-gtk3 # Tauri
	librsvg              # Tauri
	nodejs               # Custom scripts & web build
	npm                  # Node dependencies & web build
	protobuf             # Yaak
)
provides=(yaak-app)
conflicts=(
	yaak-app-beta
	yaak-appimage
	yaak-bin
	yaak-git
)
options=(
	!lto       # Some Rust dependencies don't support link time optimization
	!strip     # Stripping symbols would break the output binary
	!emptydirs # Remove empty directories from package because why not
)
source=(
	"yaak::git+https://github.com/mountain-loop/yaak.git#tag=v${pkgver}"
	"yaak-plugins::git+https://github.com/mountain-loop/plugins.git#commit=${_plugins_commit}"
)
b2sums=('03633d07cf5d8dc685dea8cc3f94ea3cf6b0eee67364a4aacd85b6e44902ca851ceeb68470d46b991a0c5cb5625ae8b486f41d3577189b10266dcb48a067acfa'
        'df35f4bd3e14f2f2d1e6e675c5411401256536c165ac9da5c328a2c0277b685f6fd5daafdece511622632673726d09f92472149dcad93abf97356aa88d30e0e1')

build() {
	export YAAK_VERSION="${pkgver}"
	export YAAK_PLUGINS_DIR="${srcdir}/yaak-plugins/"
	export CI=true

	cd "${srcdir}/yaak/"
	npm ci
	npm install @yaakapp/cli
	npm run replace-version
	npm run tauri build -- --verbose --bundles deb

	sed -e 's|Name=yaak|Name=Yaak|' \
		-e '$aGenericName=API Client' \
		-i "${srcdir}/yaak/src-tauri/target/release/bundle/deb/yaak_${pkgver}_"*/data/usr/share/applications/yaak.desktop
}

package() {
	cp -a \
		"${srcdir}/yaak/src-tauri/target/release/bundle/deb/yaak_${pkgver}_"*/data/usr/ \
		"${pkgdir}/usr/"
	install -Dm644 \
		"${srcdir}/yaak/LICENSE" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
