# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname='yaak'
pkgver='2024.8.2'
pkgrel='1'

pkgdesc='The most intuitive desktop API client to organize and execute REST, GraphQL, and gRPC requests'
arch=('x86_64' 'arm' 'armv6h' 'armv7h' 'aarch64' 'riscv64' 'i686' 'pentium4')
url='https://yaak.app/'
license=('MIT')

depends=(

)
makedepends=(
    'nodejs'
    'npm'
    'cargo'
    'gtk3'
    'webkit2gtk-4.1'
    'protobuf'
    #'libappindicator-gtk3' # Yaak does not use the system tray at the moment
    #'appmenu-gtk-module' # Yaak does not use an app menu on Linux at the moment
    #'librsvg' # TODO: Check if this is needed

    #'git' 'file' 'openssl' 'appmenu-gtk-module' 'libappindicator-gtk3'
    #'base-devel' 'curl' 'wget' 'dpkg' 'librsvg' 'patchelf'
    #'cairo' 'desktop-file-utils' 'gdk-pixbuf2' 'glib2' 'hicolor-icon-theme' 'libsoup' 'pango'
)

options=(
    '!lto' # Some Rust dependencies don't support Link Time Optimization
    #'!strip'
    #'!emptydirs'
)

_app_repo='app'
_app_archive="${pkgname}-${pkgver}.tar.gz"
_plugins_repo='plugins'
_plugins_commit='75df5f8094c758567395ac6cf752cd8feb33d2a6'
_plugins_archive="${pkgname}-${_plugins_repo}-${_plugins_commit}.tar.gz"
_license="${pkgname}-${pkgver}.LICENSE"
# TODO: Use license from app archive after next release
source=(
    "${_app_archive}::https://github.com/yaakapp/${_app_repo}/archive/v${pkgver}.tar.gz"
    "${_plugins_archive}::https://github.com/yaakapp/${_plugins_repo}/archive/${_plugins_commit}.tar.gz"
    "${_license}::https://raw.githubusercontent.com/yaakapp/${_repo}/master/LICENSE"
)
b2sums=('b9a12d8671e3af73921cd08fbf0eb503935a5ddd5884c77238fb54ca5a0c4acb046e2ae6fbdce9e7bb0758882255b650d7281f64c4d338a7704be5adbd493529'
        'f7595c65d6e3aad3492b3781ef89003303b5b66159497c5c579942c76b9ec8c4e07f7a367b1f651587614ee742d3ac0626788dce24679f1226a5bd74536d68b6'
        '011fb406bfe4a8944efbae1f9cfa420fe421f1de3ae628802548676a1fe1318850a5f98c60cd29899efe3946dec329b6607f04917e966808f62f9e4ecaaea13b')

# Dirs after extraction
_app_dir="${_app_repo}-${pkgver}" # (No idea why there's no "v" prefix here...)
_plugins_dir="${_plugins_repo}-${_plugins_commit}"

prepare() {
    cd "${srcdir}/${_app_dir}"

    # Install Node dependencies
    npm ci

    # Install plugin-runtime Node dependencies
    cd "${srcdir}/${_app_dir}/plugin-runtime"
    npm ci
    cd "${srcdir}/${_app_dir}"

    # Set version
    YAAK_VERSION="${pkgver}" \
        npm run replace-version
}

build() {
    cd "${srcdir}/${_app_dir}"

    # Run Tauri build (we only need the deb bundle)
    YAAK_PLUGINS_DIR="${srcdir}/${_plugins_dir}" \
    CI=true \
        npm run tauri build -- --verbose --bundles deb
}

package() {
    echo TODO
}
