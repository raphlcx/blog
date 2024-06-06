---
title: "Minimal and non-root user in container images"
date: 2024-06-06T22:51:51+08:00
---

This post provides a collection of Dockerfile/Containerfile templates for creating container images. These templates are designed to reduce footprint during system package installations and run using a non-root user during runtime.

## Alpine

```
FROM alpine

# No cache
RUN apk add --no-cache some_pkg

# Non-root user
ARG USER=appuser
RUN addgroup -S $USER && adduser -S -H -G $USER $USER
USER $USER

CMD ["/bin/ash"]
```

## Debian-based

```
FROM debian

# No cache
RUN apt-get update && apt-get install -y \
    some_pkg \
  && rm -rf /var/lib/apt/lists/*

# Non-root user
ARG USER=appuser
RUN groupadd --system $USER && useradd --system --no-create-home --gid $USER $USER
USER $USER

CMD ["/bin/bash"]
```

## RHEL-based

```
FROM fedora

# No cache
RUN dnf install -y \
    some_pkg \
  && rm -rf /var/cache/dnf

# Non-root user
ARG USER=appuser
RUN groupadd --system $USER && useradd --system --no-create-home --gid $USER $USER
USER $USER

CMD ["/bin/bash"]
```
