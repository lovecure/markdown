
#NGINX 概念

## 总结

<br>
</br>

### 一、介绍

Nginx (Engine x) 俄罗斯人开发，开源的WWW服务软件，体积小,高并发,占用资源少，功能多，且跨平台

<br>
</br>

### 二、nginx 三大功能

>1. WWW web服务 http协议 端口80

>2. 负载均衡（反向代理）

>3. web cache （web 缓存）
![1.2  大型企业动静分离架构](https://ozxcyqizw.bkt.clouddn.com/1.2  大型企业动静分离架构.png)
<br>
</br>

### 三、nginx优点

>0. 配置简单灵活
>1. 高并发(静态小文件) 静态1-2W
>2. 占用资源少 3W并发 10个线程，内存消耗几百M
>3. 功能种类多(web,cache,proxy) 
>4. 支持epoll模型，所以nginx可以支持高并发
>5. nginx配合动态服务和apache是不一样的
>6. 利用nginx可以对ip限速，限制连接数(自身的模块)

<br>
</br>



### 四、nginx应用场合

>1. 静态服务器（图片、视频服务），另一个lighttpd
  html,js,css,flv等



>2. 动态服务 nginx+fastcgi的方式运行php,jsp
>> 动态并发:  php 500 ~ 1500
>>         mysql 300 ~ 1500



>3. 反向代理(整体过程代替处理区别于LVS)，负载均衡

>> 正向代理：宽带路由器上网
>> 反向代理：代替外面来的客户请求内部的服务器 竞争:haproxy,F5,A10

>> 日PV2000万以下(参考)都可以使用nginx做代理



>4.缓存服务   竞争:squid,varnish

<br>
</br>


