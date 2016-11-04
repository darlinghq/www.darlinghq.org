---
title:	"Low Hanging Fruit"
---
# Low Hanging Fruit

Darling still lacks some [OS X system calls](http://opensource.apple.com//source/xnu/xnu-1504.3.12/bsd/kern/syscalls.master). Some are easy to implement, some are a lot more complicated.

Darling's implementation of OS X system calls for Linux resides in [src/kernel/emulation/linux](https://github.com/darlinghq/darling/tree/master/src/kernel/emulation/linux). You can see which ones are completely missing by looking into [syscalls.c](https://github.com/darlinghq/darling/blob/master/src/kernel/emulation/linux/syscalls.c). You can also look for TODOs in existing implementations.

## CoreCrypto

Darling desperately needs a [reimplementation](https://github.com/darlinghq/darling-corecrypto) of Apple's CoreCrypto. This library is not open source, although its source code can be [downloaded from Apple](https://developer.apple.com/security/) (see at the bottom). CoreCrypto is needed for any recent version of CommonCrypto and for the Security.framework (which is needed for CMake, codesign etc.).


