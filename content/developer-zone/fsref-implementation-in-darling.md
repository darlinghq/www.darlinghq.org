---
title: "FSRef Implementation in Darling"
---
# FSRef Implementation in Darling

`FSRef` is an opaque data type that uniquely identifies a file in OS X. The problem with `FSRef` is that it can be freely copied and doesn’t need to be allocated or freed. That means the opaque structure may not contain any dynamically allocated memory.

The size of `FSRef` is 80 bytes, which means it cannot contain the path to the file. How does Darling deal with this problem? Inode numbers.

In Darling, FSRef is an array of `ino_t` values. With these inode values, Darling can reconstruct the path to the file. Darling is indeed limited by a maximum depth it can represent this way.

```
struct FSRef
{
        union
        {
                uint8_t hidden[80];

                // Inode numbers leading to the file in the directory structure
                //
                // EXAMPLES:
                // All zeroes: /
                // sequence 1,2,0: [dir with inode 1]/[dir or file with inode 2]
                ino_t inodes[FSRef_MAX_DEPTH];
        };
};
```

In future, it would be better to use ULEB128-encoded inode numbers to represent the path, since inode numbers are often relatively small. This way they could occupy less than `sizeof(ino_t)`.


