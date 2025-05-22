# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak
# renovate: datasource=github-releases depName=getyaak/app
pkgver=2025.2.2
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
b2sums=('d84238d6c3b7ede9b9538dad5506d606692ad4b7e52b5569655156c5abef2dd5d9e80ba7c9e4466b8ae2a45691f70466ea349a7813d9709e2b0c9fca29041dae'
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
