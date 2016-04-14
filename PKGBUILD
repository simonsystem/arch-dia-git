# $Id$
# Maintainer: Thorsten Töpper <atsutane-tu@freethoughts.de>
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Gregor Ibic <gregor.ibic@intelicom.si>

pkgname=dia-git
_pkgname=dia
pkgver=0.97.0.1776.gfbc3061
pkgrel=1
pkgdesc="A GTK+ based diagram creation program"
arch=('i686' 'x86_64')
license=('GPL')
url="http://live.gnome.org/Dia"
install=dia.install
depends=('libxslt' 'desktop-file-utils' 'libart-lgpl' 'gtk2' 'hicolor-icon-theme')
makedepends=('intltool' 'python2' 'docbook-xsl')
optdepends=('python2')
options=('docs' '!emptydirs')
source=("dia::git://git.gnome.org/dia#branch=master")
noextract=()
md5sums=('SKIP')

pkgver() {
  cd "${srcdir}/${_pkgname}"
  git describe --always | sed -e 's/-/./g' -e 's/DIA_//' -e 's/_/./g'
}

prepare() {
  cd "${srcdir}/${_pkgname}"
  for file in `find -type f -name '*.py'`; do
      sed -i 's_#!/usr/bin/env python_#!/usr/bin/env python2_' "$file"
  done
  sed -i 's#python python2\.1#python2 python2.1#' acinclude.m4
  sed -i 's#python-config#python2-config#' acinclude.m4
}

build() {
  cd "${srcdir}/${_pkgname}"

  export PYTHON=/usr/bin/python2
  ./autogen.sh --prefix=/usr \
    --with-cairo \
    --with-python \
    --disable-gnome \
    --with-hardbooks
  sed -i 's#SUBDIRS = lib objects plug-ins shapes app bindings samples po sheets data doc tests installer#SUBDIRS = lib objects plug-ins shapes app bindings samples po sheets data tests installer#' Makefile
  make
  cd doc
  make html
}

package() {
  cd "${srcdir}/${_pkgname}"
  make DESTDIR="${pkgdir}" install
  cd doc
  make DESTDIR="${pkgdir}" install-html
  ln -sf ../doc/dia/html "${pkgdir}"/usr/share/dia/help
}
