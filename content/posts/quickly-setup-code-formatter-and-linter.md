---
title: "Quickly setup code formatter and linter"
date: 2025-03-01T11:52:36+08:00
---

*Last updated: June 2023*

Setting up a code linter and formatter for every new project can be a hassle. If you'd rather not spend time configuring these tools repeatedly, this guide is for you.

## Go

Navigate to the project's root directory and run:

```
go fmt ./...
```

## JavaScript

Install ESLint and Prettier:

```
npm install --save-dev --save-exact eslint eslint-config-prettier prettier
```

Create an ESLint flat config file, `eslint.config.js`, in the project root with the following content:

```
import js from "@eslint/js";
import prettier from "eslint-config-prettier";

export default [js.configs.recommended, prettier];
```

To lint your code, use:

```
eslint <dir>
```

To format your code, run:

```
prettier --write <dir>
```

## Python 3

Install Pylint and Black:

```
pip3 install pylint black
```

To lint your code, run:

```
python3 -m pylint <dir>
```

To format your code, use:

```
python3 -m black <dir>
```
