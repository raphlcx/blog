---
title: "Ordering HTTP mirror in Maven 3.8.x"
date: 2022-01-28T12:12:16+08:00
---
Apache Maven version 3.8.1, or higher, disables the use of insure HTTP URI scheme for repositories.

The recommendation is to adopt HTTPS, and to use HTTP mirrors for repositories that do not support HTTPS (seriously, in 2022?).

My `~/.m2/settings.xml` contains a repository configuration that points to an internal repository. It also has a mirror to `miredot` repository, which does not have HTTPS available:

```
  <servers>
    <server>
      <id>repo.internal</id>
      <username>...</username>
      <password>...</password>
    </server>
  </servers>

  <mirrors>
    <mirror>
      <id>miredot.mirror</id>
      <url>http://nexus.qmino.com/content/repositories/miredot</url>
      <mirrorOf>miredot</mirrorOf>
    </mirror>
  </mirrors>

  <profiles>
    <profile>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <repositories>
        <repository>
          <id>repo.internal</id>
          <url>https://...</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>false</enabled>
          </snapshots>
        </repository>
      </repositories>
    </profile>
  </profiles>
```

However, with this setup, Maven will first consult `miredot.mirror` even when installing internal artefacts, leading to a redundant round-trip to miredot every time.

The workaround is to create a mirror as well for the internal repository, and setting it as the first mirror in the mirror list, example:

```
  <servers>
    <server>
      <!-- repo.internal credentials
      ...
      -->
    </server>
    <server>
      <id>repo.internal.mirror</id>
      <username>...</username>
      <password>...</password>
    </server>
  </servers>

  <mirrors>
    <mirror>
      <id>repo.internal.mirror</id>
      <url>https://...</url>
      <mirrorOf>repo.internal</mirrorOf>
    </mirror>
    <mirror>
      <!-- miredot mirror
      ...
      -->
    </mirror>
  </mirrors>

```

Now, Maven will first consult the internal repository with higher priority, falling back to miredot only if the artefact is not found within the internal repository.
