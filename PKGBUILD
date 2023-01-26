# Maintainer: Brenton Horne <brentonhorne77 at gmail dot com>

pkgname=openra-ra2-git
_pkgname=${pkgname/-git}
pkgver=1110.git.6bfd2da
pkgrel=1
pkgdesc="An OpenRA mod inspired by Command & Conquer: Red Alert 2 "
arch=(x86_64)
url="https://github.com/OpenRA/ra2"
license=('GPL3')
install=openra-ra2.install
depends=('mono' 'ttf-dejavu' 'openal' 'libgl' 'freetype2' 'sdl2' 'lua51' 'hicolor-icon-theme' 'gtk-update-icon-cache'
         'desktop-file-utils' 'xdg-utils' 'zenity')
makedepends=('dos2unix' 'git' 'unzip' 'msbuild')
provides=('openra-ra2')
options=(!strip)
source=("git+${url}.git"
"openra-ra2"
"openra-ra2.appdata.xml"
"openra-ra2.desktop")
md5sums=('SKIP'
         '02a35f538ddf34eb00489b15560bec53'
         '5f9d4e39293302ff69f7a701c870e635'
         '882b9d629dde1ecbcd2098a2e0b96b1b')

pkgver() {
    cd $srcdir/ra2
    no=$(git rev-list --count HEAD)
    hash=$(git log | head -n 1 | cut -d ' ' -f 2 | head -c 7)
    printf "${no}.git.${hash}"
}

prepare() {
    cd $srcdir/ra2
    dos2unix *.md
    chmod +x *.sh
}

build() {
    cd $srcdir/ra2
    make
    make version VERSION="${pkgver}"
}

package() {
    cd $srcdir/ra2
    mkdir -p $pkgdir/usr/{lib/${_pkgname}/mods,bin,share/pixmaps,share/doc/packages/openra-ra2,share/applications,share/appdata}
    cp -r engine/{glsl,lua,AUTHORS,COPYING,bin/*.{dll,so,exe}*,'global mix database.dat',VERSION} $pkgdir/usr/lib/openra-ra2
    cp -r mods/ra2 $pkgdir/usr/lib/${_pkgname}/mods
    cp -r engine/mods/{common,modcontent} $pkgdir/usr/lib/${_pkgname}/mods
    install -Dm755 $srcdir/openra-ra2 $pkgdir/usr/bin/openra-ra2
    cp -r $srcdir/../openra-ra2.appdata.xml $pkgdir/usr/share/appdata/openra-ra2.appdata.xml
    cp -r README.md $pkgdir/usr/share/doc/packages/${_pkgname}/README.md
    ln -sf /usr/share/icons/hicolor/512x512/apps/${_pkgname}.png ${pkgdir}/usr/share/pixmaps/${_pkgname}.png
    install -Dm644 $srcdir/openra-ra2.desktop $pkgdir/usr/share/applications/openra-ra2.desktop
    for size in 16 32 48 64 128 256 512; do
      size="${size}x${size}"
      mkdir -p "$pkgdir/usr/share/icons/hicolor/${size}/apps"
      cp packaging/artwork/icon_${size}.png "$pkgdir/usr/share/icons/hicolor/${size}/apps/${_pkgname}.png"
    done
}
