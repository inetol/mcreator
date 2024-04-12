# Maintainer: Ivan Gabaldon <aur[at]inetol.net>

pkgname=mcreator
_pkgvermajor=2024.1
_pkgverbuild=15821
pkgver=$_pkgvermajor.$_pkgverbuild
pkgrel=1
pkgdesc='Make Minecraft Java Edition mods, Bedrock Edition Add-Ons, and data packs using visual graphical programming or integrated IDE'
arch=('x86_64')
url='https://mcreator.net'
license=('GPL-3.0-or-later WITH GPL-3.0-interface-exception')
noextract=("$pkgname-$pkgver.tar.gz")
source=("$pkgname-$pkgver.tar.gz::https://github.com/$pkgname/$pkgname/releases/download/$pkgver/MCreator.$_pkgvermajor.Linux.64bit.tar.gz"
        "$pkgname.desktop")
b2sums=('50961ea41842dc88fc09aef8faa71bd51b116e93ed36eed6279560fae5c1f04b3741f08f59a2655523718940fc1611cc1b8a3fc9c146f08fbed5385b2bc8b3cd'
        'ac5a53421869276596ebbc7cccfa7d20aeb63ea3a556d0b77a0b9b936176bcbabe7771be60fc5b034ec79a49da5f4aa538b79e1c6edca478a5614f49ec478d6f')

prepare() {
    PKGBUILD_JAVA='/usr/lib/jvm/$(archlinux-java status | awk '\''/java-17/{print $NF}'\'')/bin/java'

    mkdir -p "$pkgname-$pkgver/"
    bsdtar -xpf "$pkgname-$pkgver.tar.gz" --strip-components=1 -C "$pkgname-$pkgver/"

    # Convert
    cd "$pkgname-$pkgver/"

    cat >"mcreator.sh"<<EOF
#!/usr/bin/env bash
export CLASSPATH="./$pkgname/lib/mcreator.jar:./lib/*"

cd /opt/$pkgname/
$PKGBUILD_JAVA --add-opens=java.base/java.lang=ALL-UNNAMED net.mcreator.Launcher "\$1"
EOF

    cp "$srcdir/$pkgname.desktop" "$pkgname.desktop"

    rm -rf jdk/
}

package() {
    depends=('bash'
             'glibc'
             'hicolor-icon-theme'
             'java-runtime=17')

    install -d "$pkgdir/opt/$pkgname/"
    cp -a "$pkgname-$pkgver/." "$pkgdir/opt/$pkgname/"

    chmod 755 "$pkgdir/opt/$pkgname/$pkgname.sh"

    install -d "$pkgdir/usr/bin/"
    ln -s "/opt/$pkgname/$pkgname.sh" "$pkgdir/usr/bin/$pkgname"

    install -d "$pkgdir/usr/share/applications/"
    ln -s "/opt/$pkgname/$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"

    install -d "$pkgdir/usr/share/icons/hicolor/64x64/apps/"
    ln -s "/opt/$pkgname/icon.png" "$pkgdir/usr/share/icons/hicolor/64x64/apps/$pkgname.png"

    install -Dm644 "$pkgname-$pkgver/LICENSE.txt" -t "$pkgdir/usr/share/licenses/$pkgname/"
}
