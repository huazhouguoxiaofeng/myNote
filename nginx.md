## 安装

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
  重新加载所修改的配置文件：./nginx -s reload
  ```

## nginx.conf

原来可以有多个location的，注意这里有什么不一样

```shell
http {
	    ## http://ngtest02.com/app/img/034.jpg
        server {
            listen       80;
            server_name  ngtest02.com;
            location /app/img {  ## 对外路径
               alias /home/hzgxf/img/; ## 服务器内实际路径
            }
        }
	    ## 静态资源服务器：ngtest03.com
        server {
                 listen 80; 
                 server_name ngtest03.com;
                 location / {
                         root  /usr/local/nginx-1.18.0/html; ## 服务器里面的绝对路径
                         index index02.html; ## 上面那个路径的具体要访问的页面
                 }
         }
		## 负载均衡，按照不同的策略分发到不同的服务器上面：http://ngtest01.com/api/v1/pub/web    
		## server 192.168.207.140:8080 weight=5; 按照一定的权重分发流量
		## server 192.168.159.133:8080 down; down 表示当前的server暂时不参与负载
		## server 192.168.159.133:8080 backup; 其它所有的⾮backup机器down的时候，会请求backup机器，这台机器压⼒会最轻，配置也会相对低
		## server 192.168.0.106:8080 max_fails=2 fail_timeout=60s; 请求连续失败的次数达到两次之后，在接下去的60s内，不会在请求，60秒之后再尝试请求。（具体什么是nginx认为的失败呢：可以通过指令proxy_next_upstream来配置什么是失败的尝试；注意默认配置时，http_404状态不被认为是失败的尝试。）
	    upstream haha {
    	    server 192.168.207.140:8080; 
        	server 192.168.207.140:8081;
	    }
		server {
	        listen       80;
    	    server_name  ngtest01.com;
            location /api/v1 { ## 这里是指那个请求地址的路径哦
                     proxy_pass http://haha; ## 对应上面的upstream
                     proxy_redirect default;
                     proxy_next_upstream error timeout http_500 http_503 http_404;
                     # 存放用户的真实ip
                     proxy_set_header Host $host;
                     proxy_set_header X-Real-IP $remote_addr;  
                     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  
                     proxy_next_upstream error timeout http_503 non_idempotent;
                     # 开启错误拦截配置,一定要开启
                     proxy_intercept_errors on;
            }
            # 不加 =200，则返回的就是原先的http错误码；配上后如果出现500等错误都返回给⽤户200状态，并跳转⾄/default_api
            # 返回404,404转向200,200的话，就跳向路径 /default_api ，这个路径的话，就返回一个json
		   error_page 404 500 502 503 504 =200  /default_api;
             location = /default_api {
                 default_type application/json;
                 return 200 '{"code":"-1","msg":"invoke fail, not found "}';
             }
	}

}
```

openresty

```shell
[root@AliYun openresty]# pwd
/usr/local/openresty
[root@AliYun openresty]# ll
total 56
drwxr-xr-x  2 root root  4096 Feb 21 13:36 bin
-rw-r--r--  1 root root 22924 Nov  6 16:26 COPYRIGHT
drwxr-xr-x  6 root root  4096 Feb 21 13:36 luajit
drwxr-xr-x  5 root root  4096 Feb 21 21:03 lualib
drwxr-xr-x 12 root root  4096 Feb 21 17:09 nginx
drwxr-xr-x  4 root root  4096 Feb 21 13:36 openssl111
drwxr-xr-x  3 root root  4096 Feb 21 13:36 pcre
drwxr-xr-x  3 root root  4096 Feb 21 13:36 site
drwxr-xr-x  3 root root  4096 Feb 21 13:36 zlib
[root@AliYun openresty]# cd lualib
[root@AliYun lualib]# ll
total 60
-rwxr-xr-x 1 root root 33288 Nov  6 16:26 cjson.so
-rwxr-xr-x 1 root root  6000 Nov  6 16:26 librestysignal.so
drwxr-xr-x 3 root root  4096 Feb 21 13:36 ngx
drwxr-xr-x 2 root root  4096 Feb 21 13:36 redis
drwxr-xr-x 8 root root  4096 Feb 21 13:36 resty
-rw-r--r-- 1 root root  1374 Nov  6 16:26 tablepool.lua
[root@AliYun lualib]# cd resty/
[root@AliYun resty]# ll
total 176
-rw-r--r-- 1 root root  6129 Nov  6 16:26 aes.lua
drwxr-xr-x 2 root root  4096 Feb 21 13:36 core
-rw-r--r-- 1 root root   644 Nov  6 16:26 core.lua
drwxr-xr-x 2 root root  4096 Feb 21 13:36 dns
drwxr-xr-x 2 root root  4096 Feb 21 13:36 limit
-rw-r--r-- 1 root root  4682 Nov  6 16:26 lock.lua
drwxr-xr-x 2 root root  4096 Feb 21 13:36 lrucache
-rw-r--r-- 1 root root  7068 Nov  6 16:26 lrucache.lua
-rw-r--r-- 1 root root  1211 Nov  6 16:26 md5.lua
-rw-r--r-- 1 root root 14506 Nov  6 16:26 memcached.lua
-rw-r--r-- 1 root root 34779 Nov  6 16:26 mysql.lua
-rw-r--r-- 1 root root   616 Nov  6 16:26 random.lua
-rw-r--r-- 1 root root 15438 Nov  6 16:26 redis.lua
-rw-r--r-- 1 root root  1192 Nov  6 16:26 sha1.lua
-rw-r--r-- 1 root root  1045 Nov  6 16:26 sha224.lua
-rw-r--r-- 1 root root  1221 Nov  6 16:26 sha256.lua
-rw-r--r-- 1 root root  1045 Nov  6 16:26 sha384.lua
-rw-r--r-- 1 root root  1359 Nov  6 16:26 sha512.lua
-rw-r--r-- 1 root root   236 Nov  6 16:26 sha.lua
-rw-r--r-- 1 root root  4992 Nov  6 16:26 shell.lua
-rwxr-xr-x 1 root root  2854 Nov  6 16:26 signal.lua
-rw-r--r-- 1 root root   731 Nov  6 16:26 string.lua
-rw-r--r-- 1 root root  5178 Nov  6 16:26 upload.lua
drwxr-xr-x 2 root root  4096 Feb 21 13:36 upstream
drwxr-xr-x 2 root root  4096 Feb 21 13:36 websocket
```

lualib：lua里面带的类库，包括redis，nginx等等

可以看到 /usr/local/openresty/lualib/resty 有很多开发好的类库了



ip限制：

```
[root@AliYun lua]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.18.220.210  netmask 255.255.240.0  broadcast 172.18.223.255
        ether 00:16:3e:10:dd:4f  txqueuelen 1000  (Ethernet)
        RX packets 3106467  bytes 816236996 (778.4 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4270224  bytes 511457692 (487.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1  (Local Loopback)
        RX packets 2358  bytes 37064319 (35.3 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2358  bytes 37064319 (35.3 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

在浏览器上面可以访问：

http://47.107.126.43//api/v1/pub/web

在服务器内部：

curl http://172.18.220.210/api/v1/pub/web

网盘下载路径

 wget "127.0.0.1/download/demo-1.jar"

http://47.107.126.43//download/demo-1.jar

实现原理：

漏桶算法：均匀流量

令牌桶算法：瞬时流量

