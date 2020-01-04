---
title: Docker笔记
date: 2020-01-03 11:20:01
tags: ['Note']
categories: Docker
---

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。[点击查看菜鸟教程链接](https://www.runoob.com/docker/docker-tutorial.html)
<!-- more -->

# 基础

1. 版本
`docker version`

2. 查看当前运行的镜像
`docker stats -a`

# 镜像操作

1. 拉取镜像
`docker pull hyperledger/fabric-orderer`

2. 启动一个镜像
`docker run hyperledger/fabric-orderer`

3. 开启运行的镜像交互式终端
peer0.org1.example.com是镜像的名称，/bin/bash是镜像终端文件
`docker exec -i -t peer0.org1.example.com /bin/bash`

4. 查看所有容器
相比stats更详细，但是不是实时的
`docker ps -a`

5. 启动一个被停止的容器
需要提供容器ID
`docker start 34xxxxx456`

6. 镜像保存与导入

```shell

# 查看所有的镜像
docker images

# 强制删除指定镜像
# 全部删除 docker rmi -f $(docker images -q)
docker rmi -f 34xxxxx456

# 给镜像打上Tag
# docker tag [镜像ID] [仓库：标签]
docker tag 34xxxxx456 docker.io/hyperledger/fabric-tools:latest

# 备份镜像
docker save 34xxxxx456 > /var/images /fabric-tool.tar

# 加载本地镜像
docker load < /var/images/fabric-tool.tar
```

