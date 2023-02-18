# Maintainer: Jonathan Steel <jsteel at archlinux.org>
# Contributor: Benjamin Klettbach <b.klettbach@gmail.com>
# Contributor: Rachel Mant <aur@dragonmux.network>
# Contributor: Piotr Esden-Tempski <piotr@esden.net>

pkgname=obs-studio
pkgver=29.0.2
pkgrel=1
pkgdesc="Free, open source software for live streaming and recording"
arch=('x86_64')
url="https://obsproject.com"
license=('GPL2')
depends=('ffmpeg' 'jansson' 'libxinerama' 'libxkbcommon-x11' 'mbedtls' 'rnnoise' 'pciutils'
		 'qt6-svg' 'curl' 'jack' 'gtk-update-icon-cache' 'pipewire' 'libxcomposite')
makedepends=('git' 'cmake' 'libfdk-aac' 'x264' 'vlc' 'swig' 'python' 'luajit' 'sndio' 'libajantv2')
optdepends=('libfdk-aac: FDK AAC codec support'
			'libxcomposite: XComposite capture support'
			'libva-intel-driver: hardware encoding'
			'libva-mesa-driver: hardware encoding'
			'luajit: scripting support'
			'python: scripting support'
			'sndio: Sndio input client'
			'v4l2loopback-dkms: virtual camera support')
source=(
	"$pkgname-$pkgver::git+https://github.com/obsproject/obs-studio#tag=$pkgver"
	"git+https://github.com/obsproject/obs-browser#commit=845adc7"
	"cef-5060_linux64.tar.bz2::https://cdn-fastly.obsproject.com/downloads/cef_binary_5060_linux64.tar.bz2"
	"fix_python_binary_loading.patch"
)
sha256sums=(
	'SKIP'
	'SKIP'
	'ac4e2a8ebf20700e4e36353e314f876623633dd5b474778a2548bb66bdbea11d'
	'bdfbd062f080bc925588aec1989bb1df34bf779cc2fc08ac27236679cf612abd'
)

prepare() {
	cd "$srcdir/$pkgname-$pkgver"
	patch -Np1 < "$srcdir/fix_python_binary_loading.patch"
	git config submodule.plugins/obs-browser.url "$srcdir/obs-browser"
	git submodule init plugins/enc-amf plugins/obs-outputs/ftl-sdk plugins/obs-websocket
	git -c protocol.file.allow=always submodule update
	git submodule foreach --recursive 'git submodule update --init'
}

build() {
	cd $pkgname-$pkgver

	mkdir -p build; cd build

	cmake -DUNIX_STRUCTURE=1 \
		-DCMAKE_INSTALL_PREFIX="/usr" \
		-DCMAKE_INSTALL_LIBDIR="lib" \
		-DBUILD_BROWSER=ON \
		-DBUILD_VST=OFF \
		-DENABLE_NEW_MPEGTS_OUTPUT=OFF \
		-DCEF_ROOT_DIR="$srcdir/cef_binary_5060_linux64" \
		-DOBS_VERSION_OVERRIDE="$pkgver-$pkgrel" ..

	make
}

package() {
	cd $pkgname-$pkgver/build

	make install DESTDIR="$pkgdir"
}
