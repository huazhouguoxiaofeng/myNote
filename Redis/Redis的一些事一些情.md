## 部署

***这些官网都有说的啊，所以一定要学会看官网，但是也要记笔记，因为官网很简略，坑很多***

***基于 redis-6.0.6***

### 一、下载

### 二、解压到指定的目录

```shell
tar -zxvf redis-6.0.6.tar.gz -C /usr/local
```

### 三、编译

```shell
make
```

* *如果报错提示缺少gcc（报错gcc: Command not found），则安装gcc ： yum install -y gcc*
* 如果报错提示：Newer version of jemalloc required 则在make时加参数：make MALLOC=libc （实践中，我换了一个相对旧的redis版本：从5版换到了2版）
      make distclean
* redis-6.0.6 的安装 服务器默认的linux版本过低，需要升级gcc。[参考文档](https://blog.csdn.net/u014539465/article/details/106650955?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

### 四、安装redis

实际上是安装可执行文件，即是那个bin客户端服务器启动目录啦，你就可以在/usr/local/redis 这个目录看到redis 的bin目录了

```shell
 make install PREFIX=/usr/local/software/redis
```

### 五、设置全局变量

```shell
vi /etc/profile
```

```shell
##set redis environment
export REDIS_HOME=/home/richmail/redis/redis-conf
export PATH=$PATH:$REDIS_HOME/bin
```

```shell
source
```

### 六、启动、关闭

```shell
service redis_6379 status/stop/start
ps -ef | grep redis
redis-cli shutdown  ## 服务器安全关闭，没有密码
redis-cli -a yourPassword shutdown    ## 服务器安全关闭，带密码
kill -9 pid  ## 强制关闭，可能会造成Redis内存数据丢失。
redis-server /home/xiaofeng01/redis-4.0.11/redis.conf   ## 启动服务器，一般都指定配置文件启动的啦
redis-cli -h 10.203.238.198 -p 8080    ## 启动客户端，指定ip端口号启动，h表示host，p表示ip
redis-cli -h 127.0.0.1 -a "1q2w3e"     ## 带密码启动客户端
/home/xiaofeng01/redis-4.0.11/src/redis-sentinel sentinel.conf   ## 启动哨兵模式服务
```

### 六、这个过程就是创建redis集群了

```shell
cd /home/richmail/redis/redis-6.0.8/utils
./install_server.sh  
```

```shell
Welcome to the redis service installer
This script will help you easily set up a running redis server

Please select the redis port for this instance: [6379]
Selecting default: 6379
Please select the redis config file name [/etc/redis/6379.conf]
Selected default - /etc/redis/6379.conf
Please select the redis log file name [/var/log/redis_6379.log]
Selected default - /var/log/redis_6379.log
Please select the data directory for this instance [/var/lib/redis/6379]
Selected default - /var/lib/redis/6379
Please select the redis executable path [/usr/local/bin/redis-server]
Selected config:
Port           : 6379
Config file    : /etc/redis/6379.conf
Log file       : /var/log/redis_6379.log
Data dir       : /var/lib/redis/6379
Executable     : /usr/local/bin/redis-server
Cli Executable : /usr/local/bin/redis-cli
Is this ok? Then press ENTER to go on or Ctrl-C to abort.
Copied /tmp/6379.conf => /etc/init.d/redis_6379
Installing service...
Successfully added to chkconfig!
Successfully added to runlevels 345!
Starting Redis server...
Installation successful!
```

### 七、redis-conf

 * 通用配置

   ```shell
   daemonize yes      ## 守护线程
   protected-mode no  ## 保护模式，一般关闭，以保持同信
   bind:0.0.0.0       ## 0.0.0.0表示对外所有ip都开放
   port:6380          ## 修改端口号，默认6379启动
   requirepass yourPassword   ##  设置密码验证
   ```

* 主从模式(redis6不是这个了，待修正)

  ```shell
  slaveof masterip masterport
  ```

* 集群模式

  ```shell
  cluster-enabled yes  ## 开启集群 把注释#去掉
  cluster-config-file nodes.conf  ## 集群的配置 配置文件首次启动自动生成
  cluster-node-timeout 5000  ## 请求超时 设置5秒够了
  appendonly yes
  bind 127.0.0.1 192.168.34.63  ## 这个东西，官网没有，实际验证是需要的，待深入理解
  ```

### 八、sentinel.conf（待验证）

```shell
## 主节点名  主节点ip  主节点port  判断主服务器客观失效至少多少个哨兵（一般情况下主要改这个就可以了，其它很多时候都是默认配置）
sentinel monitor mymaster 127.0.0.1 6379 1         
## Sentinel 认为服务器已经断线所需的毫秒数
sentinel down-after-milliseconds mymaster 10000    
## 当failover开始后，在此时间内仍然没有触发任何failover操作，当前sentinel 将会认为此次failover失败
sentinel failover-timeout mymaster 60000           
## 指定了在执行故障转移时， 最多可以有多少个从服务器同时对新的主服务器进行同步， 这个数字越小， 完成故障转移所需的时间就越长。
sentinel parallel-syncs mymaster 1                 
```

### 九、集群

```shell
## 开启集群，各个配置好后，各个服务器开启后：
redis-cli --cluster create  192.168.34.63:6380 192.168.34.63:6381 192.168.34.63:6382 192.168.34.63:6383 192.168.34.63:6384 192.168.34.63:6385  --cluster-replicas 1
## 关闭集群，一个个关闭
redis-cli -p 7000 shutdown
## 查看集群
redis-cli -c -p 7000   开启客户端，测试集群是否正常，一定要-c才能同信
redis-cli -p 6380 cluster nodes[| grep master/slave]  查看集群信息
## 模拟宕机
redis-cli -p 7000 debug segfault
```

### 十、客户端模式下

```shell
## 帮助
cluster help
## 查看集群状态信息
cluster info
## 查看集群节点信息
cluster nodes
```

### 十一、脚本

* 启动

  ```shell
  #!/bin/bash
  
  ## redis start
  
  echo "redis is starting......"
  
  cd /etc/redis
  
  for port in {6380..6385}
  
  do
  
  redis-server ./$port.conf
  
  done
  
  ## 首次启动才需要这句
  #redis-cli --cluster create  192.168.34.63:6380 192.168.34.63:6381 192.168.34.63:6382 192.168.34.63:6383 192.168.34.63:6384 192.168.34.63:6385  --cluster-replicas 1
  
  echo "redis started......"
  ```

* 关闭

  ```shell
  #!/bin/bash
  
  ** redis cluster stop
  
  echo "redis cluster is stopping......"
  
  for port in {6380..6385}
  
  do
  
  redis-cli -p $port shutdown
  
  done
  
  echo "redis cluster stopped......"
  ```

  
