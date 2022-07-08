# _     _            _        _          _____
#| |__ | | __ _  ___| | _____| | ___   _|___ /
#| '_ \| |/ _` |/ __| |/ / __| |/ / | | | |_ \
#| |_) | | (_| | (__|   <\__ \   <| |_| |___) |
#|_.__/|_|\__,_|\___|_|\_\___/_|\_\\__, |____/
#                                  |___/

#Maintainer: blacksky3 <blacksky3@tuta.io> <https://github.com/blacksky3>

#Upstream pages
#https://aur.archlinux.org/packages/opencl-legacy-amdgpu-pro
#https://repo.radeon.com/amdgpu

major=21.20
minor=1271047
ubuntu_ver=20.04

pkgbase=opencl-amdgpu-pro-21.20
pkgname=(opencl-amdgpu-pro-21.20 lib32-opencl-amdgpu-pro-21.20)
pkgver=${major}_${minor}
pkgrel=1
arch=(x86_64)
url='https://repo.radeon.com/amdgpu'
license=(custom: multiple)
makedepends=(wget)
source=(https://github.com/blacksky3/OPENCL-AMDGPU-PRO-21.20/releases/download/21.20/amdgpu-pro-${major}-${minor}-ubuntu-${ubuntu_ver}.tar.xz)

# extracts a debian package
# $1: deb file to extract
extract_deb(){
  local tmpdir="$(basename "${1%.deb}")"
  rm -Rf "$tmpdir"
  mkdir "$tmpdir"
  cd "$tmpdir"
  ar x "$1"
  tar -C "${pkgdir}" -xf data.tar.xz
}

# move ubuntu specific /usr/lib/x86_64-linux-gnu to /usr/lib
# $1: debian package library dir (goes from opt/amdgpu or opt/amdgpu-pro and from x86_64 or i386)
# $2: arch package library dir (goes to usr/lib or usr/lib32)
move_libdir(){
  local deb_libdir="$1"
  local arch_libdir="$2"

  if [ -d "${pkgdir}/${deb_libdir}" ]; then
    if [ ! -d "${pkgdir}/${arch_libdir}" ]; then
      mkdir -p "${pkgdir}/${arch_libdir}"
    fi
    mv -t "${pkgdir}/${arch_libdir}/" "${pkgdir}/${deb_libdir}"/*
    find ${pkgdir} -type d -empty -delete
  fi
}

# move copyright file to proper place and remove debian changelog
move_copyright(){
  find ${pkgdir}/usr/share/doc -name "changelog.Debian.gz" -delete
  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  find ${pkgdir}/usr/share/doc -name "copyright" -exec mv {} ${pkgdir}/usr/share/licenses/${pkgname} \;
  find ${pkgdir}/usr/share/doc -type d -empty -delete
}

package_opencl-amdgpu-pro-21.20(){
  pkgdesc='Non-free AMD OpenCL ICD Loaders. Version 21.20 (32-bit)'
  arch=(x86_64)
  license=(custom: AMDGPU-PRO EULA)
  conflicts=(opencl-amd opencl-amdgpu-pro)
  provides=(opencl-driver)
  optdepends=(clinfo rocm-opencl-runtime)

  extract_deb "${srcdir}"/amdgpu-pro-${major}-${minor}-ubuntu-${ubuntu_ver}/opencl-orca-amdgpu-pro-icd_${major}-${minor}_amd64.deb
  move_libdir "opt/amdgpu-pro/lib/x86_64-linux-gnu" "usr/lib"
  move_copyright
  touch ${pkgdir}/etc/OpenCL/vendors/amdocl-orca.icd
  echo libamdocl-orca.so >> ${pkgdir}/etc/OpenCL/vendors/amdocl-orca.icd
  touch ${pkgdir}/etc/OpenCL/vendors/amdocl-orca-ld-64.icd
  echo /usr/lib/libamdocl-orca64.so >> ${pkgdir}/etc/OpenCL/vendors/amdocl-orca-ld-64.icd
  ln -s /usr/lib/libamdocl-orca64.so ${pkgdir}/usr/lib/libamdocl-orca.so
}

package_lib32-opencl-amdgpu-pro-21.20(){
  pkgdesc='Non-free AMD OpenCL ICD Loaders. Version 21.20 (32-bit)'
  arch=(i686 x86_64)
  license=(custom: AMDGPU-PRO EULA)
  depends=(opencl-amdgpu-pro-21.20=${major}_${minor}-${pkgrel})
  conflicts=(opencl-amd ib32-opencl-amdgpu-pro)
  provides=(lib32-opencl-driver)

  extract_deb "${srcdir}"/amdgpu-pro-${major}-${minor}-ubuntu-${ubuntu_ver}/opencl-orca-amdgpu-pro-icd_${major}-${minor}_i386.deb
  move_libdir "opt/amdgpu-pro/lib/i386-linux-gnu" "usr/lib32"
  move_copyright
  touch ${pkgdir}/etc/OpenCL/vendors/amdocl-orca-ld-32.icd
  echo /usr/lib32/libamdocl-orca32.so >> ${pkgdir}/etc/OpenCL/vendors/amdocl-orca-ld-32.icd
  ln -s /usr/lib32/libamdocl-orca32.so ${pkgdir}/usr/lib32/libamdocl-orca.so
}

sha256sums=('8ea051de8c9c6814eb45ce18d102e639bb6edb5786e948b50c5105e3e21978f9')
