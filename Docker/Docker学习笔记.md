## 1、Docker的使用

创建一个简单的docker镜像并使用

创建 Docker 镜像的方法通常包括编写 `Dockerfile` 文件和使用 Docker CLI（命令行界面）。以下是一般步骤：

1. **编写 Dockerfile：** 创建一个文本文件，通常称为 Dockerfile，其中包含了构建 Docker 镜像的指令。Dockerfile 定义了基础镜像、依赖项安装、文件拷贝、环境变量设置、运行命令等。

   示例 Dockerfile：

   ```
   # 使用基础镜像
   FROM ubuntu:20.04
   
   # 设置工作目录
   WORKDIR /app
   
   # 拷贝应用程序文件到容器中
   COPY . /app
   
   # 安装应用程序依赖
   RUN apt-get update && \
       apt-get install -y python3
   
   # 设置环境变量
   ENV LANG C.UTF-8
   
   # 暴露端口
   EXPOSE 80
   
   # 定义容器启动时执行的命令
   CMD ["python3", "app.py"]
   ```

   如下图是一个dockerfile文件，其作用是通过从openjdk11容器内（内含jdk环境），将源jar文件添加到容器内并执行

   ![image-20240114231323067](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114231324.png)

2. **构建 Docker 镜像：** 打开终端，`进入包含 Dockerfile 的目录`，然后运行以下命令以构建 Docker 镜像。

   ```
   docker build -t your_image_name:tag . 
   ```

   例如：

   ```
   docker build -t myapp:1.0 .
   ```

   这将根据 Dockerfile 中的指令构建一个名为 `myapp`，标签为 `1.0` 的镜像。

3. **运行容器：** 构建成功后，可以使用以下命令在容器中运行应用程序：

   ```
   docker run -p 8080:80 myapp:1.0
   ```

   这将把容器内的端口 80 映射到主机的端口 8080。

## 2、进入Docker

```
 docker exec -it 775c7c9ee1e1 /bin/bash  
```



# Docker学习

## 一、Docker基本组成

**镜像（image）**：docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat镜像—>run---->tomcat01容器（提供服务），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中运行的）。

**容器（container）**：docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的，启动、停止、删除、基本命令，目前就可以把这个容器理解为就是一个简易的Linux系统。

**仓库（repository）**：仓库就是存放镜像的地方，仓库分为公有仓库和私有仓库，Docker Hub（默认是国外的）、阿里云…都有容器服务器（配置镜像加速）

> 即通过`镜像`克隆出`多个容器`去使用Docker服务

## 二、Docker容器和虚拟机的区别

服务器虚拟化解决的核心问题是`资源调度`，而容器解决的核心问题是`应用开发、测试和部署`。

- Docker守护进程可以直接与主操作系统进行通信，为各个Docker容器分配资源；还可以将容器与主操作系统隔离，并将各个容器互相隔离
- 而虚拟机更擅长于彻底隔离整个运行环境，Docker容器是应用级别的隔离；如云服务提供商通常采用虚拟机技术隔离不同的用户，Docker通常用于隔离不同的应用，如前后端，数据库等
- 虚拟机启动需要数分钟，而Docker容器可以在数毫秒之内启动，相对于虚拟机，Docker更轻量和简洁
- 虚拟机通常是虚拟化出一套硬件和内核，操作系统，从而在这个操作系统上安装和运行软件；而Docker容器内的应用是直接应用在宿主机上的，DOcker容器利用宿主操作系统的核心功能，而不是虚拟化出一个完整的操作系统

## 三、Docker安装

公司docker安装：[Docker-CE 镜像使用帮助 (jaguarmicro.com)](https://mirrors.jaguarmicro.com/.help/docker-ce.html)

**安装之前卸载历史版本的Docker**

```
第一步：卸载依赖
命令：apt remove docker-ce docker-ce-cli containerd.io

第二步：删除资源 安装的资源都在/var/lib/docker目录下
命令：rm -rf /var/lib/docker
```

**安装docker**

```
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
curl -fsSL http://mirrors.jaguarmicro.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] http://mirrors.jaguarmicro.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce

# 安装指定版本的Docker-CE:
# Step 1: 查找Docker-CE的版本:
# apt-cache madison docker-ce
#   docker-ce | 17.03.1~ce-0~ubuntu-xenial | http://mirrors.jaguarmicro.com/docker-ce/linux/ubuntu xenial/stable amd64 Packages
#   docker-ce | 17.03.0~ce-0~ubuntu-xenial | http://mirrors.jaguarmicro.com/docker-ce/linux/ubuntu xenial/stable amd64 Packages
# Step 2: 安装指定版本的Docker-CE: (VERSION例如上面的17.03.1~ce-0~ubuntu-xenial)
# sudo apt-get -y install docker-ce=[VERSION]
```

**安装校验**

```
docker version
```

**启动Docker**

```
systemctl daemon-reload # 重新加载systemd系统管理守护进程
systemctl restart docker # 重启docker服务
systemctl status docker # 查看docker状态
```

![image-20240123165843881](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123165845.png)

**测试Docker**

