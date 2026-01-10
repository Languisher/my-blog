---
title: 无 sudo 权限时安装二进制包：Conda
date: 2026-01-08
description: ""
tags:
  - shell
image: ""
imageAlt: ""
imageOG: false
hideCoverImage: false
hideTOC: false
targetKeyword: ""
draft: false
---
Motivation.

In many cases, we would like to install software packages of our own choosing on company or university servers. Most such servers today are based on Debian-derived Linux distributions, where `apt` is the only available package manager. Like many system package managers, apt requires **sudo** privileges.


However, it is often inconvenient—or simply unreasonable—to ask system administrators to install niche tools that are only needed by a single user. A typical example is `zsh`: despite being a common shell, it is not installed by default on many servers. In such situations, the only remaining option seems to be compiling the software from source.

An alternative approach is to use conda. Although conda is primarily known as a package and environment manager for Python, it can also manage general-purpose binaries. In particular, **conda-forge** is a community-maintained channel that provides a wide range of precompiled packages.

```sh
conda install conda-forge::<package_name> # or
conda install -c conda-forge package_name

# Example
conda install -c conda-forge zsh
```

## References

[Miniconda：无需 sudo 权限的软件包管理工具](https://www.zhihu.com/people/pan-wen-bo-71)](https://zhuanlan.zhihu.com/p/688624885)