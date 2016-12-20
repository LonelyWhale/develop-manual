#### ubuntu 14.04 -> 16.04升级
```
$ sudo apt update
$ sudo apt dist-upgrade
$ sudo do-release-upgrade -d
```
#### 查看版本
```
$ cat /etc/lsb-release
```
#### 检查kernel版本
```
$ uname -r
```
#### ubuntu 安装Docker
```
1.切换到root权限或者用sudo
2.升级source列表并保证https和ca证书成功安装
$ apt-get update
$ apt-get install apt-transport-https ca-certificates
3.增加新的GPG 密钥
$ apt-key adv \
               --keyserver hkp://ha.pool.sks-keyservers.net:80 \
               --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
4.新增或编辑source列表里的docker.list文件
$ vi /etc/apt/sources.list.d/docker.list  //如果不存在就新增
5.按照系统版本增加entry(Ubuntu Xenial 16.04 (LTS)）
deb https://apt.dockerproject.org/repo ubuntu-xenial main
6.重新执行更新操作，并删除老的repo
$ apt-get purge lxc-docker  //没有安装的话，跳过
7.查看是否有正确的可用版本
$ apt-cache policy docker-engine
8.从14.04版本以上开始docker推荐安装linux-image-extra
$ apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
9.安装docker
$ apt-get update
$ apt-get install docker-engine
$ service docker start
$ docker run hello-world
```
