
pkgbase=linux-amd
pkgname=('linux-amd' 'linux-amd-headers' 'linux-amd-docs') 
_kernelname=${pkgname#linux}
_basekernel=5.5
_srcname=linux-5.5
pkgver=5.5.9
pkgrel=1
arch=('x86_64')
url="http://www.kernel.org/"
license=('GPL2')
makedepends=('xmlto' 'docbook-xsl' 'kmod' 'inetutils' 'bc' 'elfutils' 'cpio')
options=('!strip')
source=("https://www.kernel.org/pub/linux/kernel/v5.x/${_srcname}.tar.xz"
#source=("https://git.kernel.org/torvalds/t/linux-5.3-rc8.tar.gz"
        "https://www.kernel.org/pub/linux/kernel/v5.x/patch-${pkgver}.xz"
        # the main kernel config files
        'config.amd'
        #aufs patches
        'aufs5-loopback.patch'
        'aufs5-base.patch'
        'aufs5-mmap.patch'
        'aufs5-standalone.patch'
        'aufs5-kbuild.patch'
        'aufs5.patch'
        'config.aufs'
        'tmpfs-idr.patch'
        'vfs-ino.patch'
        # standard config files for mkinitcpio ramdisk
        'linux-amd.preset'
        '99-linux-amd.hook'
        "https://github.com/torvalds/linux/commit/e16c4790de39dc861b749674c2a9319507f6f64f.diff"
		# TkG Patches
        '0001-add-sysctl-to-disallow-unprivileged-CLONE_NEWUSER-by.patch'
        '0002-clear-patches.patch'
        '0003-glitched-base.patch'
        '0004-5.5-ck1.patch'
        '0004-glitched-muqss.patch'
        '0007-v5.5-fsync.patch')
md5sums=('0a78b1dc48dc032fe505b170c1b92339'
         '2270d981592960013a79d1dd876a8885'
         SKIP
         #'faa7d9a6e7928b8af7e293cfb333969f'
         '31cd97f6bc52de17999ecaed60fd90ad'
         'a8a0c0923217837f151ef007f47a4e80'
         'aae8c706fcf69d62559106e11e251699'
         '7594929a2842d5ff0dcf022cd766100a'
         'f2ef50d205218a05a75c4f8383160016'
         '6d099aad60d0d620171f6a431a850b12'
         '180e55a128db2641e04374ce07d131a7'
         'c9dca7bab329ae7c927301b8de8723be'
         '91b628bf4d4beb80fa53a8eb7332023c'
         'b7762a636a3b4c8a3881f47df72d5a23'
         'c7b90e44dcc0d09c54dae0786ba43768'
         'a71035252857af0cf5d84c591b919996'
         'f89cd458929e9817a41861119bf8fcc1'
         'b10e4c612d5240d66fad8f1c50fe3242'
         'f77abde7382fea61719cfc1f190ee768'
         '00400184a5db4095b4ac93ee43e022a8'
         '75602fa70033aef9cb42f3df16ec2eb3'
         '2f4c782ab1e2c2327882eb5b44ff462d')
         
prepare() {
  cd "${srcdir}/linux-${_basekernel}"

  # add upstream patch
  patch -p1 -i "${srcdir}/patch-${pkgver}"

  # add latest fixes from stable queue, if needed
  # http://git.kernel.org/?p=linux/kernel/git/stable/stable-queue.git
  patch -Np1 -i "${srcdir}/0001-add-sysctl-to-disallow-unprivileged-CLONE_NEWUSER-by.patch"

  #TkG
  patch -Np1 -i "${srcdir}/0002-clear-patches.patch"
  patch -Np1 -i "${srcdir}/0003-glitched-base.patch"

  #MuQSS
  patch -Np1 -i "${srcdir}/0004-5.5-ck1.patch"
  patch -Np1 -i "${srcdir}/0004-glitched-muqss.patch"

  #aufs patches for Live:
  patch -p1 -i "${srcdir}/aufs5.patch"
  patch -p1 -i "${srcdir}/aufs5-base.patch"
  patch -p1 -i "${srcdir}/aufs5-kbuild.patch"
  patch -p1 -i "${srcdir}/aufs5-loopback.patch"
  patch -p1 -i "${srcdir}/aufs5-mmap.patch"
  ##patch -p1 -i "${srcdir}/aufs5-standalone.patch"
  ##patch -p1 -i "${srcdir}/tmpfs-idr.patch"
  ##patch -p1 -i "${srcdir}/vfs-ino.patch"
  #fix error: too few arguments to function 'loop_switch' error with RC aufs patches
  #sed -i "s|return loop_switch(lo, NULL);|return loop_switch(lo, NULL, NULL);|g" drivers/block/loop.c
  
  cat "${srcdir}/config.amd" > ./.config

  cat "${srcdir}/config.aufs" >> ./.config

  # Set some -tkg defaults
  echo "# CONFIG_DYNAMIC_FAULT is not set" >> ./.config
  sed -i -e 's/CONFIG_DEFAULT_FQ_CODEL=y/# CONFIG_DEFAULT_FQ_CODEL is not set/' ./.config
  echo "CONFIG_DEFAULT_CAKE=y" >> ./.config
  echo "CONFIG_NR_TTY_DEVICES=63" >> ./.config
  echo "CONFIG_TP_SMAPI=m" >> ./.config
  echo "CONFIG_RAID6_USE_PREFER_GEN=y" >> ./.config
  echo "# CONFIG_NTP_PPS is not set" >> ./.config
  sed -i -e 's/CONFIG_CRYPTO_LZ4=m/CONFIG_CRYPTO_LZ4=y/' ./.config
  sed -i -e 's/CONFIG_CRYPTO_LZ4HC=m/CONFIG_CRYPTO_LZ4HC=y/' ./.config
  sed -i -e 's/CONFIG_LZ4_COMPRESS=m/CONFIG_LZ4_COMPRESS=y/' ./.config
  sed -i -e 's/CONFIG_LZ4HC_COMPRESS=m/CONFIG_LZ4HC_COMPRESS=y/' ./.config
  #sed -i -e 's/CONFIG_RCU_BOOST_DELAY=500/CONFIG_RCU_BOOST_DELAY=0/' ./.config
  sed -i -e 's/# CONFIG_CMDLINE_BOOL is not set/CONFIG_CMDLINE_BOOL=y/' ./.config
  echo "CONFIG_CMDLINE=\"${_custom_commandline}\"" >> ./.config
  echo "# CONFIG_CMDLINE_OVERRIDE is not set" >> ./.config
  sed -i -e 's/CONFIG_FONT_TER16x32=y/# CONFIG_FONT_TER16x32 is not set\nCONFIG_FONT_AUTOSELECT=y/' ./.config

  # Inject cpuopts options
  echo "# CONFIG_MK8SSE3 is not set" >> ./.config
  echo "# CONFIG_MK10 is not set" >> ./.config
  echo "# CONFIG_MBARCELONA is not set" >> ./.config
  echo "# CONFIG_MBOBCAT is not set" >> ./.config
  echo "# CONFIG_MJAGUAR is not set" >> ./.config
  echo "# CONFIG_MBULLDOZER is not set" >> ./.config
  echo "# CONFIG_MPILEDRIVER is not set" >> ./.config
  echo "# CONFIG_MSTEAMROLLER is not set" >> ./.config
  echo "# CONFIG_MEXCAVATOR is not set" >> ./.config
  echo "# CONFIG_MZEN is not set" >> ./.config
  echo "# CONFIG_MZEN2 is not set" >> ./.config
  echo "# CONFIG_MATOM is not set" >> ./.config
  echo "# CONFIG_MNEHALEM is not set" >> ./.config
  echo "# CONFIG_MWESTMERE is not set" >> ./.config
  echo "# CONFIG_MSILVERMONT is not set" >> ./.config
  echo "# CONFIG_MSANDYBRIDGE is not set" >> ./.config
  echo "# CONFIG_MIVYBRIDGE is not set" >> ./.config
  echo "# CONFIG_MHASWELL is not set" >> ./.config
  echo "# CONFIG_MBROADWELL is not set" >> ./.config
  echo "# CONFIG_MSKYLAKE is not set" >> ./.config
  echo "# CONFIG_MSKYLAKEX is not set" >> ./.config
  echo "# CONFIG_MCANNONLAKE is not set" >> ./.config
  echo "# CONFIG_MICELAKE is not set" >> ./.config
  echo "# CONFIG_MGOLDMONT is not set" >> ./.config
  echo "# CONFIG_MGOLDMONTPLUS is not set" >> ./.config
  echo "# CONFIG_MCASCADELAKE is not set" >> ./.config

  # Disable some debugging
  sed -i -e 's/CONFIG_SLUB_DEBUG=y/# CONFIG_SLUB_DEBUG is not set/' ./.config
  sed -i -e 's/CONFIG_PM_DEBUG=y/# CONFIG_PM_DEBUG is not set/' ./.config
  sed -i -e 's/CONFIG_PM_ADVANCED_DEBUG=y/# CONFIG_PM_ADVANCED_DEBUG is not set/' ./.config
  sed -i -e 's/CONFIG_PM_SLEEP_DEBUG=y/# CONFIG_PM_SLEEP_DEBUG is not set/' ./.config
  sed -i -e 's/CONFIG_ACPI_DEBUG=y/# CONFIG_ACPI_DEBUG is not set/' ./.config
  sed -i -e 's/CONFIG_SCHED_DEBUG=y/# CONFIG_SCHED_DEBUG is not set/' ./.config
  sed -i -e 's/CONFIG_DEBUG_PREEMPT=y/# CONFIG_DEBUG_PREEMPT is not set/' ./.config

  # MuQSS default config
  echo "CONFIG_SCHED_MUQSS=y" >> ./.config

  # Disable CFS
  sed -i -e 's/CONFIG_FAIR_GROUP_SCHED=y/# CONFIG_FAIR_GROUP_SCHED is not set/' ./.config
  sed -i -e 's/CONFIG_CFS_BANDWIDTH=y/# CONFIG_CFS_BANDWIDTH is not set/' ./.config

  # zenify
  echo "CONFIG_ZENIFY=y" >> ./.config

  # compiler optimization level
  echo "# CONFIG_CC_OPTIMIZE_FOR_PERFORMANCE_O3 is not set" >> ./.config

  # cpu opt
  echo "# CONFIG_MNATIVE is not set" >> ./.config
  sed -i -e 's/CONFIG_GENERIC_CPU=y/# CONFIG_GENERIC_CPU is not set/' ./.config
  sed -i -e 's/# CONFIG_MZEN2 is not set/CONFIG_MZEN2=y/' ./.config 

  # irq threading
  echo "# CONFIG_FORCE_IRQ_THREADING is not set" >> ./.config

  # smt nice
  echo "CONFIG_SMT_NICE=y" >> ./.config

  # random trust cpu
  sed -i -e 's/# CONFIG_RANDOM_TRUST_CPU is not set/CONFIG_RANDOM_TRUST_CPU=y/' ./.config

  # rq sharing
  echo -e "# CONFIG_RQ_NONE is not set\n# CONFIG_RQ_SMT is not set\nCONFIG_RQ_MC=y\n# CONFIG_RQ_MC_LLC is not set\n# CONFIG_RQ_SMP is not set\n# CONFIG_RQ_ALL is not set" >> ./.config

  # timer freq
  sed -i -e 's/CONFIG_HZ=300/CONFIG_HZ=100/' ./.config
  echo "# CONFIG_HZ_500 is not set" >> ./.config
  echo "# CONFIG_HZ_750 is not set" >> ./.config
  echo "# CONFIG_HZ_1000_NODEF is not set" >> ./.config
  echo "# CONFIG_HZ_750_NODEF is not set" >> ./.config
  echo "# CONFIG_HZ_500_NODEF is not set" >> ./.config
  echo "# CONFIG_HZ_250_NODEF is not set" >> ./.config
  echo "# CONFIG_HZ_300_NODEF is not set" >> ./.config
  echo "CONFIG_HZ_100=y" >> ./.config
  echo "CONFIG_HZ_100_NODEF=y" >> ./.config

  # default cpu gov
  sed -i -e 's/CONFIG_CPU_FREQ_DEFAULT_GOV_SCHEDUTIL=y/# CONFIG_CPU_FREQ_DEFAULT_GOV_SCHEDUTIL is not set/' ./.config
  sed -i -e 's/# CONFIG_CPU_FREQ_DEFAULT_GOV_PERFORMANCE is not set/CONFIG_CPU_FREQ_DEFAULT_GOV_PERFORMANCE=y/' ./.config

  # ACPI_CPUFREQ disablement
  sed -i -e 's/CONFIG_X86_ACPI_CPUFREQ=m/# CONFIG_X86_ACPI_CPUFREQ is not set/' ./.config

  # ftrace
  sed -i -e 's/CONFIG_FUNCTION_TRACER=y/# CONFIG_FUNCTION_TRACER is not set/' ./.config
  sed -i -e 's/CONFIG_FUNCTION_GRAPH_TRACER=y/# CONFIG_FUNCTION_GRAPH_TRACER is not set/' ./.config

  # disable NUMA since 99.9% of users do not have multiple CPUs but do have multiple cores in one CPU
  sed -i -e 's/CONFIG_NUMA=y/# CONFIG_NUMA is not set/' \
      -i -e '/CONFIG_AMD_NUMA=y/d' \
      -i -e '/CONFIG_X86_64_ACPI_NUMA=y/d' \
      -i -e '/CONFIG_NODES_SPAN_OTHER_NODES=y/d' \
      -i -e '/# CONFIG_NUMA_EMU is not set/d' \
      -i -e '/CONFIG_NODES_SHIFT=6/d' \
      -i -e '/CONFIG_NEED_MULTIPLE_NODES=y/d' \
      -i -e '/CONFIG_USE_PERCPU_NUMA_NODE_ID=y/d' \
      -i -e '/CONFIG_ACPI_NUMA=y/d' ./.config

  # tickless
  echo "# CONFIG_NO_HZ_FULL_NODEF is not set" >> ./.config
  sed -i -e 's/CONFIG_HZ_PERIODIC=y/# CONFIG_HZ_PERIODIC is not set/' ./.config
  sed -i -e 's/# CONFIG_NO_HZ_IDLE is not set/CONFIG_NO_HZ_IDLE=y/' ./.config
  sed -i -e 's/CONFIG_NO_HZ_FULL=y/# CONFIG_NO_HZ_FULL is not set/' ./.config
  sed -i -e 's/# CONFIG_NO_HZ is not set/CONFIG_NO_HZ=y/' ./.config
  sed -i -e 's/# CONFIG_NO_HZ_COMMON is not set/CONFIG_NO_HZ_COMMON=y/' ./.config

  # fsync support
  patch -Np1 -i "${srcdir}/0007-v5.5-fsync.patch"

  if [ "${_kernelname}" != "" ]; then
    sed -i "s|CONFIG_LOCALVERSION=.*|CONFIG_LOCALVERSION=\"${_kernelname}\"|g" ./.config
  fi

  # set extraversion to pkgrel
  sed -ri "s|^(EXTRAVERSION =).*|\1 -${pkgrel}|" Makefile
}
  
build() {
  cd "${srcdir}/linux-${_basekernel}"

  export KBUILD_BUILD_USER="k"
  export KBUILD_BUILD_HOST="`uname -m`.kaosx.us"

  # get kernel version
  make prepare
  
  # rewrite configuration
  yes "" | make config >/dev/null

  # build!
  make ${MAKEFLAGS} bzImage modules
}

package_linux-amd() {
  pkgdesc="The Linux Kernel and modules"
  depends=('coreutils' 'linux-firmware' 'kmod' 'mkinitcpio>=0.7')
  provides=("linux=$pkgver")
  backup=("etc/mkinitcpio.d/${pkgname}.preset")
  install=linux-amd.install

  cd "${srcdir}/linux-${_basekernel}"

  # get kernel version
  _kernver="$(make kernelrelease)"

  mkdir -p "${pkgdir}"/{lib/modules,lib/firmware,boot}
  make INSTALL_MOD_PATH="${pkgdir}" modules_install
  cp arch/x86/boot/bzImage "${pkgdir}/boot/vmlinuz-${pkgname}"

  # add vmlinux
  install -D -m644 vmlinux "${pkgdir}/usr/src/linux-${_kernver}/vmlinux"

  # install fallback mkinitcpio preset file and pacman hook for kernel
  install -D -m644 "${srcdir}/linux-amd.preset" "${pkgdir}/etc/mkinitcpio.d/linux-amd.preset"
  install -D -m644 "${srcdir}/99-linux-amd.hook" "${pkgdir}/usr/share/libalpm/hooks/99-${pkgbase}.hook"

  # set correct depmod command for install
  sed \
    -e  "s/KERNEL_NAME=.*/KERNEL_NAME=${_kernelname}/" \
    -e  "s/KERNEL_VERSION=.*/KERNEL_VERSION=${_kernver}/" \
    -i "${startdir}/linux-amd.install"
  sed \
    -e "1s|'linux.*'|'${pkgname}'|" \
    -e "s|ALL_kver=.*|ALL_kver=\"/boot/vmlinuz-${pkgname}\"|" \
    -e "s|default_image=.*|default_image=\"/boot/initramfs-${pkgname}.img\"|" \
    -e "s|fallback_image=.*|fallback_image=\"/boot/initramfs-${pkgname}-fallback.img\"|" \
    -i "${pkgdir}/etc/mkinitcpio.d/linux-amd.preset"

  # remove build and source links
  rm -f "${pkgdir}"/lib/modules/${_kernver}/{source,build}
  # remove the firmware
  rm -rf "${pkgdir}/lib/firmware"
  # gzip -9 all modules to save 100MB of space
  find "${pkgdir}" -name '*.ko' -exec gzip -9 {} \;
  # make room for external modules
  ln -s "../extramodules-${_basekernel}${_kernelname:--AMD}" "${pkgdir}/lib/modules/${_kernver}/extramodules"
  # add real version for building modules and running depmod from post_install/upgrade
  mkdir -p "${pkgdir}/lib/modules/extramodules-${_basekernel}${_kernelname:--AMD}"
  echo "${_kernver}" > "${pkgdir}/lib/modules/extramodules-${_basekernel}${_kernelname:--AMD}/version"
  
}

package_linux-amd-headers() {
  pkgdesc="Header files and scripts for building modules for linux kernel"
  provides=("linux-headers=$pkgver")

  install -dm755 "${pkgdir}/lib/modules/${_kernver}"

  cd "${pkgdir}/lib/modules/${_kernver}"
  ln -sf ../../../usr/src/linux-${_kernver} build

  cd "${srcdir}/linux-${_basekernel}"
  install -D -m644 Makefile \
    "${pkgdir}/usr/src/linux-${_kernver}/Makefile"
  install -D -m644 kernel/Makefile \
    "${pkgdir}/usr/src/linux-${_kernver}/kernel/Makefile"
  install -D -m644 .config \
    "${pkgdir}/usr/src/linux-${_kernver}/.config"

  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/include"

  for i in acpi asm-generic config crypto drm generated linux math-emu \
    media net pcmcia scsi rdma soc sound trace uapi video xen; do
    cp -a include/${i} "${pkgdir}/usr/src/linux-${_kernver}/include/"
  done

  # copy arch includes for external modules
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/arch/x86"
  cp -a arch/x86/include "${pkgdir}/usr/src/linux-${_kernver}/arch/x86/"

  # copy files necessary for later builds, like nvidia and vmware
  cp Module.symvers "${pkgdir}/usr/src/linux-${_kernver}"
  cp -a scripts "${pkgdir}/usr/src/linux-${_kernver}"

  # fix permissions on scripts dir
  chmod og-w -R "${pkgdir}/usr/src/linux-${_kernver}/scripts"
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/.tmp_versions"

  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/arch/x86/kernel"

  cp arch/x86/Makefile "${pkgdir}/usr/src/linux-${_kernver}/arch/x86/"

  cp arch/x86/kernel/asm-offsets.s "${pkgdir}/usr/src/linux-${_kernver}/arch/x86/kernel/"

  # add headers for lirc package
  # pci
  for i in bt8xx cx88 saa7134; do
    mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/pci/${i}"
    cp -a drivers/media/pci/${i}/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/pci/${i}"
  done
  # usb
  for i in cpia2 em28xx pwc; do
    mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/usb/${i}"
    cp -a drivers/media/usb/${i}/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/usb/${i}"
  done
  # i2c
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c"
  cp drivers/media/i2c/*.h  "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c/"
  for i in cx25840; do
    mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c/${i}"
    cp -a drivers/media/i2c/${i}/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c/${i}"
  done

  # add dm headers
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/md"
  cp drivers/md/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/md"

  # add inotify.h
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/include/linux"
  cp include/linux/inotify.h "${pkgdir}/usr/src/linux-${_kernver}/include/linux/"

  # add wireless headers
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/net/mac80211/"
  cp net/mac80211/*.h "${pkgdir}/usr/src/linux-${_kernver}/net/mac80211/"

  # add dvb headers for external modules
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/include/config/dvb/"
  cp include/config/dvb/*.h "${pkgdir}/usr/src/linux-${_kernver}/include/config/dvb/"

  # add dvb headers for http://mcentral.de/hg/~mrec/em28xx-new
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-frontends/"
  cp drivers/media/dvb-frontends/lgdt330x.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-frontends/"
  cp drivers/media/i2c/msp3400-driver.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/i2c/"

  # add dvb headers
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/usb/dvb-usb"
  cp drivers/media/usb/dvb-usb/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/usb/dvb-usb/"
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-frontends"
  cp drivers/media/dvb-frontends/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/dvb-frontends/"
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/tuners"
  cp drivers/media/tuners/*.h "${pkgdir}/usr/src/linux-${_kernver}/drivers/media/tuners/"

  # add xfs and shmem for aufs building
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/fs/xfs/libxfs"
  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}/mm"
  cp fs/xfs/libxfs/xfs_sb.h "${pkgdir}/usr/src/linux-${_kernver}/fs/xfs/libxfs/xfs_sb.h"

  #aufs5-util need
  sed -i "s:__user::g" "${pkgdir}/usr/src/linux-${_kernver}/include/uapi/linux/aufs_type.h"

  # copy in Kconfig files
  for i in `find . -name "Kconfig*"`; do
    mkdir -p "${pkgdir}"/usr/src/linux-${_kernver}/`echo ${i} | sed 's|/Kconfig.*||'`
    cp ${i} "${pkgdir}/usr/src/linux-${_kernver}/${i}"
  done
  # Removing documentation, part of docs package
  rm -r "${pkgdir}"/usr/src/linux-${_kernver}/Documentation
  
  # save signing files, needed for modules signing
  cp certs/signing_key.pem "/$HOME/signing_key.pem"
  cp certs/signing_key.x509 "/$HOME/signing_key.x509"

  chown -R root.root "${pkgdir}/usr/src/linux-${_kernver}"
  find "${pkgdir}/usr/src/linux-${_kernver}" -type d -exec chmod 755 {} \;

  # strip scripts directory
  find "${pkgdir}/usr/src/linux-${_kernver}/scripts" -type f -perm -u+w 2>/dev/null | while read binary ; do
    case "$(file -bi "${binary}")" in
      *application/x-sharedlib*) # Libraries (.so)
        /usr/bin/strip ${STRIP_SHARED} "${binary}";;
      *application/x-archive*) # Libraries (.a)
        /usr/bin/strip ${STRIP_STATIC} "${binary}";;
      *application/x-executable*) # Binaries
        /usr/bin/strip ${STRIP_BINARIES} "${binary}";;
    esac
  done
  
  # add objtool, nvidia needed
  install -Dt "${pkgdir}/usr/src/linux-${_kernver}/tools/objtool" tools/objtool/objtool

  # remove unneeded architectures
  rm -rf "${pkgdir}"/usr/src/linux-${_kernver}/arch/{alpha,arm,arm26,arm64,avr32,blackfin,c6x,cris,frv,h8300,hexagon,ia64,m32r,m68k,m68knommu,mips,microblaze,mn10300,openrisc,parisc,powerpc,ppc,s390,score,sh,sh64,sparc,sparc64,tile,unicore32,um,v850,xtensa}
}

package_linux-amd-docs() {
  pkgdesc="Kernel hackers manual - HTML documentation that comes with the Linux kernel."
  provides=("linux-docs=$pkgver")

  cd "${srcdir}/linux-${_basekernel}"

  mkdir -p "${pkgdir}/usr/src/linux-${_kernver}"
  cp -al Documentation "${pkgdir}/usr/src/linux-${_kernver}"
  find "${pkgdir}" -type f -exec chmod 444 {} \;
  find "${pkgdir}" -type d -exec chmod 755 {} \;

  # remove files already in linux-headers
  #rm -f "${pkgdir}/usr/src/linux-${_kernver}/Documentation/kbuild/Kconfig.recursion-issue-01"
  #rm -f "${pkgdir}/usr/src/linux-${_kernver}/Documentation/kbuild/Kconfig.recursion-issue-02"
  #rm -f "${pkgdir}/usr/src/linux-${_kernver}/Documentation/kbuild/Kconfig.select-break"
  
}
