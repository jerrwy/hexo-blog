---
title: docker从入门到实战
date: 2017-02-25 22:14:10
tags: docker
---

![docker](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1488044106906&di=6fd3d55d6f17f2c8a74d539637abbaed&imgtype=0&src=http%3A%2F%2Fs2.51cto.com%2Fwyfs02%2FM02%2F5C%2F09%2FwKioL1UZ_2XSppDSAAAjmR1-eDE735.jpg)

## 简述

Docker是一个开源的引擎，可以轻松的为任何应用创建一个轻量级的、可移植的、自给自足的容器。

开发者在笔记本上编译测试通过的容器可以批量地在生产环境中部署，包括VMs（虚拟机）、bare metal、OpenStack 集群和其他的基础应用平台。 

Docker通常用于如下场景：

1. web应用的自动化打包和发布；
2. 自动化测试和持续集成、发布；
3. 在服务型环境中部署和调整数据库或其他的后台应用；
4. 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。

---

## 准备

Docker系统有两个程序：docker服务端和docker客户端。

其中docker服务端是一个服务进程，管理着所有的容器。

docker客户端则扮演着docker服务端的远程控制器，可以用来控制docker的服务端进程。

大部分情况下，docker服务端和客户端运行在一台机器上。

安装docker (我是在ubuntu上安装的 其他环境类似)
``` shell 
sudo apt-get install -y docker.io
```

查看version版本
``` shell
docker version

Client:
 Version:      1.12.3
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   6b644ec
 Built:        Wed Oct 26 21:44:32 2016
 OS/Arch:      linux/amd64

Server:
 Version:      1.12.3
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   6b644ec
 Built:        Wed Oct 26 21:44:32 2016
 OS/Arch:      linux/amd64 
```

以上可知docker环境已经装好了。

## hello-world

现在让我们运行一下，docker的hello world~

``` shell
docker run hello-world 

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```

由上可知，在我们运行 `docker run hello-world`命令后，docker经历了一下四个步骤。

1. docker客户端连接服务器端的守护进程。
2. docker服务器端的守护进程从 Docker Hub拉取"hello-world"镜像。
3. docker服务器端的守护进程根据拉取的镜像创建一个容器，在容器中运行了一个可执行程序输出了你刚才看到的一串信息。
4. docker服务器端进程将输出传给docker客户端，显示在你的终端上。

提示中，还建议我们运行`docker run -it ubuntu bash`命令。

当我们运行以上命令时，可以发现我们进入了一个ubunt bash环境。

就好像进入到一个运行ubuntu系统的虚拟机一样。

## Images & Container

Images和Container就好比是系统镜像ISO镜像文件和虚拟机的关系。

Images就好比系统镜像，Vmware可以通过系统镜像创建虚拟机，docker也可以通过Images创建Container。

Container就好比一个虚拟机，我们可以在里面运行服务，也可以登录进去运行命令，查看文件。

相关命令：

``` shell
docker ps //查看系统中运行的docker容器
docker kill [container] //删除docker容器
docker stop [container] //停止正在运行的docker容器
docker attach/exec [container] //进入容器
docker run //运行镜像，生成容器
docker images //查看系统中存在的docker镜像
docker rmi [image] //删除镜像
docker build //生成镜像
docker pull //拉取镜像
docker push //上传镜像 
docker search //搜索镜像
```

### Dockerfile详解

1. 指定基础image
``` shell
FROM <image>:<tag>  
```

2. 指定镜像创建者信息
``` shell
MAINTAINER <name>  
```

3. 安装软件 (该指令有两种形式)
``` shell
RUN <command> (the command is run in a shell - `/bin/sh -c`)  
RUN ["executable", "param1", "param2" ... ]  (exec form)  
```

4. 设置container启动时执行的操作
``` shell
CMD ["executable","param1","param2"] (like an exec, this is the preferred form)  
CMD command param1 param2 (as a shell)

//当Dockerfile指定了ENTRYPOINT，那么使用下面的格式：
CMD ["param1","param2"] (as default parameters to ENTRYPOINT)  
```

5. 设置container启动时执行的操作
``` shell
ENTRYPOINT ["executable", "param1", "param2"] (like an exec, the preferred form)  
ENTRYPOINT command param1 param2 (as a shell)   

<!--该指令的使用分为两种情况，一种是独自使用，另一种和CMD指令配合使用。
当独自使用时，如果你还使用了CMD命令且CMD是一个完整的可执行的命令，那么CMD指令和ENTRYPOINT会互相覆盖只有最后一个CMD或者ENTRYPOINT有效。
另一种用法和CMD指令配合使用来指定ENTRYPOINT的默认参数，这时CMD指令不是一个完整的可执行命令，仅仅是参数部分；
ENTRYPOINT指令只能使用JSON方式指定执行命令，而不能指定参数。-->
```

6. 设置container容器的用户(默认root)
``` shell
USER root 
```

7. 指定容器需要映射到宿主机器的端口
``` shell
EXPOSE <port> [<port>...]   

# 映射一个端口  
EXPOSE port1  
# 相应的运行容器使用的命令  
docker run -p port1 image  
  
# 映射多个端口  
EXPOSE port1 port2 port3  
# 相应的运行容器使用的命令  
docker run -p port1 -p port2 -p port3 image  
# 还可以指定需要映射到宿主机器上的某个端口号  
docker run -p host_port1:port1 -p host_port2:port2 -p host_port3:port3 image  
```

8. 设置环境变量
``` shell
ENV <key> <value> 
```

9. 从src复制文件到container的dest路径
``` shell
COPY <src> <dest>
```

10. 从src复制文件到container的dest路径
``` shell
ADD <src> <dest>

<src> 是相对被构建的源目录的相对路径，可以是文件或目录的路径，也可以是一个远程的文件url,如果是压缩包会被自动解压。
<dest> 是container中的绝对路径s
```

11. 指定挂载点
``` shell
//设置指令，使容器中的一个目录具有持久化存储数据的功能，该目录可以被容器本身使用，也可以共享给其他容器使用。
VOLUME ["<mountpoint>"]  

eg:
VOLUME ["/tmp/data"] 
```

12. 切换目录
``` shell
WORKDIR /path/to/workdir  

# 在 /p1/p2 下执行 vim a.txt  
WORKDIR /p1 WORKDIR p2 RUN vim a.txt   
```

13. 在子镜像中执行
``` shell
ONBUILD <Dockerfile关键字>  
```
---

## docker中运行express项目

现在让我们开始实战一下，生成一个express项目，将之使用docker部署。

### 生成express项目

使用express-generator生成expess项目。
``` shell
npm install -g express-generator
express express-jerrwy

//可以看到项目创建出来了，目录如下
app.js  bin  node_modules  package.json  public  routes  views
```

安装依赖
``` shell
npm i 

//运行项目
npm start 
```

访问localhost:3000可以看到express 欢迎页面，表示express项目创建成功。

### 编写Dokerfile

在项目根目录，新建一个Dockerfile文件，该文件名就叫Dockerfile,注意大小写，没有后缀，否则会报错。

Dockerfile文件定义了如何创建Docker镜像。

我的Dockerfile如下：
``` shell
FROM node:6.9.1

USER root

RUN npm config set registry https://registry.npm.taobao.org

WORKDIR /var/workspace
COPY package.json /var/workspace/package.json
RUN npm install  && npm cache clean
COPY . /var/workspace 
```

大致解释一下里面做了什么：

1. 我使用基础镜像 node:6.9.1,也就是一个镜像，里面装了node 6.9.1
2. 我镜像里面使用的用户是root
3. 执行命令，设置 npm源
4. 设置镜像的工作目录
5. 将package.json拷贝到镜像的工作目录中
6. 安装依赖
7. 将项目代码拷贝到工作目录

### 生成镜像

Dockerfile写好之后，我们就可以生成镜像了。
``` shell 
docker build . -t moyunchen/express-jerrwy:test
```

`moyunchen/express-jerrwy:test`中`moyunchen`是我docker hub的账号名，`express-jerrwy`是镜像名称，`test`是镜像标签，相当于版本号。

第一次生成镜像由于要下载基础镜像，速度可能比较慢，稍等十几分钟，出去喝杯茶~。

生成成功之后，运行命令：
``` shell
docker images

//可以看到 
REPOSITORY                TAG   IMAGE ID      CREATED       SIZE
moyunchen/express-jerrwy  test  754d9122fa3e  13 hours ago  663.7 MB
```
表明你的docker镜像已经生成啦~

其实，现在你就已经可以运行镜像，生成容器了。
``` shell
docker run  -itd -p 3000:3000 --name express01  moyunchen/express-jerrwy:test  npm start 
```
打开localhost:3000 我们可以看到express欢迎信息。说明我们的exress项目在docker部署成功了。

查看docker容器
``` shell
docker ps

//可以看到
CONTAINER ID  IMAGE                         COMMAND      CREATED        STATUS        PORTS                  NAMES
b8106d910823  moyunchen/express-jerrwy:test "npm start"  6 seconds ago  Up 4 seconds  0.0.0.0:3000->3000/tcp express01 
```

这就是我们正在运行中的docker容器，里面跑了我们的express服务。

登录进去看看
``` shell 
docker exec -it b8106d910823  bash

//可以看到
root@b8106d910823:/var/workspace# ls
Dockerfile  app.js  bin  node_modules  package.json  public  routes  views
```
这个就是docker中项目目录中我们的项目代码。

### push镜像到docker hub

docker hub就好比github,是官方的镜像公有仓库。

我们将镜像发布到这个上面，其他人就可以直接将你的镜像pull下来，然后运行。

就不用单独的把代码pull下来，自己build镜像了。

登录docker账号
``` shell
docker login
//接下来他会让你输入账号密码邮箱 
Username: [username]
Password: [password]
Email: xxxx@foxmail.com
WARNING: login credentials saved in /root/.docker/config.json
Login Succeeded
```

push镜像到docker hub仓库
``` shell
docker push moyunchen/express-jerrwy:test
```
`moyunchen`是你的docker账号名，生成镜像的时候也必须是 `[username]/[imagename]` 这种格式

push的过程异常缓慢。。。我这里用了几个小时。。。只是第一次才慢，后面是增量更新就会快很多。。

成功之后，登录docker hub就可以看到你的镜像了。

### 从docker hub拉取镜像，生成容器

现在，你的镜像推送到了docker hub上面了，让你的项目伙伴拉取项目镜像，运行起来。

拉取镜像
``` shell
docker pull moyunchen/express-jerrwy:test
```

运行镜像，创建容器的步骤，跟上面一样。

---

## docker-compose

docker-compose是用于定义和运行复杂Docker应用的工具。

你可以在一个文件中定义一个多容器的应用，然后使用一条命令来启动你的应用，然后所有相关的操作都会被自动完成。

在上面过程中，我们运行容器的命令过于复杂，而且一次只能启动一个docker应用，管理起来也不是很方便。

于是就有`懒惰`的程序员创建了docker-compose

### 安装

以ubuntu系统举例
``` shell
curl -L https://github.com/docker/compose/releases/download/1.3.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose 

//这个装起来也好慢。。。是因为墙的原因吧。。
```

安装完成之后
``` shell
docker-compose --version

//可以看到 
docker-compose 1.8.1
```

到这里，你的docker-compose就算安装成功了。

### docker-compose.yml

docker-compose.yml文件的目的是定义了一组应用，可以很方便的对多个应用进行发布。

我的理解是取代了`docker run`，因为docker run 命令使用起来过于繁琐。

当然，如果你不想用docker-compose，你可以将对于的docker-compose.yml翻译成docker run语法。

还是以上面的express-jerrwy镜像为例，对应的 docker-compose.yml文件
``` shell
version: '2'
services:
  express-jerrwy:
      ports:
        - "3000:3000"
      image: "docker.io/moyunchen/express-jerrwy:test"
      container_name: "express-jerrwy"
      restart: always
      command: "npm start" 
```

现在docker-compose.yml写好了,上面我们只定义了express-jerrwy一个docker服务，我们完全可以一次定义多个。

我们现在创建容器
``` shell
docker-compose up -d 
```
关闭容器
```  
docker-compose down
```

以后我们部署项目，就只需要写好docker-compose.yml文件，就可以利用docker-compose进行项目部署。

是不是简单了很多。

---

## daocloud

上面我们用的docker hub 为公有仓库。

我们发布的应用镜像是所有人都可以下载得到的。

如果使我们公司的项目，里面含有一个不能公开的东西，那公有仓库也就不适合我们了。

所以我们就可以使用私有仓库，例如 [daocloud](https://www.daocloud.io/)

使用方法跟公有仓库区别不大。

---