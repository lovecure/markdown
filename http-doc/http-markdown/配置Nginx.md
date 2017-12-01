
## 总结
-

<br>
</br>
<br>
</br>

### 一、Nginx配置文件位置
     
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
│   ├── fastcgi.conf    ---->动态网站配置文件
│   ├── fastcgi.conf.default
│   ├── fastcgi_params  ---->动态网站配置参数文件
│   ├── fastcgi_params.default
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types
│   ├── mime.types.default
│   ├── nginx.conf   -----> <mark>核心配置文件,静态网站用</mark>
│   ├── nginx.conf.default -----> 配置文件的备份
│   ├── scgi_params
│   ├── scgi_params.default
│   ├── uwsgi_params
│   ├── uwsgi_params.default
│   └── win-utf
├── fastcgi_temp
├── html
│   ├── 50x.html
│   └── index.html
├── logs
│   ├── access.log
│   ├── error.log
│   └── nginx.pid
├── proxy_temp
├── sbin
│   └── nginx   -----><mark>核心内容</mark>
├── scgi_temp
└── uwsgi_temp
</pre>


