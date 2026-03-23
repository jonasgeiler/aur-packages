# Maintainer: Jonas Geiler <aur@jonasgeiler.com>
# Maintainer: Rafael Baboni Dominiquini <rafaeldominiquini at gmail dot com>

pkgname=supercronic-bin
# renovate: datasource=github-releases depName=aptible/supercronic
pkgver=0.2.44
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
b2sums_aarch64=('85f84d27fae2e279ed6cd70c69aa41cb85373440592e5332c1aa914008a2beced0d81f387f9b4f6f96ac604f7b0152a70aaf5a4bcaf5b3e8cd802da12121f295')
b2sums_armv7h=('fd751d4cdf2c5f22dae60380658dab544c01c81f31d768563e78d0fe9b754a6e88c628d09e00550cb2cf282da667e14837beb85a5c9fc44437dc0488f5a3ebdb')
b2sums_i686=('263da8cb8ee77796aa447cb95f019cfce4cf3a5ae0626f7674c362d826894d266a4b608d1ac2fb251d6667391a87a3464ff7c5bdaf85b01d28d5e45eeedd8897')
b2sums_x86_64=('f81eeb323ce6103a8418cb1dabb7006e0a0d74617dcc8514064c1152f4187974436071f39b88c57cd14605e78bd693f1fe73acce1aba3d11b555f75dce04b76b')

package() {
	install -Dm755 \
		"${srcdir}/${pkgname}-${pkgver}"-* \
		"${pkgdir}/usr/bin/supercronic"
	install -Dm644 \
		"${srcdir}/LICENSE-${pkgname}-${pkgver}.md" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.md"
}
