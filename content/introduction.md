---
title:	"Introduction"
---
# Introduction

The aim of the Darling project is to provide the following:

* a dynamic loader that can load OS X executables (Mach-O);
* basic command-line utilities (e.g. OS X Bash);
* runtime, system library and framework reimplementations to enable OS X executables to function;
* additional tools to assist with application installation.

## Docs

This is just an introductory article. You can always learn more by visiting the [documentation](https://docs.darlinghq.org/).

## Running Applications

OS X applications on x86 are either 64-bit or 32-bit. Darling can run both, depending on whether you have built / installed 32-bit support.

Darling has support for DPREFIXes, which are very similar to WINEPREFIXes. They are virtual “chroot” environments with an OS X-like filesystem structure, where you can install software safely. The default DPREFIX location is `~/.darling`, but this can be changed by exporting an identically named environment variable.

Note that Darling needs Internet access to download initial prefix contents.

The `/home` directory is symlinked to your real `/home`, so you can feel at home right away. The real root directory is accessible through `/system-root`.

Darling bundles a whole collection of elementary console binaries, namely Bash and most of coreutils. To enter Darling’s shell, run

```
$ darling shell
Darling [~]$ 
```

You can try out some simple commands, such as `ls`, `uname`, `sw_vers` and others.

You can directly execute an OS X binary inside the chroot with a real (non-chrooted) path:

```
$ darling /somewhere/binary
```

or run it through Bash with a chroot-relative path:

```
$ darling shell echo "Hello world"
Hello world
```

## Installing Software

You can install .pkg packages with the installer tool available inside shell. It is a somewhat limited cousin of OS X’s installer:

```
$ darling shell
Darling [~]$ installer -pkg mc-4.8.7-0.pkg -target /
```

If you have previously downloaded the Midnight Commander package from [Rudix](http://www.rudix.org), you can now run `mc` to start MC for OS X.

You can uninstall and list packages with the `uninstaller` command.

### Rudix Package Manager

For easier installation of common Unix utilities, install the [Rudix Package Manager](http://rudix.org/):

```
$ darling shell
Darling [~]$ curl -s https://raw.githubusercontent.com/rudix-mac/rpm/2015.10.20/rudix.py | sudo python - install rudix
```

Then you can install Midnight Commander with just:

```
Darling [~]$ sudo rudix -i mc
```

## Working with DMG Images

DMG images can be attached and deattached from inside darling shell with `hdiutil`. This is how you can install Xcode (which you can get from Apple’s Developer Area) along with its toolchain and SDKs (note that Xcode itself doesn’t run yet):

```
Darling [~]$ hdiutil attach Xcode_7.2.dmg
/Volumes/Xcode_7.2
Darling [~]$ cp -r /Volumes/Xcode_7.2/Xcode.app /Applications
Darling [~]$ export SDKROOT=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk
Darling [~]$ echo 'void main() { puts("Hello world"); }' > helloworld.c
Darling [~]$ /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang helloworld.c -o helloworld
Darling [~]$ ./helloworld
Hello world
```

Congratulations, you have just compiled and run your own Hello world application with Apple’s toolchain.

## Tools

Darling contains a pair of tools to assist with diagnostics.

### motool

`motool` can be used to analyze contents of Mach-O executables. It is similar (but simpler than) `readelf` for ELF executables or Apple’s `otool`.

### dyldd

`dyldd` can be used to analyze what libraries/frameworks are required by given executable and how the dynamic linker will try to resolve them (similar to ldd on Linux).

<p style="text-align: center">
<a href="/img/articles/dyldd-1.png"><img src="/img/articles/dyldd-1.png" alt="dyldd" width="1152" height="122" /></a></p>


