# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=supercronic
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.36
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
b2sums=('bba71400efefd032e6422e9a1b20776bdd99171ea225e4cc28826e7be93bac6d989278679576da9227a8d888b95c3175ccab83c61e18d893d6595801ad6fda01')

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
