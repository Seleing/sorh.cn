# Docker 详解

> Docker 学习

- Docker概述

- Docker安装

- Docker命令

  镜像命令

  容器命令

  操作命令

- Docker镜像

- 容器数据卷

- DockerFile！

- Docker网络原理

- IDEA整合Docker

- Docker Compose!

- Docker Swarm! 

- CI\CD Jenkins!

  

# Docker概述

  ## Docker为什么出现

---

一款产品： 开发-上线  两套环境！应用环境，应用配置！

开发  ---- 运维，问题：我在我的电脑上可以运行！版本更新，导致服务不可用！对于运维来说，考验十分大？

环境配置是十分麻烦，每一个机器都要部署（集群Redis,ES,Hadoop....）! 费时费力。

发布一个项目(jar + (Redis Mysql JDK ES)),项目能不能带上环境安装打包！

之前在服务器配置一个应用的环境 Redis Mysql JDK ES Hadoop,配置超麻烦，不能跨平台使用。

windows 上先测试，最后发布到Linux !

传统：开发 打包 jar ，运维来做！

现在：开发打包部署上线，一套流程做完！

java -- apk -- 发布（应用商店）-- 使用apk -- 安装即可用

java -- jar (环境) -- 打包项目并附带上运行环境 (镜像) -- (Docker: 商店) -- 下载发布的镜像 -- 直接运行即可！

Docker 给以上的问题，提出了解决方案！

![image-20210419153002866](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210419153002866.png)
		Docker的思想来源与集装箱！

JRE -- 多个应用(端口冲突) -- 原来都是交叉的！

隔离：Docker核心思想！打包箱子！每个箱子都是互相隔离的。

Docker通过隔离机制，可以将服务器利用到极致！



本质：<span style='color:red'>所有的技术都是因为出现了一些问题，我们需要去解决，才去学习！</span>

## Docker 历史

---

2010年，几个的年轻人，就在美国成立了一家公司 `dotcloud`

做一些pass的云计算服务！LXC（Linux Container容器）有关的容器技术！

> Linux Container容器是一种内核虚拟化技术，可以提供轻量级的虚拟化，以便隔离进程和资源。

他们将自己的技术（容器化技术）命名就是 Docker!

Docker刚刚延生的时候，没有引起行业的注意！`dotCloud`，就活不下去！

开源

2013年，Docker开源！

越来越多的人发现docker的优点！火了。Docker每个月都会更新一个版本！

2014年4月9日，Docker1.0发布！

docker为什么这么火？十分的轻巧！

在容器技术出来之前，我们都是使用虚拟机技术！

虚拟机：在window中装一个VMware，通过这个软件我们可以虚拟出来一台或者多台电脑！笨重！

虚拟机也属于虚拟化技术，Docker容器技术，也是一种虚拟化技术！

```shell
1 | vm : linux centos 原生镜像（一个电脑！） 隔离、需要开启多个虚拟机！ 几个G 几分钟
2 | docker: 隔离，镜像（最核心的环境 4m + jdk + mysql）十分的小巧，运行镜像就可以了！小巧！ 几个M 秒级启动！

```

> 聊聊Docker

 Docker基于Go语言开发的！开源项目！

docker官网：https://www.docker.com/

![image-20210419161414645](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210419161414645.png)
		文档：https://docs.docker.com/ Docker的文档是超级详细的！

仓库：https://hub.docker.com/

## Docker能干什么

---

> 之前的虚拟化技术！

![image-20210419161525620](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210419161525620.png)

``` shell
虚拟出来一台电脑，把用的App放在虚拟出来的电脑上，浪费资源！

虚拟机技术缺点：
1.资源占用十分多！
2.冗余步骤多！
3.启动很慢！

```

> 容器化技术

容器化技术不是模拟一个完整的操作系统

![image-20210419162038845](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210419162038845.png)

```shell
将每一个App 隔离出来，防止因交叉问题导致体统崩溃从而使整个系统崩塌，
```

比较Docker 和虚拟机技术的不同：

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了
- 每个容器间是互相隔离，每个容器内都有一个属于自己的文件系统，互不影响

> DevOps（开发、运维）

**应用更快速的交付和部署 **

传统：一堆帮助文档，安装程序

Docker : 打包镜像发布测试，一键运行

**更快捷的升级和扩缩容**

使用Docker之后，部署应用就和搭积木一样！

项目打包为一个镜像，扩展 服务器A! 只需要在服务器B上下在镜像！

**更加简单的系统运维**

在容器化之后，我门的开发，测试环境都是高度一致的。

**更高效的计算资源利用**

Docker 使内核级别的虚拟化，可以在一个物理机上可以运行很多的容器实例！服务器的性能可以被压榨到极致.

# Docker 安装

## Docker的基本组成

---

![image-20210419170110020](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210419170110020.png)
		**镜像(image):**

docker镜像就好比是一个目标，可以通过这个目标来创建容器服务，tomcat镜像==>run==>容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

**容器(container):**

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的.
		启动，停止，删除，基本命令
		目前就可以把这个容器理解为就是一个简易的 Linux系统。

**仓库(repository):**

仓库就是存放镜像的地方！
		仓库分为公有仓库和私有仓库。(很类似git)
		Docker Hub是国外的。
		阿里云…都有容器服务器(配置镜像加速!)

## 安装Docker

---

> 环境准备

1.需要一点的Linux的基础
		2.CentOS 7
		3.使用FinalShell 链接远程服务器进行操作

> 环境查看

``` shell
# Linux要求内核3.10以上
[root@Kyushu /]# uname -r
3.10.0-1127.el7.x86_64
```

``` shell
# 系统版本
[root@Kyushu /]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

> 安装

帮助文档：https://docs.docker.com/engine/install/

``` shell
#1.卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
#2.需要的安装包
[root@Kyushu /]# yum install -y yum-utils
#3.设置镜像的仓库
[root@Kyushu /]# yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
#默认是从国外的，不推荐
#推荐使用国内的
[root@Kyushu /]# yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
#更新yum软件包索引
[root@Kyushu /]# yum makecache fast
#4.安装docker相关的 docker-ce 社区版 而ee是企业版
[root@Kyushu /]# yum install docker-ce docker-ce-cli containerd.io
#5、启动docker
[root@Kyushu /]# docker systemctl start docker
#6. 使用docker version查看是否按照成功
[root@Kyushu /]# docker version
```
![image-20210420120342237](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420120342237.png)

```shell
#7. 测试
[root@Kyushu /]# docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

![image-20210420121010072](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420121010072.png)

```shell
#8.查看一下下载的镜像 是否存在
[root@Kyushu /]# docker images         
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
hello-world           latest              bf756fb1ae65        4 months ago        13.3kB
```



了解：卸载docker

```shell
#1. 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
#2. 删除资源
rm -rf /var/lib/docker
# /var/lib/docker 是docker的默认工作路径！
```

## 阿里云镜像加速

1、登录阿里云找到容器服务

![image-20210420140914150](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420140914150.png)
2、找到镜像加速器

![image-20210420140948882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420140948882.png)
3、配置使用

![image-20210420141032689](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420141032689.png)

## **Docker run 流程图**

![image-20210420140658176](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420140658176.png)

## 底层原理

Docker**是怎么工作的**？

Docker是一个Client-Server结构的系统4，Docker的守护进程运行在主机上。通过Socket从客户端访问！

Docker-Server接收到Docker-Client的指令，就会执行这个命令！

![image-20210420142311334](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420142311334.png)
		**为什么Docker比Vm快**

1、docker有着比虚拟机更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。
		2、docker利用的是宿主机的内核,而不需要Guest OS。

![image-20210420142537795](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420142537795.png)

因此,当新建一个 容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引导、加载操作系统内核返个比较费时费资源的过程,当新建一个虚拟机时,虚拟机软件需要加载GuestOS,返个新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统,则省略了这个复杂的过程,因此新建一个docker容器只需要几秒钟。

|            | Docker容器  | LXC         |     VM     |
| ---------- | ----------- | ----------- | :--------: |
| 虚拟化类型 | OS虚拟化    | OS虚拟化    | 硬件虚拟化 |
| 性能       | =物理机性能 | =物理机性能 | 5%-20%损耗 |
| 隔离性     | NS隔离性    | NS隔离性    |     强     |
| Qos        | Cgroup弱    | Cgroup弱    |     强     |
| 安全性     | 中          | 差          |     强     |
| GuestOS    | 全部        | 只支持Linux |    全部    |



# Docker常用命令

## 帮助命令

---

```shell
docker version    #显示docker的版本信息。
docker info       #显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help #帮助命令
```

帮助文档的地址：https://docs.docker.com/engine/reference/commandline/

## 镜像命令

---

```shell
docker images # 查看所有本地主机上的镜像 可以使用docker image ls代替

docker search # 搜索镜像

docker pull # 下载镜像 docker image pull

docker rmi # 删除镜像 docker image rm
```

**docker images** 查看所有本地的主机上的镜像

```shell
[root@hadoop1 /]# docker images
REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
hello-world              latest    d1165f221234   6 weeks ago     13.3kB

# 解释
REPOSITORY	镜像的仓库源
TAG			镜像的标签
IMAGE ID	镜像的id
CREATED		镜像的创建时间
SIZE		镜像的大小

# 可选项
	-a, --all		# 列出所有的镜像
	-q,	--quiet		# 只显示镜像的id
```

**docker search  搜索镜像**

```shell
[root@hadoop1 /]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10768     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4056      [OK]       

# 可选项， 通过搜藏来过滤
	--filter=STARS=500		# 搜索出来的镜像就是STARS大于500的

[root@hadoop1 /]# docker search mysql --filter=STARS=500
NAME                 DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                MySQL is a widely used, open-source relation…   10768     [OK]       
mariadb              MariaDB Server is a high performing open sou…   4056      [OK]       
mysql/mysql-server   Optimized MySQL Server Docker images. Create…   792                  [OK]
percona              Percona Server is a fork of the MySQL relati…   533       [OK] 
```

**docker pull 下载镜像**

```shell
# 下载镜像 docker pull 镜像名[:tag]
[root@hadoop1 /]# docker pull tomcat:8
8: Pulling from library/tomcat #如果不写tag，默认就是latest
90fe46dd8199: Already exists   #分层下载： docker image 的核心 联合文件系统
35a4f1977689: Already exists 
bbc37f14aded: Already exists 
74e27dc593d4: Already exists 
93a01fbfad7f: Already exists 
1478df405869: Pull complete 
64f0dd11682b: Pull complete 
68ff4e050d11: Pull complete 
f576086003cf: Pull complete 
3b72593ce10e: Pull complete 
Digest: sha256:0c6234e7ec9d10ab32c06423ab829b32e3183ba5bf2620ee66de866df640a027  # 签名 防伪
Status: Downloaded newer image for tomcat:8
docker.io/library/tomcat:8 #真实地址

#等价于
docker pull tomcat:8
docker pull docker.io/library/tomcat:8

# 指定版本下载
[root@hadoop1 /]# docker pull mysql:5.7
5.7: Pulling from library/mysql
5b54d594fba7: Already exists
07e7d6a8a868: Already exists
abd946892310: Already exists
dd8f4d07efa5: Already exists
076d396a6205: Already exists
cf6b2b93048f: Already exists
530904b4a8b7: Already exists
a37958cbebcf: Pull complete
04960017f638: Pull complete
e1285def0d2a: Pull complete
670cb3a9678e: Pull complete
Digest: sha256: e4d39b85118358ffef6adc5e8c7d00e49d20b25597e6ffdc994696f10e3dc8e2
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

**docker rmi 删除镜像**

```shell
[root@hadoop1 /]# docker rmi -f 镜像id #删除指定的镜像
[root@hadoop1 /]# docker rmi -f 镜像id 镜像id 镜像id 镜像id#删除指定的镜像
[root@hadoop1 /]# docker rmi -f $(docker images -aq) #删除全部的镜像
```

## 容器命令

---

```shell
docker run 			# 镜像id 新建容器并启动

docker ps 			# 列出所有运行的容器 docker container list

docker rm 			# 容器id 删除指定容器

docker start 容器id 	#启动容器
docker restart 容器id #重启容器
docker stop 容器id 	#停止当前正在运行的容器
docker kill 容器id 	#强制停止当前容器
```

**说明：我们有了镜像才可以创建容器，Linux，下载centos镜像来学习**

**新建容器并启动**

```shell
[root@hadoop1 /]# docker run [可选参数] image

#参书说明
--name="Name"		容器名字 tomcat01 tomcat02 用来区分容器
-d					后台方式运行
-it 				使用交互方式运行，进入容器查看内容
-p					指定容器的端口 -p 8080(宿主机):8080(容器)
		-p ip:主机端口:容器端口
		-p 主机端口:容器端口(常用)
		-p 容器端口
		容器端口
-P(大写) 				随机指定端口
# 测试、启动并进入容器
[root@hadoop1 /]# docker run -it centos :7 /bin/bash
[root@2ceed1979498 /]# ls
anaconda-post.log  bin  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

[root@95039813da8d /]# exit #从容器退回主机
exit
[root@hadoop1 /]# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  wsm

```

**列出所有的运行的容器**

```shell
#docker ps命令 
	# 列出当前正在运行的容器
-a 	# 列出当前正在运行的容器+带出历史运行过的容器
-n=? 	# 显示最近创建的容器
-q	# 只显示容器的编号

[root@hadoop1 /]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

[root@hadoop1 /]# docker ps -a
CONTAINER ID   IMAGE         COMMAND             CREATED         STATUS                      PORTS     NAMES
2ceed1979498   centos:7      "/bin/bash"         6 minutes ago   Exited (0) 5 minutes ago              fervent_mahavira
41524bab356d   hello-world   "/hello"            4 hours ago     Exited (0) 4 hours ago                agitated_boyd
03a876eddd2b   tomcat        "catalina.sh run"   28 hours ago    Exited (143) 26 hours ago             tomcat03
89d4bfa7cac3   tomcat        "catalina.sh run"   28 hours ago    Exited (143) 26 hours ago             tomcat02
5b519503be73   tomcat        "catalina.sh run"   29 hours ago    Exited (143) 26 hours ago             tomcat01

  
[root@hadoop1 /]# docker ps -aq
2ceed1979498
41524bab356d
03a876eddd2b
89d4bfa7cac3
5b519503be73

```

**退出容器**

```shell
exit	# 直接容器停止并退出
Ctrl + P + Q # 容器不停止退出
```

**删除容器**

```shell
docker rm 容器id   #删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -rf
docker rm -f $(docker ps -aq)  #删除指定的容器
docker ps -a -q|xargs docker rm  #删除所有的容器
```

**启动和停止容器的操作**

```shell
docker start 容器id	#启动容器
docker restart 容器id	#重启容器
docker stop 容器id	#停止当前正在运行的容器
docker kill 容器id	#强制停止当前容器
```

## 常用其他命令

---

**后台启动命令**

```shell
# 命令 docker run -d 镜像名！
[root@hadoop1 /]# docker run -d centos

# 问题docker ps， 发现 centos 停止了
# 常见的坑： docker 容器使用后台运行，就必须要有一个前台进程，docker 发现没有应用， 就会停止
# nginx, 容器启动后， 发现自己没有提供服务，就会立刻停止，就是没有程序了

```

**查看日志**

``` shell
docker logs -f -t --tail 容器，没有日志

# 自己编写一段shell脚本
[root@hadoop1 /]# docker run -d centos /bin/bash -c "while true;do echo hadoop;sleep 1;done"

[root@hadoop1 /]# docker ps
CONTAINER ID		IMAGE
a82d4a6638ae		centos

# 显示日志
-tf			# 显示日志
--tail		# 要显示日志的条数
[root@hadoop1 /]# docker logs -tf --tail 10 a82d4a6638ae

```

**查看容器中进程信息  ps**

```shell
# 命令 docker top 容器id
[root@hadoop1 /]# docker top a82d4a6638ae
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                14781               14761               0                   17:21               pts/0               00:00:00            /bin/bash
		
```

**查看镜像的元数据**

```shell
# 命令 docker inspect 容器 id

# 测试
[root@hadoop1 /]# docker inspect a82d4a6638ae
[
    {
    	# 容器镜像id 是 这个全路径id 的缩写
        "Id": "a82d4a6638ae80073111d60c29c7a5fc21117ffeb19c7fda093d087ac87b889f",
        "Created": "2021-04-20T09:21:30.560356018Z", # 创建时间
        "Path": "/bin/bash", # 默认bash 控制台
        "Args": [], # 传递的参数
        "State": { # 容器当前属性
            "Status": "running", # 当前状态 运行状态
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 14781, # 父进程的id
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-04-20T09:21:30.880517787Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        # 从那个镜像过来的
        "Image": "sha256:7e6257c9f8d8d4cdff5e155f196d67150b871bbe8c02761026f803a704acb3e9",
        "ResolvConfPath": "/var/lib/docker/containers/a82d4a6638ae80073111d60c29c7a5fc21117ffeb19c7fda093d087ac87b889f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/a82d4a6638ae80073111d60c29c7a5fc21117ffeb19c7fda093d087ac87b889f/hostname",
        "HostsPath": "/var/lib/docker/containers/a82d4a6638ae80073111d60c29c7a5fc21117ffeb19c7fda093d087ac87b889f/hosts",
        "LogPath": "/var/lib/docker/containers/a82d4a6638ae80073111d60c29c7a5fc21117ffeb19c7fda093d087ac87b889f/a82d4a6638ae80073111d60c29c7a5fc21117ffeb19c7fda093d087ac87b889f-json.log",
        "Name": "/boring_heyrovsky",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/6d4e11b5377ea22e94a5d62b8320263b88b123f668313ef230d74d4775982874-init/diff:/var/lib/docker/overlay2/617b497164873d7e816399364f69e0d0927b0e4f6774887aaa8d99d5c6ae5e70/diff",
                "MergedDir": "/var/lib/docker/overlay2/6d4e11b5377ea22e94a5d62b8320263b88b123f668313ef230d74d4775982874/merged",
                "UpperDir": "/var/lib/docker/overlay2/6d4e11b5377ea22e94a5d62b8320263b88b123f668313ef230d74d4775982874/diff",
                "WorkDir": "/var/lib/docker/overlay2/6d4e11b5377ea22e94a5d62b8320263b88b123f668313ef230d74d4775982874/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [], # 挂载内容
        "Config": {
            "Hostname": "a82d4a6638ae", # 默认主机名
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [ # 基本环境命令
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [ # cmd的命令
                "/bin/bash"
            ],
            "Image": "centos:7",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS",
                "org.opencontainers.image.created": "2020-08-09 00:00:00+01:00",
                "org.opencontainers.image.licenses": "GPL-2.0-only",
                "org.opencontainers.image.title": "CentOS Base Image",
                "org.opencontainers.image.vendor": "CentOS"
            }
        },
        "NetworkSettings": { # 关于网络的命令
            "Bridge": "",
            "SandboxID": "cfb64034d61e1aff836ccc3de0e176214edd2f81df74d1169b7d7ccb56a2358f",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/cfb64034d61e",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "a6a42bc091ffd5d6286b002208b3f75d57ad9b7227ca9feb31de1dac94a7eae4",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": { # 桥接网卡
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "13aee592285d93d1c957239a3e572ec97531acd3737071ac3ed5187af4360336",
                    "EndpointID": "a6a42bc091ffd5d6286b002208b3f75d57ad9b7227ca9feb31de1dac94a7eae4",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]

```

**进入当前正在运行的容器**

```shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令 
docker exec -it 容器id bashshell

# 测试
[root@hadoop1 /]# docker ps
CONTAINER ID   IMAGE      COMMAND       CREATED          STATUS          PORTS     NAMES
a82d4a6638ae   centos:7   "/bin/bash"   15 minutes ago   Up 15 minutes             boring_heyrovsky
[root@hadoop1 /]# docker exec -it a82d4a6638ae /bin/bash
[root@a82d4a6638ae /]# ls
anaconda-post.log  bin  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@a82d4a6638ae /]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 09:21 pts/0    00:00:00 /bin/bash
root         15      0  0 09:38 pts/1    00:00:00 /bin/bash
root         30     15  0 09:38 pts/1    00:00:00 ps -ef

# 方式二
docker attach 容器id
#测试
[root@hadoop1 /]# docker attach a82d4a6638ae
正在执行当前的代码...
区别
#docker exec #进入当前容器后开启一个新的终端，可以在里面操作。（常用）
#docker attach # 进入容器正在执行的终端
```

**从容器内拷贝到主机上**

```shell
docker cp 容器id:容器内路径   主机目的路径
# 查看当前主机目录下
[root@hadoop1 home]# ls
elsearch  sxz.java  test  tomcat
[root@hadoop1 home]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED    STATUS    PORTS     		NAMES
80e8a7cf812b   centos    "/bin/sh" 10 minutes ago        Up 10 minutes  bold_bell

# 进入docker容器内部
[root@80e8a7cf812b home]# ls
sxz.java
[root@80e8a7cf812b home]# exit
exit
[root@hadoop1 home]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@hadoop1 home]# docker ps -a
CONTAINER ID   IMAGE      COMMAND       CREATED              STATUS                     PORTS     NAMES
80e8a7cf812b   centos:7   "/bin/bash"   About a minute ago   Exited (0) 9 seconds ago             nostalgic_goodall
# 将文件拷贝到主机上
[root@hadoop1 home]# docker cp 80e8a7cf812b:/home/sxz.java /home
[root@hadoop1 home]# ls
elsearch  sxz.java  test  tomcat

# 拷贝是一个手动过程， 未来我们使用 -v 卷的技术，可以实现

```

## 小结

![image-20200831184312339.png](https://www.yaconit.com/md/server/docker/base/resources/image-20200831184312339.png)


```shell
 attach      Attach local standard input, output, and error streams to a running container
  #当前shell下 attach连接指定运行的镜像
  build       Build an image from a Dockerfile # 通过Dockerfile定制镜像
  commit      Create a new image from a container's changes #提交当前容器为新的镜像
  cp          Copy files/folders between a container and the local filesystem #拷贝文件
  create      Create a new container #创建一个新的容器
  diff        Inspect changes to files or directories on a container's filesystem #查看docker容器的变化
  events      Get real time events from the server # 从服务获取容器实时时间
  exec        Run a command in a running container # 在运行中的容器上运行命令
  export      Export a container's filesystem as a tar archive #导出容器文件系统作为一个tar归档文件[对应import]
  history     Show the history of an image # 展示一个镜像形成历史
  images      List images #列出系统当前的镜像
  import      Import the contents from a tarball to create a filesystem image #从tar包中导入内容创建一个文件系统镜像
  info        Display system-wide information # 显示全系统信息
  inspect     Return low-level information on Docker objects #查看容器详细信息
  kill        Kill one or more running containers # kill指定docker容器
  load        Load an image from a tar archive or STDIN #从一个tar包或标准输入中加载一个镜像[对应save]
  login       Log in to a Docker registry #
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```

## 安装步骤

> 安装nginx

```shell
#1. 搜索镜像 search 建议大家去docker搜索，可以看到帮助文档
 docker search nginx
#2. 拉取镜像 pull
docker pull nginx
#3、运行测试
# -d 后台运行
# --name 给容器命名
# -p 宿主机端口：容器内部端口
# 结尾跟着要启动的容器镜像
[root@hadoop1 home]# docker run -d --name nginx01 -p 5555:80 nginx
d59369a381d8cb7f95dce0d8cd41a3478f1d4548e061e113e2b870a776be927b
[root@hadoop1 home]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
d59369a381d8   nginx     "/docker-entrypoint.…"   55 seconds ago   Up 54 seconds   0.0.0.0:5555->80/tcp, :::5555->80/tcp   nginx01
# curl 用于内部测试用的命令
[root@hadoop1 home]# curl localhost:5555

# 进入容器
[root@hadoop1 home]# docker exec -it nginx01 /bin/bash
root@a82d4a6638ae:/#  whereis nginx
nginx: /usr/sbin/nginx .......
root@a82d4a6638ae:/# cd /etc/nginx
root@a82d4a6638ae:/etc/nginx# ls
conf.d ......


```

**端口暴露概念**

![image-20210423230537708](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210423230537708.png)
思考问题：我们每次改动nginx配置文件，都需要进入容器内部？十分的麻烦，要是可以在容器外部提供一个映射路径，达到在容器修改文件名，容器内部就可以自动修改？√数据卷！

> 安装tomcat

```shell
# 官方的使用
docker run -it --rm tomcat:9.0
# 之前的启动都是后台，停止了容器，容器还是可以查到， docker run -it --rm image 一般是用来测试，用完就删除
--rm       Automatically remove the container when it exits
#下载
docker pull tomcat
#启动运行
docker run -d -p 8080:8080 --name tomcat01 tomcat
#测试访问有没有问题
curl localhost:8080

#进入容器
➜  ~ # docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
db09851cf82e        tomcat              "catalina.sh run"   28 seconds ago      Up 27 seconds       0.0.0.0:8080->8080/tcp   tomcat01
➜  ~ # docker exec -it db09851cf82e /bin/bash             
root@db09851cf82e:/usr/local/tomcat# 
# 发现问题：1、linux命令少了。 2.没有webapps 阿里云镜像的原因，默认是最小镜像，所以不必要的都提出掉
# 保证最小可运行的环境
```

思考问题：我们以后要部署项目，如果每次都要进入容器是不是十分麻烦？要是可以在容器外部提供一个映射路径，webapps，我们在外部放置项目，就自动同步内部就好了！

> es + kibana

```shell
# es 暴露的端口很多！
# es 非常耗内存
# es 的数据一般需要放置到安全目录！挂载
# --net somenetwork ? 网络配置 


# 启动elasticsearch 如果没有会自动pull 当前最新版本7.6.2
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2


# 启动es后就会瞬间占用大量内存，并导致服务器卡顿， 
# docker stats 查看 cpu 的状态
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O        PIDS
88a37faa25b0   elasticsearch   0.27%     1.281GiB / 2.691GiB   47.61%    1.63kB / 942B   312MB / 1.77MB   43
^C


# 当服务器内存比较小时，又比较想用es，需要增加内存限制
# 修改es配置文件 -e 环境配置修改
docker run -d --name elasticsearch01 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms128m -Xmx512M" elasticsearch:7.6.2
# 更改内存配置后的cpu状态
CONTAINER ID   NAME              CPU %     MEM USAGE / LIMIT     MEM %     NET I/O     BLOCK I/O     PIDS
bb4e15b456b2   elasticsearch01   98.48%    336.3MiB / 2.691GiB   12.20%    656B / 0B   16.4kB / 0B   26
^C

```

```shell
[root@hadoop1 ~]# curl localhost:9200
{
  "name" : "bb4e15b456b2",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "vVuHciihSo-YN-vWbYReKg",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

```

作业：使用kibana连接es？思考网络如何才能连接

![image-20210424094735438](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424094735438.png)

## 可视化

**portainer(初学先用这个)**

```shell
docker run -d -p 8080:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer


```

**Rancher(CI/CD后面在用这个)**

### 什么是portainer?

Docker图形化界面管理工具！提供一个后台面板供我们操作！

```shell
# 通过内网9000外网8088 
# --restart 启动方式
# -v 挂载 将容器内部的东西挂载到主机上
# --privileged 授权
# 最后就是安装
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer

```

初次访问：

![image-20210424101414779](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424101414779.png)

注册完成后再次访问：

![image-20210424101445178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424101445178.png)
选择仓库，一般选择本地仓库：

![image-20210424101633738](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424101633738.png)
进入之后的面板界面：

![image-20210424101814022](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424101814022.png)

# Docker镜像讲解

## 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，他包含运行某个软件所需的所有内容，包括代码、运行时库、环境变量和配置文件
		所有的应用，直接打包docker镜像，就可以直接跑起来！
		如何得到镜像:

- 从远程仓库下载
- 朋友拷贝给你
- 自己制作一个镜像DockerFile

## Docker镜像加载原理

> UnionFs （联合文件系统）

UnionFs（联合文件系统）：Union文件系统（UnionFs）是一种分层、轻量级并且高性能的文件系统，他支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（ unite several directories into a single virtual filesystem)。Union文件系统是 Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像
特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

> Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。
boots(boot file system）主要包含 bootloader和 Kernel, bootloader主要是引导加 kernel, Linux刚启动时会加bootfs文件系统，在 Docker镜像的最底层是 boots。这一层与我们典型的Linux/Unix系统是一样的，包含boot加載器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由 bootfs转交给内核，此时系统也会卸载bootfs。
rootfs（root file system),在 bootfs之上。包含的就是典型 Linux系统中的/dev,/proc,/bin,/etc等标准目录和文件。 rootfs就是各种不同的操作系统发行版，比如 Ubuntu, Centos等等。

![image-20210426202954453](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426202954453.png)

平时我们安装进虚拟机的CentOS都是好几个G，为什么Docker这里才200M？

![image-20210426203019718](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426203019718.png)
对于个精简的OS,rootfs可以很小，只需要包合最基本的命令，工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的Linux发行版， boots基本是一致的， rootfs会有差別，因此不同的发行版可以公用bootfs.

虚拟机是分钟级别，容器是秒级！

## 分层理解

> 分层镜像

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层层的在下载

![image-20210426210436577](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426210436577.png)

**思考：为什么Docker镜像要采用这种分层的结构呢？**

最大的好处，我觉得莫过于资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。

查看镜像分层的方式可以通过docker image inspect 命令

```shell
[root@hadoop1 ~]# docker image inspect redis
[
    {
        "Id": "sha256:739b59b96069d2edfa60f66b5efb4960e7129053ccbb2ac5fd1c9351e7731918",
        "RepoTags": [
            "redis:latest"
        ],
        "RepoDigests": [
            "redis@sha256:e10f55f92478715698a2cef97c2bbdc48df2a05081edd884938903aa60df6396"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2021-04-20T23:46:01.04261128Z",
        "Container": "88b3715d12efac631774aebef922d5cb0abb62ca233e4c626ebd26c81e5a4408",
        "ContainerConfig": {
            "Hostname": "88b3715d12ef",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.2.2",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.2.2.tar.gz",
                "REDIS_DOWNLOAD_SHA=7a260bb74860f1b88c3d5942bf8ba60ca59f121c6dce42d3017bed6add0b9535"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"redis-server\"]"
            ],
            "Image": "sha256:ae6907a3adb277fada24702893484644ffc8de5cdedae7a9baa7d9fb5350b5d4",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "19.03.12",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.2.2",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.2.2.tar.gz",
                "REDIS_DOWNLOAD_SHA=7a260bb74860f1b88c3d5942bf8ba60ca59f121c6dce42d3017bed6add0b9535"
            ],
            "Cmd": [
                "redis-server"
            ],
            "Image": "sha256:ae6907a3adb277fada24702893484644ffc8de5cdedae7a9baa7d9fb5350b5d4",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 105375876,
        "VirtualSize": 105375876,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/7e9e22d9bbdd67fe2f438e56f7fe378141a09644cba8d1b90ce72c382d995c69/diff:/var/lib/docker/overlay2/b323ec467fa49f15b11d37dfd13e57dc2d57a6fcd1341ad627fab21244aafe39/diff:/var/lib/docker/overlay2/92cacd913ebd5cb144fe86dd3950614527a6bfd6c26d55a92d566469e5b9dcf4/diff:/var/lib/docker/overlay2/d75e9cd39d7d121ac2e29f563fbc7a62e0975c0394664b10723908f2ac3a167f/diff:/var/lib/docker/overlay2/a94a39eea44856b0ad420b4b5485ef931172495f64d18eb562f2791dcb4de525/diff",
                "MergedDir": "/var/lib/docker/overlay2/01db3fcfc54b3b18b06f647671db8ea5f2e0283ef91430045901aac21606cd15/merged",
                "UpperDir": "/var/lib/docker/overlay2/01db3fcfc54b3b18b06f647671db8ea5f2e0283ef91430045901aac21606cd15/diff",
                "WorkDir": "/var/lib/docker/overlay2/01db3fcfc54b3b18b06f647671db8ea5f2e0283ef91430045901aac21606cd15/work"
            },
            "Name": "overlay2"
        },
        "RootFS": { # 分层卷
            "Type": "layers",
            "Layers": [ # 每一行就是一层，如果之前下载过这一层就会跳过该层，下在没有下载过的
                "sha256:7e718b9c0c8c2e6420fe9c4d1d551088e314fe923dce4b2caf75891d82fb227d",
                "sha256:89ce1a07a7e4574d724ea605b4877f8a73542cf6abd3c8cbbd2668d911fa5353",
                "sha256:9eef6e3cc2937e452b2325b227ca28120a70481be25404ed9aad27fa81219fd0",
                "sha256:6eeff2593e885a7801c56f15be93e3d9968a9d3afb03df7602a89357c2d28c4d",
                "sha256:0302cabf4dde0b7c18a0cc372cd7e1038656815bd9300aeb64d7033faf77700a",
                "sha256:35613d8e624ae292403c8e291353c3b654729c92963a7ae88be11684522a77db"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]

```

**理解：**

所有的 Docker镜像都起始于一个基础镜像层，当进行修改或培加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，
就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创健第三个镜像层该像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点.

![image-20210426210809128](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426210809128.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。

![image-20210426210825920](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426210825920.png)

上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件
下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版。

![image-20210426210843698](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426210843698.png)

文种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统

Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的
件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。

Docker在 Windows上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW [1]。

下图展示了与系统显示相同的三层镜像。所有镜像层堆并合井，对外提供统一的视图。

![image-20210426210903286](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426210903286.png)

> 特点

Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

![image-20210426210932637](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426210932637.png)

## commit镜像

```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[版本TAG]
```

实战测试

```shell
# 1、启动一个默认的tomcat
[root@hadoop1 ~]# docker run -d -p 8080:8080 tomcat
0b497f614bf6002412729324c265e05bc79a69e6753c77e172cc6a0cafb8ecf9

# 2、发现这个默认的tomcat 是没有webapps应用，官方的镜像默认webapps下面是没有文件的！
#docker exec -it 容器id /bin/bash
[root@hadoop1 ~]# docker exec -it 0b497f614bf6 /bin/bash
root@0b497f614bf6:/usr/local/tomcat# 

# 3、从webapps.dist拷贝文件进去webapp
root@0b497f614bf6:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@0b497f614bf6:/usr/local/tomcat# cd webapps
root@0b497f614bf6:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager

# 4、将操作过的容器通过commit打包为一个镜像！我们以后就使用我们修改过的镜像即可，而不需要每次都重新拷贝webapps.dist下的文件到webapps了，这就是我们自己的一个修改的镜像。
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[TAG]
docker commit -a="sxz" -m="add webapps app" 容器id tomcat02:1.0

[root@hadoop1 ~]# docker commit -a="sxz提交的" -m="add webapps app" 0b497f614bf6 tomcat02:1.0
sha256:4c3bee823cf9f9b89540f5e3d77312aa568dc00a57064042d36a69da6e6b09d0

[root@hadoop1 ~]# docker images
REPOSITORY               TAG       IMAGE ID       CREATED          SIZE
tomcat02                 1.0       4c3bee823cf9   23 seconds ago   672MB
tomcat                   latest    bd431ca8553c   2 weeks ago      667MB
```

如果你想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像，就好比我们我们使用虚拟机的快照。

入门成功！！！！

# 容器数据卷

## 什么是容器数据卷

> 方式一：直接使用命令来挂载 	-v

```shell
docker run -it -v 主机目录：容器内目录

# 测试
[root@hadoop1 home]# docker run -it -v /home/ceshi:/home centos:7 /bin/bash
# 启动起来时我们可以通过 docker inspect 容器id 
```

```shell
"Mounts": [  ##  挂载  -v  卷
            {
                "Type": "bind", 
                "Source": "/home/ceshi",  ### 主机内地址
                "Destination": "/home",   ### docker容器内地址
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
```

![image-20210426220938209](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426220938209.png)

![image-20210426220958312](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426220958312.png)

## 实战安装 : MySQL

思考：数据库MYQL数据持久化问题

```shell
# 获取镜像
[root@hadoop1 home]# docker pull mysql:5.7

# 运行容器，需要作数据挂载！
# 安装启动MYsql 需要配置用户密码，这点需要注意！
# 官方测试： docker run --name some-mysql  -e  MYSQL_ROOT_PASSWORD=my-secret -pw -d mysql:tag

# 启动我们的
-d 后台运行
-p 端口映射
-v 数据卷挂载
-e 环境配置
--name 容器名字
[root@hadoop1 ~]# docker run -d -p 3316:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root --name mysql01 mysql:5.7

# 启动成功后， 我本在本地使用 Navicat 测试一下
# Navicat 连接端口3316 --- 映射到容器内容端口3306 ，这个时候我们就可以连接到了
```

下图显示mysql数据成挂载到了本地内部

![image-20210426225106777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426225106777.png)

假设我们将容器删除

![image-20210426225413922](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426225413922.png)

发现我们挂载到的本地数据卷依旧没有丢失，这就完成了容器数据持久化操作！

## 具名和匿名挂载

```shell
# 匿名挂载
-v 容器内路径
docker run -d -p --name nginx01 -v /ect/nginx nginx

# 查看所有的 volume 的情况
[root@hadoop1 /]# docker volume ls
DRIVER    VOLUME NAME
local     0f1e24fd11a2001e7f11da2f23b127956ab7723653619161fb81bfbd07e1cae5

# 这里发现，这种就是匿名挂载，我们在 -v 的时候只写了容器内的路径，没有写容器外的路径！

# 具名挂载
[root@hadoop1 /]# docker run -d -P --name nginx02 -v juming-nginx:/ect/nginx nginx
128ca014b96d45c999c44e45d6c3544cc84d52f96dd994903e0016c8cd1f194e
[root@hadoop1 /]# docker volume ls
DRIVER    VOLUME NAME
local     juming-nginx

# 通过 -v 卷名：容器内路径
# 查看这个卷
```

![image-20210426232653932](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426232653932.png)

 我们通过具名挂载可以很方便的找到我们的一个卷 ，大多数情况下使用的是 `具名挂载`

```shell
# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载!
-v 容器内路径		# 匿名挂载
-v 卷名：容器内路径		# 具名挂载
-v /宿主机路径：：容器内路径	# 指定路径挂载
```

**拓展**

```shell
# 通过 -v 容器内路径：ro rw 改变读写权限
ro		readonly	# 只读
rw		readwrite	# 只写

# 一旦这个设置了容器权限，容器对我们挂载出来的内容就有了限定！
docker run -d -P --name nginx02 -v juming-nginx:/ect/nginx：ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/ect/nginx：rw nginx

# ro 只要看到 ro 就说明这个路径只能通过宿主机来操作，容器内部都是无法操作
```

## 初识DockerFile

> 方式二

DockerFile 就是用来构建 docker 镜像的构建文件！命令脚本！ 初次体验！

通过这个脚本可以生成镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层。

```shell
# 创建一个dockerfile 文件，名字随机，建议初次以 Dockerfile
# 文件中的内容 指令（大写） 参数
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "----end------"

CMD /bin/bash

# 这里的每个命令，就是镜像的一层
docker build -f /home/test-dockerfile/dockerfile1 -t sxz/centos:1.1 .
-f 所构建镜像文件目录
-t 构建后的容器名字：TAG版本号
. 一定要注意 结尾处有个 · 点
```

![image-20210427223827489](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427223827489.png)

![image-20210427224541295](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427224541295.png)

这个卷和外部一定有一个同步的目录！

![image-20210427224809940](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427224809940.png)

```shell
# 指令
docker inspect 容器 id
# 查看一下卷挂载的路径
```

![image-20210427225351156](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427225351156.png)

测试一下刚才的文件是否同步持久化完成了！

这种方式我们未来使用的十分多，因为我们通常会构建自己的镜像！

假设构建镜像时候没有挂载卷，要手动镜像挂载 - v 卷名:容器内部路径！

## 数据卷容器

多个MySQL同步数据！

![image-20210427232115947](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427232115947.png)

```shell
# 启动多个容器，通过刚才写的镜像启动 测试多个镜像之间如何完成数据同步
```

![image-20210427233134050](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427233134050.png)

![image-20210427233932265](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427233932265.png)

![image-20210427234124398](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427234124398.png)

```shell
# 测试：可以删除docker01 ，产看一下docker02 和 docker03 是否可以访问这个文件
# 经测试依旧可以访问
```

![image-20210427234241521](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427234241521.png)

多个MySQL实现数据共享

```shell
[root@hadoop1 ~]# docker run -d -p 3316:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=root --name mysql01 mysql:5.7

[root@hadoop1 ~]# docker run -d -p 3316:3306  -e MYSQL_ROOT_PASSWORD=root --name mysql02 --volumes-from mysql01 mysql:5.7

# 这个时候，只需要继承就能实现两个容器数据同步！
```

**结论**

容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用位置。

？？？但是一旦持久化到了本地，除非本地持久化数据被删除否则即便容器删除点也不会使本地数据被删掉？？？

但是一旦持久化到了本地，这个时候，本地数据时不会删除的！

# DockerFile

## DockerFile介绍

```shell
# dockerfile 时用来构建docker镜像文件！命令参数脚本！

# 构建步骤:
# 1.编写一个dockerfile文件
# 2.docker build构建成一个镜像
# 3.docker run 运行镜像
# 4.docker push 发布镜像（DockerHub,阿里云镜像仓库)
```

查看一下官方时怎么做的？

![image-20210427235047501](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427235047501.png)

![image-20210427235102994](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427235102994.png)

很多官方镜像都是基础包，很多功能都没有。我们通常会自己搭建自己的镜像！

官方既然可以制作镜像，那我们也可以

## DockerFile构建过程

```shell
# 基础知识：
# 1.每个保留关键字（指令） 都必须时大写字母
# 2.执行从上到下的顺序
# 3.# 表示注解
# 4.每一个指令都会创建提交一个新的镜像层，并提交！
```

![image-20210427235344696](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427235344696.png)

dockerfile是面像开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件。这个文件十分简单

Docker镜像逐渐成为企业交付的标准，必须要掌握！

步骤：开发，部署，运维。。。缺一不可！

DockerFile:构建文件，定义了一切的步骤，源代码

Dockerimages:通过DockerFile构建生成的镜像，最终发布和运行的产品

Docker容器：容器就是镜像运行起来提供服务器

## DockerFile指令

以前的话我们就是使用别人的，现在我们知道了这些指令后，我们来练习自己写一个镜像!|

```shell
FROM			#基础镜镜像,一切从这里开始构建
MAINTAINER		#镜像是谁写的,姓名+邮箱
RUN				#镜像构建的时侯需要运行的命令
ADD				#步骤:tomcat镜像，这个tomcat压缩包!添加内容
wORKDIR			#镜像的工作目录
vOLUME			#挂载的目录
EXPOSE			#保留端口配置
CMD				#指定这个容器启动的时候要运行的命令,只有最后一个会生效，可被替代
ENTRYPOINT		#指定这个容器启动的时候要运行的命令,可以追加命令
ONBUILD			#当构建一个被继承DockerFile 这个时候就会运行ONBUILD 的指令。触发指令。
COpY			#类似ADD ，将我们文件拷贝到镜像中
ENV				#构建的时候设置环境变量!
```

![image-20210428015441991](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210428015441991.png)

## 实战测试

Docker Hub中 99%镜像都是从这个基础镜像过来的FROM scratch，然后配置需要的软件和配置来进行的构建

![image-20210428015525242](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210428015525242.png)

