# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak
# renovate: datasource=github-releases depName=yaakapp/app
pkgver=2024.11.4
# should be the same commit hash used in the GitHub Actions run for the current release tag
# (check the step where it checks out the plugins repository)
_plugins_commit=9d24aefba186d1c3ebec6eeec90eba2a66435a98
pkgrel=2
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
	"yaak::git+https://github.com/yaakapp/app.git#tag=v${pkgver}"
	"yaak-plugins::git+https://github.com/yaakapp/plugins.git#commit=${_plugins_commit}"
)
b2sums=('d23eb5b28824715154f7d0b002e1af71383c92b44fe0a58c42c5b5e8197a0ee5d61eb3fd6083b0ddbea6b5333ce2ad790dd5598dc3616c7d38dc4fe5924f1ec1'
        '862b6920177f90b9b13e3737cfc57f415050a1e40257bd298713488df71b03d606c253e1d2b4d20bf51314ce075a6349aa7338a9e76174df122be856d2120b6a')

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
