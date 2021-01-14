## NG日志切割

+ nginx的日志文件路径
+ 每天0点对nginx 的access与error日志进行切割
+ 以前一天的日期为命名

```shell
#!/bin/bash 
#Auto cut nginx log script. 
#create by 小C 
#create date:2019-08-21 

#nginx日志路径 
logs_path=/usr/local/nginx/logs 
YesterDay=$(date -d 'yesterday' +%Y-%m-%d) 

#移动日志并以日期改名 
mv ${logs_path}/access.log ${logs_path}/access_${YesterDay}.log 
mv ${logs_path}/error.log ${logs_path}/error_${YesterDay}.log 

#向nginx主进程发送信号，重新生成日志文件 
kill -USR1 $(cat /usr/local/nginx/logs/nginx.pid)
```

## redis 集群启动/关闭

```shell
#!/bin/bash
echo "redis is starting......"
cd /etc/redis
for port in {6380..6385}
do
redis-server ./$port.conf
done
## 首次启动才需要这句
#redis-cli --cluster create  192.168.34.63:6381 192.168.34.63:6382 192.168.34.63:6383 192.168.34.63:6384 192.168.34.63:6385  --cluster-replicas 1
echo "redis started......"

#!/bin/bash
echo "redis cluster is stopping......"
for port in {6380..6385}
do
redis-cli -a 1q2w3e  -p $port shutdown
done
echo "redis cluster stopped......"
```

## springboot 启动关闭

+ -jar -Drmbasic.conf.path=   指定什么鬼配置文件启动
+ -Dspring.config.location=    springboot指定配置文件启动
+ -agentlib   服务器调试用的
+ 两个> ${LOG_PATH}/RichMonitorCore.log 2>&1  把输出的nohup.out输出到特定的日志路径

```shell
#!/bin/bash
echo starting richMonitor......
APP_NAME=rich.monitor-0.0.1-SNAPSHOT.jar
APP_PATH=/home/richmail/web/monitor
CONFIG_PATH=/home/richmail/web/monitor/config
LOG_PATH=/home/richmail/logs/webapp/RichMonitorCore
nohup java -jar -Drmbasic.conf.path=${CONFIG_PATH}/  -Dspring.config.location=${CONFIG_PATH}/application.properties -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5999 ${APP_PATH}/${APP_NAME} >> ${LOG_PATH}/RichMonitorCore.log 2>&1  &
echo richMonitor started!!!

#!/bin/bash
PID=$(ps -ef | grep rich.monitor-0.0.1-SNAPSHOT.jar | grep -v grep | awk '{ print $2 }')
if [ ${PID} ]; 
then
    echo 'Application is stpping...'
    echo kill $PID DONE
    kill $PID
else
    echo 'Application is already stopped...'
fi
```

