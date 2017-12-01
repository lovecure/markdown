
## 总结
-

<br>
</br>
<br>
</br>

### 一、Nginx默认站点位置
     
     nginx_install_dir/conf (没有extra目录)
     
     grep "html" nginx.conf : Nginx默认主页位置
     
     
     /nginx_install_dir/html/ <--- Nginx默认站点位置
     
>    Apache默认站点位置:
>>   grep "DocumentRoot" /application/apache/conf/httpd.conf
>>   /application/apache/htdoc 

<br>
</br>

### 二、Nginx模块

Nginx使用不同的模块实现不同的功能,大多数软件使用模块的方式是为了<mark>解耦</mark>

    1.核心模块 Main
    
    2.标准模块 Core
> 以上两个模块缺省都会安装
> 查询模块的功能去官网:http://nginx.org/en/docs/查找
   
   <br>
</br>

     
### 三、Nginx目录结构

    
    
<pre>
tree /appliction/nginx


/application/nginx
├── client_body_temp   
├── conf
│   ├── fastcgi.conf    ----><mark>动态网站配置文件</mark>
│   ├── fastcgi.conf.default
│   ├── fastcgi_params  ----><mark>动态网站配置参数文件</mark>
│   ├── fastcgi_params.default
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types
│   ├── mime.types.default
│   ├── nginx.conf   -----> <mark>核心配置文件,静态网站用</mark>
│   ├── nginx.conf.default -----><mark>配置文件的备份</mark>
│   ├── scgi_params
│   ├── scgi_params.default
│   ├── uwsgi_params
│   ├── uwsgi_params.default
│   └── win-utf
├── fastcgi_temp
├── html   -----><mark>默认站点目录</mark> 
│   ├── 50x.html ----><mark>出错页面</mark>
│   └── index.html
├── logs
│   ├── access.log -----><mark>访问日志</mark>
│   ├── error.log  -----><mark>错误日志</mark>
│   └── nginx.pid  -----><mark>进程锁文件</mark>
├── proxy_temp
├── sbin
│   └── nginx   -----><mark>核心启动文件</mark>
├── scgi_temp
└── uwsgi_temp
</pre>

 <br>
</br>

### 四、模块化的Nginx配置文件

<pre>

<mark>--整体是一个Main指令</mark>
worker_processes  1;
events {
    worker_connections  1024;
} <mark>--每个模块由"{}"括起来</mark>
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server { <mark>--不同的模块又可以包含多个子模块</mark>
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
<mark>--整体是一个Main指令</mark> 
</pre>

 <br>
</br>

### 五、Nginx配置文件详解

<pre>
#user  nobody;
worker_processes  1;  

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
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

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
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


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
            

</pre>


