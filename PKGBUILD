# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=yaak-git
pkgver=0.0.0.r1135.gdb416ce
pkgrel=1
pkgdesc='Simple and intuitive API client for calling REST, GraphQL, and gRPC APIs (Development version)'
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
    appmenu-gtk-module # Tauri
    cargo # Rust dependencies & Tauri build
    git # Sources
    libappindicator-gtk3 # Tauri
    librsvg # Tauri
    nodejs # Custom scripts & web build
    npm # Node dependencies & web build
    protobuf # Yaak
)
provides=(yaak)
conflicts=(yaak)
options=(
    !lto # Some Rust dependencies don't support link time optimization
    !strip # Stripping symbols would break the output binary
    !emptydirs # Remove empty directories from package because why not
)
source=(
    "yaak::git+https://github.com/yaakapp/app.git"
    "yaak-plugins::git+https://github.com/yaakapp/plugins.git"
)
b2sums=('SKIP'
        'SKIP')

prepare() {
    cd "${srcdir}/yaak/"
    npm ci

    cd "${srcdir}/yaak/plugin-runtime/"
    npm ci
}

pkgver() {
    cd "${srcdir}/yaak/"
    printf "0.0.0.r%s.g%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"

    # TODO: For some reason `git describe` doesn't work?
    #git describe --long --tags --abbrev=7 2> /dev/null | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
    cd "${srcdir}/yaak/"
    local _pkgver="${pkgver/.r/-}"
    export YAAK_VERSION="${_pkgver/.g/+g}" # Needs to be a valid semver, so we do some funky stuff
    export YAAK_PLUGINS_DIR="${srcdir}/yaak-plugins/"
    export CI=true
    npm run replace-version
    npm run tauri build -- --verbose --bundles deb

    sed -e 's|Name=yaak|Name=Yaak|' \
        -e '$aGenericName=API Client' \
        -i "${srcdir}/yaak/src-tauri/target/release/bundle/deb/yaak_"*/data/usr/share/applications/yaak-app.desktop
}

package() {
    cp -a \
        "${srcdir}/yaak/src-tauri/target/release/bundle/deb/yaak_"*/data/usr/ \
        "${pkgdir}/usr/"
    install -Dm644 \
        "${srcdir}/yaak/LICENSE" \
        "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
