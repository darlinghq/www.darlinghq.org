---
title: "Exception Handling"
---
# Exception Handling

Darling bundles Apple's library for stack unwinding (which is a crucial step in exception handling) named libunwind. Darling provides support in finding the appropriate EH sections in Mach-O and ELF objects.

Mach-O libraries contain both traditional `__eh_data` sections (which have a format nearly identical to `.eh_frame` sections in ELF) and and the compact `__unwind_info` sections. The fact that the former is still supported means we can adapt EH information from Darling's ELF libraries and use them with Apple's libunwind as well.

* On x86\_64, there is no difference between unwind information in ELF and Mach-O with the exception that ELF contains an additional terminating null entry.
* On i386 though, Apple’s engineers have (accidentally?) swapped the DWARF codes for ESP and EBP registers. This must be [fixed](https://github.com/LubosD/darling/tree/master/src/libdyld/eh) before passing the data to libunwind.

## Objective-C Exceptions

Objective-C exceptions on OS X/x86-64 (and iOS/arm for that matter) use the same zero-cost exception handling mechanism as C++ exceptions. You may find more information about this in [LLVM’s documentation](http://llvm.org/docs/ExceptionHandling.html#itanium-abi-zero-cost-exception-handling). This also means that ObjC exceptions can be caught by handlers in C++ and vice versa.

The Objective-C runtime on OS X/i386 uses setjmp/longjmp exceptions (see [LLVM’s documentation](http://llvm.org/docs/ExceptionHandling.html#setjmp-longjmp-exception-handling)). This requires Darling’s ObjC support code to implement try-catch-finally block registration and handling as well.

