---
title: "Memory layout"
date: 2024-11-09T00:25:30+08:00
---

In a little-endian system, bytes are organized such that the most significant byte is at the higher address, while the least significant byte occupies the lower address.

Each address corresponds to a one-byte cell:

```
Offset  =>   0    1    2    3    4    5    6    7    8    9    10   11
Cell    => |    |    |    |    |    |    |    |    |    |    |    |    |
```

When storing `0xdeadbeef` into memory, the layout on a 64-bit machine appears as follows:

```
Offset  =>   0    1    2    3    4    5    6    7    8    9    10   11
Cell    => | ef | be | ad | de | 00 | 00 | 00 | 00 |    |    |    |    |
```

This representation makes little-endian format intuitive for those accustomed to reading right-to-left.

When an array is stored, it fills from lower to higher addresses. For instance, consider this C code:

```
int main() {
  char a[4];
  a[0] = 'a';
  a[1] = 'b';
  a[2] = 'c';
  a[3] = 'd';
  return 0;
}
```

Compiled on macOS 10.15.4 with:

```
Apple clang version 11.0.3 (clang-1103.0.32.29)
Target: x86_64-apple-darwin19.4.0
Thread model: posix
```

The generated assembly output:

```
100000f96: c7 45 fc 00 00 00 00         movl    $0, -4(%rbp)
100000f9d: c6 45 f8 61                  movb    $97, -8(%rbp)
100000fa1: c6 45 f9 62                  movb    $98, -7(%rbp)
100000fa5: c6 45 fa 63                  movb    $99, -6(%rbp)
100000fa9: c6 45 fb 64                  movb    $100, -5(%rbp)
```

Visually, the memory layout looks like this:

```
                                                            rbp ----
                                                                    |
                                                                    v
Offset  =>   0    1    2    3    4    5    6    7    8    9    10   11
Cell    => |    |    |    | a  | b  | c  | d  | \0 |    |    |    | xx |
```

To retrieve the value pointed to by RBP, data is accessed from [RBP+0] to [RBP+7]:

```
          rbp ----            start of return address ----
                  |                                       |
                  v                                       v
Offset  =>   0    1    2    3    4    5    6    7    8    9    10   11
Cell    => | 9a | 54 | 6a | 8c | da | d0 | 64 | 3c | e7 | 1f | ef | d8 |
```

If RSP is aligned with RBP, executing `POP RAX` loads RAX with the value `0xe73c64d0da8c6a54`.

It is important to note that the value that RBP holds is a memory address. In this case, its address is at offset 1, while the value it points to is `0xe73c64d0da8c6a54`.
