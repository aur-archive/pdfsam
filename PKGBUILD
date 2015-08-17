# Contributor: Tom < reztho at archlinux dot us >
pkgname=pdfsam
pkgver=2.2.2e
pkgrel=2
pkgdesc="A free open source tool to split and merge pdf documents"
arch=('any')
url="http://www.pdfsam.org/"
license=('GPL')
depends=('java-runtime')
makedepends=('java-environment' 'apache-ant' 'libarchive')
provides=('pdfsam')
source=("http://downloads.sourceforge.net/project/${pkgname}/${pkgname}-enhanced/${pkgver}/${pkgname}-${pkgver}-out-src.zip" 
	"${pkgname}.desktop")

_branch_dir=pdfsam-maine

build() {
  _build_dir=${srcdir}/${pkgname}-${pkgver/_/-}/build
  
  cd ${srcdir}/

  # Now we unzip all the zips
  for _i in ${srcdir}/*.zip
  do
	  if [ "${_i}" != "${srcdir}/pdfsam-${pkgver/_/-}-out-src.zip" ]
		  then
			  echo "Inflating ${_i}..."
			  bsdtar -xf ${_i}
	  fi
  done

  # We make our build directory
  mkdir -p ${_build_dir}

  # The main program is needed to be built first
  # NOTE: Be sure to "source /etc/profile.d/apache-ant.sh | reboot | relogin" 
  # after installing apache-ant and before running makepkg
  cd ${srcdir}/${_branch_dir}/ant
  ant -Dbuild.dir=${_build_dir} \
      -Dworkspace.dir=${srcdir}
#      -Dlangpack.dir=${srcdir}/${pkgname}-langpack \
#      -Dlibs.dir=${srcdir}/libraries-1
}

package(){
  _build_dir=${srcdir}/${pkgname}-${pkgver/_/-}/build

  # Now we have the whole thing compiled, so let's install it

  # The main program...
  mkdir -p ${pkgdir}/usr/share/java/${pkgname}/
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/pdfsam-config.xml ${pkgdir}/usr/share/java/${pkgname}/
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/${pkgname}-${pkgver/_/-}.jar ${pkgdir}/usr/share/java/${pkgname}/

  # The plugins...
  cd ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/plugins/
  for _i in *
  do
	  mkdir -p ${pkgdir}/usr/share/java/pdfsam/plugins/${_i}
	  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/plugins/${_i}/* ${pkgdir}/usr/share/java/${pkgname}/plugins/${_i}/
  done

  # The libs...
  mkdir -p ${pkgdir}/usr/share/java/pdfsam/lib/ ${pkgdir}/usr/share/java/pdfsam/ext/
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/lib/* ${pkgdir}/usr/share/java/${pkgname}/lib/
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/ext/* ${pkgdir}/usr/share/java/${pkgname}/ext/
  
  # The scripts to run it which need to be modified...
  mkdir -p ${pkgdir}/usr/bin/
  install -m755 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/bin/run.sh ${pkgdir}/usr/bin/${pkgname}
  sed -i "s@DIRNAME=\"\`dirname \$0\`\"@DIRNAME=\"/usr/share/java/${pkgname}\"@g" ${pkgdir}/usr/bin/${pkgname}
#  sed -i "s/pdfsam-1.1.1.jar/${pkgname}-${pkgver/_/-}.jar/g" ${pkgdir}/usr/bin/${pkgname}
  install -m755 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/bin/run-console.sh ${pkgdir}/usr/bin/${pkgname}-console
  sed -i "s@DIRNAME=\"../lib/\"@DIRNAME=\"/usr/share/java/${pkgname}/lib/\"@g" ${pkgdir}/usr/bin/${pkgname}-console

  # The program is GPL, but because of the libraries there is a mix of licenses...
  cd ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/doc/license/
  for _i in *
  do
	  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}/${_i}
	  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/doc/license/${_i}/* ${pkgdir}/usr/share/licenses/${pkgname}/${_i}/
  done

  # The icon and the .desktop shortcut...
  mkdir -p ${pkgdir}/usr/share/pixmaps ${pkgdir}/usr/share/applications
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/doc/icons/pdfsam_enhanced.png \
  ${pkgdir}/usr/share/pixmaps/pdfsam.png
  install -m644 ${srcdir}/${pkgname}.desktop ${pkgdir}/usr/share/applications/

  # The tutorial and other docs...
  mkdir -p ${pkgdir}/usr/share/doc/${pkgname}/examples ${pkgdir}/usr/share/doc/${pkgname}/xsd
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/doc/pdfsam-1.5.0e-tutorial.pdf ${pkgdir}/usr/share/doc/${pkgname}/
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/doc/changelog-enhanced.txt ${pkgdir}/usr/share/doc/${pkgname}/
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/doc/readme.txt ${pkgdir}/usr/share/doc/${pkgname}/
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/doc/examples/* ${pkgdir}/usr/share/doc/${pkgname}/examples/
  install -m644 ${_build_dir}/${_branch_dir}/release/dist/pdfsam-enhanced/doc/xsd/* ${pkgdir}/usr/share/doc/${pkgname}/xsd/
}

md5sums=('d0ac4747ad1d6678e89706c4726890ee'
         'a068594a157dd85f3f1f811cab44250b')
