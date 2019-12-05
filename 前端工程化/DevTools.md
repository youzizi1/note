## Elements

### 拖拽元素

可以直接通过拖拽来改变元素位置。

### 调试状态

通过`force state`可以指定到显示某个状态时的样式。

## Console面板

### 获取元素

* 通过`document.getElementById()`这些DOM API
* 选中元素后，通过`$0`
* 右击元素，选中`Copy JS path`来选中元素

## Source面板

### 快捷命令

* `ctrl+p`：搜索文件
* `ctrl+shift+p`：运行命令

> 运行`show sensors`可以进行移动端调试。

### Filesystem

在Filesystem中修改代码会映射到源文件。

### 格式化代码

点击`{}`可以对代码进行格式化。

## Network

### 性能分析

waterfall中可能出现的各项内容具体含义如下：

* Queueing（排队）：出现下面情况，浏览器会把当前请求放入队列中进行排队

  * 有更高优先级的请求
  * 和目标服务器已经建立了6个TCP链接（最多六个，适用于HTTP1.0和HTTP1.1）
  * 浏览器正在硬盘缓存上简单的分配空间
* Stalled（停滞）：请求会因为队列中任意一个原因被阻塞
* DNS Lookup（DNS查询）：浏览器正在解析IP地址
* Initial connection：在发送请求之前，必须建立TCP链接
* Proxy negotiation（代理协商）：浏览器正在和代理服务器进行协商请求
* Request sent（发送请求）：请求正在发送
* ServiceWorker Preparation（ServiceWorker准备）：浏览器正在启动ServiceWorker
* Request to ServiceWorker：请求正在发送至Service Worker
* Waiting（TTFB）：浏览器正在等待响应的第一个字节，TTFB（Time to First Byte）可以表示表示第一个字节到达的时间，包含来回延迟的时间和服务器准备响应的时间
* Content Download：浏览器正在接收响应

注意：如果是HTTP2协议，可能还会出现：

* SSL/TLS Negotiation：如果页面是通过SSL/TSL这类安全协议加载资源，这段时间就是浏览器建立安全链接的过程

* Receiving Push：浏览器正在接收`HTTP2`服务器推送的响应数据
* Reading Push：浏览器正在之前接收到的本地数据

优化手段：

* 减少所有资源的加载时间，即宽度越窄，访问速度越快
* 减少请求数量，即降低瀑布图的高度

当然Network面板还有`priority`查看请求优先级，以及`Connection ID`查看哪些HTTP请求共用一个TCP链接。