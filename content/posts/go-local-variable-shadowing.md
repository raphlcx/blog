---
title: "Go local variable shadowing"
date: 2020-12-12T16:11:47+08:00
---
I encountered a weird case when using the `init()` function in Go.

Consider this case:

```
package main

import "fmt"

var a int

func doSomething() (int, error) {
	return 10, nil
}

func init() {
	a = 1

	a, err := doSomething()

	if err != nil {
		panic(err)
	}

	fmt.Printf("In init(), a is %d\n", a)
}

func main() {
	fmt.Printf("In main(), a is %d\n", a)
	return
}
```

When we execute this, it prints out:

```
In init(), a is 10
In main(), a is 1
```

Weird, the `init()` function does set the variable `a` with the value 10.

But if we don't use short assignment in the `init()` function, and do this instead:

```
func init() {
	a = 1

	var err error
	a, err = doSomething()

	(the rest of the code ...)
```

This would print as what we expect:

```
In init(), a is 10
In main(), a is 10
```

Using short assignment will declare a new local variable `a`, and this shadows the global variable `a`, so when we update `a`, we are actually updating the local variant, not the global one. :)
