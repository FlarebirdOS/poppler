pkgname=(
    poppler
    poppler-glib
)
pkgbase=poppler
pkgver=25.09.1
pkgrel=1
arch=('x86_64')
url="https://poppler.freedesktop.org/"
license=(
    'GPL-2.0-only'
    'GPL-3.0-or-later'
    'LGPL-2.0-or-later'
    'LGPL-2.1-or-later'
    'MIT'
    'HPND-sell-variant'
)
makedepends=(
    'boost'
    'cairo'
    'cmake'
    'curl'
    'fontconfig'
    'gcc-libs'
    'glib2'
    'glib2-devel'
    'gobject-introspection'
    'gpgmepp'
    'gtk3'
    'gtk-doc'
    'icu'
    'lcms2'
    'libjpeg-turbo'
    'ninja'
    'nss'
    'openjpeg'
    'poppler-data'
    'python'
)
options=('!emptydirs')
source=(https://poppler.freedesktop.org/${pkgbase}-${pkgver}.tar.xz)
sha256sums=(0c1091d01d3dd1664a13816861e812d02b29201e96665454b81b52d261fad658)

build() {
    cd ${pkgbase}-${pkgver}

    local cmake_args=(
        -B flarebird-build
        -G Ninja
        -D CMAKE_BUILD_TYPE=Release
        -D CMAKE_INSTALL_PREFIX=/usr
        -D CMAKE_INSTALL_LIBDIR=lib64
        -D TESTDATADIR=${PWD}/testfiles
        -D ENABLE_QT5=OFF
        -D ENABLE_QT6=OFF
        -D ENABLE_GTK_DOC=ON
        -D ENABLE_UNSTABLE_API_ABI_HEADERS=ON
    )

    cmake "${cmake_args[@]}"

    cmake --build flarebird-build
}

package_poppler() {
    pkgdesc="PDF rendering library based on xpdf 3.0"
    depends=(
        'cairo'
        'curl'
        'fontconfig'
        'freetype2'
        'gcc-libs'
        'glibc'
        'gpgmepp'
        'lcms2'
        'libjpeg-turbo'
        'libpng'
        'libtiff'
        'nspr'
        'nss'
        'openjpeg'
        'zlib'
    )

    cd ${pkgbase}-${pkgver}

    DESTDIR=${pkgdir} cmake --install flarebird-build

    # cleanup for splitted build
    rm -vrf ${pkgdir}/usr/include/poppler/{glib,qt6}
    rm -vf  ${pkgdir}/usr/lib64/libpoppler-{glib,qt6}.*
    rm -vf  ${pkgdir}/usr/lib64/pkgconfig/poppler-{glib,qt6}.pc
    rm -vrf ${pkgdir}/usr/{lib64,share}/gir*
    rm -vrf ${pkgdir}/usr/share/gtk-doc
}

package_poppler-glib() {
    pkgdesc="Poppler glib bindings"
    depends=(
        'cairo'
        'freetype2'
        'gcc-libs'
        'glib2'
        'glibc'
        "poppler=${pkgver}"
    )

    cd ${pkgbase}-${pkgver}

    DESTDIR=${pkgdir} cmake --install flarebird-build/glib

    install -m755 -d ${pkgdir}/usr/lib64/pkgconfig
    install -m644 flarebird-build/poppler-glib.pc ${pkgdir}/usr/lib64/pkgconfig/

    rm -vf ${pkgdir}/usr/lib64/libpoppler.*
    rm -vf ${pkgdir}/usr/bin/poppler-glib-demo
}
