---
title: "SSH tunneling"
date: 2024-10-26T22:57:11+08:00
---

## Local tunnel

Command:

```
ssh -L 8888:example.com:80 tunnel-host -N
```

Diagram:

```
| You (ingress 8888) | --> | tunnel-host | --> | example.com (egress 80) |
```

Explanation: This setup allows you to execute curl localhost:8888 locally, sending the request to example.com on port 80 through tunnel-host.

## Remote tunnel back to local

Command:

```
ssh -R 9191:localhost:9090 tunnel-host -N
```

Diagram:

```
| You (egress 9090) | <-- | tunnel-host (ingress 9191) |
```

Explanation: This configuration enables tunnel-host to access your local machine on port 9090 through its port 9191.
