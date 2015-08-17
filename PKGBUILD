# Maintainer: ponsfoot <cabezon dot hashimoto at gmail dot com>

pkgname=ttf-mplus-cvs
pkgver=20120426
pkgrel=1
pkgdesc="M+ sans serif fonts with different weights"
arch=('any')
url="http://mplus-fonts.sourceforge.jp/"
license=('custom')
depends=('fontconfig' 'xorg-font-utils')
makedepends=('fontforge' 'perl' 'python2' 'cvs')
provides=('ttf-mplus')
conflicts=('ttf-mplus')
install=ttf.install

_cvsroot=":pserver:anonymous:@cvs.sourceforge.jp:/cvsroot/mplus-fonts"
_cvsmod="mplus_outline_fonts"

build() {
  cd "$srcdir"

  msg "Connecting to $_cvsmod.sourceforge.net CVS server...."
  if [ -d ${_cvsmod}/CVS ]; then
    cd $_cvsmod
    cvs -z3 update -d -P
  else
    cvs -z3 -d $_cvsroot co -f $_cvsmod
  fi
  msg "CVS checkout done or server timeout"
  msg "Starting make..."

  rm -rf "${srcdir}/${_cvsmod}-build"
  cp -r "${srcdir}/${_cvsmod}" "${srcdir}/${_cvsmod}-build"
  cd "${srcdir}/${_cvsmod}-build"

  sed -i -e 's|/usr/local/|/usr/|g' scripts/*.p[el]

  # Get make -j option from $MAKEFLAGS to pass it to split-svg.pl
  _concurrency=`sed -n -e "s/.*--jobs=\([0-9]\+\).*/\1/p" -e "s/.*-j *\([0-9]\+\).*/\1/p" <<< "$MAKEFLAGS"`

  MPLUS_FULLSET=yes make SPLIT_CONCURRENCY=${_concurrency:-1}
}

package() {
  cd "${srcdir}/${_cvsmod}-build"

  install -d ${pkgdir}/usr/share/fonts/TTF
  install -m644 work.d/targets/mplus-?[^k]*/*/*.ttf "${pkgdir}/usr/share/fonts/TTF/"
  # mplus-[12]k are dummy fonts to build Kanji characters in each fonts.

  install -D -m644 release/LICENSE_E  "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE_E"
}
