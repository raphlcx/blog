---
title: "How to manage SSH keys"
date: 2020-10-30T22:31:41+08:00
---
I'm jotting down some tips to manage SSH keys.

Always generate an SSH key with the default comment, which uses the format `[user]@[host]`. It contains information about the user and host of the key. When you add SSH keys from multiple hosts to GitHub, the default comments will help you identify which key belongs to which host easily on GitHub.

Generate a new key for each service, e.g. GitHub, Heroku. It ensures that if one key or one of the services is compromised, you contain the blast radius to that particular service.
