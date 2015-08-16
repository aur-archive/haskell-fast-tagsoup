# Maintainer: Nick Hu <nickhu00 at gmail dot com>
# custom variables
_hkgname=fast-tagsoup
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-fast-tagsoup
pkgver=1.0.4
pkgrel=1
pkgdesc="A fast TagSoup parser that works with only strict bytestrings."
url="http://community.haskell.org/~ndm/tagsoup/"
license=("BSD3")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc=7.6.3-1"
         "haskell-text=0.11.2.3-1"
         "haskell-text-icu=0.6.3.5-1"
         "haskell-tagsoup=0.12.8-3")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
sha256sums=('8d80aac7c5c3812c215e26375bd145640c843aadbae2d48253629a271471164b')

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}

    runhaskell Setup configure -O -p --enable-split-objs --enable-shared --prefix=/usr \
      --docdir=/usr/share/doc/${pkgname} --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
