# Yocto Knowledge Base

## Find Yocto version

`grep DISTRO_ yocto_dir/poky/meta-poky/conf/distro/poky.conf` 


## Auditd

### Auditd recipe

```
git clone git://git.yoctoproject.org/meta-selinux
cd meta-selinux
git checkout <current-yocto-release>
cd yocto_dir/build
bitbake-layers add-layer yocto_dir/meta-selinux
echo "IMAGE_INSTALL_append = " auditd" >> conf/local.conf
bitbake development-image
```

### Enable Audit in Kernel

```
bitbake -c menuconfig virtual/kernel

General setup --> Auditing support

# bitbake -c savedefconfig virtual/kernel
bitbake virtual/kernel
```

### Kernel boot parameters

`cat /proc/cmdline`

Check `audit=1` parameter

### Audit rules

```
-w /etc/passwd -p wa -k passwd_changes
-w /etc/shadow -p wa -k shadow_changes
-w /etc/group -p wa -k group_changes
-a always,exit -F arch=b32 -S execve,execveat -F key=exec_tracking
-w /bin -p wa -k system_binaries
-w /sbin -p wa -k system_binaries
-w /etc/audit -p wa -k audit_config
-a always,exit -F arch=b32 -S open,creat,openat -F exit=-EACCES -F key=access_denied
-a always,exit -F arch=b32 -S open,creat,openat -F exit=-EPERM -F key=permission_denied
-w /var/run/utmp -p wa -k session
-w /var/log/wtmp -p wa -k session
-w /var/log/btmp -p wa -k session
-a always,exclude -F msgtype=CWD
```

## Populate SDK

`bitbake development-image -c populate_sdk`

Results in the following error:
```
DEBUG: Executing python function sstate_task_prefunc
DEBUG: Python function sstate_task_prefunc finished
DEBUG: Executing python function extend_recipe_sysroot
NOTE: Direct dependencies are ['virtual:native:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-extended/pigz/pigz_2.4.bb:do_populate_sysroot', '/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-devtools/qemu/qemuwrapper-cross_1.0.bb:do_populate_sysroot', 'virtual:nativesdk:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-devtools/qemu/qemuwrapper-cross_1.0.bb:do_populate_sysroot', 'virtual:native:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-devtools/opkg/opkg_0.4.2.bb:do_populate_sysroot', 'virtual:native:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-devtools/opkg-utils/opkg-utils_0.4.2.bb:do_populate_sysroot', 'virtual:native:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-core/update-rc.d/update-rc.d_0.8.bb:do_populate_sysroot', 'virtual:nativesdk:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-core/glibc/glibc-locale_2.31.bb:do_populate_sysroot', 'virtual:native:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-devtools/makedevs/makedevs_1.0.1.bb:do_populate_sysroot', 'virtual:native:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-devtools/pseudo/pseudo_git.bb:do_populate_sysroot', 'virtual:native:/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-extended/xz/xz_5.2.4.bb:do_populate_sysroot', '/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-core/glibc/cross-localedef-native_2.31.bb:do_populate_sysroot', '/home/kris/workspace/gateway-qemu/acos/poky/meta/recipes-core/glibc/ldconfig-native_2.12.1.bb:do_populate_sysroot']
NOTE: Installed into sysroot: ['pigz-native', 'qemuwrapper-cross', 'nativesdk-qemuwrapper-cross', 'opkg-native', 'opkg-utils-native', 'update-rc.d-native', 'nativesdk-glibc-locale', 'makedevs-native', 'pseudo-native', 'xz-native', 'cross-localedef-native', 'ldconfig-native', 'quilt-native', 'zlib-native', 'systemd-systemctl-native', 'shadow-native', 'qemu-native', 'patch-native', 'chrpath-native', 'libsolv-native', 'gnu-config-native', 'libarchive-native', 'pkgconfig-native', 'automake-native', 'libtool-native', 'autoconf-native', 'shared-mime-info-native', 'perl-native', 'debianutils-native', 'openssl-native', 'nativesdk-glibc', 'kmod-native', 'depmodwrapper-cross', 'gettext-minimal-native', 'libsdl2-native', 'glib-2.0-native', 'attr-native', 'expat-native', 'cmake-native', 'ninja-native', 'e2fsprogs-native', 'lzo-native', 'bzip2-native', 'texinfo-dummy-native', 'm4-native', 'python3-native', 'libxml2-native', 'itstool-native', 'db-native', 'gdbm-native', 'nativesdk-linux-libc-headers', 'gtk-doc-native', 'libxrandr-native', 'libxext-native', 'libx11-native', 'libxrender-native', 'libpcre-native', 'util-linux-native', 'gettext-native', 'libffi-native', 'meson-native', 'curl-native', 'ncurses-native', 're2c-native', 'readline-native', 'libtirpc-native', 'libnsl2-native', 'sqlite3-native', 'util-macros-native', 'xorgproto-native', 'xtrans-native', 'libxcb-native', 'libpcre2-native', 'libcap-ng-native', 'python3-setuptools-native', 'xcb-proto-native', 'libxau-native', 'libpthread-stubs-native', 'libxdmcp-native', 'unzip-native']
NOTE: Skipping as already exists in sysroot: []
DEBUG: sed -e 's:^[^/]*/:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/:g' /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/quilt-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/gnu-config-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/pkgconfig-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/automake-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/libtool-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/autoconf-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/perl-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/openssl-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/libsdl2-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/glib-2.0-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/e2fsprogs-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/python3-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/libxml2-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/itstool-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/gtk-doc-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/libpcre-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/gettext-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/curl-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/ncurses-native/fixmepath /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/libpcre2-native/fixmepath | xargs sed -i -e 's:FIXMESTAGINGDIRTARGET:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot:g; s:FIXMESTAGINGDIRHOST:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native:g' -e 's:FIXME_PSEUDO_SYSROOT:/home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/pseudo-native:g' -e 's:FIXME_HOSTTOOLS_DIR:/home/kris/workspace/gateway-qemu/acos/build/tmp/hosttools:g' -e 's:FIXME_PKGDATA_DIR:/home/kris/workspace/gateway-qemu/acos/build/tmp/pkgdata/qemux86:g' -e 's:FIXME_PSEUDO_LOCALSTATEDIR:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/pseudo/:g' -e 's:FIXME_LOGFIFO:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/temp/fifo.484304:g'
DEBUG: sed -e 's:^[^/]*/:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot/:g' /home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/qemux86/depmodwrapper-cross/fixmepath | xargs sed -i -e 's:FIXMESTAGINGDIRTARGET:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot:g; s:FIXMESTAGINGDIRHOST:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native:g' -e 's:FIXME_PSEUDO_SYSROOT:/home/kris/workspace/gateway-qemu/acos/build/tmp/sysroots-components/x86_64/pseudo-native:g' -e 's:FIXME_HOSTTOOLS_DIR:/home/kris/workspace/gateway-qemu/acos/build/tmp/hosttools:g' -e 's:FIXME_PKGDATA_DIR:/home/kris/workspace/gateway-qemu/acos/build/tmp/pkgdata/qemux86:g' -e 's:FIXME_PSEUDO_LOCALSTATEDIR:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/pseudo/:g' -e 's:FIXME_LOGFIFO:/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/temp/fifo.484304:g'
DEBUG: Python function extend_recipe_sysroot finished
DEBUG: Executing python function do_populate_sdk
NOTE: Initializing intercept dir for /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/sdk/image/usr/local/oe-sdk-hardcoded-buildpath/sysroots/i586-poky-linux
DEBUG: Collected intercepts:
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/delay_to_first_boot
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/postinst_intercept
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_desktop_database
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_font_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_gio_module_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_gtk_icon_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_gtk_immodules_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_mime_database
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_pixbuf_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_udev_hwdb

NOTE: Initializing intercept dir for /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/sdk/image
DEBUG: Collected intercepts:
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/delay_to_first_boot
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/postinst_intercept
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_desktop_database
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_font_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_gio_module_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_gtk_icon_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_gtk_immodules_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_mime_database
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_pixbuf_cache
  /home/kris/workspace/gateway-qemu/acos/poky/scripts/postinst-intercepts/update_udev_hwdb

NOTE: Installing TARGET packages
NOTE: Executing '/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/usr/bin/opkg-make-index --checksum md5 --checksum sha256 -r /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/sdk-provides-dummy-target/Packages -p /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/sdk-provides-dummy-target/Packages -m /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/sdk-provides-dummy-target' ...
NOTE: Executing '/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/usr/bin/opkg-make-index --checksum md5 --checksum sha256 -r /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/x86_64-nativesdk/Packages -p /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/x86_64-nativesdk/Packages -m /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/x86_64-nativesdk' ...
NOTE: Executing '/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/usr/bin/opkg-make-index --checksum md5 --checksum sha256 -r /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/all/Packages -p /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/all/Packages -m /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/all' ...
NOTE: Executing '/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/usr/bin/opkg-make-index --checksum md5 --checksum sha256 -r /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/sdk-provides-dummy-nativesdk/Packages -p /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/sdk-provides-dummy-nativesdk/Packages -m /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/sdk-provides-dummy-nativesdk' ...
NOTE: Executing '/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/usr/bin/opkg-make-index --checksum md5 --checksum sha256 -r /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/qemux86/Packages -p /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/qemux86/Packages -m /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/qemux86' ...
NOTE: Executing '/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/usr/bin/opkg-make-index --checksum md5 --checksum sha256 -r /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/i586/Packages -p /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/i586/Packages -m /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/oe-sdk-repo/i586' ...
NOTE: Installing the following packages: audit auditd coreutils default-hostname default-ports logrotate openssh-external openssh-external-enable openssh-sshd-cfg-extra opkg packagegroup-base-extended packagegroup-core-boot packagegroup-core-ssh-openssh packagegroup-core-standalone-sdk-target packagegroup-opcua-gw packagegroup-opcua-gw-dev-pkgs polkit run-postinsts sudo target-sdk-provides-dummy users-application
NOTE: /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/usr/bin/opkg --volatile-cache -f /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/opkg.conf -t /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/temp/ipktemp/ -o /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/sdk/image/usr/local/oe-sdk-hardcoded-buildpath/sysroots/i586-poky-linux  --force_postinstall --prefer-arch-to-version   --add-ignore-recommends busybox-syslog install audit auditd coreutils default-hostname default-ports logrotate openssh-external openssh-external-enable openssh-sshd-cfg-extra opkg packagegroup-base-extended packagegroup-core-boot packagegroup-core-ssh-openssh packagegroup-core-standalone-sdk-target packagegroup-opcua-gw packagegroup-opcua-gw-dev-pkgs polkit run-postinsts sudo target-sdk-provides-dummy users-application
ERROR: Unable to install packages. Command '/home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/recipe-sysroot-native/usr/bin/opkg --volatile-cache -f /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/opkg.conf -t /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/temp/ipktemp/ -o /home/kris/workspace/gateway-qemu/acos/build/tmp/work/qemux86-poky-linux/development-image/1.0-r0/sdk/image/usr/local/oe-sdk-hardcoded-buildpath/sysroots/i586-poky-linux  --force_postinstall --prefer-arch-to-version   --add-ignore-recommends busybox-syslog install audit auditd coreutils default-hostname default-ports logrotate openssh-external openssh-external-enable openssh-sshd-cfg-extra opkg packagegroup-base-extended packagegroup-core-boot packagegroup-core-ssh-openssh packagegroup-core-standalone-sdk-target packagegroup-opcua-gw packagegroup-opcua-gw-dev-pkgs polkit run-postinsts sudo target-sdk-provides-dummy users-application' returned 1:
Collected errors:
 * Solver encountered 1 problem(s):
 * Problem 1/1:
 *   - package postgresql-12.18-r0.i586 requires perl >= 5.30.1, but none of the providers can be installed
 * 
 * Solution 1:
 *   - do not ask to install a package providing target-sdk-provides-dummy

 * Solution 2:
 *   - do not ask to install a package providing packagegroup-opcua-gw-dev-pkgs

Disfavor package: busybox-syslog



DEBUG: Python function do_populate_sdk finished
```

