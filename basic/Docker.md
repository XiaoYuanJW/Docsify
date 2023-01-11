---
title: Docker
date: 2022-06-26 10:31:22
tags: Docker
---

# Docker

## Docker简介

### 前言

#### 问题

大型项目组件和运行环境相对复杂，部署时会碰到很多问题：

- 依赖关系复杂，容易出现兼容性问题
- 开发、测试、生产环境存在差异，不同部署的服务器存在差异

![image-20220626105249118](https://s2.loli.net/2022/06/26/r5743fEk9wOiKhv.png)

#### Docker问题解决

Docker如何解决依赖兼容问题

- 将应用的Libs（函数库），Deps（依赖）、配置与应用一起打包

- 将某个应用放到一个隔离容器去运行，避免相互干扰

Docker如何解决不同环境的操作系统不同的问题：

要解决这个问题，先要了解一下操作系统结构

![](https://s2.loli.net/2022/06/26/DHvj5VXgmzCwYaS.png)

> 内核与硬件交互，提供操作硬件的指令
>
> 系统应用封装内核指令为函数，便于程序员调用，用户程序基于系统函数库实现功能

例如：Ubuntu和CentOS都是基于Linux内核，只是系统应用不同，提供的函数库存在差异，这就是用户程序不能跨系统运行的原因。

- Docker将用户程序与所需要调用的系统函数库一起打包
- 用户程序在运行到不同操作系统时，直接基于打包好的库函数，借助于操作系统的Linux内核来运行

![image-20220626111505029](https://s2.loli.net/2022/06/26/iKTZhF1REUst5e4.png)

#### 总结

Docker是一个开源的应用容器引擎

- Docker允许将开发中将应用、依赖、函数库、配置打包形成可移植的镜像

- Docker应用运行在容器中，使用沙箱机制，相互隔离

- Docker镜像中包含完整运行环境，包括系统库函数，仅依赖系统的Linux内核，因此可以发布到任意Linux操作系统上运行，实现虚拟化

### Docker和虚拟机的区别

![image-20220630181037698](https://s2.loli.net/2022/07/12/ei2P8JBlgIRf7zM.png)

Docker是一个系统进程，而虚拟机是操作系统中的操作系统

 虚拟机在操作系统中模拟硬件设备，然后运行另一个操作系统，例如在window系统中运行一个Ubuntu系统。

可以运行任意的Ubuntu应用。

![image-20220626121645356](https://s2.loli.net/2022/06/26/vfJxEPaWudS8htT.png)

| 特性     | Docker   | 虚拟机   |
| -------- | -------- | -------- |
| 性能     | 接近原生 | 性能较差 |
| 硬盘占用 | 一般为MB | 一般为GB |
| 启动     | 秒级     | 分钟级   |

### Docker架构

![image-20220630184857540](https://s2.loli.net/2022/07/12/b19DXow3ejd7qgi.png)

Docker包括三个基本概念

- 镜像（Image）：相当于一个root文件系统，Docker将应用程序及其依赖、函数库、环境、配置等文件打包在一起形成镜像文件。

- 容器（Container）：镜像中的应用程序运行后形成的进程就是容器，Docker对容器进行隔离，有独立的CPU、内存 等，对外不可见。

  > 镜像和容器的关系，类似于面向对象程序设计中的类和实例一样，Docker容器通过Docker镜像来创建。镜像是静态的定义,时用于创建Docker容器的模板，容器是镜像运行时的实体，是独立运行的一个或一组应用。容器可以被创建、启动、停止、删除、暂停等。

  ![image-20220626125142458](https://s2.loli.net/2022/06/26/zVtjlXbnEsQu6CT.png)

- 仓库（Repository）：一个代码控制中心，用来保存镜像。

![image-20220626124824925](https://s2.loli.net/2022/06/26/DpigMkWVTxPrv9N.png)



Docker使用客户端-服务端（C/S）架构模式

![img](https://s2.loli.net/2022/06/26/479uHrjBdZ6IQWv.png)

- 服务端（server）：Docker守护进程，负责处理Docker指令、管理镜像、容器等
- 客户端（client）：通过命令或RestAPI向Docker服务端发送指令，与Docker的守护进程通信。

![image-20220626125845647](https://s2.loli.net/2022/06/26/D9wCelVNydBpKWU.png)

## Docker的安装

### CentOS Docker安装

### 安装

#### 卸载

##### 查看已经安装的docker

```
yum list installed |grep docker
```

##### 下载已经安装的docker

```
yum -y remove xxx
```

##### 删除旧版本即其相关依赖项

```
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

#### 安装Docker Engine-Community

> 使用 Docker 仓库即Docker Engine-Community进行安装
>
> 在新主机安装Docker Engine-Community前，需要设置Docker仓库，之后可以从仓库安装和更新Docker

##### 设置仓库

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2 --skip-broken
```

![image-20220626210712373](https://s2.loli.net/2022/06/26/8adFTPgOYx6wfDv.png)

##### 设置稳定仓库

```
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

![image-20220626210628192](https://s2.loli.net/2022/06/26/8BivwWVtUyIg3kf.png)

##### 安装Docker Engine-Community

```
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

![image-20220626211100004](https://s2.loli.net/2022/06/26/Di92yF4EO6RWuTw.png)

#### 关闭防火墙

```
systemctl stop firewalld
systemctl disable firewalld
```

![image-20220626132908943](https://s2.loli.net/2022/06/26/b8mBlpvDMurXg6Y.png)

#### 启动Docker

```
systemctl start docker
systemctl status docker
systemctl enable docker
```

![image-20220626133049860](https://s2.loli.net/2022/06/26/GH635EITaPvzWiA.png)

####  查看docker版本

```
docker -v
```

![image-20220626211521257](https://s2.loli.net/2022/06/26/eSZQqO39kFYt6jR.png)

#### 配置镜像加速

阿里容器镜像服务地址：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

![image-20220626211723068](https://s2.loli.net/2022/06/26/CgsplG6LcAraZm4.png)

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://fsfhxt97.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## Docker基本操作

### 镜像相关命令

镜像名称一般分两部分组成：[repository]:[tag]

> 没有指定tag时，默认是latest，代表最新版本的镜像

![image-20220626212326961](https://s2.loli.net/2022/06/26/37jHDyA8S1flMYz.png)

### 镜像操作命令

通过Dockerfile构建本地构建镜像

```
docker build
```

从Docker Registry镜像服务器拉取镜像

```
docker pull
```

查看镜像

```
docker images
```

删除镜像

```
docker rmi
```

推送镜像

```
docker push
```

镜像压缩

```
docker save
```

加载压缩镜像

```
docker load
```

![image-20220626212914211](https://s2.loli.net/2022/06/26/1BFy2okuU94wtTb.png)

查看docker命令

```
docker --help
docker xxx 
```

![image-20220626213134386](https://s2.loli.net/2022/06/26/1HKiPmadVqhY9yx.png)

### 容器相关命令

![image-20220709164840868](https://s2.loli.net/2022/07/09/wOaoPthBv8ZgYA1.png)

#### 创建容器

```
 docker run --name nginx -p 8080:80 -d nginx
```

> 参数说明：
>
> - docker run：创建并运行一个容器
>
> - -name：容器名称
> - -p 8080:80： 端口进行映射，将宿主机端口与容器端口映射，将本地 8080 端口映射到容器内部的 80 端口
> - -d nginx： 设置容器在后台运行
> - niginx：后台名称

#### 查看日志

```
docker logs -f nginx
```

> -f   持续更新日志信息

#### 进入容器

```
docker exec -it nginx bash
```

> 参数说明：
>
> - docker exec：进入容器内部，执行命令
> - -it：给当前进入的容器构建一个标准输入输出终端，实现与容器的交互
> - bash：进入容器后执行的命令，bash是一个Linux终端交互命令

## Docker安装MySQL

### 查看可用MySQL版本

访问 MySQL 镜像库地址：https://hub.docker.com/_/mysql

![image-20220626213644151](https://s2.loli.net/2022/06/26/R2cBAZDhO8XtWav.png)

### 拉取MySQL镜像

```
docker pull mysql
```

>  Dockerhub没有提供arm64架构的MySQL，但是MySQL官方提供了mysql/mysql-server

arm架构下拉取MySQL镜像

```
docker pull mysql/mysql-server
```

![image-20220626224920659](https://s2.loli.net/2022/06/26/x4NVdpZn78uoKbW.png)

```
docker pull --platform linux/x86_64 mysql
```

![image-20220627091852496](https://s2.loli.net/2022/06/27/QywWo7JFVDuieqX.png)

### 查看本地镜像

```
docker images
```

![image-20220627091949600](https://s2.loli.net/2022/06/27/bd2Skcmn8grwRCD.png)

### 搜索可用镜像

```
docker search mysql
```

![image-20220627101743575](https://s2.loli.net/2022/06/27/FizNbMqhpwWCZya.png)

### 运行MySQL

```
docker run --name mysql  -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d  mysql
```

> 参数说明：
>
> - **-p 3306:3306** ：映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 **宿主机ip:3306** 访问到 MySQL 的服务
> - **MYSQL_ROOT_PASSWORD=123456**：设置 MySQL 服务 root 用户的密码

![image-20220709153240490](https://s2.loli.net/2022/07/09/TW4DoMgOteXNSnj.png)

#### 数据挂载

```
docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql/conf.d  \
-v /mydata/mysql/log:/logs \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql
```

> arm架构下运行

```
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql/mysql-server
```

> --name：容器命名
>
> -p：端口映射

![image-20220627202412060](https://s2.loli.net/2022/06/27/PMCQYl9m57UIy2K.png)

### 登录MySQL

```
docker exec -it mysql bash
mysql -uroot -p123456
```

![](https://s2.loli.net/2022/06/27/UGgCWoljKZxSwHT.png)

### 连接MySQL

#### 添加远程登录用户

```
#添加远程登录用户
CREATE USER 'root'@'%' IDENTIFIED BY 'root';
#
GRANT ALL ON *.* TO 'root'@'%'；
flush privileges;
#修改密码
ALTER USER 'root'@'%' IDENTIFIED BY '13851176590++';
```

![image-20220627210219722](https://s2.loli.net/2022/06/27/6ghaeCGo15cmDdy.png)

#### 查看用户数据库

```
show databases;
use mysql;
select host, user, plugin from user;
```

![image-20220627210420003](https://s2.loli.net/2022/06/27/gnCJSlbzGy3HWV2.png)

#### 远程连接测试

![image-20220627211152025](https://s2.loli.net/2022/06/27/Ptq6WZL9RdjBJNl.png)

![image-20220627211216753](https://s2.loli.net/2022/06/27/VX71Qtwv4ByNsfW.png)

## Docker安装Nginx

### 查看可用Nginx版本

访问Nginx镜像库版本地址：https://hub.docker.com/_/nginx

![image-20220627212318153](https://s2.loli.net/2022/06/27/bsDTCmwg6iEX9qj.png)



### 拉取Nginx镜像

```
docker pull nginx:latest
```

![image-20220627212905626](https://s2.loli.net/2022/06/27/G4lPrCzxdRiKcSE.png)

### 查看本地镜像

```
docker images
```

![image-20220627213027744](https://s2.loli.net/2022/06/27/CYQADKfuhZpoqHF.png)

### 运行Nginx

```
 docker run --name nginx -p 8080:80 -d nginx
```

![image-20220627213342836](https://s2.loli.net/2022/06/27/pGgndqcEivYKoyM.png)

参数说明：

> - **--name nginx**：容器名称
> - **-p 8080:80**： 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口
>   - **-d nginx**： 设置容器在在后台一直运行

### 镜像导出和加载

#### 镜像导出

```
docker save -o nginx.tar nginx:latest
```

![image-20220709155017994](https://s2.loli.net/2022/07/09/e6F5AgK9tu13SCs.png)

#### 镜像加载

##### 删除镜像

```
docker rmi nginx:latest
```

![image-20220709155218222](https://s2.loli.net/2022/07/09/1x2WUYieaoN4bJO.png)

```
docker load -i nginx.tar
```

![image-20220709155420618](https://s2.loli.net/2022/07/09/UyiwadYqObkfHJP.png)

### 运行容器

```
docker run --name nginx -p 8080:80 -d nginx
```

>参数说明：
>
>- **--name nginx-test**：容器名称
>- **-p 8080:80**： 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口
>- **-d nginx**： 设置容器在在后台一直运行

### 访问服务

![image-20220709164017925](C:\Users\YuanJW\AppData\Roaming\Typora\typora-user-images\image-20220709164017925.png)

### 查看容器日志

```
docker logs -f nginx
```

![image-20220709170822636](https://s2.loli.net/2022/07/09/Kix9IsezgZoF46k.png)

## Docker安装Redis

### 查看Redis可用版本

![image-20220709160742122](https://s2.loli.net/2022/07/09/1qnMl8STNKvhoaV.png)

### 拉取Redis镜像

```
docker pull redis:latest
```

![image-20220709160912161](https://s2.loli.net/2022/07/09/Adchiq7RFbPEuKx.png)

### 运行Redis容器

```
docker run -itd --name redis -p 6379:6379 redis
```

![image-20220709161215298](https://s2.loli.net/2022/07/09/35EhZDXBjGdgQUL.png)

### 持久化运行Redis容器

```
docker run -itd --name redis -p 6379:6379 -d redis redis-server --appendonly yes
```

![image-20220709184650774](https://s2.loli.net/2022/07/09/Yh2McU9m5EOpeXi.png)

### 使用Redis客户端

```
docker exec -it redis /bin/bash
redis-cli
```

```
docker exec -it redis redis-cli
```

![image-20220709161905065](https://s2.loli.net/2022/07/09/5Yl2MbRAWGgcUBN.png)

### 可视化工具连接Redis

![image-20220709190614232](https://s2.loli.net/2022/07/09/NncaSrQ5B8R4j1W.png)

### 打包镜像

```
docker save -o redis.tar redis:latest
```

![image-20220709162053955](https://s2.loli.net/2022/07/09/ZRbAUB5VyFCrncT.png)

### 删除容器和镜像

```
docker rm
docker rmi
```

![](https://s2.loli.net/2022/07/09/ARQNT9ixhtC6pmZ.png)

```
docker load -i redis.tar
```

![image-20220709162740876](https://s2.loli.net/2022/07/09/OGA5VeUFJBru6h2.png)

## 数据卷

### 数据卷定义

> 数据卷主要用于解决容器与数据耦合的问题，即将容器与数据分离，解耦合，方便操作容器内数据，保证数据安全。

![image-20220716114244206](https://s2.loli.net/2022/07/16/DJxtMSWCkmgQB5o.png)

数据卷(volume)是一个虚拟目录，指向宿主机文件系统中的一个目录。

![image-20220716114631748](https://s2.loli.net/2022/07/16/zleZEKkjFW5ayqV.png)

### 数据卷操作

```
docker volume [COMMAND]
```

> docker volume命令是数据卷操作，根据命令后跟随command确定下一步操作

- create-创建一个volume
- inspect-显示一个或多个volume的信息
- ls-列出所有的volume
- prune-删除未使用的volume
- rm-删除一个或多个指定的volume

![image-20220716125125309](https://s2.loli.net/2022/07/16/u1FRUSje8QlnWCq.png)

![image-20220716125559541](https://s2.loli.net/2022/07/16/kNRY8gADQxTl24S.png)

![image-20220716125638308](https://s2.loli.net/2022/07/16/9DSWUIhsv5Bn6EV.png)

### 数据卷挂载

#### 创建数据卷

```
docker volume create html
docker volume ls
docker volume inspect html
```

![image-20220716135129339](C:\Users\YuanJW\AppData\Roaming\Typora\typora-user-images\image-20220716135129339.png)

#### 运行容器并挂载数据卷

```
docker run --name nginx -p 8080:80 -v html:/usr/share/nginx/html -d nginx
```

> 数据卷挂载方式：-v nameName : /targetContainerPath
>
> 如果容器运行时Volume不存在，会被自动创建出来

![image-20220716135701171](https://s2.loli.net/2022/07/16/IL4EQfiAr21qKtp.png)

#### 查看挂载目录

```
docker volume inspect html
cd /var/lib/docker/volumes/html/_data
```

![image-20220716140349300](https://s2.loli.net/2022/07/16/M5tlDuegwx97Y2i.png)

#### 修改内容

```
vim index.html
```

![image-20220716141200880](https://s2.loli.net/2022/07/16/cNDIsf4ZS1oiqLA.png)

#### 访问服务

![image-20220719225705897](https://s2.loli.net/2022/07/19/xsvw6pOES3fVhuX.png)

### 目录挂载

> 目录挂载和数据卷挂载的语法类似：
>
> - -v [宿主机目录]:[容器内目录]
> - -v [宿主机文件]:[宿主机文件]

#### 创建目录

```
mkdir -p /tmp/mysql/data
mkdir -p /tmp/mysql/conf
```

> 挂载/tmp/mysql/data到mysql容器内数据存储目录
>
> 挂载/tmp/mysql/conf/hmy.cnf到mysql容器的配置文件
>
> 设置MySQL密码

#### 运行容器

```123456
docker run \
--name mysql \ 
-e MYSQL_ROOT_PASSWORD=123456 \
-p 3306:3306 \
-v /tmp/mysql/conf/hmy.cnf:/etc/mysql/conf.d/hmy.cnf \
-v /tmp/mysql/data:/var/lib/mysql \
-d mysql
```

### 数据卷挂载方式对比

![image-20220719232331514](https://s2.loli.net/2022/07/19/2GKXxnvreMsHiY3.png)

- 数据卷挂载耦合度低，由dokcer来管理目录
- 目录挂载耦合高，需要我们自己来管理目录

## Dockerfile自定义镜像

### 镜像结构

> 镜像时将应用程序及其需要的系统函数库、环境、配置、依赖打包而成

![image-20220719233304550](C:\Users\YuanJW\AppData\Roaming\Typora\typora-user-images\image-20220719233304550.png)

镜像是分层结构，每一层称为一个Layer

- BaseImage层：包含基本的系统函数库、环境变量、文件系统

- Entrypoint层：入口，是镜像中应用启动的命令
- 其他层：在BaseImage基础上添加依赖、安装程序、完成整个应用的安装和配置

### Dockerfile

Dockerfile是一个文本文件，其中包含了一个个指令(Instruction)，用指令来说明要执行什么操作来构建镜像。每一个指令都会形成一层Layer。

![image-20220720120921273](https://s2.loli.net/2022/07/20/6MqsXBp9juyNeiv.png)

#### 基于java:8-alpine镜像构建Java项目为镜像

- 新建一个空目录，在目录中新建一个文件

![image-20220720141558745](https://s2.loli.net/2022/07/20/4BV8zpP7qYUeC5u.png)

- 拷贝jar包到目录

![image-20220720142621552](https://s2.loli.net/2022/07/20/LACTlmpDMqR1j92.png)

- 编写Dockerfile文件
  - 基于java:8-alpine作为基础镜像
  - 将app.jar拷贝到镜像中
  - 暴露端口
  - 编写人口ENTRYPOINT

```
# 指定基础镜像
FROM java:8-alpine

# 拷贝java项目的包
COPY ./docker-demo.jar /tmp/app.jar

# 暴露端口
EXPOSE 8090
# 入口，java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```

![image-20220720142611628](https://s2.loli.net/2022/07/20/BZCoeyRv8EUucTd.png)

- 使用docker build命令构建镜像

```
docker build -t javaweb:1.0 .
```

> 最后的.指从当前目录构建

![image-20220720142801112](https://s2.loli.net/2022/07/20/HMjCW9DOutfxvbR.png)

- 使用docker run创建容器并运行

```
docker run --name javaweb -p 8090:8090 -d javaweb:1.0
```

![image-20220720145638751](https://s2.loli.net/2022/07/20/JuRgaCeTK91VUXj.png)

- 访问网址：http://ip:8090/hello/count

![image-20220720145713672](https://s2.loli.net/2022/07/20/Ipw8Jdk1c7nGyah.png)

## DockerCompose

> Docker Compose是一个用于定义和运行多个Docker容器应用的工具，
>
> Docker  Compose可以基于Compose文件帮助我们快速部署分布式应用，而无需手动一个个创建和运行容器。
>
> Compose使用YAML文件配置应用服务，通过指令定义集群中的每一个容器如何运行。

![image-20220721094915629](https://s2.loli.net/2022/07/21/WdSYkMLzcljgHvD.png)



### DockerCompose的安装

```
curl -L "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

![image-20220721110308195](https://s2.loli.net/2022/07/21/DoeYcXgaQERKbU5.png)

![image-20220721110437236](https://s2.loli.net/2022/07/21/KuSj45c9rhgCOEA.png)

#### 修改文件权限

> 修改文件权限为可执行

```
chmod +x /usr/local/bin/docker-compose
```

![image-20220721110623950](https://s2.loli.net/2022/07/21/64qi9zxDyJIbwKs.png)

#### 测试是否安装成功

```
docker-compose --version
```

![image-20220721132752027](https://s2.loli.net/2022/07/21/2TGAwCYW5hIPXJa.png)

#### Base自动补全命令

```
curl -L https://raw.githubusercontent.com/docker/compose/1.29.2/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose
```

![image-20220721133210786](https://s2.loli.net/2022/07/21/25gijMbw1GnFhcD.png)

### Docker Compose使用步骤

- 使用DockerFile定义应用程序环境，一般需要修改初始镜像行为时才需要使用
- 使用docker-compose.yml定义需要部署的应用程序服务
- 使用docker-compose up命令将所有应用服务一次性部署

### Docker-compose.yml常用配置

#### `image`

> 指定运行镜像的名称

```yml
image: <image_name>:<tags>
```

#### `container_name`

> 配置容器名称

```yml
container_name: <container_name>
```

#### `ports`

> 指定宿主机和容器的端口映射（HOST-宿主机端口:CONTAINER-容器端口）

```yml
# 将宿主机的3306端口映射到容器的3306端口
ports:
  - <host_port>:<container_port>
```

#### `volumes`

> 将宿主机的文件或目录挂载到容器中（HOST:CONTAINER）

```yml
# 将宿主机文件挂载到容器内
volumes:
  - <host_volumes>:<container_volumes>
```

#### `environment`

> 配置环境变量

```yml
environment:
  - <env_param>=<env_value>
```

#### `links`

> 连接其他容器的服务（SERVICE:ALIAS）

```yml
# 可以以database为域名访问服务名称为db的容器
links:
  - db:database
```

### Docker Compose常用命令

#### 构建、创建、启动相关容器

```yml
# -d表示后台运行
docker-compose up -d
```

#### 指定文件启动

```
docker-compose -f docker-compose.yml up -d
```

#### 停止所有相关容器

```yml
docker-compose stop
```

#### 列出所有容器信息

```
docker-compose ps
```

## 容器网络管理

### 网络管理意义

> 容器的网络默认与宿主机、与其他容器相互隔离，且容器中可以运行一些网络应用，如nginx、web应用、数据库等，如果需要让外部可以访问这些容器中运行的网络应用，需要配置网络来实现。
>
> 不同的需求下，容器和宿主机的通信有不同的业务状态时，需要容器网络管理以达到管理不同业务下相关的网络配置。

### Docker网络驱动模式的类型

- bridge-桥接模式：默认的网络模式，类似虚拟机的nat模式
- host-宿主模式：容器与宿主机之间的网络无隔离，即容器直接使用宿主机网路
- none-禁用模式：容器禁用所有网络
- overlay-覆盖模式：利用vxlan实现的bridge模式
- macvlan-模式：容器具备MAC地址，使其在外部看来一台真实的网络设备

Docker在安装时，会自动创建bridge、host、none三个网络驱动

```
docker network ls
```

![image-20220728101349811](https://img-blog.csdnimg.cn/img_convert/2635e0a53f37de6a71092edbcb9df7c8.png)

### Docker网络管理命令

#### 查看帮助文档

```
docker network --help
```

![image-20220728103531652](https://img-blog.csdnimg.cn/img_convert/080650e39754817256f875e574850201.png)

#### 查看网络

```
docker network ls
```

![image-20220728103852636](https://img-blog.csdnimg.cn/img_convert/3c6e94ae24ae9d3b1901efbc01b4ca02.png)

#### 创建网络

```
docker network create
```

![image-20220728104125387](https://img-blog.csdnimg.cn/img_convert/fe8c0f41e5adc328cb7a7474149d20cb.png)

> -d 指定网络的驱动（不指定默认为bridge）
>
> --  subnet 指定子网网段（192.168.0.0/16）
>
> -- ip-range 指定容器的I范围
>
> -- gateway 子网的网关

#### 网络删除

```
docker network rm
```

![image-20220728105123725](https://img-blog.csdnimg.cn/img_convert/d661fc8ec44fcb6a7e49ab54db8fc2ab.png)

#### 查看网络详细信息

```
docker network inspect 
```

![image-20220728105257769](https://img-blog.csdnimg.cn/img_convert/5ee04037b0e0a3a328c184b6bc9e380f.png)

#### 使用网络

```
docker run/create --network 
```

> 默认情况下，docker创建或启动容器时，会默认使用名为bridge的网络

#### 网络连接与断开

```
docker network connect/disconnect 
```

![image-20220728105838471](https://img-blog.csdnimg.cn/img_convert/6e199f57e6d70218ae3d3c46024e0901.png)

### 单主机常见三种Docker网络类型

![image-20220729231934850](https://img-blog.csdnimg.cn/img_convert/2be886532787b3da9f2f3f24b07a8b2b.png)

Docker默认情况下有bridge、host、none三种网络类型

#### Centos容器镜像处理

##### Centos安装并启用ifconfig命令

```
yum provides ifconfig
```

![image-20220729233311573](https://img-blog.csdnimg.cn/img_convert/5a12193a12a6dc221d034e942486d8d6.png)

##### 运行ipconfig命令

![image-20220729235046317](https://img-blog.csdnimg.cn/img_convert/896e1fecf8c2bf43eb1d84d7be4a423c.png)

##### 拉取centos镜像

```
docker pull centos:centos7
```

##### 运行centos容器并启用ifconfig命令

```
docker run -it centos:centos7
yum install net-tools
```

![image-20220730003749530](https://img-blog.csdnimg.cn/img_convert/43d96a7ba9adc52a6745eb877b6887af.png)

##### 从容器中创建镜像

```
docker commit adoring_noyce centos-net
```

![image-20220730001949456](https://img-blog.csdnimg.cn/img_convert/b93dc82c356b8a4598356ea7f2dd115c.png)

![image-20220730002038580](https://img-blog.csdnimg.cn/img_convert/379383b327a0acec97d629062bb70222.png)

#### bridge网络模式

> 容器默认使用的网络类型
>
> - 宿主机上需要单独的bridge网卡，docker默认创建的是docker0
> - 容器之间、容器与主机之间的网络通信是借助为每一个容器生成的一对veth pair虚拟网络设备进行通信的(一个在容器上，另一个在宿主机上)
> - 每创建一个基于bridge网络的容器，都会自动在宿主机上创建一个veth*虚拟网络设备。 外部无法直接访问容器。需要建立端口映射才能访问
> - 容器借由veth虚拟设备通过如docker0这种bridge网络设备进行通信
> - 每一容器具有单独的IP
> - bridge网络模式下宿主机与容器服务使用的端口可以重复

![img](https://img-blog.csdnimg.cn/img_convert/0a6e4eb9b89cd14fb2741b5b6a671e8c.webp?x-oss-process=image/format,png)

##### 查看宿主机网络信息

![image-20220730005753492](https://img-blog.csdnimg.cn/img_convert/75047e571a74402f528dda3d56ae08c5.png)

##### 创建桥接模式容器

```
docker run -it centos-net   #由于容器创建默认使用桥接模式，所以这里不需要使用--network指定
```

![image-20220730005943098](https://img-blog.csdnimg.cn/img_convert/a3de866165e449fdec1224153babf3e5.png)

> bridge网络模式下的端口映射：由于bridge网络模式的上述特点，访问bridge网络模式的设备时需要端口映射

##### 端口映射格式

```
docker run -p
```

![image-20220730010611166](https://img-blog.csdnimg.cn/img_convert/68dd303cc9d3ebbc898bc5d720b2806e.png)

#### host网络模式

> 容器完全共享宿主机的网络，网络没有隔离，可以说宿主机的网络就是容器的网络
>
> - 容器、宿主机上的应用所使用的端口不能重复
> - 外部能够直接访问容器，不需要端口映射
> - 容器ip就是宿主机的ip

![img](https://img-blog.csdnimg.cn/img_convert/71940ccf7233f016e59b7a8d29428ca4.webp?x-oss-process=image/format,png)

```
docker run -it --network=host centos-net
```

![image-20220730011055805](https://img-blog.csdnimg.cn/img_convert/2d6d9e0c541b242deb12a5a8a1556c26.png)

#### none网络模式

> 容器上没有网络，无需任何网络设备，外部无法访问

```
docker run -it --network=none centos-net
```

![image-20220730004641932](https://img-blog.csdnimg.cn/img_convert/d541690715d041bbdc06f2e0c599c358.png)

> 只有一个本地回环lo网络设备

### 自定义网络

#### 创建网络

```
docker network create --driver bridge test
```

![image-20220730012139747](https://img-blog.csdnimg.cn/img_convert/b78fcb487b9ce0ef955268b5d6828b92.png)

#### 自定义网络创建容器

```
docker run -it --network=test centos-net
```

![image-20220730012749262](https://img-blog.csdnimg.cn/img_convert/fa53f37df23a85247d151f5d783ad67b.png)

成功分配网络内的ip地址（注意不同的网络之间是隔离的）

![image-20220730014800371](https://img-blog.csdnimg.cn/img_convert/3112346b7e835df793b05e0f5d4a04ab.png)

不同的网络是相互隔离的，无法进行通信

![image-20220730013923962](https://img-blog.csdnimg.cn/img_convert/06c3dcaec68c83c394369061b763dc33.png)

#### 网络连接

```
docker network connect test 881852a511a6
```

![image-20220730014615772](https://img-blog.csdnimg.cn/img_convert/7f7d9d7c1166d22cdd28d3e8719354d6.png)

#### 连接测试

```
docker attach 
```

![image-20220730020059128](https://img-blog.csdnimg.cn/img_convert/020d377599638397befaeeeb9afdbd3f.png)

### 容器通信

#### 同一网络容器通信

> 创建两个容器使用同一个网络，在同一个子网分配ip地址，可以相互访问
>
> 因此只要保证两个容器处于同一个网络中，就可以直接通过容器的ip地址在容器间进行通信

![image-20220730115943556](https://img-blog.csdnimg.cn/img_convert/1fb1cc1e6ce0c0f82ee37b028832dbd9.png)

#### Docker域名解析服务器

> 在大多数场景下，容器部署的ip地址是自动分配的，因此无法提前知道ip地址
>
> 我们可以借助Docker提供的DNS服务器，将域名解析成对应容器的ip地址，这样就直接访问域名来实现通信（注意：域名解析只能在用户自定义网络下生效，默认网络是不行的）

```
docker run -it --name=test01 --network=test centos-net 
docker run -it --name=test02 --network=test centos-net
```

#### ![image-20220730121326930](https://img-blog.csdnimg.cn/img_convert/a180b2561bf774d950466a3ee52fd30c.png)

#### 容器共享网络

创建并运行一个新的容器，将其网络指定一个容器的网络，使得两个容器使用同一个网络

```
docker run -it --name=test03 --network=container:test01 centos-net
```

![image-20220730121726308](https://img-blog.csdnimg.cn/img_convert/aa2597ff4391dd7ed108a12829927d7b.png)

### 容器和外部通信

> 容器创建默认使用桥接模式，桥接模式创建一个虚拟网络，让容器在虚拟网络中，通过桥接器与外界相连。其中，数据包从容器内部的网络搭到达宿主主机再发送网络的过程最关键的是依靠NATj将地址进行转换，再利用宿主机的ip地址将数据包转发出去。

![image-20220702232449520](https://img-blog.csdnimg.cn/img_convert/efa041e60beb97b4d37b5494d8e48446.png)

通过NAT，容器可以实现访问外界，但是当外部主动连接容器时，我们可以直接创建容器时配置端口映射

```
 docker run --name nginx -p 8080:80 -d nginx
```

> -p：端口映射配置，将容器对外提供访问的端口映射搭到宿主机的端口上
>
> -p 宿主端口:容器端口
>
> 当外部访问到宿主主机的对应端口时会直接转发给容器内