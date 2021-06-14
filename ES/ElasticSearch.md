### 单机

一）下载、解压

```shell
tar -zxvf elasticsearch-7.2.0-linux-x86_64.tar.gz -C /usr/local
```

二）要有普通用户（增加组，增加普通用户，更改权限）

```shell
groupadd hz
useradd xiaofeng -g hz
chown -R xiaofeng:hz elasticsearch-7.2.0/
```

三）更改配置文件

1. vi /etc/sysctl.conf 
   * 添加配置：vm.max_map_count=655360
   * 保存后执行命令：sysctl -p
2. vi elasticsearch-7.2.0/config/elasticsearch.yml
   * 添加配置：
     * cluster.initial_master_nodes: ["node-1"]
     * node.name: node-1
     * network.host: 0.0.0.0

四）切换用户

```shell
su xiaofeng
```

五）启动

```shell
vi elasticsearch-7.2.0/bin
./elasticsearch
```

六）kibana

1. 下载、解压

2. 解决内存空间不足问题，要修改内存：vi elasticsearch-7.2.0/config/jvm.options

   ```shell
   -Xms2g
   -Xmx2g
   ```

3. vi kibana-7.2.0-linux-x86_64/config/kibana.yml

   * 都能访问：server.host: "0.0.0.0"
   * 超时时间：elasticsearch.requestTimeout: 40000

4. 启动：./kibana-7.2.0-linux-x86_64/bin/kibana

### 集群

记得要结合上面的哦哦哦哦哦

elasticSearch.yml

```yaml
#集群名称
cluster.name: my-application

#节点名称
node.name: node-1 

#是不是有资格主节点
node.master: true

#是否存储数据
node.data: true

#最大集群节点数
node.max_local_storage_nodes: 3

#网关地址
network.host: 0.0.0.0

#端口
http.port: 9200

#内部节点之间沟通端口
transport.tcp.port: 9300

#es7.x 之后新增的配置，写入候选主节点的设备地址，在开启服务后可以被选为主节点
discovery.seed_hosts: ["localhost:9300","localhost:9400","localhost:9500"]

#es7.x 之后新增的配置，初始化一个新的集群时需要此配置来选举master
cluster.initial_master_nodes: ["node-1", "node-2","node-3"] 
```

vi /etc/security/limits.conf

```shell
* soft nofile 65536
* hard nofile 65536
* soft nproc 4096
* hard nproc 4096
```

kibana.yml

```shell
elasticsearch.hosts: ["http://localhost:9200","http://localhost:9201","http://localhost:9202"]
```













