#Docker教程
--------
>This is used to learn docker in http://www.runoob.com
##Docker
###简介：
 - 基于Go 语言
 - 沙箱机制
 - 应用容器引擎
 - Apache2.0协议开源
###用途：
 - 打包应用以及依赖包到一个轻量的、可移植的容器当中，以发布到任何机器上
 - 实现虚拟化
 
---------
##Docker使用
 - Docker容器是独立运行的一个或一组应用
 - 根据Docker镜像创建Docker容器
 - Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库
 - C/S 架构，远程API来与 Docker 的守护进程通信，以管理和创建Docker容器
###Docker安装
安装 Docker：
`yum -y install docker`
启动 Docker 后台服务：
`systemctl start docker
systemctl enable docker`
从镜像源下载Hello-World并测试运行：
`docker run hello-world`
###Docker镜像源
本地：
docker images 列出本地主机上的镜像
`docker images 
REPOSITORY  TAG IMAGE ID    CREATED SIZE
busybox latest  f6e427c148a7    11 days ago 1.15MB
10.217.248.21/library/nginx latest  e548f1a579cf    2 weeks ago 109MB`
删除本地镜像
`docker rmi [OPTIONS] IMAGE [IMAGE...] `
仓库：
本地私有仓库：Harbor
远程官方仓库：Docker Hub
从仓库查找镜像httpd
`docker search httpd`
本地镜像与仓库中镜像进行链接
`docker tag image:tag ip:port/harbor-project-name/image:tag
docker tag 10.217.248.21/library/nginx:latest 10.217.248.21/test1/nginx:latest`
推送本地镜像至仓库
`docker push ip:port/harbor-project-name/image:tag
docker push 10.217.248.21/test1/nginx:latest`
从仓库拉取镜像
`docker pull ip:port/harbor-project-name/image:tag`
设置仓库后不需要完整指定路径，修改/etc/docker/daemon.json
`{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}`
docker save命令将镜像存出到本地文件
`docker save –o /data/testimage.tar testimage:latest`
docker loader命令将镜像载入
`docker load —input testimage.tar`
###Docker镜像制作
docker commit更新镜像
`docker commit -m="mention" -a="author" Container_ID image:tag`
Dockerfile 文件：
 - 每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的
 - 第一条FROM，指定使用哪个镜像源
 - RUN 指令告诉docker 在镜像内执行命令，安装了什么
 - EXPOSE 指定监听哪个端口
docker build构建镜像
`docker build -t(tag) runoob/centos:6.7 .`
docker tag 命令，为镜像添加一个新的标签
`docker tag 860c279d2fec runoob/centos:dev`
>cat Dockerfile 
FROM    centos:6.7
MAINTAINER      Fisher "fisher@sudops.com"
RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd runoob
RUN     /bin/echo 'runoob:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D


###Docker容器使用
 - docker run 命令来在容器内运行一个应用程序
`docker run *image:tag* *command*
docker run ubantu:15.10 /bin/echo "Hello World"`
 - 交互式的容器-i(interaction) -t（terminal)
`docker run -i -t ubuntu:15.10 /bin/bash`
 - 后台启动容器-d(detach）
`docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"`
 - docker ps 来查看有哪些容器在运行，以及其他信息
 - docker ps -l 查询最后一次创建的容器
 - docker restart *Container_ID* 来重启容器
 - docker logs *Container_ID* 查看指定容器日志信息
 - docker stop *Container_ID* 命令来停止指定容器
 - 停止所有容器
 - docker rm *Container_ID* 删除不需要的容器
 - docker inspect *Container_ID* 来查看指定容器的底层信息
 - docker top *Container_ID* 来查看容器内部运行的进程
###Docker 容器连接
docker port *Container_ID* 5000 命令可以快捷地查看容器的5000端口绑定情况
docker run中使用端口映射
	-P :是容器内部端口随机映射到主机的高端口
	-p :是容器内部端口绑定到指定的主机端口
设置后通过访问127.0.0.1:5001来访问容器的TCP5000端口
`docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py
95c6ceef88ca3e71eaf303c2833fd6701d8d1b2572b5613b5a932dfdfe8a857c`
设置后通过访问127.0.0.1:5000来访问容器的UDP5000端口
`docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
6779686f06f6204579c1d655dd8b2b31e8e809b245a97b2d3a8e35abe9dcd22a`

---------
##Docker实例
###Docker安装Nginx
####创建Dockerfile

 1. 创建Dockerfile
 2. 通过Dockerfile创建一个镜像

####使用nginx镜像
###Docker安装Python
###Docker安装MySQL
