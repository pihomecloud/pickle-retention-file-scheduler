pkgname=shinken-pickle-retention-file-scheduler
_moddir=pickle-retention-file-scheduler
pkgver=1.4
pkgrel=1
pkgdesc='Webui module for shinken, latest release'
arch=('any')
license=('custom')
depends=('shinken')
makedepends=('curl' 'bc')
backup=("etc/shinken/modules/$_moddir.cfg")
source=("http://shinken.io/grab/$_moddir")
md5sums=('SKIP')

pkgver(){
  cat $srcdir/package.json | grep '^  "version":' | sed -e 's/^  "version": "//' -e 's/".*//'
}

package() {
  rm $srcdir/$_moddir
  mkdir -p $pkgdir/var/lib/shinken/{modules,inventory}/
  mkdir -p $pkgdir/var/lib/shinken/inventory/$_moddir
  cd $srcdir
  chmod -R o-rwx .
  echo Creating $pkgdir/var/lib/shinken/inventory/$_moddir/content.json
  find . | while read fic
  do
    mode8=$(stat $fic --printf=%a)
    mode10=$(echo "obase=8; ibase=10; $mode8" | bc)
    LANG=C stat $fic --printf='{"size":"%s", "type":"%t", "name":"%n", "mode","'$mode8'"},'
  done > $pkgdir/var/lib/shinken/inventory/$_moddir/content.json
  sed -i 's/^\(.*\),$/[\1]/' $pkgdir/var/lib/shinken/inventory/$_moddir/content.json
  mv module $pkgdir/var/lib/shinken/modules/$_moddir
  mv package.json $pkgdir/var/lib/shinken/inventory/$_moddir
  mkdir -p $pkgdir/etc/shinken/modules/
  mv etc/modules/$_moddir.cfg $pkgdir/etc/shinken/modules/
  chmod 0640 $pkgdir/etc/shinken/modules/$_moddir.cfg
}

# vim:set ts=2 sw=2 et: