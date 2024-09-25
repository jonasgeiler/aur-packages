# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=yaak
# renovate: datasource=github-releases depName=yaakapp/app
pkgver=2024.10.0
# should be the same commit hash used in the GitHub Actions run for the current release tag
# (check the step where it checks out the plugins repository)
_plugins_commit=92ac91733ea2aa647504db70dee42d7495d870cc
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
	yaak-app
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
b2sums=('7701eb90ac6490e3ba911fffcfb12d38baa050420997d809ec8ec363bdae3732a0c5fd98938f51a201f9b0d3163ff8f659ce2a420eef198c2c40b49dfbfd58c2'
        '8bbce3928bb0286ecaa9862ede34c00b8cc63c6d98b339a8c1b7c55392ba74abb793b9ef9e343d48247cbd2af540e0d290e73cacafbffe5648cc8f377758e76e')

build() {
	export YAAK_VERSION="${pkgver}"
	export YAAK_PLUGINS_DIR="${srcdir}/yaak-plugins/"
	export CI=true

	cd "${srcdir}/yaak/plugin-runtime/"
	npm ci

	cd "${srcdir}/yaak/"
	npm ci
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
