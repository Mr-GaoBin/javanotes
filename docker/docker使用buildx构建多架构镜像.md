# **docker使用buildx构建多架构镜像**

Docker for Linux 不支持构建 arm 架构镜像，我们可以运行一个新的容器让其支持该特性，Docker 桌面版无需进行此项设置。

```shell
 docker run --rm --privileged tonistiigi/binfmt:latest --install all
```

由于 Docker 默认的 builder 实例不支持同时指定多个 --platform，我们必须首先创建一个新的 builder 实例。同时由于国内拉取镜像较缓慢，我们可以使用配置了 镜像加速地址 的 dockerpracticesig/buildkit:master 镜像替换官方镜像。

```shell
docker buildx create --use --name=mybuilder-cn --driver docker-container --driver-opt image=dockerpracticesig/buildkit:master
docker buildx create --use --name=mybuilder-cn --driver docker-container --driver-opt image=dockerpracticesig/buildkit:master-tencent
docker buildx use mybuilder-cn
```

新建 Dockerfile 文件

```dockerfile
FROM --platform=$TARGETPLATFORM nginx:alpine

MAINTAINER li7hai26@gmail.com

RUN /bin/sh -c 'echo init ok,platform is '$TARGETPLATFORM
```

使用 docker buildx build 命令构建镜像，注意将 dlhf替换为自己的 Docker Hub 用户名。--push 参数表示将构建好的镜像推送到 Docker 仓库。

```shell
docker buildx build --platform linux/arm,linux/arm64,linux/amd64 -t dlhf/testnginx. --push
docker buildx imagetools inspect dlhf/testnginx
```



## 架构相关变量

Dockerfile 支持如下架构相关的变量

**TARGETPLATFORM**

构建镜像的目标平台，例如 linux/amd64, linux/arm/v7, windows/amd64。

**TARGETOS**

TARGETPLATFORM 的 OS 类型，例如 linux, windows

**TARGETARCH**

TARGETPLATFORM 的架构类型，例如 amd64, arm

**TARGETVARIANT**

TARGETPLATFORM 的变种，该变量可能为空，例如 v7

**BUILDPLATFORM**

构建镜像主机平台，例如 linux/amd64

**BUILDOS**

BUILDPLATFORM 的 OS 类型，例如 linux

**BUILDARCH**

BUILDPLATFORM 的架构类型，例如 amd64

**BUILDVARIANT**

BUILDPLATFORM 的变种，该变量可能为空，例如 v7