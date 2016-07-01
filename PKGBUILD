# Maintainer: Yunhui Fu <yhfudev@gmail.com>
# Contributor: Dan McCurry <dan.mccurry at linux dot com>
# Contributor: 0xfc <sfc_0@yahoo.com.cn>
# Modified from rtl-sdr-git PKGBUILD

# Nonfree components are, by default, disabled for GPL compatability
# Uncomment the next line to enable nonfree components, such as sdrplay
#_nonfree='-DENABLE_NONFREE=TRUE'

pkgname=gr-osmosdr-git
pkgver=0.1.4.72.g164a09f
pkgrel=1
arch=('i686' 'x86_64')
url="http://sdr.osmocom.org/trac/"

if [[ "$_nonfree" ]]; then
  license=('custom: Do Not Distribute');
  pkgdesc="GNU Radio source block for OsmoSDR, nonfree components enabled."
else
  license=('GPL');
  pkgdesc="GNU Radio source block for OsmoSDR, which is a 100% Free Software based small form-factor inexpensive SDR (Software Defined Radio) project."
fi

depends=(
    'gnuradio'
    'airspy'
    )
makedepends=(
    'git'
    'cmake'
    'boost'
    'python2-cheetah'
    'swig'
    )
optdepends=(
    'gnuradio-iqbal: Osmocom IQ imbalance correction support'
    'rtl-sdr-git: Osmocom RTLSDR support'
    'libosmosdr-git: sysmocom OsmoSDR support'
    'libmirisdr-git: Osmocom MiriSDR support'
    'soapysdr-git: SoapySDR support'
    'libsdrplay: SDRplay RSP support, nonfree must be enabled'
    'gnuradio-fcdproplus: FUNcube Dongle Pro+ support'
    'hackrf: HackRF and rad1o Badge support'
#    'bladerf: nuand bladeRF support'   ## This fails to build when installed
    'doxygen: documentation'
    )

provides=('gr-osmosdr-git' 'gr-osmosdr' 'gnuradio-osmosdr')
conflicts=('gr-osmosdr-git')

install=${pkgname}.install

source=('git://git.osmocom.org/gr-osmosdr')
md5sums=('SKIP')

_gitname="gr-osmosdr"

pkgver() {
  cd $_gitname
  # Use the tag of the last commit
  if [[ "$_nonfree" ]]; then
    echo $(git describe --always | sed 's|-|.|g; s|^.||').nonfree
  else
    git describe --always | sed 's|-|.|g; s|^.||'
  fi
}

prepare() {
  cd "${srcdir}"/$_gitname

  if [[ "$_nonfree" ]]; then
   msg2 "The binaries no longer follow GPL terms and cannot be distributed due to nonfree components."
   sed -i '1s/^/NONFREE components have been enabled. The resulting\nbinaries cannot be distributed under GPL terms.\n\n/' COPYING
  fi
}

build() {
  cd "$srcdir/$_gitname"
  mkdir -p build
  cd build
  cmake -DPYTHON_EXECUTABLE=$(which python2) \
        -DPYTHON_INCLUDE_DIR=$(echo /usr/include/python2*) \
        -DPYTHON_LIBRARY=$(echo /usr/lib/libpython2.*.so) \
        -DCMAKE_INSTALL_PREFIX=/usr \
        ${_nonfree} ../
  make
}

package() {
  cd "$srcdir/$_gitname/build/"
  make DESTDIR=${pkgdir} install

  if [[ "$_nonfree" ]]; then
    install -Dm644 $srcdir/$_gitname/COPYING $pkgdir/usr/share/licenses/$pkgname/LICENSE
  fi
}

# vim:set ts=2 sw=2 et:
