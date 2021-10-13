---
title: "Complementing Vim with Visual Studio Code"
date: 2020-12-19T16:16:53+08:00
---
I use Vim as my regular text editor. I like to keep its configuration to be as succinct as possible, preferring to use it without adding in too much of my own flavour in it. It helps to train my muscle memories, so I could SSH into any newly setup box at any time, and get productive with bare Vim as it is.

But working with new programming languages and type-heavy languages like Java isn't as pleasant in Vim.

Side tracking for a little bit, let's talk about auto completion. Auto completion from the editor helps to reduce typing, but what I learn from auto completion, is that it is not just a feature to reduce typing, but also providing instant feedbacks to the developers whether we are using the right API, whether the functions or the methods that we are calling are indeed provided by the object type of the underlying variable where the editor cursor is right on. This aid from the editor is immensely helpful, especially for us to build and gain confidence while learning a new programming language.

For type-heavy language like Java, I'm not a seasoned Java developer, so chances are, I would not know what is entire package name of most libraries, and most of them are pretty long. This is where editor can help. Finding the right package that is currently in use, and importing them if they are missing in the code.

At first I turned to using IntelliJ IDEA for Java. The IDE helped a lot in just getting the things that I need done. But soon, as I started to experiment with more programming languages, like Go and Rust, I'm missing the editor aid that I need in getting started with them. JetBrains did released GoLand for the Go language, but it was only for a free trial.

After some pondering, turning to use JetBrains products as my everyday tooling isn't something that I would want too. I like the fact that there is a huge company behind these tooling that offer great product support, but at the same time, I want something that is not as GUI-intensive.

I have always known that there is Visual Studio Code. Granted, it is yet another GUI tool, but it comes with first-class support for Language Server Protocol (LSP), and extensions that supports LSP for most of the major programming languages - Java, Go, TypeScript, and more. I was reluctant to pick this tool up again, after my initial switch from it to Vim.

Idealism to stick to only using Vim is beautiful, but it isn't pragmatic. I ended up wiring up my Vim with [`coc.nvim`](https://github.com/neoclide/coc.nvim) plugin, and making it work like an IDE that supports LSP. It isn't perfect though. Java extension for `coc.nvim` takes a couple of seconds to start without any indicator of startup progress. Go extension behaves weirdly, most of the time I had to execute `:e!` on the current buffer to get it behave correctly again. As of commit `23c531e633421ebbe4a5372a770248f7c2d96d90` in `coc.nvim`, it still recommends to turn off backup in Vim, with is a trade-off for availability.

And that is when I decided to adopt Visual Studio Code. Not as a regular editor, but one to complement my Vim usage. Visual Studio Code helps immensely in learning a new programming language, and making them much more friendlier to work with. I configured the keymaps and editing behaviour on Visual Studio Code to be as close as to Vim as possible, even [porting the colour scheme](https://github.com/raphlcx/modest-vscode/) that [I use in Vim](https://github.com/raphlcx/modest) to Visual Studio Code, and [publishing it to Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=raphx-3269.modest).

So for most text editing, I still use Vim. But when it comes to working with new programming languages and getting familiar with them, I benefit from the editor features of Visual Studio Code.
