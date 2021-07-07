## 安装

* 下载阿里云docker社区版 yum源，并查看docker 安装包

  ```shell
  cd /etc/yum.repos.d/
  wget http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  yum list | grep docker
  ```

* 安装Docker Ce 社区版本

  ```shell
  yum install -y docker-ce.x86_64
  ```

* 设置开机启动

  ```shell
  systemctl enable docker
  ```

* 更新xfsprogs

  ```shell
  yum -y update xfsprogs
  ```

* 启动docker

  ```shell
  systemctl start docker
  ```

* 查看版本、查看详细信息

  ```shell
  docker version
  docker info
  ```

## 镜像

查看本地镜像：**docker images**     **docker image ls**

搜索镜像：**docker search centos**

搜索镜像并过滤是官方的： **docker search --fifilter "is-offiffifficial=true" centos**

搜索镜像并过滤大于多少颗星星的：**docker search --fifilter stars=10 centos**

下载centos7镜像：**docker pull centos:7**

修改本地镜像名字（小写）：**docker tag centos:7 mycentos:1**

本地镜像的删除：**docker rmi centos:7**

## 容器

* 构建容器：**docker run -itd --name=mycentos centos:7**
  * **-i** ：表示以交互模式运行容器（让容器的标准输入保持打开）
  * **-d**：表示后台运行容器，并返回容器ID
  * **-t**：为容器重新分配一个伪输入终端
  * **--name**：为容器指定名称
* 查看本地所有的容器：**docker ps -a**
* 查看本地正在运行的容器：**docker ps**
* 停止容器：**docker stop CONTAINER_ID / CONTAINER_NAME**
* 一次性停止所有容器：**docker stop $(docker ps -a -q)**
* 启动容器：**docker start CONTAINER_ID / CONTAINER_NAME**
* 重启容器：**docker restart CONTAINER_ID / CONTAINER_NAME**
* 删除容器：**docker rm CONTAINER_ID / CONTAINER_NAME**
* 强制删除容器：**docker rmi -f CONTAINER_ID / CONTAINER_NAME**
* 查看容器详细信息：**docker inspect CONTAINER_ID / CONTAINER_NAME**
* 进入容器：**docker exec -it 0ad5d7b2c3a4 /bin/bash**

## 复挂

* 从宿主机复制到容器：**docker cp** **宿主机本地路径 容器名字****/ID****：容器路径**

  ```shell
  docker cp /root/123.txt mycentos:/home/
  ```

* 从容器复制到宿主机：**docker cp** **容器名字****/ID****：容器路径 宿主机本地路径**

  ```shell
  docker cp mycentos:/home/456.txt /root
  ```

* 宿主机文件夹挂载到容器里：**docker run -itd -v** **宿主机路径****:****容器路径 镜像****ID**

  ```shell
  docker run -itd -v /root/xdclass/:/home centos:7
  ```

  

## 构建

```shell
# this is a dockerfile 
FROM centos:7 
MAINTAINER XD 123456@qq.com 
RUN echo "正在构建镜像！！！" 
WORKDIR /home/xdclass 
COPY 123.txt /home/xdclass  ## 123.txt  要是相对路径
RUN yum install -y net-tools
```

然后执行命令：docker build -t mycentos:v2 .  ## 后面的 . 表示 到当前目录

* FROM：基于哪个镜像
* MAINTAINER：注明作者
* COPY：复制文件进入镜像（只能用相对路径，不能用绝对路径）
* ADD：复制文件进入镜像（假如文件是.tar.gz文件会解压）
* WORKDIR：指定工作目录，假如路径不存在会创建路径
* ENV：设置环境变量
* EXPOSE：暴露容器端口
* RUN：在构建镜像的时候执行，作用于镜像层面
* ENTRYPOINT：在容器启动的时候执行，作用于容器层，dockerfifile里有多条时只允许执行最后一条
* CMD：1）在容器启动的时候执行，作用于容器层，dockerfifile里有多条时只允许执行最后一条；2）容器启动后执行默认的命令或者参数，允许被修改
* 命令格式：
  * shell命令格式：RUN yum install -y net-tools
  * exec命令格式：RUN [ "yum","install" ,"-y" ,"net-tools"]
