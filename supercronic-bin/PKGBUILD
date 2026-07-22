# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: Rafael Baboni Dominiquini <rafaeldominiquini at gmail dot com>
pkgname=supercronic-bin
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.47
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
source=("${pkgname}-${pkgver}.LICENSE.md::https://raw.githubusercontent.com/aptible/supercronic/refs/tags/v${pkgver}/LICENSE.md")
source_aarch64=("${pkgname}-${pkgver}-aarch64::https://github.com/aptible/supercronic/releases/download/v${pkgver}/supercronic-linux-arm64")
source_armv7h=("${pkgname}-${pkgver}-armv7h::https://github.com/aptible/supercronic/releases/download/v${pkgver}/supercronic-linux-arm")
source_i686=("${pkgname}-${pkgver}-i686::https://github.com/aptible/supercronic/releases/download/v${pkgver}/supercronic-linux-386")
source_x86_64=("${pkgname}-${pkgver}-x86_64::https://github.com/aptible/supercronic/releases/download/v${pkgver}/supercronic-linux-amd64")
b2sums=('a6b6216404b1faba0feec03f5948ee5f3663288afdce33c63c159dd964f24e29197943d38c7288d446ee328e2553952dada226a41326387f2d21cca4a3b928c5')
b2sums_aarch64=('bb2f70621eba50166457a4bca93f3f60414c13275d77fa014bdd42b2fb3e83b95163495fb80b8f30b2bca43b6384caf50d170cedd0134593063d8a8604606dd6')
b2sums_armv7h=('cfb1599ca6aa4bdcda973bad8ee04543cb262ee2fad36e2016d4e74d1099244d96c7ab416ee3f78d8ebded128444e844e6bd6f6d2c5554b834132a0c7284b1b1')
b2sums_i686=('4311dacc600aeba8220ae6d3bfde1b0488a6c48b53c88bb843a5273e94e47f3d9c86d2897637a80ab5bcb8a34060cdd180204a014c583b3b8e0445bbefd74ecb')
b2sums_x86_64=('720f7f4b1c2e92ae8b6443ebed9de969e4ae277e370ff4144b095015203be7104f9a9d7f4489667807945077971e18e3b2c35036f53572dc54c563b846ac09d6')

package() {
	install -Dm755 \
		"${srcdir}/${pkgname}-${pkgver}-${CARCH}" \
		"${pkgdir}/usr/bin/supercronic"
	install -Dm644 \
		"${srcdir}/${pkgname}-${pkgver}.LICENSE.md" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.md"
}
