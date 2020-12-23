## jdk安装

首先要记住：安装虚拟机 的时候，如果当时选 了desktop的时候，系统会给你自动带一个桌面版的 java，也就是说一开始你是什么都不装的，但是系统也有一个了：

```shell
[root@mini1 jdk1.8.0_181]# which java
/usr/bin/java
[root@mini1 jdk1.8.0_181]# java -version 
java version "1.7.0_79"   ## 系统默认给你安装的版本
OpenJDK Runtime Environment (rhel-2.5.5.4.el6-x86_64 u79-b14)
OpenJDK 64-Bit Server VM (build 24.79-b02, mixed mode)
[root@mini1 jdk1.8.0_181]# rm -rf /usr/bin/java ## 所以首先要将系统本来安装的java给删了
```

1、上传jdk压缩包，我是把安装包上传到这个目录

```shell
[root@hdp01 xiaofeng10]# pwd
/home/xiaofeng10
[root@hdp01 xiaofeng10]# ll
total 181300
-rw-r--r--. 1 root root 185646832 Jul 21 13:11 jdk-8u181-linux-x64.tar.gz
```

2、解压jdk压缩包（听说一般解压到/usr/local下面）

```shell
tar -zxvf jdk-8u181-linux-x64.tar.gz -C /usr/local
```

3、此时在一定的目录下面是可以运行的了

```shell
[root@hdp01 bin]# pwd
/usr/local/jdk1.8.0_181/bin
[root@hdp01 bin]# ./java -version
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```

4、修改环境变量PATH

```shell
vi /etc/profile
```

5、在文件最后加两行：

```shell
export JAVA_HOME=/usr/local/jdk1.8.0_181
export PATH=$PATH:$JAVA_HOME/bin
```

6、让环境变量生效，即可：

```shell
source /etc/profile
java -version    ## 检测是否安装成功
```

## tomcat 安装

```shell
tar -zxvf /soft/apache-tomcat-7.0.47.tar.gz -C /usr/local/
cd /usr/local/apache-tomcat-7.0.47/bin/
./startup.sh
```

## nginx 安装

+ 安装gcc编译环境：yum install -y gcc-c++

+ 安装zlib-devel库：yum install -y zlib-devel

+ 安装OpenSSL密码库：yum install -y openssl openssl-devel 

+ 安装pcre正则表达式库：

  ```shell
  下载地址：https://ftp.pcre.org/pub/pcre/
  解压：tar -xf pcre-8.43.tar.gz
  进入解压目录：cd pcre-8.43
  创建将要编译进去的目录：mkdir -p /usr/local/pcre-8.43
  编译的时候用来指定程序存放的路径：./configure --prefix=/usr/local/pcre-8.43
  编译：make && make install
  ```

+ 下载安装NG

  ```shell
  下载：wget http://nginx.org/download/nginx-1.18.0.tar.gz
  创建将要编译进去的目录：mkdir -p /usr/local/nginx-1.18.0
  解压：tar -xf nginx-1.18.0.tar.gz
  进入解压目录：cd nginx-1.18.0
  编译的时候用来指定程序存放的路径：./configure --prefix=/usr/local/nginx-1.18.0 --with-http_ssl_module --with-http_stub_status_module --with-pcre
  编译：make && make install
  ```

+ 停止启动

  ```shell
  指定配置文件启动： /usr/local/nginx-1.18.0/sbin/nginx -c /usr/local/nginx-1.18.0/conf/nginx.conf 
  测试里面的配置文件是否正确： /usr/local/nginx-1.18.0/sbin/nginx -t 
  关闭： /usr/local/nginx-1.18.0/sbin/nginx -s stop
  ```

  