---
title: "Mach-O Dynamic Loader"
---
# Mach-O Dynamic Loader

This article explains how OS X applications can be loaded on Linux and why Darling is not an emulator.

## Executable File Structure

Executable files don’t have an overly complicated structure. Mach-O files contain a list of commands for the loader that tell it what to do.

The most important commands are `LC_SEGMENT` and `LC_UNIXTHREAD`/`LC_MAIN`. `LC_SEGMENT` tells that a part of the executable should be memory mapped to a specific location in memory along with a bunch of flags. `LC_UNIXTHREAD`/`LC_MAIN` tells the loader where the entry point of the executable will reside after loading, so the dynamic loader knows where to “jump” after it is done loading. The entry point can be either the main() function directly or some startup code added by the compiler at link time.

If it hadn’t been for the dynamic libraries (`.so` on Linux, `.dylib` on OS X), then these commands would pretty much be enough. Dynamic libraries add a lot of complexity into the mix.

After mapping all the segments into the memory, the loader must resolve all of the executable’s dependencies and then resolve all imported symbols (functions or variables) while following a set of rules. Additional work may be required for some dynamic libraries: as their location in memory is not set in stone, the loader may need to update certain pointers within the library according to the instructions contained within the executable file.

The loader can easily make any changes to the executable file thanks to the copy-on-write mapping (`MAP_PRIVATE`).

## Symbol Resolution

When searching for a symbol named `test123`, Darling looks for `test123@DARWIN` in ELF libraries (e.g. a symbol named `test123` with a `DARWIN` version) and for `_test123` in Mach-O libraries.

When resolving a dependency, the loader follows a specific order to find the right library.

Unlike on Linux where all dependencies have a relative name (e.g. `libc.so.6`), absolute paths\* are used on OS X (e.g. `/usr/lib/libSystem.dylib`). Darling considers both the whole path and the file name in its search.

The search order is at follows:

1. Search for aliases in the configuration file (`/etc/darling/dylib.conf`). This file is also used to map OS X framework names to native Linux libraries.
2. Search in paths specified in `DYLD_LIBRARY_PATH` (the counterpart of `LD_LIBRARY_PATH` on Linux).
3. Try the path as is.
\*) OS X also uses relative paths such as `@rpath/...` or `@executable_path/...`, but always uses absolute paths for system libraries and frameworks.

## What are the Frameworks?

When dealing with OS X development, you hear the word “framework” a lot.

From a technical standpoint, frameworks are ordinary Mach-O libraries with the following differences:

* They don’t have a file extension.
* They don’t carry the version in the file name. Instead, the version is specified with the directory structure (e.g. `/.../SomeFramework.framework/Versions/A/SomeFramework` where “A” is the version).
* They are located in bundles. The bundle is the whole “SomeFramework.framework” directory tree which follows a specific structure. The bundle contains all resources for the given executable/library, unlike on Linux where resources are stored elsewhere (e.g. in `/usr/share`).
* With Apple’s compilers, you can easily link to a framework with the -framework switch.


## Other Responsibilities

Darling’s dynamic loader (dyld and libdyld) is also responsible for the following tasks:

* [Processing exception handling sections](/developer-zone/exception-handling/).
* Supporting Thread-Local Storage (TLS).

