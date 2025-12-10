# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=supercronic-bin
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.40
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
b2sums_aarch64=('436a3ecd5a231c9283b7c2df7f1499cf9f1a0fbacb23ed68bea72d02b3e9cdb18b82923bffe115ff8c0856976363b3c6f313c1eb6d7fa578adecbf20db9f50c6')
b2sums_armv7h=('e46e6cdeb6f30c06a6f302bcbf1f452735ad39df2db7675db9080933b7b1ff88dabc4b8a93c7b527c67adeeac77506319e08600d461ff672147ff729b2eea058')
b2sums_i686=('b2dbec51d098dd74b544aed48e2a2c8c01132972c026a3b6ba31fa161b9a36ca169cf3399ae6f0faa325c9c1d6d354cdad3a4d26b009db60e0bdf77d001bc402')
b2sums_x86_64=('8e749c3757098e911f4540cc5de80c72fb3199fe218ad9775291a30686fe24cbc16d4879136ffe55de586c2296c5337308c3fe756a22b63724a21eeb31279c33')

package() {
	install -Dm755 \
		"${srcdir}/${pkgname}-${pkgver}"-* \
		"${pkgdir}/usr/bin/supercronic"
	install -Dm644 \
		"${srcdir}/LICENSE-${pkgname}-${pkgver}.md" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.md"
}
