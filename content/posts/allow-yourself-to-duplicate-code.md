---
title: "Allow yourself to duplicate code"
date: 2024-05-05T14:11:43+08:00
---

## Scenario

Imagine you are writing a function that returns an object:

```
func do() {
  return {a: 1, b: 2, c: 3}
}
```

However, you noticed that there are three other similar functions across different packages that perform almost the same thing:

```
// package-a
func do() {
  return {a: 1}
}

// package-b
func do() {
  return {a: 1, b: 2}
}

// package-c
func do() {
  return {a: 1, b: 2, c: 3, d: 4}
}
```

As a responsible developer, you feel the urge to extract the common pieces, allowing all existing functions as well as your new function to reference that common piece.

After all, duplicating code is a practice that should be avoided whenever possible.

## Why we avoid duplicating code

The Don't Repeat Yourself (DRY) principle is a fundamental concept in reducing code duplication and information repetition. While there are technical reasons for practising DRY, there may also be psychological and social reasons to do so.

Here's my take.

During college, undergraduates often focus solely on getting a system or program to work, without much concern for code structure and modularity.

However, upon entering the workplace, there is a shift towards advocating for better code structure and hygiene, including reducing code duplication through the DRY principle. Practising DRY may be seen as a sign of technical competency. If seasoned developers are still duplicating code, they may be perceived as no better than fresh software engineering talents who have just joined the workforce.

This subconscious desire to appear technically competent to our peers could be a subconscious, yet major force driving us to apply the DRY principle.

## "Allow yourself" to duplicate code

The title of this post starts with "Allow yourself". If the reason for avoiding code duplication is psychological or social, it's not something that can be reasoned with rationally. Instead, it's an ingrained thought process that each developer must question.

Duplicating code might seem like something a rookie would do, but sometimes it is also the right thing to do. Consider the following technical benefits of duplicating code:

- **It allows repetition patterns to surface over time**, so we can create the right abstraction with a richer context.

- **It prevents overly generalised and complex code**. A code is complex when we do not know what it does, primarily because it does too many things. The new feature that you are implementing might not be needed any more in the next 3 months. If the commonly abstracted code is handling the use case of your feature, it may still be handling it even when the feature has been deprecated and removed. Over time, the abstraction grows in complexity as the use case that it handles grows as well.

- **It can lead to a more (subjectively) maintainable code base**. Imagine three duplicate blocks of code exist in three different packages in a single code base, with each duplicate handling a slightly different use case. During project maintenance phase, if one of those use cases needs amendment, you can confidently change the code for that specific use case without worrying that it will cause a chain reaction in other parts of code, as can happen if you were generalising and amending abstracted code.

Besides the DRY principle, there are other principles that suggest interesting alternatives, including WET (Write Every Time) and DAMP (Don't Abstract Methods Prematurely), although they may not receive as much spotlight as the DRY principle.
