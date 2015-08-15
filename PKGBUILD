# Contributor: Michael Eckert <michael.eckert@linuxmail.org>
pkgname=cobbler-git
pkgver=20130217
pkgrel=1
pkgdesc="Cobbler is a Linux installation Server"
arch=('i686' 'x86_64')
url="http://cobbler.github.com"
license=('GPLv2+')
depends=( 'python>=2.3' apache tftp-hpa mod_wsgi2 createrepo python-augeas python2-cheetah python2-netaddr python2-simplejson urlgrabber python2-yaml rsync cdrkit python2-distribute)
makedepends=( git python2-cheetah )
provides=( )
backup=( )
source=( 'cobbler.conf' )
md5sums=( 'a7002f68f558495ac28c5b88a1c9902f' )
_gitroot="git://github.com/cobbler/cobbler.git"
_gitname="cobbler"

build() {
  cd $srcdir
  echo "Fetching source from GIT"
  if ! [ -d "$_gitname" ] ; then
    # if git dir does not exist yet
    # then clone git repo
    git clone "$_gitroot" || return 1
    cd "$_gitname"
  else
    # else pull sources
    cd "$_gitname" && git pull origin || return 1
  fi
  # compile
  python2 setup.py build
  python2 setup.py install --optimize=1 --root=${pkgdir} $PREFIX
  mkdir -p ${pkgdir}/etc/httpd/conf.d/
  cp ../cobbler.conf ${pkgdir}/etc/httpd/conf.d/

  mkdir -p ${pkgdir}/var/spool/koan
  mkdir -p ${pkgdir}/srv/tftp/images

  rm -f ${pkgdir}/etc/cobbler/cobblerd

  rm -rf ${pkgdir}/etc/init.d/
  mkdir -p ${pkgdir}/usr/lib/systemd/system/
  install -m0644 config/cobblerd.service ${pkgdir}/usr/lib/systemd/system/
}

