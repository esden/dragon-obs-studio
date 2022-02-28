# Maintainer: Jonathan Steel <jsteel at archlinux.org>
# Contributor: Benjamin Klettbach <b.klettbach@gmail.com>
# Contributor: Rachel Mant <aur@dragonmux.network>

pkgname=obs-studio
pkgver=27.2.1
pkgrel=1
pkgdesc="Free, open source software for live streaming and recording"
arch=('x86_64')
url="https://obsproject.com"
license=('GPL2')
depends=('ffmpeg' 'jansson' 'libxinerama' 'libxkbcommon-x11' 'mbedtls'
		'qt5-svg' 'curl' 'jack' 'gtk-update-icon-cache' 'pipewire' 'libxcomposite')
makedepends=('git' 'cmake' 'libfdk-aac' 'x264' 'vlc' 'swig' 'python' 'luajit' 'sndio')
optdepends=('libfdk-aac: FDK AAC codec support'
		'libxcomposite: XComposite capture support'
		'libva-intel-driver: hardware encoding'
		'libva-mesa-driver: hardware encoding'
		'luajit: scripting support'
		'python: scripting support'
		'sndio: Sndio input client'
		'v4l2loopback-dkms: virtual camera support')
source=(
	"$pkgname-$pkgver::git+https://github.com/obsproject/obs-studio.git#tag=$pkgver"
	"git+https://github.com/obsproject/obs-browser.git"
	"cef-4638_linux64.tar.bz2::https://cdn-fastly.obsproject.com/downloads/cef_binary_4638_linux64.tar.bz2"
	"fix_python_binary_loading.patch"
)
md5sums=(
	'SKIP'
	'SKIP'
	'34fb1c611b3e278ca4d0d1d50e7bfb9b'
	'051b90f05e26bff99236b8fb1ad377d1'
)

prepare() {
	cd "$srcdir/$pkgname-$pkgver"
	patch -Np1 < "$srcdir"/fix_python_binary_loading.patch	
	git config submodule.plugins/obs-browser.url "$srcdir"/obs-browser
	git submodule update
}

build() {
	cd $pkgname-$pkgver

	mkdir -p build; cd build

	cmake -DUNIX_STRUCTURE=1 \
		-DCMAKE_INSTALL_PREFIX="/usr" \
		-DCMAKE_INSTALL_LIBDIR="lib" \
		-DBUILD_BROWSER=ON \
		-DBUILD_VST=OFF \
		-DCEF_ROOT_DIR="$srcdir/cef_binary_4638_linux64" \
		-DOBS_VERSION_OVERRIDE="$pkgver-$pkgrel" ..

	make
}

package() {
	cd $pkgname-$pkgver/build

	make install DESTDIR="$pkgdir"
}
