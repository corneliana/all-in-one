
## What is Docker?
- 定义
	- a person moving goods on and off ships = 码头工人。
	- Docker allows you to **package** an application with **all of its dependencies** into a **standardized unit**(i.e. containers) for software development.
	- 主要应用在Linux和后台一些应用的一个虚拟机。

docker ps: process status. Each docker container is actually a process.

## Docker Basics
### Concepts
- Registry 仓库 
- Image 镜像 => 创建虚拟机所需的系统镜像文件
- Container 容器 => 正在运行的虚拟机
- Dockerfile => 配置文件：如何构建一个镜像
- tar 文件 => 镜像保存为tar文件，他人可以通过load指令重新加载成一个镜像

### Dockerfile
Automate the process of assembling an image in Docker.
```shell
FROM # 设置基础镜像
WORKDIR # 指定shell命令运行在哪个folder下，没有就自动创建
COPY <source> <destination> # 复制当前host的A目录下文件到镜像的B目录下
RUN # 运行shell命令，构建镜像时执行
CMD # 容器启动时执行的命令，构建镜像完成后执行


COPY v.s. ADD
COPY # 地址是文件系统的root
ADD # ADD自带解压，不仅可以是文件系统的root，也可以是一个URL。如果用到网络资源，可以使用ADD

CMD v.s. ENTRYPOINT # 指定容器启动之后的核心脚本
执行顺序与格式有关，ENTRYPOINT非json则以ENTRYPOINT为准，如果ENTRYPOINT和CMD都是JSON，则ENTRYPOINT+CMD拼接成shell。ENTRYPOINT指定的命令可以在run时追加参数。

——————

EXPOSE # 指定当前image暴露的port
VOLUME /a/b # 指定映射文件. e.g. 把容器中的/a/b文件映射到了host machine的一个目录下。一般是映射到匿名卷

——————参数
ENV # 指定当前容器的环境变量，构建时到运行时一直生效
e.g. ENV A=10
     CMD echo $A
ARG # 参数，构建时有效，但运行时失效。构建时临时修改内部变量

———————
LABEL # 声明一些metadata
ONBUILD ENV C=100 # 其他基于本镜像的镜像执行

———————
STOPSIGNAL # 容器用什么样的信号可以停止
HEALTHCHECK # 检查容器健康状况的配置
SHELL # 指定当前容器运行的shell是哪一种 bin/sh bin/bash

```

## Why Docker? What is Virtualization?

- Virtual Machine 虚拟机 => Virtualization 虚拟化 
	- 虚拟化是一项技术
	- 创建虚拟环境和虚拟机器的技术
	- 在电脑上，通过抽象真实硬件为虚拟硬件，让虚拟硬件和所创建的虚拟环境/机器进行交互
	- think computer as a box, virtualization is the magical dividers to create smaller boxes from the big one => abstract and divide hardware's capabilities among various users and tasks => do lots of different things at the same time => easy, safe, smart
	- Docker是虚拟化技术的产物


## Docker advanced
### Docker Network
How does container communicate with each other?
Docker provides a network that offers connection, DNS resolution, and ?
There are 7 types of network. Why networks have types?
bridge
link

### Docker Storage
Volume
Containers are short-lived, so if we want to persist data. Store it on your host file system, i.e. volume, i.e. a designated directory. Also, this volume can be mapped to your own or other containers.

### Docker Compose
docker compose
- orchestration of containers by docker-compose.yml, which manages:
	- network
	- storage
	- interaction
	- other config

## Summary

Docker 也有自己的问题 => Kubernates