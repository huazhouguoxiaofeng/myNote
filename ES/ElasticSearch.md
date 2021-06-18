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
./elasticsearch -d ## 后台启动
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

   后台启动：nohup ./kibana &

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





### index

说白了就是个数据库

```json
PUT nba
GET nba
GET nab,nba ## 批量请求
GET _all ## 获取所有
DELETE nba
```

```json
GET _cluster/settings ## 查看是否自动创建索引，默认为true
PUT _cluster/settings ## 设置为false
{
 "persistent": {
 	"action.auto_create_index": "false"
 }
}

```



```shell
GET _cat/indices?v ## 查看是否健康
```

### mapping

说白了就是数据库表结构

```json
PUT nab/_mapping ## 新增，也可以是修改，注意对应的 text 或者 Key 不能改，否则报错
{
  "properties": {
    "name": {
      "type": "text"
    },
    "team_name": {
      "type": "text"
    },
    "position": {
      "type": "keyword"
    },
    "play_year": {
      "type": "keyword"
    },
    "jerse_no": {
      "type": "keyword"
    }
  }
}
```

```json
GET nab/_mapping
GET nab,nba/_mapping  ## 批量请求
GET _mapping ## 获取所有
GET _all/_mapping ## 获取所有
```

### doc

说白了就是数据库的行记录

```json
PUT nab/_doc/1 ## 后面不带1的话就不指定索引
{
  "name": "哈登",
  "team_name": "⽕箭",
  "position": "得分后卫",
  "play_year": "10",
  "jerse_no": "13"
}
```

```json
DELETE nab/_doc/2

GET nab/_doc/1

GET nab/_doc/_mget ## 同时获取多个
{
  "docs": [
    {
      "_index": "nab",
      "_type": "_doc",
      "_id": "1"
    },
    {
      "_index": "nab",
      "_type": "_doc",
      "_id": "2"
    }
  ]
}
## 修改
POST nab/_doc/2 
{
 "name":"杨超越",
 "team_name":"梦之队",
 "position":"组织后卫aaaaaa",
 "play_year":"0",
 "jerse_no":"18"
}
POST nab/_update/2
{
  "doc": {
    "name": "库⾥",
    "team_name": "勇⼠",
    "position": "控球后卫",
    "play_year": 10,
    "jerse_no": "30",
    "title": "the best shooter"
  }
}

POST nab/_update/2 ## 增加一个字段
{
 "script": "ctx._source.age = 20"
}

POST nab/_update/2 ## 不写死
{
  "script": {
    "source": "ctx._source.age += params.age",
    "params": {
      "age": 4
    }
  }
}

POST nab/_update/2 ## 去掉一个字段
{
 "script": "ctx._source.remove(\"age\")"
}
```

### term

全匹配

```json
POST nba/_search
{
  "query": {
    "term": {
      "jerseyNo": "23"
    }
  }
}
POST nab/_search
{
  "query": {
    "terms": {
      "jerse_no": ["23","13"]
    }
  }
}
```

### match_all

全部都搜索出来了

```json
POST nab/_search
{
  "query": {
    "match_all": {}
  },
  "from": 0,
  "size": 20
}
```

### match

模糊匹配

```json
POST nab/_search
{
  "query": {
    "match": {
      "position": "后卫"
    }
  }
}
```

### multi_match

title 和 name，凡是有shooter的，都找出来

```json
POST nab/_search
{
  "query": {
    "multi_match": {
      "query": "shooter",
      "fields": ["title","name"]
    }
  }
}
```

match_phrase

查出得分后卫和控球后卫

```json
POST nab/_search
{
  "query": {
    "match_phrase": {
      "position": "后卫"
    }
  }
}
```

### match_phrase_prefix

最左匹配原则

```json
POST nab/_search
{
  "query": {
    "match_phrase_prefix": {
      "title": "the best s"
    }
  }
}
```

### range

* g: greate
* t: than
* e: equal
* l: less

```json
POST nba/_search
{
  "query": {
    "range": {
      "playYear": {
        "gte": 2,
        "lte": 10
      }
    }
  }
}
## 好似birthDayStr的type类型一定要是date
POST nba/_search
{
  "query": {
    "range": {
      "birthDayStr": {
        "gte": "01/01/1999",
        "lte": "01/01/2022",
        "format": "dd/MM/yyyy"
      }
    }
  }
}
```

### sort

首先要查出是火箭队的嘛

```json
POST nba/_search
{
  "query": {
    "match": {
      "teamNameEn": "Rockets"
    }
  },
  "sort": [
    {
      "playYear": {
        "order": "desc"
      }
    }
  ]
}
```

max,min,sum,avg

* aggs：表示是聚合
* avgAge：表示是到时候要显示的字段名
* avg：表示是求平均值

```json
POST /nba/_search
{
  "query": {
    "match": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": {
    "avgAge": {
      "avg": {
        "field": "age"
      }
    }
  }
}
```





