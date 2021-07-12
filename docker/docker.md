## 安装

* 下载阿里云docker社区版 yum源，并查看docker 安装包

  ```shell
  cd /etc/yum.repos.d/
  wget http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  yum list | grep docker
  ```

* 安装Docker Ce 社区版本：**yum install -y docker-ce.x86_64**

* 设置开机启动：**systemctl enable docker**

* 更新xfsprogs：**yum -y update xfsprogs**

* 启动docker：**systemctl start docker**

* 查看版本、查看详细信息：**docker version；docker info**

## 镜像

* 查看本地镜像：
  * **docker images**     
  * **docker image ls**
* 搜索镜像：**docker search centos**
* 搜索镜像并过滤是官方的： 
  * **docker search --fifilter "is-offiffifficial=true" centos**
  * **docker search --fifilter stars=10 centos**
* 下载centos7镜像：**docker pull centos:7**
* 修改本地镜像名字（小写）：**docker tag centos:7 mycentos:1**
* 本地镜像的（强制）删除：
  * **docker rmi centos:7**
  * **docker rmi -f CONTAINER_ID / CONTAINER_NAME**

## 容器

* 基于哪一个镜像启动容器：**docker run -itd --name=mycentos centos:7/imageID**
  * **-i** ：表示以交互模式运行容器（让容器的标准输入保持打开）
  * **-d**：表示后台运行容器，并返回容器ID
  * **-t**：为容器重新分配一个伪输入终端
  * **--name**：为容器指定名称
* 查看本地所有的容器：**docker ps -a**
* 得到所有容器id：**docker ps -a -q**
* 查看本地正在运行的容器：**docker ps**
* 停止容器：**docker stop CONTAINER_ID / CONTAINER_NAME**
* 一次性停止所有容器：**docker stop $(docker ps -a -q)**
* 启动容器：**docker start CONTAINER_ID / CONTAINER_NAME**
* 重启容器：**docker restart CONTAINER_ID / CONTAINER_NAME**
* （强制）删除容器：
  * **docker rm CONTAINER_ID / CONTAINER_NAME**
  * **docker rm -f CONTAINER_ID / CONTAINER_NAME**
* 查看容器详细信息：**docker inspect CONTAINER_ID / CONTAINER_NAME**
* 进入容器：**docker exec -it CONTAINER_ID /bin/bash**

## 复挂

* 从宿主机复制到容器：**docker cp** **宿主机本地路径 容器名字 /ID：容器路径**

  ```shell
  docker cp /root/123.txt mycentos:/home/
  ```

* 从容器复制到宿主机：**docker cp** **容器名字/ID：容器路径 宿主机本地路径**

  ```shell
  docker cp mycentos:/home/456.txt /root
  ```

* 宿主机文件夹挂载到容器里：**docker run -itd -v  宿主机路径:容器路径 镜像ID**

  -v：表示挂载目录
  
  -p：端口映射

  挂载目录后面那个centos:7 是 REPOSITORY:TAG
  
  ```shell
  docker run -itd -v /root/xdclass/:/home centos:7
  ```
  
  

## 构建

* FROM：基于哪个镜像
* MAINTAINER：注明作者
* COPY：复制文件进入镜像（只能用相对路径，不能用绝对路径）
* ADD：复制文件进入镜像（假如文件是.tar.gz文件会解压）
* WORKDIR：指定工作目录，假如路径不存在会创建路径
* ENV：设置环境变量
* EXPOSE：暴露容器端口
* RUN：在构建镜像的时候执行，作用于镜像层面
* ENTRYPOINT：在容器启动的时候执行，作用于容器层，dockerfifile里有多条时只允许执行最后一条
* CMD：
  * 1）在容器启动的时候执行，作用于容器层，dockerfifile里有多条时只允许执行最后一条；
  * 2）容器启动后执行默认的命令或者参数，允许被修改
* 命令格式：
  * shell命令格式：RUN yum install -y net-tools
  * exec命令格式：RUN [ "yum","install" ,"-y" ,"net-tools"]



## tomcat

编写：dockerfile

```shell
# this is a dockerfile 
FROM centos:7 
ADD jdk-8u211-linux-x64.tar.gz /usr/local
RUN mv /usr/local/jdk1.8.0_211 /usr/local/jdk
ENV JAVA_HOME=/usr/local/jdk 
ENV JRE_HOME=$JAVA_HOME/jre 
ENV CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH 
ENV PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
ADD apache-tomcat-8.5.35.tar.gz /usr/local
RUN mv /usr/local/apache-tomcat-8.5.35 /usr/local/tomcat
MAINTAINER huazhouguoxiaofeng
RUN echo "正在构建镜像！！！" 
EXPOSE 8080
ENTRYPOINT ["/usr/local/tomcat/bin/catalina.sh","run"]
```

构建镜像

```shell
docker build -t mycentos:nginx .   ## 后面的 . 表示 到当前目录
```

启动容器：

```shell
docker run -itd -p 80:8080 -v /root/test/ROOT:/usr/local/tomcat/webapps/ROOT mycentos:jdk /bin/bash
```







## nginx

编写脚本：nginx_install.sh

```shell
#!/bin/bash 
yum install -y gcc gcc-c++ make pcre pcre-devel zlib zlib-devel 
cd /usr/local/nginx-1.16.0 
./configure --prefix=/usr/local/nginx && make && make install
```

编写：dockerfile

```shell
FROM centos:7 
ADD nginx-1.16.0.tar.gz /usr/local 
COPY nginx_install.sh /usr/local 
RUN sh /usr/local/nginx_install.sh 
EXPOSE 80
```

构建镜像

```shell
docker build -t mycentos:nginx .
```

这个时候执行 docker images

```shell
[root@aliyun ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
mycentos     nginx     9863d0591f4b   3 minutes ago   447MB
mycentos     jdk       8d7f82707e0b   2 hours ago     1.04GB
centos       7         8652b9f0cb4c   7 months ago    204MB
```

启动容器：

```shell
docker run -itd -p 80:80 mycentos:nginx /usr/local/nginx/sbin/nginx -g "daemon off;"
```

## redis

编写脚本：redis_install.sh

```shell
#!/bin/bash yum install -y gcc gcc-c++ make openssl openssl-devel 
cd /home/redis-4.0.9 
make && make PREFIX=/usr/local/redis install 
mkdir -p /usr/local/redis/conf/ 
cp /home/redis-4.0.9/redis.conf /usr/local/redis/conf/ 
sed -i '69s/127.0.0.1/0.0.0.0/' /usr/local/redis/conf/redis.conf 
sed -i '88s/protected-mode yes/protected-mode no/' /usr/local/redis/conf/redis.conf
```

编写：dockerfile

```shell
FROM centos:7 
ADD redis-4.0.9.tar.gz /home 
COPY redis_install.sh /home 
RUN sh /home/redis_install.sh 
ENTRYPOINT /usr/local/redis/bin/redis-server /usr/local/redis/conf/redis.conf
```

