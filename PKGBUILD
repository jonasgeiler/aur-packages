# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: SoftExpert <softexpert at gmail dot com>
pkgname=yaak-git
pkgver=2025.4.0.r0.gaadfbfd
pkgrel=1
pkgdesc='Fast, offline and Git-friendly API client for HTTP, GraphQL, WebSockets, SSE, and gRPC (Development version)'
arch=(aarch64 armv7h i686 pentium4 x86_64)
url='https://yaak.app/'
license=(MIT)
depends=(
	# As reported by namcap
	bzip2
	cairo
	dbus
	fontconfig
	gcc-libs
	gdk-pixbuf2
	glib2
	glibc
	gtk3
	hicolor-icon-theme
	libsoup3
	pango
	webkit2gtk-4.1
	xz
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
provides=(yaak yaak-app)
conflicts=(
	yaak
	yaak-app-beta
	yaak-appimage
	yaak-bin
)
options=(
	!lto       # Some Rust dependencies don't support link time optimization
	!strip     # Stripping symbols would break the output binary
	!emptydirs # Remove empty directories from package because why not
)
source=("yaak::git+https://github.com/mountain-loop/yaak.git")
b2sums=('SKIP')

pkgver() {
	cd "${srcdir}/yaak/"
	# The tags don't belong to a branch so we have to get the latest tag across all branches
	git describe --long --tags --abbrev=7 "$(git rev-list --tags --max-count=1)" |
		sed -e 's/^v//' -e 's/-\([[:digit:]]\+\)-\(g[[:alnum:]]\{7\}\)$/.r\1.\2/' -e 's/-/./g'
}

_semver() {
	cd "${srcdir}/yaak/"
	# Same as above but with a different sed replacement to make the output valid semver
	git describe --long --tags --abbrev=7 "$(git rev-list --tags --max-count=1)" |
		sed -e 's/^v//' -e 's/-\([[:digit:]]\+\)-\(g[[:alnum:]]\{7\}\)$/-r.\1+\2/'
}

build() {
	local _semver && _semver="$(_semver)"
	export YAAK_VERSION="${_semver}"
	export TAURI_APP_PATH="${srcdir}/yaak/src-tauri/"
	export CI=true

	cd "${srcdir}/yaak/"
	npm ci
	npm run build
	npm run replace-version
	npm run tauri build -- --bundles deb --config '{ "bundle": { "createUpdaterArtifacts": false } }'

	sed -e 's|Name=yaak|Name=Yaak|' \
		-e '$aGenericName=API Client' \
		-i "${srcdir}/yaak/src-tauri/target/release/bundle/deb/yaak_${_semver}_"*/data/usr/share/applications/yaak.desktop
}

package() {
	local _semver && _semver="$(_semver)"
	cp -a \
		"${srcdir}/yaak/src-tauri/target/release/bundle/deb/yaak_${_semver}_"*/data/usr/ \
		"${pkgdir}/usr/"
	install -Dm644 \
		"${srcdir}/yaak/LICENSE" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
