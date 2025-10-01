# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
pkgname=supercronic-bin
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.36
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
b2sums_aarch64=('164091a69ca17b718fd0bd832c6de1a0b73f4a445f7e9c2b7beb1355633add295c8e948714269e9e3215ecc05fff8b8a79fdbbc7fa49cdefa9101768f034538f')
b2sums_armv7h=('d44e04597b28a3a41f46856c2c51030e1e93039c764b5cc5a361196977bce4a24f0b5949359d5571a869bada831bc4f7b3a2299fccd7635b29af86d08f857a14')
b2sums_i686=('6038b55ba04d165c0d32a3333402198aac5d0043ceba1bc0b08f8b19bd8beb3df9cc1cd2437be53d4f27ba0664d914d699bb9510b59a18f645153b9ab42b786c')
b2sums_x86_64=('3e56c621caa216916fc7294b47ccb45b0c5d13535063eb69415e31d64d09127b549ff05331ea5dabeb8432c67294144ea2cdce0f4117770f096e150761a1474f')

package() {
	install -Dm755 \
		"${srcdir}/${pkgname}-${pkgver}"-* \
		"${pkgdir}/usr/bin/supercronic"
	install -Dm644 \
		"${srcdir}/LICENSE-${pkgname}-${pkgver}.md" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.md"
}
