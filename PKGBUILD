# Contributor: ChugunovRoman <Zebs-BMK@yandex.ru>
# Contributor: PedroHLC <root@pedrohlc.com>

pkgname="figma-linux"
pkgver="0.7.1"
pkgrel="4"
pkgdesc="The collaborative interface design tool. Unofficial Figma desktop client for Linux + system-wide electron"
arch=("x86_64")
url="https://github.com/Figma-Linux/figma-linux"
license=('GPL2')
conflicts=("figma-bin")
replaces=("figma-bin")
source=("https://github.com/Figma-Linux/figma-linux/archive/v${pkgver}.tar.gz"
        "figma-linux.desktop")
depends=('electron' 'gtk3' 'xdg-utils' 'libxss' 'nss' 'nspr')
makedepends=('nodejs' 'rust')
optdepends=('libappindicator-gtk3')
sha256sums=("f68af043cb7873db7f79795c350810a2474e2f701bc0bd209b17a15a86d088de"
            "8ec0db0813fb20c1f22450505fb17a89d73013e2e81960ce83c1f8fe645a8259")
options=(!strip)

prepare() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  npm install
}

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  npm rebuild node-sass
  npx --no-install electron-webpack app

  cp package-lock.json dist/
  cp -r node_modules dist/
  cd dist
  npm install
  npm prune
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  install -D "${srcdir}"/figma-linux.desktop "${pkgdir}"/usr/share/applications/figma-linux.desktop
  install -D resources/icons/256x256.png "${pkgdir}"/usr/share/pixmaps/figma-linux.png

  for size in 24 36 48 64 72 96 128 192 256 384 512; do
    install -D "resources/icons/${size}x${size}.png" \
               "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/apps/figma-linux.png"
  done
  
  install -D resources/icons/scalable.svg \
    "${pkgdir}/usr/share/icons/hicolor/scalable/apps/figma-linux.png"

  install -d "${pkgdir}/opt/${pkgname}"

  cd dist
  cp -dr --no-preserve=ownership -t "${pkgdir}/opt/${pkgname}" * 
}
