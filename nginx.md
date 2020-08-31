# 一、特点

* 占有内存少
* 并发能力能力强（有报告称可以高达50000）

## 1.1 反向代理(请求转发)

* 正向代理：如果把局域网外的Internet想象成一个巨大的资源库。则局域网中的客户端要访问Internet的话，则需要通过代理服务器来访问，这种代理服务就称为正向代理（需要在浏览器中设置代理服务器的地址，不然浏览器会直接访问，比如翻墙插件）

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20200715155546318.png" alt="image-20200715155546318" style="zoom:67%;" />

* 反向代理：【浏览器不需要进行任何的配置】，浏览器只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，然后由反向代理服务器把数据还给浏览器，对浏览器而言，是没有任何感知的。

```
location / {
    root   html;
    proxy_pass http://127.0.0.1:8080 #如果浏览器访问http://localhost:80 就会直接访问这个网站(但是网址还是显示原来的)
    index  index.html index.htm;
}
```

```
location ~ /edu/ {

    proxy_pass http://127.0.0.1:8080 #如果浏览器访问http://localhost:80 就会直接访问这个网站(但是网址还是显示原来的)

}
```



## 1.2 负载均衡

* 在并发多，传输数据量大的情况下 要进行负载均衡
* 使用服务器集群

![image-20200715160732914](../../AppData/Roaming/Typora/typora-user-images/image-20200715160732914.png)

![image-20200715160933379](../../AppData/Roaming/Typora/typora-user-images/image-20200715160933379.png)

![image-20200715161006910](../../AppData/Roaming/Typora/typora-user-images/image-20200715161006910.png)

```
在http模块中添加
upstream myserve { 【nginx收到浏览器的请求将会转发到这两个不同的服务器上】
	ip_hash;
	server 192.168.17.129:8080 weight = 1;
	server 192.168.17.129:8081 weight = 1;
	fair;
}

server {
	location / {
		【这里的myserver要和上面的一样,那么浏览器请求/路由就会被分发到上面的两台不同的服务器中】
		proxy_pass http://myserver; 
	}
}
```

* 分配方式

```
1.轮询(默认) 每个请求按照先后顺序逐一分配到不同的服务器，如果后端服务器down掉，能自动剔除
2.weight 权重，权重越大被分配到的比例越大
3.ip_hash 每个请求按照访问ip的hash结果分配，这样每一个访客固定访问一个后端服务器，可以解决session的问题
4.fair 按照后台服务器的响应时间分配，响应时间短的优先访问
```





## 1.3 动静分离

* 为了加快网站的解析速度，可以把动态资源（数据库查询）和静态资源（图片等）由不同的服务器来解析。加快解析速度，降低原来单个服务器的压力

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20200715161653396.png" alt="image-20200715161653396" style="zoom:80%;" />

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20200715195611042.png" alt="image-20200715195611042" style="zoom:80%;" />

```

```





# 二、安装

```
在系统的cmd窗口中
1.查看电脑端口的占用情况
netstat -ano | findstr "80" 这个会找到包含80的

2.关闭nginx
nginx -s stop(快速停止nginx)
nginx -s quit(完整有序的停止nginx)

3.开启ngnix
start nginx

4.查看ngnix是否启动成功
tasklist /fi "imagename eq nginx.exe"
```

# 三、nginx常用命令

* 必须进入到nginx的目录中才行

```
1.查看版本号
nginx -v

2.启动Nginx
start nginx

3.关闭ngnix
nginx -s stop

4.重新加载(修改配置后)
nginx -s reload
```



# 四、nginx配置文件

* 由三大部分组成

![image-20200715190128813](../../AppData/Roaming/Typora/typora-user-images/image-20200715190128813.png)

## 4.1 全局模块

```
#user  nobody;
【最大为电脑的cpu数目】
worker_processes  1; 【这是Nginx服务器并发处理服务的关键配置，值越大表示支持的并发处理也越多，但是会受到硬件的限制】

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;
```



## 4.2 events模块

![image-20200715170802094](../../AppData/Roaming/Typora/typora-user-images/image-20200715170802094.png)

```
events {
    worker_connections  1024;
}
```



## 4.3 http模块(最频繁的部分)

### 4.3.1 http全局模块

可以配置如下参数

* 包括文件引入
* MIME-TYPE定义
* 日志自定义
* 连接超时时间
* 单链接请求数上限

```
include       mime.types;
default_type  application/octet-stream;

#log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
#                  '$status $body_bytes_sent "$http_referer" '
#                  '"$http_user_agent" "$http_x_forwarded_for"';

#access_log  logs/access.log  main;

sendfile        on;
#tcp_nopush     on;

#keepalive_timeout  0;
keepalive_timeout  65;

#gzip  on;
```

### 4.3.2 server模块

该部分和虚拟主机有密切的联系，虚拟主机从用户角度看，和一台独立的硬件主机完全一样

每个http模块可以包含多个server模块，而每一个server块都相当于一个虚拟主机

```
    server {
        listen       81; 【主机名】
        server_name  localhost; 【服务器名字】
        #root	【设置网站文件所在的目录】
        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }
        
        location = /zhxf {
            root   zhxf; 【网站文件的目录】【这个路径是相对于全局的root 这里相当于www/zhxf】
            index  index.html index.htm; 【默认显示的页面】
        }
        
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
```

#### location模块

* 设置匹配的路由

```
指令格式为：
location [ = | ~ | ~* | ^~ ] url {
    root html
    index index.html
}

说明
“=”：用于标准url前，要求请求字符串与uri严格匹配，一旦匹配成功则停止
“~”：用于正则url前，并且区分大小写
“~*”：用于正则url前，但不区分大小写
“^~”：用于标准url前，要求Nginx找到标识url和请求字符串匹配度最高的location后，立即使用此location处理请求，而不再使用location块中的正则url和请求字符串做匹配
```



```
##该路由会匹配http://xxxxxx/
location / { ##这里也可以写正则匹配
      root   html;
      index  index.html index.htm;
}
```

# 五、运行原理

* nginx运行的时候包括：一个master进程和多个worker进程（数目可以在配置文件中设置）

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20200715203935664.png" alt="image-20200715203935664" style="zoom:80%;" />

* 当浏览器发起一个请求的时候，master进程通知worker进程，worker进程采用争抢的方式

  <img src="../../AppData/Roaming/Typora/typora-user-images/image-20200715204124733.png" alt="image-20200715204124733" style="zoom:80%;" />

  

```
nginx普通的静态访问的最大并发数：worker_connections * worker_processes / 2
nginx作为反向代理的最大并发数：worker_connections * worker_processes / 4
```

