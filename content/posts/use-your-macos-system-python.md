---
title: "Use your macOS system Python"
date: 2022-05-19T23:01:33+08:00
---
Installing and managing Python versions in macOS and perhaps other OSes can be as convoluted as we want them to be.

You might have come across this [xkcd on Python environment](https://xkcd.com/1987/) and blog posts that dissuade you from using your system Python. Use virtualenv, pyenv, asdf, or [insert some other tool] instead.

Officially, macOS Monterey 12.3 has stopped bundling any version of Python, so there isn't a system Python anymore. But, when installing Xcode Command Line Tools (CLT) or Xcode, it comes with Python 3. I'll refer to this as the "system" Python.

If you are using Homebrew, you would have installed Xcode CLT any way, and with it, Python 3. If your use case is simple, you don't have to install yet another Python after that.

The first change to work well with macOS system Python is setting pip to install in user mode by default.

```
; $HOME/.config/pip/pip.conf

[install]
user = true
```

Now, when you run `pip3 install <some-pkg>`, it will imply a user-mode install and place the installed packages in your user site directory, which defaults to `$HOME/Library/Python/X.Y/lib/python/site-packages`.

Without the pip configuration, you would have to run `pip3 install` with `sudo` so it could place the installed packages in the system Python directory, which you should NEVER do. You do not need `sudo` to install any Python packages.

With the pip configuration, when you run `pip3 list`, it lists all the system packages and the packages you have installed. It only applies user mode when performing package installation.

You can still use virtualenv. First, install virtualenv using pip. Then, ensure `$HOME/Library/Python/X.Y/bin` is in your shell PATH. Restart your shell, and you can run `virtualenv` command right after that.

That is all you need to work with the macOS system Python.

Even if you install an additional Python, either explicitly from Homebrew or accidentally pulled as part of another Homebrew package's dependency, the same pip configuration can still help you.

Since `pip3 install` will place installed packages in the user site directory, it will not tamper with Homebrew's `/usr/local/lib/pythonX.Y/site-packages` directory. `/usr/local/` is user-writable, so even without `sudo`, you can still install packages into this directory, inadvertently or not.

In a nutshell, use your system Python when that is all you need. Always prepare a pip configuration to instruct pip to install in user mode. It allows pip to install your packages to the user site directory deterministically. You will never have to mess with any system directories nor use `sudo` anywhere when installing Python packages.
