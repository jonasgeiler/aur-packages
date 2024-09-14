# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=yaak
pkgver=2024.8.2
_plugins_commit=75df5f8094c758567395ac6cf752cd8feb33d2a6
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
    appmenu-gtk-module # Tauri
    cargo # Rust dependencies & Tauri build
    git # Sources
    libappindicator-gtk3 # Tauri
    librsvg # Tauri
    nodejs # Custom scripts & web build
    npm # Node dependencies & web build
    protobuf # Yaak
)
options=(
    !lto # Some Rust dependencies don't support link time optimization
    !strip # Stripping symbols would break the output binary
    !emptydirs # Remove empty directories from package because why not
)
source=(
    "yaak::git+https://github.com/yaakapp/app.git#tag=v${pkgver}"
    "yaak-plugins::git+https://github.com/yaakapp/plugins.git#commit=${_plugins_commit}"
    # TODO: Use license from yaak repo after next release
    "${pkgname}-${pkgver}.LICENSE::https://raw.githubusercontent.com/yaakapp/app/master/LICENSE"
)
b2sums=('a15241fc7230e7c0cd816d7f9db3d37b7bc5db73d412d3da884ca7e7ca68918900f71ee289cab85bdd2bd40d169fd146ef4e81555bacc1f81204be243c167475'
        'a6992a0487f33f169c88293c535e8aa5b170909fff40b0260c04d9ae57823bdeb4c38c0ac3f1cb0e317474331d92f54577ccbcb6758253ab989ca3442be7cad6'
        '011fb406bfe4a8944efbae1f9cfa420fe421f1de3ae628802548676a1fe1318850a5f98c60cd29899efe3946dec329b6607f04917e966808f62f9e4ecaaea13b')

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
        -i "${srcdir}/yaak/src-tauri/target/release/bundle/deb/yaak_${pkgver}_"*/data/usr/share/applications/yaak-app.desktop
}

package() {
    cp -a \
        "${srcdir}/yaak/src-tauri/target/release/bundle/deb/yaak_${pkgver}_"*/data/usr/ \
        "${pkgdir}/usr/"
    install -Dm644 \
        "${srcdir}/${pkgname}-${pkgver}.LICENSE" \
        "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
