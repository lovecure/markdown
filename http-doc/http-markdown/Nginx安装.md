# Nginx安装
## Nginx安装总结
1.
2.
3.

### 一、准备
> 1. Nginx安装主文件，距官方发布最新版的半年之前的稳定版本
<pre>``wget http://nginx.org/download/nginx-1.12.2.tar.gz``
``tar xf nginx-1.12.2.tar.gz`` </pre>

> 2. 安装pcre使Nginx支持HTTP Rewrite模块
<pre>``Pcre (Perl Compatible Regular Expressions perl兼容正则表达式)``
    ``yum install pcre pcre-devel`` </pre>


> 3. 安装openssl，不装会报错(SSL modules...) 
<pre>``yum install openssl openssl-devel`` </pre>

<br>
</br>

### 二、安装

<br>
</br>

> 1.创建用户与组

``user add nginx -s /sbin/nologin -M`` 


> 2.预编译

<pre>
./configure --user=nginx --group=nginx --prefix=/application/nginx1.12.2 --with-http_stub_status_module --with-http_ssl_module
</pre>



