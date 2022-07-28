# Maintainer: Caleb Macalennan <caleb@alerque.com>
# Maintainer: Justin Kromlinger <hashworks@archlinux.org>
# Contributor: Bruno Pagani <archange@archlinux.org>

pkgbase=matterbridge
pkgname=matterbridge-custom
pkgver=1.25.2
pkgrel=1
pkgdesc='Multi-protocols (IRC/XMPP/Mattermost/Slack/Matrix/etc) bridge'
arch=(x86_64)
url="https://github.com/42wim/$pkgbase"
license=(Apache GPL3)
depends=(glibc
         gcc-libs)
makedepends=(go
             git)
provides=(matterbridge)
conflicts=(matterbridge)
backup=("etc/$pkgbase.toml")
_archive="$pkgbase-$pkgver"
source=("$url/archive/v$pkgver/$_archive.tar.gz"
        "$_archive.tar.gz.asc::$url/releases/download/v$pkgver/v$pkgver.tar.gz.asc"
        "$pkgbase.service"
	"always-reply-to-channel.patch"
	"drop-thread-replies.patch"
	"fix-discord-replies.patch")

sha256sums=('e078a4776b1082230ea0b8146613679c23d3b0d322706c987261df987a04bfc5'
            'SKIP'
            '338171f409a0e55589b86959e37871d61d21dc89cec6b212b552eaf4e516e069'
            'SKIP'
            'SKIP'
            'SKIP')

validpgpkeys=(CC7D978417C1AEA1E4CDD7240E41AB4BF4C610B4) # wim <wim@42.be>

prepare() {
	cd "$_archive"
	go mod vendor
	patch -p1 < $srcdir/always-reply-to-channel.patch
	patch -p1 < $srcdir/drop-thread-replies.patch
	patch -p1 < $srcdir/fix-discord-replies.patch
}

build() {
	cd "$_archive"
	export CGO_CPPFLAGS="$CPPFLAGS"
	export CGO_CFLAGS="$CFLAGS"
	export CGO_CXXFLAGS="$CXXFLAGS"
	export CGO_LDFLAGS="$LDFLAGS"
	export GOFLAGS="-buildmode=pie -trimpath -mod=readonly -modcacherw -ldflags=-linkmode=external"
	go build -tags whatsappmulti -v -o "$pkgbase" .
}

package() {
	cd "$_archive"
	install -Dm0755 -t "$pkgdir/usr/bin/" "$pkgbase"
	install -Dm0644 -t "$pkgdir/usr/lib/systemd/system/" "../$pkgbase.service"
	install -Dm0600 matterbridge.toml.sample "$pkgdir/etc/$pkgbase.toml"
}
