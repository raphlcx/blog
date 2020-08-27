---
title: "JavaScript module import hoisting"
date: 2020-08-27T22:05:36+08:00
tags: [javascript,typescript]
---
At my current workplace, I work as a security engineer. Some of my day-to-day tasks involve submitting security patches for the software engineering teams. Over the past couple of years, the teams have written and deployed many different microservices, some baked in Java, some in Go, and some in TypeScript.

While working on one of our TypeScript projects, I discovered the concept of import hoisting, and wondered why it did not work [as expected and officially documented](https://exploringjs.com/es6/ch_modules.html#_imports-are-hoisted). That drove me curious, so I started trying out different ways to import modules in TypeScript, which eventually led to an Eureka moment.

In this post, I want to share my experience and journey of discovery.

## The tooling

Creat a new empty directory, and install the necessary tooling:

```
mkdir testing && cd testing
npm init
npm i ts-node tsc typescript
```

For my setup, these are the runtime and library versions:

```
node v12.18.3
npm v6.14.6
ts-node v9.0.0
tsc v1.20150623.0
typescript v4.0.2
```

## Does TypeScript hoist import?

In order for a file to be detected as a module, each file should have at least one `import` or `export` statement. Create these three files accordingly:

```
// index.ts
console.log(A);
console.log(B);
console.log('c');
import {A} from './a';
import {B] from './b';
```

```
// a.ts
console.log('a');
export const A = 'export A';
```

```
// b.ts
console.log('b');
export const B = 'export B';
```

Execute the main file:

```
npx ts-node main.ts
```

It prints out an error:

```
Cannot read property 'A' of undefined
```

What?

Shouldn't the `import` statement be hoisted to the top of the file in `index.ts`?

So I ran `tsc` to transpile the file to JavaScript, to inspect what actually executes:

```
npx tsc index.ts
```

Attempting to execute the transpilation result:

```
node index.js
```

Yields the same error:

```
TypeError: Cannot read property 'A' of undefined
```

So yeap, it is giving the same error as the one where we execute using `ts-node`. Let's inspect `index.js`:

```
// index.js, transpiled from index.ts using tsc
"use strict";
exports.__esModule = true;
console.log(a_1.A);
console.log(b_1.B);
console.log('c');
var a_1 = require("./a");
var b_1 = require("./b");
```

Here we see `tsc` merely converts the `import` statement into `var ... = require(...)`, and when `var` is hoisted, only the declaration is hoisted to the top, while the actual `require()` method is not executed. Essentially this is how it looks like after hoisting:

```
// index.js, after hoisting
"use strict";
var a_1;
var b_1;
exports.__esModule = true;
console.log(a_1.A);
console.log(b_1.B);
console.log('c');
require("./a");
require("./b");
```

After declaration, both `a_1` and `b_1` are still `undefined`, and that is why we get the error `Cannot read property 'A' of undefined`.

So at this point of time, TypeScript doesn't compile to ES6 modules by default.

## But does native ES6 module really hoist import?

Similarly to the experiment in TypeScript, we create three ES6 modules, adapting from the files in the previous experiment:

```
// index.mjs
console.log(A);
console.log(B);
console.log('c');
import {A} from './a.mjs';
import {B] from './b.mjs';
```

Re-use `a` and `b` modules:

```
cp a.ts a.mjs
cp b.ts b.mjs
```

Let's run them:

```
node index.mjs
```

Result:

```
(node:2210) ExperimentalWarning: The ESM module loader is experimental.
a
b
export A
export B
c
```

It works, ES6 modules are hoisted! It hoists and executes the two `import` statements first, which executes the `console.log` from both of the imported modules, printing 'a' and 'b' respectively. Then, the rest of the `console.log` in the index file are executed, printing the value of the constant `A` and `B`, and finally the literal string 'c'.

## Wrapping up

So yes, ES6 modules are indeed hoisted. For TypeScript, by default it does not seem to transpile `import` statement to native ES6 import, but maybe there is an option flag to trigger it to do so ...
