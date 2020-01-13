## 脚本嵌入

### 方法

- `<script>`元素直接嵌入代码。
- `<script>`标签加载外部脚本
- 事件属性
- `URL` 协议

### script元素

浏览器加载 JavaScript 脚本，主要通过`<script>`元素完成。正常的网页加载流程是这样的。

1. 浏览器一边下载 HTML 网页，一边开始解析。也就是说，不等到下载完，就开始解析。
2. 解析过程中，浏览器发现`<script>`元素，就暂停解析，把网页渲染的控制权转交给 JavaScript 引擎。
3. 如果`<script>`元素引用了外部脚本，就下载该脚本再执行，否则就直接执行代码。
4. JavaScript 引擎执行完毕，控制权交还渲染引擎，恢复往下解析 HTML 网页。

加载外部脚本时，浏览器会暂停页面渲染，等待脚本下载并执行完成后，再继续渲染。原因是 JavaScript 代码可以修改 DOM，所以必须把控制权让给它，否则会导致复杂的线程竞赛的问题。

如果外部脚本加载时间很长（一直无法完成下载），那么浏览器就会一直等待脚本下载完成，造成网页长时间失去响应，浏览器就会呈现“假死”状态，这被称为“阻塞效应”。

为了避免这种情况，较好的做法是将`<script>`标签都放在页面底部，而不是头部。这样即使遇到脚本失去响应，网页主体的渲染也已经完成了，用户至少可以看到内容，而不是面对一张空白的页面。如果某些脚本代码非常重要，一定要放在页面头部的话，最好直接将代码写入页面，而不是连接外部脚本文件，这样能缩短加载时间。

脚本文件都放在网页尾部加载，还有一个好处。因为在 DOM 结构生成之前就调用 DOM 节点，JavaScript 会报错，如果脚本都在网页尾部加载，就不存在这个问题，因为这时 DOM 肯定已经生成了。

如果有多个`script`标签，比如下面这样。

```
<script src="a.js"></script>
<script src="b.js"></script>
```

浏览器会同时并行下载`a.js`和`b.js`，但是，执行时会保证先执行`a.js`，然后再执行`b.js`，即使后者先下载完成，也是如此。也就是说，脚本的执行顺序由它们在页面中的出现顺序决定，这是为了保证脚本之间的依赖关系不受到破坏。当然，加载这两个脚本都会产生“阻塞效应”，必须等到它们都加载完成，浏览器才会继续页面渲染。

解析和执行 CSS，也会产生阻塞。Firefox 浏览器会等到脚本前面的所有样式表，都下载并解析完，再执行脚本；Webkit则是一旦发现脚本引用了样式，就会暂停执行脚本，等到样式表下载并解析完，再恢复执行。

此外，对于来自同一个域名的资源，比如脚本文件、样式表文件、图片文件等，浏览器一般有限制，同时最多下载6～20个资源，即最多同时打开的 TCP 连接有限制，这是为了防止对服务器造成太大压力。如果是来自不同域名的资源，就没有这个限制。所以，通常把静态文件放在不同的域名之下，以加快下载速度。

#### defer 属性

为了解决脚本文件下载阻塞网页渲染的问题，一个方法是对`<script>`元素加入`defer`属性。它的作用是延迟脚本的执行，等到 DOM 加载生成后，再执行脚本。

```
<script src="a.js" defer></script>
<script src="b.js" defer></script>
```

上面代码中，只有等到 DOM 加载完成后，才会执行`a.js`和`b.js`。

`defer`属性的运行流程如下。

1. 浏览器开始解析 HTML 网页。
2. 解析过程中，发现带有`defer`属性的`<script>`元素。
3. 浏览器继续往下解析 HTML 网页，同时并行下载`<script>`元素加载的外部脚本。
4. 浏览器完成解析 HTML 网页，此时再回过头执行已经下载完成的脚本。

有了`defer`属性，浏览器下载脚本文件的时候，不会阻塞页面渲染。下载的脚本文件在`DOMContentLoaded`事件触发前执行（即刚刚读取完`</html>`标签），而且可以保证执行顺序就是它们在页面上出现的顺序。

对于内置而不是加载外部脚本的`script`标签，以及动态生成的`script`标签，`defer`属性不起作用。另外，使用`defer`加载的外部脚本不应该使用`document.write`方法。

#### async 属性

解决“阻塞效应”的另一个方法是对`<script>`元素加入`async`属性。

```
<script src="a.js" async></script>
<script src="b.js" async></script>
```

`async`属性的作用是，使用另一个进程下载脚本，下载时不会阻塞渲染。

1. 浏览器开始解析 HTML 网页。
2. 解析过程中，发现带有`async`属性的`script`标签。
3. 浏览器继续往下解析 HTML 网页，同时并行下载`<script>`标签中的外部脚本。
4. 脚本下载完成，浏览器暂停解析 HTML 网页，开始执行下载的脚本。
5. 脚本执行完毕，浏览器恢复解析 HTML 网页。

`async`属性可以保证脚本下载的同时，浏览器继续渲染。需要注意的是，一旦采用这个属性，就无法保证脚本的执行顺序。哪个脚本先下载结束，就先执行那个脚本。另外，使用`async`属性的脚本文件里面的代码，不应该使用`document.write`方法。

`defer`属性和`async`属性到底应该使用哪一个？

一般来说，如果脚本之间没有依赖关系，就使用`async`属性，如果脚本之间有依赖关系，就使用`defer`属性。如果同时使用`async`和`defer`属性，后者不起作用，浏览器行为由`async`属性决定。

## 浏览器组成

浏览器的核心是两部分：渲染引擎和`JavaScript`引擎。

### 渲染引擎

不同的浏览器有不同的渲染引擎。

- `Firefox`：`Gecko`引擎
- `Safari`：`WebKit`引擎
- `Chrome`：`Blink`引擎
- `IE`：`Trident`引擎
- `Edge`: `EdgeHTML`引擎

渲染引擎处理网页，通常分成四个阶段。

1. 解析代码：HTML 代码解析为 DOM，CSS 代码解析为 CSSOM（CSS Object Model）。
2. 对象合成：将 DOM 和 CSSOM 合成一棵渲染树（render tree）。
3. 布局：计算出渲染树的布局（layout）。
4. 绘制：将渲染树绘制到屏幕。

以上四步并非严格按顺序执行，往往第一步还没完成，第二步和第三步就已经开始了。所以，会看到这种情况：网页的 HTML 代码还没下载完，但浏览器已经显示出内容了。

### 重流和重绘

渲染树转换为网页布局，称为“布局流”（flow）；布局显示到页面的这个过程，称为“绘制”（paint）。它们都具有阻塞效应，并且会耗费很多时间和计算资源。

页面生成以后，脚本操作和样式表操作，都会触发“重流”（reflow）和“重绘”（repaint）。用户的互动也会触发重流和重绘，比如设置了鼠标悬停（`a:hover`）效果、页面滚动、在输入框中输入文本、改变窗口大小等等。

重流和重绘并不一定一起发生，重流必然导致重绘，重绘不一定需要重流。比如改变元素颜色，只会导致重绘，而不会导致重流；改变元素的布局，则会导致重绘和重流。

大多数情况下，浏览器会智能判断，将重流和重绘只限制到相关的子树上面，最小化所耗费的代价，而不会全局重新生成网页。

作为开发者，应该尽量设法降低重绘的次数和成本。比如，尽量不要变动高层的 DOM 元素，而以底层 DOM 元素的变动代替；再比如，重绘`table`布局和`flex`布局，开销都会比较大。

```
var foo = document.getElementById('foobar');

foo.style.color = 'blue';
foo.style.marginTop = '30px';
```

上面的代码只会导致一次重绘，因为浏览器会累积 DOM 变动，然后一次性执行。

下面是一些优化技巧。

- 读取 DOM 或者写入 DOM，尽量写在一起，不要混杂。不要读取一个 DOM 节点，然后立刻写入，接着再读取一个 DOM 节点。
- 缓存 DOM 信息。
- 不要一项一项地改变样式，而是使用 CSS class 一次性改变样式。
- 使用`documentFragment`操作 DOM
- 动画使用`absolute`定位或`fixed`定位，这样可以减少对其他元素的影响。
- 只在必要时才显示隐藏元素。
- 使用`window.requestAnimationFrame()`，因为它可以把代码推迟到下一次重流时执行，而不是立即要求页面重流。
- 使用虚拟 DOM（virtual DOM）库。

下面是一个`window.requestAnimationFrame()`对比效果的例子。

```
// 重绘代价高
function doubleHeight(element) {
  var currentHeight = element.clientHeight;
  element.style.height = (currentHeight * 2) + 'px';
}

all_my_elements.forEach(doubleHeight);

// 重绘代价低
function doubleHeight(element) {
  var currentHeight = element.clientHeight;

  window.requestAnimationFrame(function () {
    element.style.height = (currentHeight * 2) + 'px';
  });
}

all_my_elements.forEach(doubleHeight);
```

上面的第一段代码，每读一次 DOM，就写入新的值，会造成不停的重排和重流。第二段代码把所有的写操作，都累积在一起，从而 DOM 代码变动的代价就最小化了。



## window对象

### 属性

* `window.name`
* `window.closed`
* `window.opener`
* `window.self`
* `window.window`
* `window.frames`
* `window.length`
* `window.frameElement`
* `window.top`
* `window.parent`
* `window.status`
* `window.devicePixelRatio`

### 方法

* `alert`
* `prompt`
* `confirm`
* `open`
* `close`
* `stop`
* `moveTo`
* `moveBy`
* `resizeTo`
* `resizeBy`
* `scrolltTo`
* `scroll`
* `scrollBy`
* `getSelection`
* `getComputedStyle`
* `requestAnimationFrame`

### 事件

* `load`
* `error`

## Navigator对象

### 属性

* `userAgent`
* `plugins`
* `platform`
* `onLine`
* `language`
* `languages`
* `geolocation`
* `cookieEnabled`

## Screen对象

### 属性

* `height`
* `width`
* `availHeight`
* `availWidth`
* `pixelDepth`
* `colorDepth`
* `orientation`

## History对象

### 属性

* `length`
* `state`

### 方法

* `back`
* `forward`

- `go`
- `pushState`
- `replaceState`

### 事件

* `popstate`

## Location对象

### 属性

`Location`对象提供以下属性。

- `Location.href`：整个 URL。
- `Location.protocol`：当前 URL 的协议，包括冒号（`:`）。
- `Location.host`：主机，包括冒号（`:`）和端口（默认的80端口和443端口会省略）。
- `Location.hostname`：主机名，不包括端口。
- `Location.port`：端口号。
- `Location.pathname`：URL 的路径部分，从根路径`/`开始。
- `Location.search`：查询字符串部分，从问号`?`开始。
- `Location.hash`：片段字符串部分，从`#`开始。
- `Location.username`：域名前面的用户名。
- `Location.password`：域名前面的密码。
- `Location.origin`：URL 的协议、主机名和端口。

```js
// 当前网址为
// http://user:passwd@www.example.com:4097/path/a.html?x=111#part1
document.location.href
// "http://user:passwd@www.example.com:4097/path/a.html?x=111#part1"
document.location.protocol
// "http:"
document.location.host
// "www.example.com:4097"
document.location.hostname
// "www.example.com"
document.location.port
// "4097"
document.location.pathname
// "/path/a.html"
document.location.search
// "?x=111"
document.location.hash
// "#part1"
document.location.username
// "user"
document.location.password
// "passwd"
document.location.origin
// "http://user:passwd@www.example.com:4097"
```

这些属性里面，只有`origin`属性是只读的，其他属性都可写。

注意，如果对`Location.href`写入新的 URL 地址，浏览器会立刻跳转到这个新地址。

```
// 跳转到新网址
document.location.href = 'http://www.example.com';
```

这个特性常常用于让网页自动滚动到新的锚点。

```
document.location.href = '#top';
// 等同于
document.location.hash = '#top';
```

直接改写`location`，相当于写入`href`属性。

```
document.location = 'http://www.example.com';
// 等同于
document.location.href = 'http://www.example.com';
```

另外，`Location.href`属性是浏览器唯一允许跨域写入的属性，即非同源的窗口可以改写另一个窗口的`Location.href`属性，导致后者的网址跳转。`Location`的其他属性都不允许跨域写入。

### 方法

* `assign`
* `replace`
* `reload`
* `toString`

## 本地存储

### cookie

### Storage

#### 属性

* `length`

#### 方法

* `setItem`
* `getItem`
* `removeItem`
* `clear`
* `key`

#### 事件

`Storage`接口储存的数据发生变化时，会触发`storage`事件，可以指定这个事件的监听函数。

```
window.addEventListener('storage', onStorageChange);
```

## XMLHttpRequest

```js
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function(){
  // 通信成功时，状态值为4
  if (xhr.readyState === 4){
    if (xhr.status === 200){
      console.log(xhr.responseText);
    } else {
      console.error(xhr.statusText);
    }
  }
};

xhr.onerror = function (e) {
  console.error(xhr.statusText);
};

xhr.open('GET', '/endpoint', true);
xhr.send(null);
```

### 事件

`XMLHttpRequest`对象可以对以下事件指定监听函数。

- `XMLHttpRequest.onloadstart`：`loadstart`事件（HTTP 请求发出）的监听函数
- `XMLHttpRequest.onprogress`：`progress`事件（正在发送和加载数据）的监听函数
- `XMLHttpRequest.onabort：abort 事件（请求中止，比如用户调用了`abort()`方法）的监听函数
- `XMLHttpRequest.onerro`r：`error`事件（请求失败）的监听函数
- `XMLHttpRequest.onload`：`load`事件（请求成功完成）的监听函数
- `XMLHttpRequest.ontimeout`：`timeout`事件（用户指定的时限超过了，请求还未完成）的监听函数
- `XMLHttpRequest.onloadend`：`loadend`事件（请求完成，不管成功或失败）的监听函数
- `XMLHttpRequest.onreadystatechange`：对不同状态进行不同处理

### 实例属性

* `readyState`
* `onreadystatechange`
* `response`
* `responseType`
* `responseText`
* `responseXML`
* `responseURL`
* `responseURL`
* `statusText`
* `status`
* `timeout`
* `withCredentials`

### 实例方法

* `open`
* `send`
* `setRequestHeader`
* `overrideMimeType`
* `getResponseHeader`
* `getAllResponseHeaders`
* `abort`

## 同源限制

所谓同源指的是三个相同：

- 协议相同
- 域名相同
- 端口相同

### 目的

同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。

### 限制范围

目前，如果非同源，共有三种行为受到限制：

* 无法读取非同源网页的`Cookie`、`LocalStorage`和`IndexedDB`
* 无法接触非同源网页的`DOM`
* 无法向非同源地址发送`AJAX`请求（可以发送，但浏览器会拒绝接受响应）

### 跨域

* `JSONP`
* `CORS`
* `websocket`

## 编码和解码

网页的 URL 只能包含合法的字符，如果出现其他字符出现在`URL`之中都必须转义，规则是根据操作系统的默认编码，将每个字节转为百分号（`%`）加上两个大写的十六进制字母。

JavaScript 提供四个 URL 的编码/解码方法。

- `encodeURI()`
- `encodeURIComponent()`
- `decodeURI()`
- `decodeURIComponent()`

