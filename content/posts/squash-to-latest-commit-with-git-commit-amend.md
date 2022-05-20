---
title: "Squash to latest commit with `git commit --amend`"
date: 2022-05-21T00:08:05+08:00
---
On my local Git branch, I make code changes and commit them.

Then, I want to make some more additional changes and combine them into that previous commit.

What I would usually do:

```
... make the initial change ....
$ git add <file>
$ git commit

... then, make additional changes ...
$ git add <file>
$ git commit --fixup=HEAD
$ git rebase -i HEAD^^

# On the text editor (using vim) that shows up
# after the last command, type `:x`.
```

That squashes the commit containing the additional changes into the first commit.

But, it can be quicker. We don't have to create a fixup commit, perform an interactive rebase, and finally confirm the fixup on the editor.

We can immediately perform squash when creating a commit:

```
... make the initial change ....
$ git add <file>
$ git commit

... then, make additional changes ...
$ git add <file>
$ git commit --amend --no-edit
```

And it's done!

Feel free to add `commit --amend --no-edit` to your Git alias for a much quicker invocation.
