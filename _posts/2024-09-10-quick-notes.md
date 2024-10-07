---
layout: post
title: Notes on tools I often use
lead: A quick reference to tools I often use and other tips that I want to keep note of.
---

# Quick notes and tools I often use

I intend to keep this page as a quick reference to tools I often use and other tips that I find useful. I'll keep updating it as I go along.

## Execute commands on multiple servers at on go

  This is only one use case for [xpanes](https://github.com/greymd/tmux-xpanes). Checkout the documentation for more use cases.

  ```bash
  xpanes -t -c "ssh root@{}" server1 server2 server3
  ```

  Where server1, server2, server3 are the IP Addresses of servers you want to connect to.

  If you often connect to the same servers, you can add them to your `~/.ssh/config` file and use the server names instead of IP Addresses as shown below:

  ```bash
  cat ~/.ssh/config | awk '$1=="Host"{print $2}' | xpanes ssh
  ```
  Refer to: [xpanes documentation](https://github.com/greymd/tmux-xpanes?tab=readme-ov-file#connecting-to-multiple-hosts-given-by-sshconfig)

## Docker
### Choosing a base image

When creating a Dockerfile, you need to choose a base image. There are many base images available on Docker Hub. Alpine, Slim, Stretch, Buster, Jessie, Bullseye, Bookworm... Which one should I choose? 🤔

  - [Alpine](https://hub.docker.com/_/alpine): A lightweight Linux distribution based on musl libc and BusyBox. It's a great choice for containers where size matters.
  - [Debian](https://hub.docker.com/_/debian): A stable and popular Linux distribution. It's a good choice for containers where stability matters.
    - Jessie, Stretch, Buster, Bullseye, Bookworm are different versions of Debian in the mentioned order. If you're not sure which one to choose, go with Bookworm, which is
    the latest stable release. You can find more information about Bookworm update in the release notes, and press release.
      - Release Notes: [Debian Releases](https://www.debian.org/releases/bookworm/releasenotes).
      - Press Release: [Debian Bookworm Press Release](https://www.debian.org/News/2023/20230610)

  If you're not sure which one to choose, go with Alpine. It's lightweight and has a smaller attack surface.

### Convert Windows line endings (CRLF to LF)

If you have team mates working on Windows, first, ask them to configure their editor to use LF line endings. If that's not possible, you can use the following command to convert CRLF to LF in any bash script added to Docker:

Example: Convert line endings and make the script executable in a Dockerfile
```bash
RUN sed -i 's/\r$//g' /entrypoint.sh \
    && chmod +x /entrypoint.sh
```

## GIT
### Conventional commits
Reference: [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

Quick example:
```
TYPE: Short commit message (up to 50 chars)

Detailed commit description of the change (up to 100 chars),
optionally spanning multiple lines...

Mention breaking changes and reference tickets

For the value of TYPE, please use one of feat, fix, docs, style, refactor, perf & test
```

## Postgres
### JSONB vs JSON
> The json and jsonb data types accept almost identical sets of values as input. The major practical difference is one of efficiency. The json data type stores an exact copy of the input text,
which processing functions must reparse on each execution; while jsonb data is stored in a decomposed binary format that makes it slightly slower to input due to added conversion overhead, but
significantly faster to process, since no reparsing is needed. jsonb also supports indexing, which can be a significant advantage.

ref: https://www.postgresql.org/docs/current/datatype-json.html#DATATYPE-JSON

> In general, most applications should prefer to store JSON data as jsonb, unless there are quite specialized needs, such as legacy assumptions about ordering of object keys.
