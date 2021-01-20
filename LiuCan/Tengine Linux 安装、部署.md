---
title: Tengine Linux 安装、部署 
tags: ops,nginx,linux
renderNumberedHeading: true
grammar_cjkRuby: true
---
---
Tengine 在Linux 系统上安装、部署。
Tengine v2.3.0
前置依赖：pcre-devel openssl openssl-devel 

---

## 安装前置依赖包

``` sh?linenums
yum -y install pcre-devel openssl openssl-devel
```
## 下载、安装Tengine

```sh?linenums
wget http://tengine.taobao.org/download/tengine-2.3.0.tar.gz

tar -zvxf tengine-2.3.0.tar.gz

cd tengine-2.3.0

./configure --prefix=/usr/local/nginx

make && make install

# 创建软链接
ln -s /usr/local/nginx/sbin/nginx /usr/local/bin/nginx

# 启动、重启
nginx 

nginx -s reload
```
## 配置nginx文件

```sh?linenums
user  root;
worker_processes  1;



events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    client_max_body_size 8M;
    client_body_buffer_size 128k;
    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;
	gzip on;
	gzip_min_length  5k;
	gzip_buffers     4 16k;
	#gzip_http_version 1.0;
	gzip_comp_level 3;
	gzip_types       text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
	gzip_vary on;

    #include /etc/nginx/conf.d/*.conf

    upstream dashboard_server{
    	server 127.0.0.1:8888 ;
    }
    upstream assistant_server{
        server 127.0.0.1:8889 ;
    }
# dashboard
    server {
	client_max_body_size 50m;
	server_name dplp-dashboard.test.emasapple.cn;
        listen      80;
        location / {
            proxy_pass         http://dashboard_server/;
            proxy_set_header   Host             $host;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_set_header Request-Url $request_uri;
	    index index.html index.htm;
        }
	location = /swagger-ui.html {
	    proxy_pass         http://dashboard_server/swagger-ui.html;
            proxy_set_header   Host             $host;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_set_header Request-Url $request_uri;	
	}

        location ^~ /webjars {
            proxy_pass         http://dashboard_server/webjars;
            proxy_set_header   Host             $host;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_set_header Request-Url $request_uri;
        }

	location ~ .*\.(woff|ttf|woff2|html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css|less|cur)$   {
            root /root/dplp/apps/dashboard;
            access_log  on;
            index  index.html index.htm;
        }

    }
# assistant
   server {
        client_max_body_size 50m;
        server_name dplp-assistant.test.emasapple.cn;
        listen      80;
        location / {
            proxy_pass         http://assistant_server/;
            proxy_set_header   Host             $host;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_set_header Request-Url $request_uri;
            index index.html index.htm;
        }
        location = /swagger-ui.html {
            proxy_pass         http://assistant_server/swagger-ui.html;
            proxy_set_header   Host             $host;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_set_header Request-Url $request_uri;
        }

        location ^~ /webjars {
            proxy_pass         http://assistant_server/webjars;
            proxy_set_header   Host             $host;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_set_header Request-Url $request_uri;
        }

        location ~ .*\.(woff|ttf|woff2|html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css|less|cur)$   {
            root /root/dplp/apps/assistant-ui;
            access_log  on;
            index  index.html index.htm;
        }

   }    



}


```