# Jenkins自动化部署
##### docker运行jenkins
```
$ docker run -d -p 8080:8080 -p 50000:50000 -v $(pwd)/data:/var/jenkins_home --name jenkins jenkins
```
##### 查看jenkins容器的当前用户
```
$ docker run -ti --rm --entrypoint="/bin/bash" jenkins -c "whoami && id"
```
##### 查看jenkins容器/var/jenkins_home目录的用户
```
$ docker run -ti --rm --entrypoint="/bin/bash" jenkins -c "ls -la /var/jenkins_home"
```
##### 本地jenkins_home目录权限
```
$ docker run -ti --rm -v $(pwd)/data:/var/jenkins_home --entrypoint="/bin/bash" jenkins -c "ls -la /var/jenkins_home"
```