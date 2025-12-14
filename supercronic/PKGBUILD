# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: Rafael Baboni Dominiquini <rafaeldominiquini at gmail dot com>
pkgname=supercronic
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.40
pkgrel=1
pkgdesc='A crontab-compatible job runner, designed specifically to run in containers'
arch=(aarch64 arm armv6h armv7h i686 pentium4 riscv64 x86_64)
url='https://github.com/aptible/supercronic'
license=(MIT)
makedepends=(
	git # Sources
	go  # Build
)
conflicts=(
	supercronic-bin
	supercronic-git
)
options=(
	!strip     # Stripping symbols would break the output binary
	!emptydirs # Remove empty directories from package because why not
)
source=("supercronic::git+https://github.com/aptible/supercronic.git#tag=v${pkgver}")
b2sums=('7356567f6208559699b3d92d26e85141bc4dc4be1a288b5f2a3797dcb36eac477f388b66793e12b1c507044df25f12e4275a615e495c522d1200cc789cba25c4')

build() {
	export GOPATH="${srcdir}/gopath/"
	export CGO_ENABLED=0

	cd "${srcdir}/supercronic/"
	go build \
		-trimpath \
		-mod=readonly \
		-modcacherw \
		-ldflags "-X main.Version=v${pkgver}"
}

check() {
	"${srcdir}/supercronic/supercronic" -version
}

package() {
	install -Dm755 \
		"${srcdir}/supercronic/supercronic" \
		"${pkgdir}/usr/bin/supercronic"
	install -Dm644 \
		"${srcdir}/supercronic/LICENSE.md" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.md"
}
