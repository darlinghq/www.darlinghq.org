---
title:	"Implementing New Frameworks from Scratch"
---
# Implementing New Frameworks from Scratch

How to start hacking on a new framework in Darling?

1. Create a new directory in `darling/src` with the name of the framework (e.g. SuperFramework). Alternatively, for large frameworks, create a new Git repository and create your framework into a submodule (in `darling/src/external`).
2. Add `add_subdirectory(src/SuperFramework)` into toplevel `CMakeLists.txt` (in `src`).
3. Create a `CMakeLists.txt` file in `darling/src/SuperFramework`. See `CMakeLists.txt` files in other directories for an inspiration. The aim is to have the library called `libSuperFramework.so` and installed into `$prefix/lib/darling`.
4. Add a framework entry into `darling/etc/dylib.conf` so that Darling knows that `libSuperFramework.so` corresponds to `SuperFramework.framework`.

A few things to remember:

* Note that the language of choice for Darling is C++. Remember to use `extern "C" { ... }` as needed.
* It is not required to follow the same header file hierarchy as on OS X. Darling doesn’t need that.
* It is not OK to simply copy over header files from OS X. Some parts of them may be copyrightable.

