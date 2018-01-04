# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://git.archlinux.org/svntogit/packages.git/log/trunk?h=packages/xorg-xdm
# 						Maintainer: Alexander Baldeck <alexander@archlinux.org>
# 						Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=xorg-xdm
pkgver=1.1.11
pkgrel=7
pkgdesc="X Display Manager"
arch=(x86_64)
url="https://xorg.freedesktop.org/"
license=('custom')
depends=('pam' 'libxaw' 'libxinerama' 'xorg-xrdb' 'xorg-sessreg' 'libxft')
makedepends=('pkgconfig' 'xorg-util-macros' 'xtrans')
backup=(etc/X11/xdm/Xaccess etc/X11/xdm/Xresources etc/X11/xdm/Xservers etc/X11/xdm/xdm-config etc/pam.d/xdm etc/X11/xdm/Xsetup_0 etc/X11/xdm/Xsession)
source=(${url}/releases/individual/app/xdm-${pkgver}.tar.bz2
        Xsession-loginshell.patch
        Xsession-xsm.patch
        xdm-1.0.5-sessreg-utmp-fix-bug177890.patch
        xdm.pam
        git_fixes.diff)
sha256sums=('d4da426ddea0124279a3f2e00a26db61944690628ee818a64df9d27352081c47'
            'fd3e7c20837b42a8ab111369fd6dc9612f9edb91c1f6904cca1d6a1fa3cfa0ff'
            '77a1ce9bdf363591b72798db08b4df3589bd4e64737fd32cf9028f9245450edb'
            '5f380a2d6f77feb910d77f7f6843fce9b00ff7610c159fc029ee44cc6c23a48a'
            'e8c4c5fd3b801a390d201166fd1fb9730e78a5c62928768103b870b6bd980ea0'
            '781b5577bb070220d018a11832d0d4a65fd16e130730ba26fb055c3aa68156b2')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

build() {
  cd "${srcdir}/xdm-${pkgver}"
  # upstream commits - Add some missing malloc failure checks 	2012-01-07
  patch -Np1 -i "${srcdir}/git_fixes.diff"
  
  patch -Np0 -i "${srcdir}/Xsession-loginshell.patch"
  patch -Np1 -i "${srcdir}/Xsession-xsm.patch"
  patch -Np0 -i "${srcdir}/xdm-1.0.5-sessreg-utmp-fix-bug177890.patch"

  autoreconf -fi
  ./configure --prefix=/usr \
      --disable-xdm-auth \
      --disable-static \
      --with-xdmconfigdir=/etc/X11/xdm \
      --with-xdmscriptdir=/etc/X11/xdm \
      --with-pixmapdir=/usr/share/xdm/pixmaps \
      --without-systemdsystemunitdir
  make
}

package() {
  cd "${srcdir}/xdm-${pkgver}"
  make DESTDIR="${pkgdir}" install
  install -m755 -d "${pkgdir}/var/lib/xdm"
  install -m755 -d "${pkgdir}/etc/pam.d"
  install -m644 "${srcdir}/xdm.pam" "${pkgdir}/etc/pam.d/xdm"
  install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/"

  sed -i -e 's/\/X11R6//g' "${pkgdir}"/etc/X11/xdm/*
}
