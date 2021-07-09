### 感悟

1）要从官网上面下载源码包，然后自己编译，编译完之后找到对应的target包，然后操作就是了

2）你可以随意移动target包到其它地方操作嘛，因为他是独立的了

### 服务器安装

其实官网都说得很清楚了，很多时候你只要直接进入官网，然后一步步按照里面的内容弄就可以了：[官网](http://rocketmq.apache.org/) ---> Documentation ---> Quick Start

　　在这里就说几个注意的点算了：

1）要安装jdk并配置全局环境变量，可以参照 [linux命令](https://www.cnblogs.com/ericguoxiaofeng/p/9352157.html) 里面的 ---> 28、软件安装  ---> 1）jdk安装（二进制安装包安装）

2）需要安装maven

3）启动 Name Server 以及 broker 时，**nohup sh bin/mqnamesrv &** 是以守护线程的形式启动的，如果启动成功还好，如果启动不成功的话就不怎么舒服了，所以一开始应该以一般形式启动 sh bin/mqnamesrv，可以成功了，在关闭服务器，再以守护线程的形式启动，启动成功之后，可以通过查看日志检查是否成功嘛`

4）启动broker的时候，一般都会报错空间不足，这个时候，在bin目录下面的文件夹里面runbroker.sh以及runserver.sh 里面的内容改为：　　

```
将：Java HotSpot(TM) 64-Bit Server VM warning: INFO: os::commit_memory(0x00007f25ec000000, 274877906944, 0) failed; error='Cannot allocate memory' (errno=12)
改为：JAVA_OPT="${JAVA_OPT} -server -Xms256m -Xmx256m -Xmn125m"
```

5）查看进程：jps

　　6961 NamesrvStartup
　　26594 jar
　　58747 Jps
　　6988 BrokerStartup

6）nameserver 的查看日志路径：~ rocketmq-all-4.4.0/distribution/target/apache-rocketmq/nohup.out

7) 官网启动 broker 的时候，`nohup sh bin/mqbroker -n localhost:9876 &` 是指往哪个nameserver服务器上连接，但是我们默认的就是本机的9876。

8）注意注意：这个target编译包是在：~ rocketmq-all-4.4.0/distribution/target/