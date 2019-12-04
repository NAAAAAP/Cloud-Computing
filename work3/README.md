# **实验三** 

## **(一) Centos 7安装Docker**

首先更新应用程序数据库

```
sudo yum check-update
```

![](./image/1.png)

添加Docker官方仓库，安装最新Docker

```
curl -fsSL http://get.docker.com/ |sh
```

![](./image/2.png)

启动Docker

```
sudo systemctl start docker
```

![](./image/3.png)

设置Docker自启动

```
sudo systemctl enable docker
```

![](./image/4.png)

查看Docker版本信息

```
docker version
```

![](./image/5.png)

##  **(二) Docker加载Centos镜像**

拉取Centos 7

```
docker pull centos:7
```

![](./image/6.png)

拉取完毕查看镜像

```
docker images
```

![](./image/7.png)

运行Docker容器（为了方便检测后续wordpress搭建是否成功，需设置端口映射（-p），将容器端口80 映射到主机端口8888，Apache和MySQL需要 systemctl 管理服务启动，需要加上参数 --privileged 来增加权，并且不能使用默认的bash，换成 init，否则会提示 Failed to get D-Bus connection: Operation not permitted ，，-name 容器名 ，命令如下 ）

```
docker run -d -it --privileged --name wordpress -p 8888:80 -d centos:7 /usr/sbin/init
```

![](./image/8.png)

查看已启动的容器

```
docker ps
```

![](./image/9.png)

进入容器前台（容器id可以缩写为前几位：a14）

```
docker exec -it a14 /bin/bash
```

![](./image/10.png)

容器中安装workpress

详细操作参照实验二[Centos安装wordpress](https://github.com/NAAAAAP/Cloud-Computing/tree/master/work2)

安装完毕通过浏览器访问 服务器:8888 查看

![](./image/11.png)

4.推送带有wordpress的镜像

前往DockerHub网站(https://hub.docker.com/)注册账号

返回容器前段，将容器生成镜像(所生成的镜像名由 "Docker用户名/Docker仓库名组成" ，否则推送会报错： denied: requested access to the resource is denied )

```
docker commit -a "Docker用户名" -m "提交描述" 容器id 镜像名:tag标签
# 举例 docker commit -a "naaaaap" -m "wordpress on centos7" a14f13398cc8 naaaaap/centos:v1
```

![](./image/12.png)

登入Docker

```
docker login
```

![](./image/13.png)

推送镜像

```
docker push 镜像名:tag标签
# 举例  docker push naaaaap/centos:v1
```

![](./image/14.png)

若docker账号中出现推送镜像即为成功 ！

![](./image/15.png)

## **(三) 利用Dockerfile文件创建wordpress的镜像**

编写Dockerfile文件

![](./image/23.png)

创建file文件夹，在下面编写dockerfile所需文件 :

编写start.sh

![](./image/17.png)

编写install.sh

![](./image/20.png)

编写start.service

![](./image/19.png)

编写mysql.sql（需对其进行修改）

![](./image/18.png)

构建镜像

```
docker build -t test .
docker images
```

![](./image/25.png)



运行并设置端口映射

```
docker run -dit --privileged -p 8080:80 test
```

![](./image/26.png)

```
docker ps
```

![](./image/27.png)

进入容器

```
docker exec -it 7c /bin/bash
```

![](./image/28.png)

浏览器中输入49.235.232.196:8080查看

![](./image/29.png)

完成！！！