---
author: Nan Lin # should be replaced
pubDatetime: 2025-01-04T00:00:00Z # should be replaced
modDatetime: 2025-01-04T00:00:00.000Z
title: Docker Image Download Solution (CN) # should be replaced
slug: docker-registry # should be replaced
featured: false # check
draft: false # check
tags:
  - docker # should be replaced
description:
  解决 Docker Image 在国内无法下载的问题 # should be replaced
---
> Update: Nov. 17th, 2024

## Table of contents

Saturday, January 4, 2025


本文解决无法进行 Docker pull 或者 push 的问题。

## 一、查找可用的代理站点

在撰写本文的时刻，可以搜到由 Coderjia 整理的可用代理表格：

[目前国内可用Docker镜像源汇总（截至2024年11月）](https://www.coderjia.cn/archives/dba3f94c-a021-468a-8ac6-e840f85867ea)

我们以文中显示可用的 `docker.unsee.tech` 为例。

接下来会有两种操作，分为长久和临时的。

## 长久更改方式

### 二、修改指定代理文件

修改 `/etc/docker/daemon.json` 文件，加入该代理。在本文中即

```bash
{
    "registry-mirrors": [
        "<https://docker.unsee.tech>"
		]
}
```

### 三、重启 Docker

执行以下两行代码：

```bash
sudo systemctl daemon-reload 
sudo systemctl restart docker
```

完成此步之后应该能进行正常的 Docker Pull condo 操作。

## 临时更改方式

### 二、执行 pull 操作

运行下行代码：

```bash
# Replace <REGISTRY> and <REPO_NAME> with your choice
docker pull <REGISTRY>/<REPO_NAME>
# e.g.
# docker pull docker.unsee.tech/hello-world
```

### 三、添加新命名标识

因为此时下载的仓库带有 `<REGISTRY>` 前缀，我们需要手动指定没有前缀的名称也能指向同样的仓库，因此：

```bash
# Replace <REGISTRY> and <REPO_NAME> with your choice
docker tag <REGISTRY>/<REPO_NAME> <REPO_NAME>
# e.g.
# docker tag docker.unsee.tech/pytorch/pytorch:2.2.1-cuda12.1-cudnn8-runtime pytorch/pytorch:2.2.1-cuda12.1-cudnn8-runtime
```