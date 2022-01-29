---
title: "Complementing Vim with Visual Studio Code"
date: 2020-12-19T16:16:53+08:00
---
I use Vim as my go-to text editor, keeping its configuration minimal without adding too much personal flavour. It helps to train my muscle memories, so I can SSH into any newly set up box and get productive with bare Vim as it is.

But working with new, unfamiliar programming languages and type-heavy languages like Java isn't as pleasant in Vim.

Let's first talk about auto-completion. It is a feature present in most integrated development environments (IDE). Auto-completion provides instant feedback to the developers on library function signature, object member variables, and of course, to reduce the number of keystrokes. It helps us tremendously in building confidence while learning a new programming language.

I am not a seasoned Java developer. When it comes to importing a package in my Java code, chances are, I would not know the exact package name for it. An IDE can help by analysing the currently in-use package and importing them if they are missing in the imports.

At first, I turned to using JetBrains IntelliJ IDEA for Java. It is an excellent IDE for the Java programming language. But when it comes to writing in Go or Rust programming language, you'll need complementary IDE plugins to support that. JetBrains has GoLand for the Go programming language but is only available for a free trial.

Using JetBrains's feature-rich and complex products as my go-to tooling isn't what I would want. I like the fact that there is a team behind these products that offer product support, but at the same time, I want something that is not as GUI-intensive, something that is simpler for my light use case.

In the modern world of text editors, Visual Studio Code stood out as the most popular among them all. Granted, it is yet another GUI tool. It comes with first-class support for Language Server Protocol (LSP) and numerous extensions that support LSP for programming languages such as Java, Go, TypeScript, and more. I had used Visual Studio Code in the past as my primary text editor and was reluctant to pick it up again after my initial switch from it to Vim.

Idealism to stick to only using Vim is beautiful, but it isn't pragmatic. I ended up wiring up my Vim with [`coc.nvim`](https://github.com/neoclide/coc.nvim) plugin and making it work like an IDE that supports LSP. Although, it isn't perfect. Java extension for `coc.nvim` takes a couple of seconds to start without any indicator of startup progress. Go extension behaves weirdly. I had to execute `:e!` on the current buffer frequently to resolve its erratic behaviour. As of commit `23c531e633421ebbe4a5372a770248f7c2d96d90` in `coc.nvim`, it still recommends turning off backup in Vim, which is a trade-off for availability.

And that was when I decided to adopt Visual Studio Code again. Not as a regular editor, but one to complement my Vim usage. I configured the keymaps and editing behaviour on Visual Studio Code to be as close as to Vim as possible, even [porting the colour scheme](https://github.com/raphlcx/modest-vscode/) that [I use in Vim](https://github.com/raphlcx/modest) to Visual Studio Code and [publishing it to the Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=raphx-3269.modest).

For most simple text editing, I still use Vim. But when it comes to working with new programming languages and getting familiar with them, I benefit from the editor features of Visual Studio Code.
