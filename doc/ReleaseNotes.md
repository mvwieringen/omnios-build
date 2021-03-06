<a href="https://omnios.org">
<img src="https://omnios.org/OmniOSce_logo.svg" height="128">
</a>

# Release Notes for OmniOSce v11 r151038
![#f03c15](https://placehold.it/15/f03c15/000000?text=+) ** These are DRAFT release notes ** ![#f03c15](https://placehold.it/15/f03c15/000000?text=+)

Stable and Long-Term-Supported (LTS) Release, TBC of May 2021

`uname -a` shows `omnios-r151038-XXX`

r151038 release repository: https://pkg.omnios.org/r151038/core

## Upgrade Notes

Before upgrading, please ensure that your `package/pkg` package is up-to-date
by running `pkg update pkg`. The `pkg` software was recently updated for all
supported releases (r151030, r151032 and r151034) to properly handle
conditional dependencies which are more widely used in r151036. The new `pkg`
also has additional diagnostic output to help troubleshoot package updates.

Upgrades are supported from the r151030, r151032, r151034 and r151036 releases.
If upgrading from before r151036, also refer to the following documents:

 * [Release notes for r151032](https://github.com/omniosorg/omnios-build/blob/r151032/doc/ReleaseNotes.md#upgrade-notes)
 * [Release notes for r151034](https://github.com/omniosorg/omnios-build/blob/r151034/doc/ReleaseNotes.md#upgrade-notes)
 * [Release notes for r151036](https://github.com/omniosorg/omnios-build/blob/r151036/doc/ReleaseNotes.md#upgrade-notes)

## New features since r151036

### System Features

* ZFS has gained support for persistent L2ARC. This is automatically enabled
  for all layer-2 ARC devices.

* Support for one-time-boot to a new boot environment, with the
  `beadm activate -t` option. After rebooting the new boot environment,
  a subsequent reboot, crash or power cycle will revert to the original.

* Userland packages are now built with extended protection against stack
  overflow bugs using the Stack Smashing Protector (SSP) compiler feature.

* The legacy OpenSSL 1.0 library is now shipped with weak ciphers disabled
  in order to protect against known security vulnerabilities.

* A number of improvements to the way that memory pages are scanned and
  paged out during low memory conditions have been incorporated in this
  release.

* The boot loader now has better support for multiple consoles, and initial
  output and menus will be displayed on each.

* A number of packages have been recategorised as `optional` which means that
  they are part of the default system installation, but can be removed if
  not required. The list of optional packages can be viewed using:
  ```
  pkg contents -Ha type=optional -o fmri entire
  ```
  > If an optional package is removed from the global zone, then the package
  > will no longer be installed by default into any newly created non-global
  > zones.

* The system boot banner has been updated.

### Commands and Command Options

* The system shell, `/bin/sh` and `/bin/ksh` has been upgraded to _ksh93u+_.
  This fixes a number of bugs and improves standards compliance.

* Python has been upgraded to Python 3.9.

* The new `developer/exuberant-ctags` package is available, delivering a
  more capable version of `/usr/bin/ctags`. This is not installed by default.

* The `roleauth` user attribute has been added. This makes it possible to
  specify which password is required to assume a role - the password from
  the role account or the assuming user's password. See
  [user_attr(4)](https://man.omnios.org/user_attr.4) for more
  details.

* Command line completion information for the `bash` shell has been updated.

* `beadm` has gained the `-t` and `-T` options for activating and de-activating
  one-time boot environments.

* `pkg` has gained the `--temp-be-activate` option to use one-time boot
  environment activation when an update requires a new BE or one has been
  requested.

### Libraries and Library Functions

* Support for the `IP_RECVTOS` socket option has been added.

* The [upanic(2)](https://man.omnios.org/upanic.2) system call
  has been added to terminate a process and generate a core file. This is
  an illumos-specific alternative to the `abort(3)` call which is used by
  the native stack protector. Refer to the manual page for more details.

* The [getgrouplist(3C)](https://man.omnios.org/getgrouplist.3c)
  function has been added to the C library.

### Zones

* The zone boot sequence has been streamlined so that zones boot faster,
  particularly when several are starting in parallel after system boot.

* The `max-processes` zone resource control is now documented and is set
  to match any configured `max-lwps` control if not explicitly set.

### LX zones

* Some processes which relied on the system `RLIMIT_NPROC` parameter could
  crash on startup. This has been resolved.

### Bhyve

* Many improvements from upstream illumos and FreeBSD.

* Improved VNC support for the built-in framebuffer console. This now works
  properly with more clients, including native MacOS screen sharing.

### ZFS

* Support for persistent L2ARC.

* Support for one-time boot environment activation.

### Package Management

### Hardware Support

* Improved support for AMD Zen 3 CPUs.

* Added support for the Hygon Dhyana Family of processors.

* The X710 10GBaseT\*L Family if parts is supported.

* Support for chipset and CPU sensors has been extended.

* There have been several improvements in support for creating network link
  aggregations with a variety of network cards.

### Installer

### Developer Features

* Almost all system binaries and libraries are now delivered with
  embedded Compact Type Format (CTF) debugging data and with the symbol
  table present. This increases observability by providing type data to
  `mdb`, `dtrace` and the kernel.

* A native stack smashing protection (SSP) implementation has been added to
  the standard C library (_libc.so_). The gcc compiler packages have been
  updated to use this implementation when building code with gcc's
  `-fstack-protector` option.

* The `-zassert-deflib` linker option did not work when building 64-bit
  objects; this has been corrected.

* The Compact Type Format (CTF) utilities delivered with the
  developer/build/onbld package have been updated and are able to more easily
  convert data in larger objects.

* The illumos `make` command now sets the `CC` variable to `gcc` by default.
** XXX - man page still to update **

### Deprecated features

* Python 2 is now end-of-life and will not receive any further updates. The
  `python-27` package is still available for backwards compatibility but will
  be maintained only on a best-efforts basis.

* OpenSSL 1.0.x is deprecated and reached end-of-support at the end of 2019.
  OmniOS has completely transitioned to OpenSSL 1.1.1 but retains the OpenSSL
  1.0.2 libraries for backwards compatibility. The 1.0.2 libraries are
  maintained solely on a best-efforts basis.

### Package changes ([+] Added, [-] Removed, [\*] Changed)

