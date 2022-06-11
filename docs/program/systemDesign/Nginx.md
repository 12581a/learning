# Nginx

## 概念介绍

Nginx 是一个十分轻量级的 HTTP 服务器,Nginx，是一个高性能的HTTP和反向代理服务器，同时也是一个 IMAP/POP3/SMTP 代理服务器。 

## 正向代理与反向代理

正向代理类似一个跳板机，代理访问外部资源

比如我们国内访问谷歌，直接访问访问不到，我们可以通过一个正向代理服务器，请求发到代理服，代理服务器能够访问谷歌，这样由代理去谷歌取到返回数据，再返回给我们，这样我们就能访问谷歌了。

正向代理的用途：

* 访问原来无法访问的资源，如google
* 可以做缓存，加速访问资源
* 对客户端访问授权，上网进行认证
* 代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息

反向代理的作用：
	保证内网的安全，阻止web攻击，大型网站，通常将反向代理作为公网访问地址，Web服务器是内网

​	负载均衡，通过反向代理服务器来优化网站的负载

##  负载均衡和动静分离

将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡。

​	**Nginx负载均衡策略**：

| 轮询               | 默认方式        |
| ------------------ | --------------- |
| weight             | 权重方式        |
| ip_hash            | 依据ip分配方式  |
| least_conn         | 最少连接方式    |
| fair（第三方）     | 响应时间方式    |
| url_hash（第三方） | 依据URL分配方式 |

为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速度。降低原来单个服务器的压力。 

## 常用命令

进入到sbin目录，查看版本号`nginx -v`，启动`nginx`，关闭`nginx -s stop`，重新开启`nginx -s reload`

## 配置文件

使用docker安装时，进入到`.etc/nginx`目录下，使用`vim`命令打开。出现打不开的情况，这是因为`vim`没安装，使用` apt-get update` ，`apt-get install vim` 安装即可。

配置文件：

```html
user nginx;
worker_processes auto;

error_log  logs/error.log;
pid        logs/nginx.pid;


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

    keepalive_timeout  65;

    #gzip  on;
}
```





























