# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=supercronic-bin
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.39
pkgrel=1
pkgdesc='A crontab-compatible job runner, designed specifically to run in containers (Pre-compiled version)'
arch=(aarch64 armv7h i686 x86_64)
url='https://github.com/aptible/supercronic'
license=(MIT)
provides=(supercronic)
conflicts=(
	supercronic
	supercronic-git
)
options=(
	!strip     # Stripping symbols would break the binary
	!emptydirs # Remove empty directories from package because why not
)
source=("LICENSE-${pkgname}-${pkgver}.md::https://raw.githubusercontent.com/aptible/supercronic/refs/tags/v${pkgver}/LICENSE.md")
source_aarch64=("${pkgname}-${pkgver}-arm64::https://github.com/aptible/supercronic/releases/download/v${pkgver}/supercronic-linux-arm64")
source_armv7h=("${pkgname}-${pkgver}-arm::https://github.com/aptible/supercronic/releases/download/v${pkgver}/supercronic-linux-arm")
source_i686=("${pkgname}-${pkgver}-386::https://github.com/aptible/supercronic/releases/download/v${pkgver}/supercronic-linux-386")
source_x86_64=("${pkgname}-${pkgver}-amd64::https://github.com/aptible/supercronic/releases/download/v${pkgver}/supercronic-linux-amd64")
b2sums=('a6b6216404b1faba0feec03f5948ee5f3663288afdce33c63c159dd964f24e29197943d38c7288d446ee328e2553952dada226a41326387f2d21cca4a3b928c5')
b2sums_aarch64=('3935eb2745424e1bdc0d206bd74ba9f6cfb67d3cdacb73984ecd8a5db1c2ec6236171ba0f1498a588c0950b5817befad644c3d893ea06179ef77a504829bd724')
b2sums_armv7h=('3a47d27e66889f55ef165c11279224b60c54103f55944ca3661afb7248784610ff3515ea1c3c010b6bcb35e323ca883179016629cceb055a14acfffdc8ff629c')
b2sums_i686=('83032185d1c2f00b9072a04411f35e30814ac7daff19b37181a4b5d767c97f3a438a3c7b149b2b9dcefff49dbdca3271d4ec2821432ebbc843a023a15ab35dc8')
b2sums_x86_64=('71e4a63fba769677ba64850a91a9b09d7d1cb9a43e582a8d5df1257a1e5f16f116b511f594b38db5041e7927e7c246855b76f5bf1ff5c712201eeab890138377')

package() {
	install -Dm755 \
		"${srcdir}/${pkgname}-${pkgver}"-* \
		"${pkgdir}/usr/bin/supercronic"
	install -Dm644 \
		"${srcdir}/LICENSE-${pkgname}-${pkgver}.md" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.md"
}
