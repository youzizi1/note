## nginx
nginx是一款轻量级的HTTP服务器，采用事件驱动的异步非阻塞处理方式框架，
这让其具有极好的IO性能，时常用于服务器的反向代理和负载均衡

优点：
  开源且性能高，可靠的HTTP中间件和代理服务器
  IO多路复用：多个描述符的IO操作都能再一个线程里卖弄并发交替顺序完成，复用线程
  CPU亲和：一种把CPU核心和Nginx工作进程绑定方式，把每个worker进程固定再一个CPU核上执行，没有切换消耗
  sendfile：零拷贝传输模式，不需要经过用户空间

## nginx的快速搭建
- Mainline version开发版
- Stable version稳定版
- Legecy version历史版本

## 安装nginx
centos通过yum安装nginx

## 链接远程服务器
putty
xshell

## linux命令
rpm -ql nginx：查看nginx安装的目录
mv：移动文件
cp：拷贝文件

## 启动nginx
systemctl start nginx
systemctl stop nginx

ps -ef | grep nginx：查看nginx是否启动

## nginx.conf配置文件
一般不主动修改nginx.conf这个主文件，而是再/etc.nginx/conf.d/下
新建conf文件进行配置，因为nginx.conf配置了自动导入该目录下的conf文件

## 重启nginx服务
nginx -s reload

## 检测nginx配置
nginx -t

## .swap
.swap结尾的是vim的缓存文件。

## ~
```conf
server {
  location ~ .*\.(html|js|css)$ {
    ## 一些规则
    ## ~表示后面要跟正则表达式
    expires 1;      ## 1h之内去缓存中拿
    gzip on;        ##  开启gzip压缩
    gzip_http_version 1.1;  ## 压缩版本
    gzip_comp_level 2;    ## 压缩等级
    gzip_types application/javascript text/css;   ## css等文件不支持gzip压缩，所以要配置一下
  }
  ## 还可以配置图片
  ## 但是图片不希望别人能够访问，也就是防盗链，因为耗费的流量是你的
  ## 所以要通过rederer去控制
  ## valid_referers none blocked http://127.0.0.1   直接访问以及http://127.0.0.1可以访问
  ## if($invalid_referer) {
  ##    return 403
  ## }
}
```

## 缓存
协商缓存：
强制缓存：

## nginx
nginx可以做静态服务器，无敌。
当配置JSON文件时，可以直接通过外网/json文件访问
但是通过ajax访问时就会出现跨域，此时就需要配置请求头。
```ngixn.conf
add_header Access-Control-Origin-Allow *;
# 还有其他头
```

## 代理
正向代理：用户通过正向代理（vpn）访问谷歌，谷歌（服务器）并不知道是用户直接访问还是代理去访问
反向代理：用户直接访问服务器，被Nginx拦截并代理到某个端口下，然后通过Nginx再去访问服务器。对于用户来说
并不知道是直接访问服务器还是访问Nginx

```nginx.conf
server {
  listen 80;
  server_name a.ugu.com

  location / {
    pass_proxy http://localhost:3000
  }
}

server {
  listen 80;
  server_name b.ugu.com

  location / {
    pass_proxy http://localhost:4000
  }
}
```

3000和4000是本地启动的服务
这样不同域名就可以访问不同的服务

## 进程守护
以进程守护的方式启动node服务
`node server.js &`

## 负载均衡
两个服务器都启动同一个node服务，然后通过nginx做负载均衡
```nginx.conf
## 配置负载均衡
upstream myserver {
  ## 这里用两个不同端口的服务来模拟不同的服务器，但是服务都是一样的。
  server http://localhost:3000 weight=2   ## weight表示权重大一些
  server http://localhost:4000 weight=1
}

server {
  listen 80;
  server_name www.ugu.com;
  location / {
    proxy_pass http://myserver
  }
}
```
使用集群式网站解决高并发和海量数据问题的常用手段。
当一台服务器的处理能力，存储空间不足时，不要企图去换更强大的服务器，对
大型网站来说，不管多强大的服务器，都满足不了网站持续增长的业务需求

负载均衡的轮询规则：
  - 默认每个请求按事件顺序逐一分配到不同的后端服务器，如果后端服务器挂掉，能自动剔除
  - ip_hash：每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决seesion的问题
  - weight：指定轮询激烈，weight和访问比率成正比，用于后端服务器性能不均的情况
  - least_conn最小连接数，哪个链接少就分给谁

注意：如果是默认轮询，那么客户端第一次请求访问第一个服务器，第二次访问访问第二个服务器，
那么就无法保存之前的登录状态。所以要选择IP_hash轮询规则。

## 合并请求
通过nginx-http-concat来实现
```js
lcoation /js {
  concat on;    # 合并
  concat_types application/javascript;  # 合并资源类型
  concat_unique off;    # 合并不同类型的资源
  concat_max_files 5;   # 合并的最大资源数目
  root /data      # 查找目录
}
```

## cdn
当然nginx可以来做cdn，此时应该将nginx服务器部署到单独的一台服务器做CDN服务。

## 适合PC与移动环境 开发环境$http_user_agent

```
location / {
  # 移动/pc设备适配
  if($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
    set $mobile_request '1';
  }
  if($mobile_request ='1') {
    rewrite ^.+http://mysite-base-h5.com
  }
}
```
