# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak
# renovate: datasource=github-releases depName=getyaak/app
pkgver=2025.1.1
# should be the same commit hash used in the GitHub Actions run for the current release tag
# (check the step where it checks out the plugins repository)
_plugins_commit=d9a1e124f549b8601ca76245112fcdb75286991d
pkgrel=1
pkgdesc='Offline and Git friendly API client for HTTP, GraphQL, WebSockets, SSE, and gRPC'
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
	zlib
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
b2sums=('c844847311a588272e515e738662053cb4cc0b0d23ea5c4924fbdb2d38f9309d150a285dd3dd05a01b055627fcba14933677258277eee2c08032b0daeecac432'
        'c6e6265de6a41c8516f64602f712db63e7cc5e71e968ec14d769615ba9b67ca044b93617e40996763a4a70ee415a15c3d8ba46310683f4b288746771c8461358')

build() {
	export YAAK_VERSION="${pkgver}"
	export YAAK_PLUGINS_DIR="${srcdir}/yaak-plugins/"
	export TAURI_APP_PATH="${srcdir}/yaak/src-tauri/"
	export CI=true

	cd "${srcdir}/yaak/"
	npm ci
	npm install @yaakapp/cli
	npm run build
	npm run replace-version
	npm run tauri build -- --bundles deb --config '{ "bundle": { "createUpdaterArtifacts": false } }'

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
