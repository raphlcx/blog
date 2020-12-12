---
title: "How to manage SSH keys"
date: 2020-10-30T22:31:41+08:00
---
Jotting down a simple way to manage SSH keys.

Always generate an SSH key with default comment, which uses the format `[user]@[host]`. This identifies which host the key is generated and used on, as well as the user who owns the key on that host. If there are two hosts, each having an SSH key, and both keys are added to, say, GitHub, we can easily identify on GitHub which key belongs to which host.

For each of the cloud services that you use, e.g. GitHub, Heroku, generate a new key for it. This ensures that if one key or one of the services is compromised, the blast radius is contained to that particular service.
