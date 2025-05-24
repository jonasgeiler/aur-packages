# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak
# renovate: datasource=github-releases depName=getyaak/app
pkgver=2025.2.3
# should be the same commit hash used in the GitHub Actions run for the current release tag
# (check the step where it checks out the plugins repository)
_plugins_commit=d02883282fa38ab127d7fc1c43bb8b55a1ca859a
pkgrel=1
pkgdesc='Fast, offline and Git-friendly API client for HTTP, GraphQL, WebSockets, SSE, and gRPC'
arch=(aarch64 armv7h i686 pentium4 x86_64)
url='https://yaak.app/'
license=(MIT)
depends=(
	# As reported by namcap
	cairo
	dbus
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
b2sums=('e64ddee27b43d325ae9418b2fc159cbc61a99631c2c3a128e34c8acb31938b9fbfda6f74e1787168cc631662f9d5e7253f54036907341d69e291d317cb79322c'
        '058add79b12f2b742ac6c0b255a9e7a8cbdc58e00e178544218e8101d5bc311822252d86b64bf6838363a3a325807d1d18468154a77849bb911ab40a5dea7d37')

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
