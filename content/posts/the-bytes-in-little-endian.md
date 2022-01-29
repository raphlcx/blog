---
title: "The bytes in little endian"
date: 2020-12-31T17:51:09+08:00
---
I have always had this confusion about little-endian. Some say it reverses the character order when read from left to right. While playing some CTF challenges, the payload to trigger a stack overflow is always in the little-endian format, so they fit into the memory in that format.

But what do the bytes look like in the memory itself?

Say I have an ASCII string `"1234"`, in hexadecimal this translates to four bytes, `0x31 0x32 0x33 0x34`. Does little-endian mean reversing everything? Is this string stored as `0x34 0x33 0x32 0x31` in the memory? Or is it a reversing on the nibble level, i.e. `0x13 0x23 0x33 0x43`? Or is it reversing on the bit level, i.e. flipping around all the bits in each byte, so `0x31` (`0b00011111`) will be `0xf8` (`0b11111000`)?

How do you little-endian them?

## How it all works

To begin, let us understand what a memory is. Logically speaking, memory is a long list of empty slots for storing data. Each slot has a label. We call these label memory addresses or memory locations. A memory address holds a single byte. When we visualise it this way, a memory looks like this:

```
|  0   |  1   |  2   |
----------------------
| 0xad | 0xfe | 0x5b |
```

Memory address 0 stores the byte `0xad`, address 1 stores the byte `0xfe`, and lastly, address 2 stores the last byte, `0x5b`.

Address 0 is lower, while address 2 is higher.

And that is it. Memory itself does not know whether a byte is the first byte of a data, the last byte, nor which part of the data it represents. It is simply storage.

Now on to endianness. If every data is a single byte, our journey in understanding endianness would already end because there is simply no need for endianness. Endianness only comes into play when we store data that consist of more than a single byte. Take the `int` type, which contains 4 bytes. So when storing a decimal value of 58 (`0x3a`) in this data type, it is stored as 4 bytes, padded with zeros, i.e. `0x00 0x00 0x00 0x3a`. In the memory in the little-endian format:

```
|  0   |  1   |  2   |  3   |
-----------------------------
| 0x3a | 0x00 | 0x00 | 0x00 |
```

Yes, you read that right, `0x3a` comes first. It is where little-endian comes in.

An `int` has 4 bytes, and when storing data with more than a single byte, the bytes are reversed, with the least significant byte (the byte that comes last when we write the representation out on a paper from left-to-right) stored on the lower address. So in this example, `0x00 0x00 0x00 0x3a` will be reversed, and stored as `0x3a 0x00 0x00 0x00` in the memory.

What if we have multiple variables of different types?

```
int a = 58;
short b = 283;
```

`short` has 2 bytes. It has more than a single byte, so little-endian applies too. A decimal value of 283 in hexadecimal is `0x01 0x1b`. The layout in memory:

```
|  0   |  1   |  2   |  3   |  4   |  5   |
-------------------------------------------
| 0x3a | 0x00 | 0x00 | 0x00 | 0x1b | 0x01 |
```

From address 0 to address 3, we still store `int a` first, but with its bytes reversed.

From address 4 to address 5, we then store `short b`, with its bytes reversed.

For an array of `short`:

```
short arr[2] = {283, 1044};
```

Decimal 1044 is `0x04 0x14`. It would look like this:

```
|  0   |  1   |  2   |  3   |
-----------------------------
| 0x1b | 0x01 | 0x14 | 0x04 |
```

Decimal 283 gets stored first from address 0 to 1. Bytes reversed.

Decimal 1044 gets stored later from address 2 to 3. Bytes reversed too.

And, that is little-endian in a nutshell. If you have multiple bytes in the data, reverse them. Otherwise, if it is only a single byte, directly fit them in the memory address. We can formulate a rule for when to apply little-endian when phrased this way:

  1. If data is a single byte, store it as it is.
  1. If data has multiple bytes, reverse the bytes, then store them with the least significant byte in the lower address.

When I say "reverse the bytes", this only applies to LTR readers because we would write decimal 58 as `0x00 0x00 0x00 0x3a` in 4-bytes hexadecimal. If you are an RTL reader, you would have written decimal 58 as `0x3a 0x00 0x00 0x00`, so you will not perceive it as reversed.

Coming back to the initial string of `"1234"`, we should now know how they layout in the memory. Each ASCII character is a single byte, so we store them as it is:

```
|  0   |  1   |  2   |  3   |
-----------------------------
| 0x31 | 0x32 | 0x33 | 0x34 |
```

We never reverse the nibbles nor the bits themselves. Little-endian applies on the byte level only.

## On the visual stack

Sometimes we do see memory stack visualised in this way:

```
0  | 0xa1b2c3d4 |
4  | 0x21314151 |
8  | 0xaabbccdd |
12 | ...        |
```

_Usually_, this means storing 4 bytes of aligned data in this manner:

  - From address 0 to address 3, the hexadecimal value `0xa1b2c3d4` is stored.
  - From address 4 to address 7, the hexadecimal value `0x21314151` is stored.
  - From address 8 to address 11, the hexadecimal value `0xaabbccdd` is stored.

I emphasised "usually", because we are not sure if `0xa1 0xb2` and `0xc3 0xd4` are two different `short` data types, but for this example, imagine a 4-bytes data in each of the rows itself.

We are smart now. We know that 4 bytes are more than a single byte. So we reverse the bytes first, then store them in the memory:

```
|  0   |  1   |  2   |  3   |
-----------------------------
| 0xd4 | 0xc3 | 0xb2 | 0xa1 |

|  4   |  5   |  6   |  7   |
-----------------------------
| 0x51 | 0x41 | 0x31 | 0x21 |

|  8   |  9   |  10  |  11  |
-----------------------------
| 0xdd | 0xcc | 0xbb | 0xaa |
```

Any little-endian machine would know how to read `0xa1b2c3d4` from address 3 to address 0.

## 32-bit vs 64-bit

Are there any differences in how little-endian works on a 32-bit and 64-bit machine?

Short answer: no.

32/64 bit is a CPU architecture. It is a CPU feature and is not concerned with how memory works. But we often hear, "Yea, with a 64-bit machine, you can have more memory". That is true, but it does not affect how programs stores bytes in the memory, nor does it affect how little-endian works in storing the bytes. The same rule of a single byte and multiple bytes still apply.

One of the main differences in 32/64 bit is the size of the CPU registers. Each register is also an empty slot to store data, but it offers faster access when compared to memory. Each CPU register also has a larger size (32 bits or 64 bits) when compared to a single memory address (8 bits).

A register stores memory addresses as well. A 32-bit register can store memory address ranging from 0 to 2^32 - 1 (4,294,967,295). That translates to 4 GiB of memory or computer RAM. If there is more RAM than 4 GiB and we attempt to store memory address with a value more than 4,294,967,295 in the register, which is beyond the upper limit, the CPU has no (direct) way of accessing this. The reason is that it is beyond 32 bits to resolve that memory address.

A 64-bit register has 64 bits, therefore capable of storing memory addresses ranging from 0 to 2^64 - 1 (18,446,744,073,709,551,615), which is a large 16 EiB of memory address range that the computer can address.

So, a 32/64 bit CPU architecture only affects how a CPU references memory addresses, not the storage of the bytes on those memory addresses.

## Conclusion

That sums up little endianness on computer memory. So far, we have covered:

  - What a memory address is
  - How different data types are stored in the memory
  - A general rule on when little-endian applies
  - Interpreting visualisation of stack
  - Effects of 32/64 bit CPU architecture on little-endian

I hope this article has helped you in some way!
