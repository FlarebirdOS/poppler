pkgname=(
    poppler
    poppler-glib
    poppler-qt
)
pkgbase=poppler
pkgver=25.10.0
pkgrel=2
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
    'pkgconf'
    'poppler-data'
    'python'
    'qt6-qtbase'
)
options=('!emptydirs')
source=(https://poppler.freedesktop.org/${pkgbase}-${pkgver}.tar.xz)
sha256sums=(6b5e9bb64dabb15787a14db1675291c7afaf9387438cc93a4fb7f6aec4ee6fe0)

# build() {
#     cd ${pkgbase}-${pkgver}
#
#     local cmake_args=(
#         -B flarebird-build
#         -G Ninja
#         -D CMAKE_BUILD_TYPE=Release
#         -D CMAKE_INSTALL_PREFIX=/usr
#         -D CMAKE_INSTALL_LIBDIR=lib64
#         -D ENABLE_QT5=OFF
#         -D ENABLE_GTK_DOC=ON
#         -D ENABLE_UNSTABLE_API_ABI_HEADERS=ON
#     )
#
#     cmake "${cmake_args[@]}"
#
#     cmake --build flarebird-build
# }

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

package_poppler-qt() {
    pkgdesc="Poppler Qt6 bindings"
    depends=(
        'freetype2'
        'gcc-libs'
        'glibc'
        'lcms2'
        "poppler=${pkgver}"
        'qt6-qtbase'
    )

    cd ${pkgbase}-${pkgver}

    DESTDIR=${pkgdir} cmake --install flarebird-build/qt6

    install -m755 -d ${pkgdir}/usr/lib64/pkgconfig
    install -m644 flarebird-build/poppler-qt6.pc ${pkgdir}/usr/lib64/pkgconfig/
}
