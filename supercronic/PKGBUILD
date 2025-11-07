# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=supercronic
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.39
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
b2sums=('85767276b7da9b2142fe5df6109daf53298ab6dd428ff057c85458eda43a2170e2d980bc8f2486949d2902f3b0ede689e1fd919275872d635c86372bd91b1eb1')

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
