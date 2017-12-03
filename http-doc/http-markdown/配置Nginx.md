
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
> 查询模块的功能去官网:http://nginx.org/en/docs/    查找
   
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
│   └── index.html ----><mark>默认站点首页</mark>  
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
worker_processes  1; --每个配置<mark>以分号</mark>结尾--
events {
    worker_connections  1024;
} --每个模块由<mark>"{}"</mark>括起来
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {         listen       80;
       server_name  localhost;
            location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html { --不同的模块又可以包含<mark>多个子模块</mark>
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
#user  nobody;  Nginx默认用户,<mark>已修改为nginx</mark>
worker_processes  1;  指定有几个主进程,与<mark>实际CPU核数</mark>相同(1cpu,8core 2cpu=16core)
<mark>「Nginx进程模式为一个主进程带许多子进程」</mark> 

网站访问量请求大的时候,需要调整此参数来增加worker进程



#error_log  logs/error.log;  <mark>错误日志,尽量记录error日志</mark>
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid; <mark>Nginx进程锁</mark>


events {      <mark>Worker的连接数,</mark>Nginx处理连接请求的最数量
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    <mark> #代表未生效<---- #</mark>access_log  logs/access.log  main;
    
    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {   <mark>server是常用标签！</mark>
       listen       80;<mark> 监听端口 </mark>
       server_name  localhost; <mark> 域名</mark>

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / { <mark>根据结果</mark>
            root   html;
            index  index.html index.htm;
        }  <mark>执行上面的任务,URL跳转</mark>

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
error_page   500 502 503 504  /50x.html; <mark>如果网站出现以上错误是直接提示用户,还是转到指定错误页面,优雅提示</mark>
        location = /50x.html { <mark>前面所提到的location标签</mark>
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

<br>
</br>

### 六、Nginx配置虚拟主机(域名)

<pre>
1.创建站点目录
mkdir -p /var/html/{www,bbs,blog}

2.为每个站点创建index首页

for name in www bbs blog;do echo "$name.zxx.org" > /var/html/$a/index.html;done
</pre>

<br>
</br>

<pre>
1.配置Nginx静态配置文件

vi /application/nginx/conf/nginx.conf


http {
    include mime.types;
    default_type application/octet-stream;
    sendfile       on;
    keepalive_timeout  65;
    server {
        listen        80;<mark>监听端口</mark>
        server_name   www.zxx.org; <mark>域名</mark>
            root_name /var/html/www; <mark>站点目录</mark>
            index     index.html index.htm; <mark>站点默认首页</mark>
    }
    server {
        listen       80;
        server_name  bbs.zxx.org;
            root   /var/html/bbs;
            index  index.html index.htm;
    }

    server {
        listen       80;
        server_name  blog.zxx.org;
            root   /var/html/blog;
            index  index.html index.htm;
    }
}


<mark>!!!注意是多个"server标签"而不是多个“http”标签!!!!</mark>


</pre>

    2.nginx -t 检查语法
>>nginx: the configuration file /etc/nginx/nginx.conf  <mark>syntax is ok</mark>
>>nginx: configuration file /etc/nginx/nginx.conf <mark>test is successful</mark>

    3.nginx -s reload


