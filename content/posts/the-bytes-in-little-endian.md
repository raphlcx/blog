---
title: "The bytes in little endian"
date: 2020-12-31T17:51:09+08:00
---
I have always had this confusion about little endian. Some says the bytes are ordered in such a way that it would be as if they are reversed when read from left-to-right. While playing some CTF challenges, the payload to trigger a stack overflow has to be arranged in the little endian format, so they could be fit into the memory as it is.

But how do the bytes actually look like in the memory itself?

Let's say I have an ASCII string `"1234"`, in hexadecimal this translates to four bytes, `0x31 0x32 0x33 0x34`. Does little endian means everything is reversed? Is this string stored as `0x34 0x33 0x32 0x31` in the memory? Or is it only reversed on the nibble level, i.e. `0x13 0x23 0x33 0x43`? Or is it reversed on the bit level, i.e. all the bits in each of the byte is reversed, so `0x31` (`0b00011111`) will be `0xf8` (`0b11111000`)?

How do you little endian them?

It is only recently that I got the aha moment to wrap my head around the concept, after spending more time reading low-level programming posts that touch a lot more on bytes and its alignment in memory. I want to share my learning so far, and hopefully it would help anyone who is reading this post.

## How it all works

To begin, let us understand what is a memory. Logically speaking, memory is a long list of empty slots in which we can store data in. Each slot is labelled with its address. We call them memory address, or memory location. On most consumer machines today, each memory address (each empty slot) is storing only a single byte. When we visualise it this way, memory would look like this:

```
|  0   |  1   |  2   |
----------------------
| 0xad | 0xfe | 0x5b |
```

Memory address 0 is storing the byte `0xad`, address 1 storing the byte `0xfe`, and address 2 storing the last byte, `0x5b`.

Address 0 is a lower address, while address 2 is a higher address.

And that's just it. Memory itself does not know whether a byte is the first byte of a data, the last byte, nor which part of the data that it represents. It is simply a storage.

Now on to endianness. If every data is a single byte, our journey in understanding endianness would be much more simpler, because there is simply no need for endianness. Endianness only comes in play when we store data that consist of more than a single bytes. Let's take the `int` type. It is represented by 4 bytes. So if we are storing a decimal value of 58 (`0x3a`) in this data type, it is actually stored as 4 bytes, padded with zeros, i.e. `0x00 0x00 0x00 0x3a`. When we store this in the memory, in little endian format:

```
|  0   |  1   |  2   |  3   |
-----------------------------
| 0x3a | 0x00 | 0x00 | 0x00 |
```

Yes, you read that right, `0x3a` comes first. This is where little endian comes in.

An `int` has 4 bytes, and when storing that has more than a single byte, the bytes are reversed, with the least significant byte (the byte that comes last when we write the representation out on a paper from left-to-right) stored on the lower address. So in this example, `0x00 0x00 0x00 0x3a` will be reversed, and stored as `0x3a 0x00 0x00 0x00` in the memory.

What if we have multiple variables of different types?

```
int a = 58;
short b = 283;
```

`short` has 2 bytes. It has more than a single byte, so little endian applies too. Decimal value of 283 in hexadecimal is `0x01 0x1b`. The layout in memory:

```
|  0   |  1   |  2   |  3   |  4   |  5   |
-------------------------------------------
| 0x3a | 0x00 | 0x00 | 0x00 | 0x1b | 0x01 |
```

From address 0 to address 3, we still store `int a` first, but with its bytes reversed.

From address 4 to address 5, we then store `short b`, with its bytes reversed.

In an array of `short`:

```
short arr[2] = {283, 1044};
```

Decimal 1044 is `0x04 0x14`. This would look like:

```
|  0   |  1   |  2   |  3   |
-----------------------------
| 0x1b | 0x01 | 0x14 | 0x04 |
```

Decimal 283 gets stored first from address 0 to 1, bytes reversed.

Then decimal 1044 gets stored later from address 2 to 3, bytes reversed as well.

And, that is little endian in a nutshell. If you have multiple bytes in the data, reverse them, otherwise if it is only a single byte, store them as it is in the memory address. We can formulate a rule for when to apply little endian when phrased this way:

  1. If data is a single byte, just store in as it is.
  1. If data has multiple bytes, reverse the bytes, then store them with the least significant byte in the lower address.

When I say "reverse the bytes", this only applies to LTR readers, because we would write decimal 58 as `0x00 0x00 0x00 0x3a` in a 4-bytes hexadecimal. If you are RTL reader, you would have already writen decimal 58 as `0x3a 0x00 0x00 0x00`, so you wouldn't need to reverse them anymore.

Coming back to the initial string of `"1234"`, we should now know how this is laid out in the memory. Each ASCII character is a single byte, so we simply store them as it is:

```
|  0   |  1   |  2   |  3   |
-----------------------------
| 0x31 | 0x32 | 0x33 | 0x34 |
```

We never reverse the nibbles nor the bits themselves, little endian applies on the byte level only.

## On the "visual" stack

Sometimes we do see memory stack visualised in this way:

```
0  | 0xa1b2c3d4 |
4  | 0x21314151 |
8  | 0xaabbccdd |
12 | ...        |
```

_Usually_, this means 4 bytes of aligned data are stored in this manner:

  - From address 0 to address 3, the hexadecimal value `0xa1b2c3d4` is stored.
  - From address 4 to address 7, the hexadecimal value `0x21314151` is stored.
  - From address 8 to address 11, the hexadecimal value `0xaabbccdd` is stored.

I emphasised "usually", because we are not sure if `0xa1 0xb2` and `0xc3 0xd4` are two different `short` data type, but for most of the time, when the stack is visualised this way, it usually means a 4-bytes data in each of the row itself.

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

Any little endian machine would know how to read `0xa1b2c3d4` from address 3 to address 0.

## 32-bit vs 64-bit

Is there any differences in how little endian works on a 32-bit and 64-bit machine?

Short answer: no.

32/64 bit is a CPU architecture, in another word, it is a "feature" of the CPU, and does not concern how memory works at all. But we often hear, "Yea with 64-bit machine you can have more memory". That is true, but it does not affect how the bytes are stored in the memory, nor does it affect how little endian works in storing the bytes. The same rule of single byte and multiple bytes still apply.

One of the main differences in 32/64 bit is the size of the CPU registers. Each register is also an empty slot where we can store data, but it offers faster access compared to memory, and each register has a larger size (32 bits or 64 bits) compared to a single memory address (8 bits).

Register is used to store memory address as well. A 32-bit register can store memory address ranging from 0 to 2^32 - 1 (4,294,967,295). That translates to 4 GiB of memory or computer RAM. Now if we were to have more RAM, and store data in a memory address beyond this upper limit, CPU has no (direct) way of accessing this, since it is beyond 32 bits to resolve that memory address.

A 64-bit register has 64 bits, so it could store memory address ranging from 0 to 2^64 - 1 (18,446,744,073,709,551,615), that is a huge 16 EiB of memory address range that the computer can address.

So, 32/64 bit is only affecting how a CPU references memory address, not how the bytes are stored on those memory addresses.

## Conclusion

That sums up little endianness on a machine's memory. So far we have covered:

  - What a memory address is
  - How different data types are stored in the memory
  - A general rule on when little endian applies
  - Interpreting visualisation of stack
  - Effects of 32/64 bit on little endian

Hope this article has helped you in some way!
