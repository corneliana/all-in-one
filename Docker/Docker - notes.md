

## What is Docker?

- 定义
	- a person moving goods on and off ships = 码头工人。
	- Docker allows you to **package** an application with **all of its dependencies** into a **standardized unit** for software development.
	- 主要应用在Linux和后台一些应用的一个虚拟机。

- 基本概念
	- Registry 仓库 
	- Image 镜像 => 创建虚拟机所需的系统镜像文件
	- Container 容器 => 正在运行的虚拟机
	- Dockerfile => 配置文件：如何构建一个镜像
	- tar 文件 => 镜像保存为tar文件，他人可以通过load指令重新加载成一个镜像

- 基础应用
	- static website


## What problem does Docker solve?

- Virtual Machine 虚拟机 => Virtualization 虚拟化 
	- 虚拟化是一项技术
	- 创建虚拟环境和虚拟机器的技术
	- 在电脑上，通过抽象真实硬件为虚拟硬件，让虚拟硬件和所创建的虚拟环境/机器进行交互
	- think computer as a box, virtualization is the magical dividers to create smaller boxes from the big one => abstract and divide hardware's capabilities among various users and tasks => do lots of different things at the same time => easy, safe, smart
	- Docker是虚拟化技术的产物


## Docker advanced

### Docker Network
Container之间如何通信

### Docker Storage
Volume

### Docker Compose


### 用 Docker 部署应用



## What problems does Docker have?
=> Kubernetes

回顾架构演进之路，想象未来的趋势