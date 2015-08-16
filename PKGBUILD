pkgname=janus-vr-browser-bin
pkgdesc='JanusVR is a 3D VR internet browser'
url='http://www.dgp.toronto.edu/~mccrae/projects/firebox/'
#janusver=$(curl http://www.dgp.toronto.edu/~mccrae/projects/firebox/ 2>/dev/null | grep janusvr_linux.tar.gz | grep -Eo "[[:digit:]]+\.[[:digit:]]+")
#janusver=$(curl http://www.dgp.toronto.edu/~mccrae/projects/firebox/site.html 2>/dev/null | grep "Release " | grep -Eo "[[:digit:]]+\.[[:digit:]]+" | head -1)
janusver=$(curl http://www.dgp.toronto.edu/~mccrae/projects/firebox/version.html)
msg "Janus Browser has Version $janusver"

pkgver=${janusver}
pkgrel=1
arch=('x86_64')
license=('custom')
                               #http://www.dgp.toronto.edu/~mccrae/projects/firebox/downloads/janusvr_linux.tar.gz
source=("janusvr${janusver}.gz::http://www.dgp.toronto.edu/~mccrae/projects/firebox/downloads/janusvr_linux.tar.gz"
        "janusvr.sh" "http://www.dgp.toronto.edu/~mccrae/projects/firebox/img/janus_logo5.jpg"
        "janusvr.desktop"
        "http://haagch.frickel.club/files/libovr.so" #Built with this hack: https://forums.oculus.com/viewtopic.php?uid=119403&f=34&t=16664&start=0#p252973
)
depends=('alsa-lib' 'libxt' 'libnotify' 'mime-types' 'nss' 'gtk2' 'sqlite' 'dbus-glib' 'qt5-multimedia')
makedepends=("imagemagick") #only for converting the icon

#pkgver() {
#}

package() {
  install -d "$pkgdir/opt"
  cp -ra JanusVRBin/ "$pkgdir/opt/janusvr"
#  chmod a+x "$pkgdir/opt/janusvr"/*.sh
  chmod a+x "$pkgdir/opt/janusvr/janusvr"
  chmod -R a+rX "$pkgdir/opt/janusvr"
  
  install -d "$pkgdir/usr/bin"
  install -m755 "$srcdir/janusvr.sh" "$pkgdir/usr/bin/janusvr"
#  ln -s "/opt/janusvr/janusvr" "$pkgdir/usr/bin/janusvr"


  install -d "$pkgdir/usr/share/icons/"
  # segfaults for me with mesa. WAT
  MAGICK_OCL_DEVICE=OFF convert "$srcdir/janus_logo5.jpg" -resize 128x128 "$pkgdir/usr/share/icons/janusvr.png" # screw the aspect ratio


  install -d "$pkgdir/usr/share/applications"
  install "${srcdir}/janusvr.desktop" "${pkgdir}/usr/share/applications/janusvr.desktop"

  # CAUTION: Everyone in the group "games" can replace the whole thing e.g. with malware
  # But for now there doesn't seem to be a better way, because it needs write accessess error_log.txt in its base directory, also temporary files are created there....
  chown -R root:games ${pkgdir}/opt/janusvr
  chmod -R g+w ${pkgdir}/opt/janusvr

#  chown -R root:games ${pkgdir}/opt/janusvr/workspace
#  chmod -R g+w ${pkgdir}/opt/janusvr/workspace

#  chown -R root:games ${pkgdir}/opt/janusvr/bookmarks.txt
#  chmod -R g+w ${pkgdir}/opt/janusvr/bookmarks.txt
mv "$pkgdir/opt/janusvr/libOVRRT64_0.so.5.0.1" "$pkgdir/opt/janusvr/libOVRRT64_0.so.5.0.1.bak"
install -m755 "$srcdir/libovr.so" "$pkgdir/opt/janusvr/libOVRRT64_0.so.5.0.1"
}

md5sums=('SKIP'
         'c05bb2fbfce12c89db560ed8b8bb036a'
         'caa1d06e46c684365cb8cddcac29ee21'
         'a8a2ce8e4dfa4e4e87a6af811b5627a4'
         '7ff8bcec7595e26c4a3912743c4d9061')
