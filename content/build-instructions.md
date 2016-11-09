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
<td>Debian Stable</td>
<td>i386</td>
<td><a href="http://teamcity.dolezel.info/viewType.html?buildTypeId=Darling_DebianStableX8664&amp;guest=1"><img src="http://teamcity.dolezel.info/app/rest/builds/buildType:(id:Darling_DebianStableX8664)/statusIcon" title="Debian stable build for i386"></a></td>
</tr>
<tr>
<td>Ubuntu 16.04</td>
<td>x86-64</td>
<td><a href="http://teamcity.dolezel.info/viewType.html?buildTypeId=Darling_UbuntuLtsX8664&amp;guest=1"><img src="http://teamcity.dolezel.info/app/rest/builds/buildType:(id:Darling_UbuntuLtsX8664)/statusIcon" title="Ubuntu build for x86-64"></a></td>
</tr>
<tr>
<td>Ubuntu 16.04</td>
<td>i386</td>
<td><a href="http://teamcity.dolezel.info/viewType.html?buildTypeId=Darling_UbuntuLtsX86&amp;guest=1"><img src="http://teamcity.dolezel.info/app/rest/builds/buildType:(id:Darling_UbuntuLtsX86)/statusIcon" title="Ubuntu build for i386"></a></td>
</tr>
</tbody></table>

### For running x86-64 OS X binaries
This is mandatory.

```
cd darling
mkdir -p build/x86-64
cd build/x86-64
cmake ../.. -DCMAKE_TOOLCHAIN_FILE=../../Toolchain-x86_64.cmake
make
make install

# Now we go into src/lkm to build the kernel module
cd ../../src/lkm
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

### For running i386 OS X binaries
You can omit this if you don’t need running 32-bit binaries.

```
cd darling
mkdir -p build/i386
cd build/i386
cmake ../.. -DCMAKE_TOOLCHAIN_FILE=../../Toolchain-x86.cmake
make
make install
```

Required additional dependencies (on top of the x86_64 dependencies):

**Debian** (stable):

```
$ sudo apt-get install libc6-dev-i386 libudev-dev:i386 lib32stdc++-4.9-dev
```

**Ubuntu** (15.10):

```
$ sudo apt-get install libc6-dev-i386 libudev-dev:i386 lib32stdc++-4.9-dev (use current versions!)
```

**Arch Linux** (4.8):

```
$ sudo pacman -S lib32-libstdc++5 lib32-clang
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

## Optional Features

Optionally, you can enable audio support with the `-DFRAMEWORK_COREAUDIO=ON`. This is still under development, so it probably only makes sense if you want to contribute. This switch enables both ALSA and PulseAudio support by default, you can disable either of them with `-DENABLE_ALSA=OFF` or `-DENABLE_PULSEAUDIO=OFF` respectively.

Required dependencies on Debian (stable) / Ubuntu (16.04):

* x86-64: libpulse-dev libasound2-dev libavresample-dev libavformat-dev libavcodec-dev
* i386: libpulse-dev:i386 libasound2-dev:i386 libavresample-dev:i386 libavformat-dev:i386 libavcodec-dev:i386

Note that most of the above -dev packages conflict between x86-64 and i386, so if you build for both platforms, you have to reinstall the right -dev variants before every build. There should be no issues at runtime.

