# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: Rafael Baboni Dominiquini <rafaeldominiquini at gmail dot com>
pkgname=supercronic
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.47
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
b2sums=('a4b322eb30bf30a3312cbfd4fa7bed51290b8600ec9b68dea51668b37744373e1934b7c973a73befd1ac4fcf780302a29a78c5481a8ee81f0cf384118988d4f6')

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
