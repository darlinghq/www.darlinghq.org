---
title:	"Build Instructions"
---
# Build Instructions

You **must** be running a 64-bit x86 Linux distribution. While using Darling on a 32-bit system may be possible (and only 32-bit OS X executables will run), this is not supported. The table below refers to building a 32-bit runtime environment on a 64-bit system.

### Build Status

<table style="border-spacing: 5px; border-collapse: separate; border: 1px solid #ccc">
<tbody><tr>
<th>Distribution</th>
<th>Platform</th>
<th>Status</th>
</tr>
<tr>
<td>Debian Stable</td>
<td>x86-64</td>
<td><a href="http://teamcity.dolezel.info/viewType.html?buildTypeId=Darling_DebianStableX8664&amp;guest=1"><img src="http://teamcity.dolezel.info/app/rest/builds/buildType:(id:Darling_DebianStableX8664)/statusIcon" title="Debian stable build for x86-64"></a></td>
</tr>
<tr>
<td>Ubuntu 16.04</td>
<td>x86-64</td>
<td><a href="http://teamcity.dolezel.info/viewType.html?buildTypeId=Darling_UbuntuLtsX8664&amp;guest=1"><img src="http://teamcity.dolezel.info/app/rest/builds/buildType:(id:Darling_UbuntuLtsX8664)/statusIcon" title="Ubuntu build for x86-64"></a></td>
</tr>
</tbody></table>


```
cd darling
mkdir build
cd build
cmake ../ -DCMAKE_TOOLCHAIN_FILE=../Toolchain.cmake
make
make install

# Now we go into src/lkm to build the kernel module
cd ../src/lkm
make
make install
```

Required dependencies

**Debian** (stable):

```
$ sudo apt-get install cmake clang bison flex xz-utils libfuse-dev libxml2-dev \
	libicu-dev libssl-dev libbz2-dev zlib1g-dev libudev-dev linux-headers-amd64
```

**Ubuntu** (16.04):

```
$ sudo apt-get install cmake clang bison flex xz-utils libfuse-dev libxml2-dev \
	libicu-dev libssl-dev libbz2-dev zlib1g-dev libudev-dev linux-headers-generic
```

**Arch Linux** (4.8):

```
$ sudo pacman -S cmake clang flex icu fuse
```

### Loading the kernel module

Darling uses a kernel module to provide certain OS X specific features, mainly Mach Ports IPC. No OS X application can be run without this module, since Libc requires Mach Ports for its initialization and even for very basic things such as `sleep()`.

**Important note for Ubuntu:** Recent Ubuntu releases use kernel module signing. It is up to you to find and apply a workaround for loading a module that you have built by e.g. disabling signature verification or by signing the module somehow.

```
modprobe darling-mach

# ATTENTION: The kernel module is likely unstable,
# full of vulnerabilities, etc.
# You SHOULD restrict access to /dev/mach to trusted
# users only and be prepared to the eventuality of
# kernel hangups (and related data loss).

chmod a+rw /dev/mach
```

