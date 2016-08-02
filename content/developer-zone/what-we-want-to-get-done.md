---
title:	"What We Want to Get Done"
---
# What We Want to Get Done

## Next Up

* Mach port set support in the kernel module (needed for launchd).
* Launchd running and working; includes automatically starting launchd for every prefix used.
* Security framework building and working (needed for CMake under Darling).

## High-Level Milestones

This page serves as a list of ideas where to start hacking in Darling.

* [Full BSD system call support](http://www.opensource.apple.com/source/xnu/xnu-2782.20.48/bsd/kern/syscalls.master).
* Improve the darling-mach kernel module’s support for Mach IPC.
* Core Audio implementation. The most critical areas are the CoreAudio, AudioToolbox, AudioUnit frameworks. The initial aim is to just have the sound working. It is not necessary to have a full implementation with all the magic it can do on OS X. [mpg123 for OS X](http://rudix.org/packages/mpg123.html) can be used for testing.
* Input device support in IOKit for “direct input” support in games.
* CoreVideo and OpenGL framework implementation to get OpenGL apps working.
* Extend the current CoreServices implementation. For example to cover more File Manager functionality etc.
* Anything else from the [issue tracker](https://github.com/darlinghq/darling/issues)…

Since the most painful area is still the missing Cocoa implementation, hackers are welcome to explore the following possibility:

* Implement AppKit from scratch on top of an existing Linux GUI framework (prefferably Qt 5) with OS X ABI compatibility as the foremost aim.
 * Integrate Qt into NSRunLoop via QAbstractEventDispatcher. QtMotifExtension serves as an example of how this can be achieved.
 * Implement NSApplication.
 * Analyze Core Animation to see how to fit it into Qt. MUST be done before the following.
 * Implement the fundamental classes: NSEvent, NSView, NSWindow, NSCell, NSControl, …
 * Implement NS* widget classes in Qt Quick and QML.
* Implement Carbon basics around Qt as well. Why? Because most “big” games for OS X still use Carbon.

