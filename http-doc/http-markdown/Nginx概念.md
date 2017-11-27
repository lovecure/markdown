
# NGINX 概念

## 总结
<br>
</br>

1. nginx 实际使用时尽量将nginx用于小文件静态数据环境中
2. apache 更擅长动态网页

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
![](http://ozxcyqizw.bkt.clouddn.com/1.2%20%20%E5%A4%A7%E5%9E%8B%E4%BC%81%E4%B8%9A%E5%8A%A8%E9%9D%99%E5%88%86%E7%A6%BB%E6%9E%B6%E6%9E%84.png)


<br>
</br>

### 三、nginx优点

>0. 配置简单灵活

>1. 高并发(静态小文件) 静态1-2W

>2. 占用资源少 2W并发 10个线程，内存消耗几百M

>3. 功能种类多(web,cache,proxy),但是每个功能都不是强项

>4. 支持epoll模型，所以nginx可以支持高并发

>5. nginx配合动态服务和apache是不一样的
![1.3  LAMP与LNMP实现原理对比](http://ozxcyqizw.bkt.clouddn.com/1.3%20%20LAMP%E4%B8%8ELNMP%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86%E5%AF%B9%E6%AF%94.png)


>6. 利用nginx可以对ip限速，限制连接数(自身的模块)

<br>
</br>



### 四、nginx应用场合

>1.静态服务器（图片、视频服务）
>>      html,js,css,flv等(还有lighttpd)



>2.动态服务：nginx+fastcgi的方式运行php,jsp
>>      动态并发
>>      php： 500 ~ 1500
>>      mysql：300 ~ 1500



>3.反向代理(整体过程代替处理区别于LVS)，负载均衡

>>      正向代理：宽带路由器上网
>>>     

![7C274974-6B98-434D-934E-9C2A36B986B7](http://ozxcyqizw.bkt.clouddn.com/7C274974-6B98-434D-934E-9C2A36B986B7.png)


>>      反向代理：代替外面来的客户请求内部的服务器 竞争:haproxy,F5,A10

>>      日PV2000万以下(参考)都可以使用nginx做代理



>4.缓存服务   
>>      竞争:squid,varnish

<br>
</br>

### 五、主流web服务产品对比

#### 1）Apache
<pre>
    . 2.2版本非常稳定强大，2.4的性能更强
    . Prefork模式取消了进程开销，性能高
    . 处理动态业务数据时，瓶颈在于引擎与数据库
    . 高并发时消耗系统资源相对多一些
    . 基于传统的select模型，<mark>性能差一些</mark>
    . 扩展库使用DSO方法安装apxs
 
</pre>

<br>
</br>

#### 2) Nginx
<pre>
    . 基于异步IO模型(epool,kqueue),性能强，能支持上万并发
    . 对小文件支持很好，性能高(限静态小于1M) 
    . 代码优美，<mark>扩展库必须编译进主程序</mark>
    . 消耗系统资源较低
</pre>

<br>
</br>

#### 3）Lighttpd
<pre>
    . 基于异步IO模型，性能和nginx相近
    . 扩展库是so模式，比nginx灵活
    . 全球使用率低,安全性较差
    . <mark>通过插件(mod_secdownload)可实现文件url地址加密</mark>
</pre>

<br>
</br>

![137B6FC9-59AB-486F-8221-707FFB2A9489](http://ozxcyqizw.bkt.clouddn.com/137B6FC9-59AB-486F-8221-707FFB2A9489.png)

<br>
</br>

![2DB596C4-65F4-4D47-8EBF-4334506C30D5](http://ozxcyqizw.bkt.clouddn.com/2DB596C4-65F4-4D47-8EBF-4334506C30D5.png)

