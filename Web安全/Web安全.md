### XSS

XSS（Cross Site Scripting），又称跨站脚本攻击，可能会造成：

* 窃取cookie

* 劫持流量恶意跳转

  ```js
  <script>window.location.href="http://www.baidu.com";</script>
  ```

XSS的攻击原理就是：在目标浏览器上执行一段JavaScript代码。

#### 类型

XSS可以分为反射型，存储型，DOM型。

**反射型**

发出请求时，XSS代码出现在URL中，作为输入提交打服务器，服务器没有做任何处理，直接返回给客户端。

```js
const express = require('express')

const app = express()

app.get('/user', (req, res) => {
  res.end(req.query.name)
})

app.listen(3000)
```

你直接在地址栏输入`http://localhost:3000/user?name=<script>alert(1)</script>`，会出现弹框。

> 这种方式常常需要攻击者拼接好一个URL，然后诱导用户去点击

**存储型**

提交的XSS代码会存储在服务器的数据库，内存或者文件系统中等，最典型的例子就是留言板，服务器没有处理用户输入的内容，而是直接将结果插入到数据库中。

**DOM型**

DOM型不需要服务器参与，只要客户端的DOM解析就会发生，属于客户端的问题。

```html
<div class="app">
    <div class="task">
        <input type="text" class="task-text" />
        <button class="task-btn">add</button>
    </div>
    <div class="task-content"></div>
</div>
<script>
    const taskBtn = document.querySelector(".task-btn");
    const taskText = document.querySelector(".task-text");
    const taskContent = document.querySelector(".task-content");

    taskBtn.addEventListener("click", () => {
        const taskVal = taskText.value;
        if (taskVal) {
            const p = document.createElement("p");
            p.innerHTML = taskVal;
            taskContent.appendChild(p);
        }
    });
</script>
```

这样在页面中直接输入`<img src="" onerror="alert(1)">`，就会出现弹窗。

> 在使用innerHTML，outerHTML等API时，一定要注意XSS攻击。

#### 解决方法

* 对用户输入的标签移除DOM属性，例如移除onerror属性

* 对用户输入的script，img以及a等标签进行过滤

* 对`<>`作为输入时进行转义

  ```js
  const escape = str => {
      str = str.replace('<'/g, '&lt;')
      str = str.replace('>'/g, '&gt;')
      return str
  }
  ```

* 服务器端设置cookie为http-only

* 设置CSP

> Chrome浏览器会默认防御反射型XSS攻击，但是你不应该依赖它，因为并不是所有的浏览器都会自动防御。

### CSRF

跨站请求伪造（CSRF，Cross-site request forgery）：攻击者诱导受害者进入第三方网站，在第三方网站中，向被攻击网站发送跨站请求。利用受害者在被攻击网站已经获取的注册凭证，绕过后台的用户验证，达到冒充用户对被攻击的网站执行某项操作的目的。 

下面是一个典型的攻击流程：

* 受害者登录a.com，并且保留了登录凭证cookie
* 攻击者诱导受害者访问b.com
* b.com向a.com发送一个请求
* a.com接收到请求，并对其进行验证，误以为是受害者本人访问
* 攻击者在受害者不知情的情况下，冒充受害者，完成自定义操作

防御手段：

* 验证码（体验码）
* 判断referer
* token验证

### 点击劫持

点击劫持（clickjacking）是一种常见的web攻击手段，原理是利用iframe标签的透明属性，让用户被粉饰后的按钮，实际上点击的是嵌入的iframe页面对应的按钮。

防御手段：

* 服务器端设置 X-Frame-Options头，来禁止页面在iframe中嵌入

### 传输安全

HTTP协议是明文协议，浏览器和真实服务器之间可能要经过各种网络节点，这个过程可能会被抓包监听。

防御手段：

* 敏感数据加密
* 登录态验证

### 数据安全

一般来说，我们需要对敏感数据进行加密，例如密码。

密码的作用就是证明你是你自己，常见的泄露方式如下：

* 数据库泄露
* 服务器被入侵
* 内部人员泄露
* 撞库，也就是与其他网站设置相同的密码

所以，密码存储一定要：

* 严禁明文存储
* 加密复杂度要求
* 原始密码复杂度要求
* 加盐，增加额外复杂度

密码一般是通过hash算法来进行加密，这种算法有如下特点：

* 明文和密文一一对应
* 雪崩效应
* 密文无法反推明文
* 密文固定长度，一般为32位

常见的hash算法有md5，sha1以及sha256。下面以`md5`作为例子：

* md5(明文)
* md5(md5(明文))
* md5(sha1(明文))

注意：第一种只加密一次不推荐，因为可以通过彩虹表查找出。

另外，密码在传输过程中也需要注意：

* 使用更加安全的https协议
* 登录失败次数限制
* 前端密码加密（js-md5模块）

当然，还有其他一些生物特征密码，如：

* 指纹
* 虹膜
* 人脸
* 声纹
* ...

### SQL注入

通过拼接SQL语句来进行攻击。

防御手段：

* 严格限制web应用的数据库的操作权限，也就是说用户的权限仅仅是满足工作的最低权限
* 对进入数据库的特殊字符进行转义处理或编码转换
* 不要拼接SQL语句，所有查询语句应该使用数据库提供的参数化查询接口

### 文件上传安全

文件上传漏洞就是利用网页代码中的文件上传路径变量过滤不严将可执行的文件上传到一个服务器中，再通过URL去访问以执行恶意代码。

防御手段：

* 服务器端做MIME类型检测
* 上传的文件重新命名
* 服务器端做内容检测，通过文件幻数来检测文件开头

> 幻数（magic number），它可以用来标记文件或者协议的格式，很多文件都有幻数标志来表明该文件的格式。例如：
>
> * JPG：`FF D8 FF E0 00 10 4A 46 49 46`
> * GIF：`47 49 46 38 39 61`
> * PNG：`89 50 4E 47`

### DOS攻击

* DOS（ denial of service ）攻击：一台或几台服务器大量模拟正常用户去请求目标服务器
* DDOS（ distributed denial of service）攻击：大规模分布式（肉鸡或者代理）模拟正常用户去大量请求目标服务器

这两种攻击手段都会大量占用服务器端资源，导致无法服务正常用户。

这种攻击手段很难防御，因为它：

* 可能攻击DNS节点
* TCP半连接
* HTTP连接

当然还是一些成效微小的防御手段：

* 防火墙：硬件防火墙（价格贵）和软件防火墙（iptables），可以用来封锁IP

* web服务器，下面是Nginx配置封锁IP

  ```nginx
  locaiton / {
      deny 1.2.3.4
  }
  ```

  当然还可以通过WAF自动识别并且拦截那些频繁请求的IP地址

* 带宽扩容：临时扩大带宽，消化大容量请求；或者临时购买更多主机，DNS会把访问量均匀分配

* CDN：将网站的静态内容分发给多个服务器，用户就近访问，也属于带宽扩容的一种。当然前提是你的网站的大部分内容是可以静态缓存的，对于动态网站，如论坛就不适用了。

  像大部分云服务器厂商提供的高防IP原理也就搭建一个小型的CDN，网站域名指向这个高防IP，它提供一个缓冲层，清洗流量，并对源服务器的内容进行缓存。注意：一旦使用CDN，不要暴露源服务器的IP，攻击者是可以绕过CDN直接攻击源服务器。

如果受到攻击导致服务下线，你需要有一个备份网站，用来临时通知用户，这种临时网站建议放到Github Pages，因为它们带宽大，支持绑定域名。

### 重放攻击

用户操作的某个接口被攻击者截取，然后原封不动的多次调用。最常见的攻击是付款接口，购买接口等。

防御手段：

* 加随机数
* 加时间戳
* 加流水号

注意：HTTPS可以有效防止报文数据不被监听，但是防御不了重放攻击。