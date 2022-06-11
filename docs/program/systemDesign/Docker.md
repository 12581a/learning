# Docker

## 为什么需要docker?

[Docker官网]( https://www.docker-cn.com/ )

环境切换、配置麻烦

解决应用之间的隔离，比如在同一个服务器上部署两个应用。

- 软件更新发布及部署低效，过程繁琐且需要人工介入
- 环境一致性难以保证
- 不同环境之间迁移成本太高

**Docker与虚拟机的区别：**

虚拟机实现资源隔离的方法是利用独立的OS，并利用Hypervisor虚拟化CPU、内存、IO设备等实现的。 docker有着比虚拟机更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有优势。

虚拟机的Hypervisor层可以简单理解为一个硬件虚拟化平台，它在Host OS是以内核态的驱动存在的。 

docker利用的是宿主机的内核，而不需要Guest OS。因此，当新建一个容器时，docker不需要和虚拟机一样重新加载一个操作系统内核。

## docker概述

 Docker基本组成：仓库、镜像、容器

 容器是用镜像创建的运行实例。

 仓库是保存镜像文件的场所。

![](D:\TyporaFile\图片\docker架构.PNG)

​																		        dokcer架构图

开启docker，`systemctl start docker`,阿里云镜像加速器配置`/etc/docker/daemon.json`文件。

执行完`docker run hello-world`命令后，由于本地没有这个镜像，会下载一个镜像在容器内运行。

Docker是一个C/S结构的系统，Docker守护进程运行在主机上，通过Socket连接从客户端访问，守护进程管理运行在主机上的容器。

**docker常用命令：**

**镜像命令：**

`docker images`:列出本地主机上的镜像

`dokcer images -q `:只显示镜像的ID

`docker search -s 30 tomcat `:搜索hub上star大于30的tomcat的镜像

* --no-trunc:显示完整的镜像描述。

* -s:列出收藏数不小于 指定值的镜像。

* -automated:只列出automated build类型的镜像。

`docker pull tomcat:latest`:下载镜像,自动添加最新版本 

`docker rmi hello-world/镜像ID`:删除镜像

`docker rmi -f redis tomcat nginx`:删除多个镜像

`docker rmi -f $(docker images -aq)`:删除全部

**容器命令：**

完整写法`docker container run`

`docker run -it 容器ID`:运行容器

* -i:以交互模式运行容器
* -t:为容器重新分配一个伪终端
* -d:后台运行容器，并返回容器ID,也即启动守护式进程
* --name="容器新名字"：为容器指定一个名称
* -P: 随机端口映射

`docker run -it -P tomcat`

* -p:指定端口映射

`docker run -it -p 8080:8080 tomcat`

`docker ps -a`:列出当前所有正在运行的容器

`exit、ctrl+P+Q`:退出容器、容器不停止退出,第二种重新进入的命令`docker attach 容器ID`

`docker start 容器ID`：启动容器

`docker stop 容器ID`:停止容器

`docker rm 容器ID`:删除容器

`docker logs -f -t tail 容器ID`:查看容器日志

`docker cp 容器ID 容器内路径 `:从容器内拷贝文件到主机上

`docker commit -m="提交的描述信息" -a="作者" 容器ID 新命名`

`docker exec -it 容器ID /bin/bash`:进入容器

**docker镜像原理**

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于环境开发的软件，它包含运行这个软件所需的所有的内容，包括代码、运行时、库、环境变量和配置文件。

**Docker容器数据卷**

卷设计的目的就是数据的持久化，完全独立于容器的生命周期。

特点：

* 数据卷可在容器之间共享或重用数据
* 对数据卷内数据的修改会立马生效，无论是容器内操作还是本地操作；
* 数据卷中的更改不会包含在镜像的更新中
* 数据卷的生命周期一直持续到没有容器使用它为止 

数据卷容器内添加有两种方式，一种是直接命令添加，还有一种是DockerFile添加

执行命令，宿主机中会出现myDataVolume文件夹，容器中的centos会出现data文件夹，两个文件夹共享数据

```java
docker run -it -v /myDataVolume:/dataVolumeContainer centos
```

DockerFile解析：

DockerFile是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本。

新建一个dockerfile文件：

```java
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "finished,-------success1"
CMD  /bin/bash
```

```java
docker build -f /mydocker/DockerFile -t zy/centos .
```





















