# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=supercronic-bin
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.34
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
b2sums_aarch64=('362685232e8842043bdfa12aa2936cf6255fb04dc9eddc8b1563c959867169cf1d43d0cfdc4c8d0b9e46fccf2b35909274ada26fa4c061e7aa3fb88c955ec800')
b2sums_armv7h=('96e81520908c881ccca2083705a6844e0cbcfad4f2a6705d676a7f57617b61a62359c369a8134cc19e8fccfd2ff2e445061b6c2b4f106124cfb286bafc045d37')
b2sums_i686=('84ff442928a2da37c13fc8e9987a0fe3a13dd694adc1764b5a296d6efa534ef2a06648f53ee9b901f29a80cf6ffb754ab1325f389091a76392b892071958cdbf')
b2sums_x86_64=('34ddd4aaefff35bd49dfb09833c2f7ba00ea9484f4aa5968bb59b6b3a1e5a6906371448510d720990a5134fe32954c2b20335e187a2539be6f4a5749f8772faa')

package() {
	install -Dm755 \
		"${srcdir}/${pkgname}-${pkgver}"-* \
		"${pkgdir}/usr/bin/supercronic"
	install -Dm644 \
		"${srcdir}/LICENSE-${pkgname}-${pkgver}.md" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.md"
}
