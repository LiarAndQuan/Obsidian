
## 一.docker安装

>传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程
>容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便 . 每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源

1. 删除老版本

```shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

2. 安装需要的软件包

```shell
yum install -y yum-utils
```

3. 设置stable镜像仓库

```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

4. 更新yum软件包索引

```shell
yum makecache fast
```

5. 安装docker ce

```shell
yum -y install docker-ce docker-ce-cli containerd.io
```

6. 测试docker

```shell
docker version
#或者
docker run hello-world
```

## 二.docker常用命令

### 1.帮助启动类

1. docker version : 查看docker版本
2. docker --help : 查看帮助文档
3. docker info : 查看docker概要信息
4. systemctl start | stop | restart | status | enable docker : 略

### 2.镜像类命令

1. docker images \[选项]: 列出本地主机所有镜像
	1. -a : 所有的镜像
	2. -q : 只显示镜像id
2. docker search name \[选项]: 搜索镜像
	1. --limit n : 只列出n个,默认25个
3. docker pull name : 下载镜像
4. docker system df : 查看镜像/容器/数据卷所占的空间
5. docker rmi name : 删除镜像, 可以接多个name
	1. -f : 强制删除

### 3.容器类命令

1. docker run \[选项] image \[command] \[arg]
	1. --name="容器新名字" 为容器指定一个名称；
	2. -d: 后台运行容器并返回容器ID，也即启动守护式容器(后台运行)；
	3. -i：以交互模式运行容器，通常与 -t 同时使用；
	4. -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用 , 也即启动交互式容器(前台有伪终端，等待交互)
	5. -P: 随机端口映射
	6. -p 主机端口:docker容器端口 : 设置端口映射,访问主机端口就会访问docker内部端口
	7. command是指定的shell解析器比如/bin/bash
	8. --network 网络名 : 指定网络
2. docker ps \[选项]
	1. -a :列出当前所有正在运行的容器+历史上运行过的
	2. -l :显示最近创建的容器。
	3. -n：显示最近n个创建的容器。
	4. -q :静默模式，只显示容器编号。
3. 退出容器的方式
	1. exit : 退出shell同时停止容器
	2. ctrl+p+q : 退出shell不停止容器
4. docker start | restart | stop | kill | rm id或者name : 略
5. docker logs id : 查看容器日志
6. docker top id : 查看容器内运行进程
7. docker inspect id : 查看容器内部细节
8. 重新进入正在运行的容器并以命令行交互 :
	1. docker exec -it id bashshell : 新开一个终端, exit不会停止容器
	2. docker attach id : 直接进入容器启动命令的终端 , exit会导致容器停止
9. docker cp id:容器内路径 主机路径 : 从容器内拷贝文件到主机
10. docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:\[标签名] : 提交一个容器副本使之成为一个新的镜像

## 三.docker镜像

>镜像是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好形成一个可交付的运行环境(包括代码、运行时需要的库、环境变量和配置文件等)，这个打包好的运行环境就是image镜像文件

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。
bootfs(boot file system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是引导文件系统bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等

当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。
所有对容器的改动 - 无论添加、删除、还是修改文件都只会发生在容器层中。只有容器层是可写的，容器层下面的所有镜像层都是只读的。


## 四.数据卷

>数据卷能将docker内的文件目录和主机的文件目录关联起来从而达到在主机内备份文件的目的,也可以通过修改主机文件从而修改容器内文件

命令 : `docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录 镜像名`

**解释**
1. --privileged=true : 使用该参数，container内的root拥有真正的root权限，否则，container内的root只是外部的一个普通用户权限
2. 默认是读写同步  即`docker run -it --privileged=true -v /宿主机绝对路径目录:/容器内目录:rw 镜像名` 
3. 如果想要容器内只读不写,则是将rw改成ro
4. 容器的数据卷还可以继承和共享 : `docker run -it --privileged=true --volumes-from 父类 --name u2 ubuntu`

## 五.Dockerfile

>Dockerfile是用来构建Docker镜像的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本

构建的三个步骤 :

1. 编写Dockerfile文件
2. docker buile创建镜像
3. docker run镜像

### Dockerfile编写命令

1. FROM : 指定基础镜像
2. MAINTAINER : 镜像维护者的姓名和邮箱地址
3. RUN : 容器构建时需要运行的命令
4. EXPOSE : 容器对外暴露的端口
5. WORKDIR : 在创建容器后进入容器终端默认的目录
6. USER : 指定镜像以什么用户去执行,默认是root用户
7. ENV : 用来设置环境变量
8. ADD : 将宿主机目录下的文件拷贝进镜像,且会自动处理url和解压tar压缩包
9. COPY : 拷贝文件进镜像
10. VOLUME : 容器数据卷
11. ENTRYPOINT : 运行命令,只有最后一条cmd会生效,docker run后面的参数会追加到后面
12. CMD : 运行命令,只有最后一条cmd会生效,且会被docker run后面的参数替换

>ENTRYPOINT和CMD的区别 : 
>如果ENTRYPOINT \["ls","-l"] , docker run xxx -a会执行ls -la,这是追加
>如果CMD \["ls","-l"] ,docker run xxx -a会执行-a,报错
>注意 : 如果同时存在ENTRYPOINT和CMD,那么cmd会作为entrypoint的参数

**执行镜像的构建 :** 

docker build :
	-t xxx:xx : 指定新镜像名字
	-f fileName : 指定Dockerfile的名字,如果是Dockerfile可以省略
	最后需要**加上Dockerfile的路径**,如果是当前目录需要加上一个 . 


## 六.docker网络

### 1.docker的四种网络

1. bridge : Docker 服务默认会创建一个 docker0 网桥（其上有一个 docker0 内部接口），该桥接网络的名称为docker0，它在内核层连通了其他的物理或虚拟网卡，这就将所有容器和本地主机都放到同一个物理网络。Docker 默认指定了 docker0 接口 的 IP 地址和子网掩码，让主机和容器之间可以通过网桥相互通信
2. host : 直接使用宿主机的 IP 地址与外界进行通信，不再需要额外进行NAT 转换
3. none : 无网络
4. container : 新建的容器和已经存在的一个容器共享一个网络ip配置而不是和宿主机共享。新创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP、端口范围等。同样，两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的

### 2.常用命令

1. docker network ls : 查看网络
2. docker network inspect 网络名 : 查看详细的网络数据
3. docker network rm 网络名 : 删除

## 七.docker-compose

>docker-compose.yml是默认文件

```yaml
services:  
  blog_client:  
    image: nginx  
    volumes:  
      - /root/quan_blog/frontend:/app/frontend  
      - /root/quan_blog/backend:/app/backend  
      - /root/quan_blog/default.conf:/etc/nginx/conf.d/default.conf  
      - /root/quan_blog/certificate/aquan.online.key:/etc/nginx/aquan.online.key  
      - /root/quan_blog/certificate/aquan.online_bundle.crt:/etc/nginx/aquan.online_bundle.crt  
      - /root/quan_blog/certificate/admin.aquan.online.key:/etc/nginx/admin.aquan.online.key  
      - /root/quan_blog/certificate/admin.aquan.online_bundle.crt:/etc/nginx/admin.aquan.online_bundle.crt  
    ports:  
      - 80:80  
      - 443:443  
    restart: always  
    container_name: blog_client  
  
  blog_server_frontend:  
    image: blog_server_frontend  
    build:  
      context: .  
      dockerfile: Dockerfile_server_frontend  
    networks:  
      - blog  
    ports:  
      - 8081:8081  
    container_name: blog_server_frontend  
  
  blog_server_backend:  
    image: blog_server_backend  
    build:  
      context: .  
      dockerfile: Dockerfile_server_backend  
    networks:  
      - blog  
    ports:  
      - 8082:8082  
    container_name: blog_server_backend  
  
networks:  
  blog:  
    external: true
```
