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
		## server 192.168.159.133:8080 backup; 其它所有的⾮backup机器down的时候，会请求
			##	backup机器，这台机器压⼒会最轻，配置也会相对低
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
       	}
	}

}
```

