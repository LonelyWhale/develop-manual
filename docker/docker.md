# Docker使用
```
# Usage:  [sudo] docker [subcommand] [flags] [arguments] ..
# Example:
$ docker run -i -t ubuntu /bin/bash
```
##### docker 版本
```
$ docker version
```
##### docker 帮助
```
$ docker --help
$ docker [subcommand] --help
```
### 镜像
##### 查看镜像
```
$ docker images
$ docker images --digests | head
# To list image digest values, use the --digests flag
```
##### 搜索镜像
```
$ docker search centos
```
##### 下载镜像
```
$ docker pull centos
```
##### 发布自己的docker镜像
```
# create Dockerfile
$ mkdir mydockerbuild
$ cd mydockerbuild
$ vim Dockerfile
-----------------------------------------------------
FROM docker/whalesay:latest
MAINTAINER Bin <liubin@glab.cn>
RUN apt-get -y update && apt-get install -y fortunes
CMD /usr/games/fortune -a | cowsay
# You use # to indicate a comment
-----------------------------------------------------
# bulid
$ docker build -t docker-whale .
# run
$ docker run docker-whale
# push
$ docker tag 7d9495d03763 lonelywhale/docker-whale:latest
$ docker login
$ docker push lonelywhale/docker-whale
```
##### 通过docker commit创建镜像
```
$ docker commit -m "Added json gem" -a "Kate Smith" \
0b2616b0e5a8 ouruser/sinatra:v2
# The -m flag allows us to specify a commit message, much like you would with a commit on a version control system. 
# The -a flag allows us to specify an author for our update.
```
##### 移除镜像
```
$ docker rmi -f hello-world
```
##### 运行镜像
```
$ docker run hello-world
$ docker run ubuntu /bin/echo 'Hello world'
# Run an interactive container
$ docker run -t -i ubuntu /bin/bash
# -t flag assigns a pseudo-tty or terminal inside the new container.
# -i flag allows you to make an interactive connection by grabbing the standard input (STDIN) of the container.
$ docker run -d -p 80:5000 training/webapp python app.py
# bind Docker containers to specific ports using the -p flag
# The -P flag maps any required network ports inside the container to your host. This lets you view the web application.
----------------------------------------
root@af8bae53bdd3:/# pwd
/
root@af8bae53bdd3:/# exit
----------------------------------------
```
##### 后台运行镜像
```
$ docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
## -d flag runs the container in the background (to daemonize it).
```
##### 检查镜像和容器大小
```
$ docker history centos:centos7
$ docker ps -s
```
### 容器
##### 查看容器
```
$ docker ps -l
# The -l flag shows only the details of the last container started.
# Use the -a flag, see stopped containers.
```
##### 查看容器日志
```
$ docker logs [name/id]
# The -f flag causes the docker logs command to act like the tail -f command and watch the container’s standard output.
```
##### 停止运行容器
```
$ docker stop [name/id]
```
##### 启用容器
```
$ docker start [name/id]
# 重启
$ docker restart [name/id]
```
##### 移除容器
```
$ docker rm [name/id]
```
##### 设置容器端口
```
$ docker port [name/id] 5000
```
##### 查看容器进程
```
$ docker top [name/id]
```
##### 探测容器
```
$ docker inspect [name/id]
$ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [name/id]
# 查看容器ip地址
```
### Network containers网络容器
By default, Docker provides two network drivers for you, the bridge and the overlay drivers.

```
$ docker network ls
$ docker run -itd --name=networktest ubuntu
$ docker network inspect bridge
$ docker network disconnect bridge networktest
# 创建自己的桥接网络
$ docker network create -d bridge my-bridge-network
# 添加容器到网络
$ docker run -d --net=my-bridge-network --name db training/postgres
$ docker exec -it db bash
$ docker network connect my-bridge-network web
```
### 容器数据
```
# Adding a data volume
$ docker run -d -P --name web -v /webapp training/webapp python app.py
# Mount a host directory as a data volume
$ docker run -d -P --name web -v /src/webapp:/webapp training/webapp python app.py
# Mount a shared-storage volume as a data volume
$ docker run -d -P \
  --volume-driver=flocker \
  -v my-named-volume:/webapp \
  --name web training/webapp python app.py
$ docker volume create -d flocker --opt o=size=20GB my-named-volume
# Mount a host file as a data volume
$ docker run --rm -it -v ~/.bash_history:/root/.bash_history ubuntu /bin/bash
# Creating and mounting a data volume container
$ docker create -v /dbdata --name dbstore training/postgres /bin/true
$ docker run -d --volumes-from dbstore --name db1 training/postgres
$ docker run -d --volumes-from dbstore --name db2 training/postgres
$ docker run -d --name db3 --volumes-from db1 training/postgres
# list volume
$ docker volume ls -f dangling=true
# remove volume
$ docker volume rm <volume name>
# Backup, restore, or migrate data volumes
$ docker run --rm --volumes-from dbstore -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
$ docker run -v /dbdata --name dbstore2 ubuntu /bin/bash
$ docker run --rm --volumes-from dbstore2 -v $(pwd):/backup ubuntu bash -c "cd /dbdata && tar xvf /backup/backup.tar --strip 1"
# Removing volumes
$ docker run --rm -v /foo -v awesome:/bar busybox top
```
