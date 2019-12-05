### 简介

文档对象模型（DOM，Document Object Model）是针对XML但经过扩展用于HTML的应用程序编程接口（API，Application Programming Interface）。DOM把整个页面映射为一个多层节点结构。HTML或XML页面中的每个组成部分都是某种类型的节点，这些节点又包含着不同类型的数据。

通过DOM创建的这个表示文档的树形图，开发人员获得了控制页面内容和结构的主动权。借助DOM提供的API，开发人员可以轻松自如地删除、添加、替换或修改任何节点。

在Internet Explorer 4和Netscape Navigator 4分别支持的不同形式的DHTML（Dynamic HTML）基础上，开发人员首次无需重新加载网页，就可以修改其外观和内容了。然而，DHTML在给Web技术发展带来巨大进步的同时，也带来了巨大的问题。由于Netscape和微软在开发DHTML方面各持己见，过去那个只编写一个HTML页面就能够在任何浏览器中运行的时代结束了。

对开发人员而言，如果想继续保持 Web 跨平台的天性，就必须额外多做一些工作。而人们真正担心的是，如果不对Netscape 和微软加以控制，Web开发领域就会出现技术上两强割据，浏览器互不兼容的局面。此时，负责制定Web通信标准的W3C（World Wide Web Consortium，万维网联盟）开始着手规划DOM。

### DOM级别

DOM1 级（DOM Level 1）于1998年10月成为W3C的推荐标准。DOM1级由两个模块组成：DOM 核心（DOM Core）和 DOM HTML。其中，DOM核心规定的是如何映射基于XML的文档结构，以便简化对文档中任意部分的访问和操作。DOM HTML模块则在DOM 核心的基础上加以扩展，添加了针对HTML的对象和方法。

> 注意：DOM并不只是针对JavaScript，很多别的语言也都实现了DOM。不过，在Web浏览器中，基于ECMAScript实现的DOM的确已经成为JavaScript这门语言的一个重要组成部分。

如果说DOM1 级的目标主要是映射文档的结构，那么DOM 2级的目标就要宽泛多了。DOM2 级在原来DOM的基础上又扩充了（DHTML 一直都支持的）鼠标和用户界面事件、范围、遍历（迭代DOM文档的方法）等细分模块，而且通过对象接口增加了对CSS（Cascading Style Sheets，层叠样式表）的支持。DOM 1级中的DOM核心模块也经过扩展开始支持 XML 命名空间。

DOM2 级引入了下列新模块，也给出了众多新类型和新接口的定义。

* DOM 视图（DOM Views）：定义了跟踪不同文档（例如，应用 CSS 之前和之后的文档）视图的接口
* DOM 事件（DOM Events）：定义了事件和事件处理的接口
* DOM 样式（DOM Style）：定义了基于 CSS 为元素应用样式的接口
* DOM 遍历和范围（DOM Traversal and Range）：定义了遍历和操作文档树的接口

DOM3 级则进一步扩展了 DOM，引入了以统一方式加载和保存文档的方法——在DOM加载和保存（DOM Load and Save）模块中定义；新增了验证文档的方法——在 DOM 验证（DOM Validation）模块中定义。DOM3 级也对 DOM 核心进行了扩展，开始支持XML 1.0规范，涉及XML Infoset、XPath和XML Base。

> 在阅读DOM标准的时候，读者可能会看到DOM 0级（DOM Level 0）的字眼。实际上，DOM 0级标准是不存在的，所谓的DOM 0级只是DOM历史坐标中的一个参照点而已。具体来说，DOM 0级是指IE4和Netscape Navigator 4.0最初支持的DHTML。

### 其他DOM标准

除了DOM核心和DOM HTML接口之外，另外几种语言还发布了只针对自己的DOM标准。下面列出的语言都是基于XML的，每种语言的DOM标准都添加了与特定语言相关的新方法和新接口：

* SVG（Scalable Vector Graphic，可伸缩矢量图）1.0
* MathML（Mathematical Markup Language，数学标记语言）1.0
* SMIL（Synchronized Multimedia Integration Language，同步多媒体集成语言）

还有一些语言也开发了自己的DOM实现，例如 Mozilla的XUL（XML User Interface Language，XML用户界面语言）。但是，只有上面列出的几种语言是W3C的推荐标准。

### 节点树

每个浏览器窗口、标签页和窗体都含有一个独立的Window对象，而每个Window对象都有一个document属性引用Document对象。Document对象本身是一个巨大的API的核心对象，称为文档对象模型(Document Object Model，DOM)。

实际上，DOM是JavaScript操作网页的接口，它的作用是将网页转为一个JavaScript对象，从而可以用脚本进行各种操作(比如增删内容)。但是严格来说，DOM并不属于JavaScript，但是操作DOM是JavaScript最常见的任务，而JavaScript也是最常用于DOM操作的语言。

浏览器会根据DOM模型，将结构化文档(比如HTML和XHTML)解析成一系列的节点，再由这些节点组成一个树状结构(DOM Tree)。所有的节点和最终的树状结构，都有规范的对外接口。下面是HTML文档抽象为DOM树的形象表示：

```html
<html>
  <head>
    <title>Sample Document</title>
  </head>
  <body>
    <h1>An HTML Document</h1>
    <p>This is a<i>simple</i>document</p>
  </body>
</html>
```

渲染成DOM树如下：

![dom tree](http://otjjz0nqd.bkt.clouddn.com/javascript/domTree.png)

上图可以看出，DOM树是由各种不同类型的节点(node)组成，节点是DOM的最小组成单位。而节点的类型可以分为如下几种：

* `Document`：整个文档树的顶层节点
* `DocumentType`：`doctype`标签（比如`<!DOCTYPE html>`）
* `Element`：网页的各种HTML标签（比如`<body>`、`<a>`等）
* `Attribute`：网页元素的属性（比如`class="right"`）
* `Text`：标签之间或标签包含的文本
* `Comment`：注释
* `DocumentFragment`：文档的片段

这七种节点都属于浏览器原生提供的节点对象(Node)的派生对象，继承了节点对象上的属性和方法。

下图可以很好表示Node节点的派生对象(主要针对HTML文档，因为文档还可能是XML和HTML等文档)。其中DOM树中最顶层节点是document类型节点，它代表了整个文档，Element类型的节点代表文档中的元素，而Attr类型的节点代表文档元素的属性。

![dom node](http://otjjz0nqd.bkt.clouddn.com/javascript/domNode.png)

此外，在HTML文档中，除了根节点(html)之外，其它节点对于周围的节点都存在如下三种关系：

* 父节点关系
* 子节点关系
* 同级节点关系

DOM中提供了接口来表示这些关系。其中，获取节点的第一个和最后一个子节点用`firstChild`和`lastChild`属性，获取节点的前一个和后一个兄弟节点用`nextSibling`和`previousSibling`属性。

### DOM级别

DOM标准经历了从DOM0级到DOM3级的发展。最初浏览器定义了方便检索页面元素的API，这种实验性质的API称为DHTML，后来又被称为DOM0级。所以，实际上DOM0级并不存在，它只是一个参考标准。

而DOM1级规定了DOM由DOM Core和DOM HTML组成，其中DOM Core规定了如何映射基于XML文档结构，而DOM HTML是对DOM核心的扩展，扩展了针对HTML文档的方法。 

DOM2级主要是由四部分组成。其中DOM Views定义跟踪不同应用视图的接口，DOM Events定义了事件与事件之间的接口，DOM Style定义了基于CSS为元素应用样式的接口，DOM Traversal and Range即DOM遍历和范围，定义了遍历和操作文档树的接口。

最后，DOM3级新增DOM Load and Save，规定了统一方式加载和保存文档的方法，以及DOM Validation，定义验证文档的方法。此外，目前的通用版本是DOM3，下一代版本DOM4正在拟定中。

### HTML嵌套规则

如果不按照HTML嵌套规则书写HTML代码，那么浏览器就不会正确解析。它会将不符合嵌套的节点放到目标节点的下面或者变成纯文本。正确的嵌套规则如下：

1. 内联元素不能包含块元素
2. 块级元素不能放在p标签里
3. 有些特殊的块级元素也不能包含块级元素，这些元素有h1-h6，p，dt标签元素
4. select标签只能包含option和optgroup标签，并且option标签只能放文本

### Node接口

所有的节点对象都继承了Node接口，拥有一些共同的属性和方法。

```js
document instanceof Node						//true
Node.prototype.hasOwnProperty("nodeName")		//true
Node.prototype.hasOwnProperty("appendChild")	//true
```

#### 属性

##### Node.prototype.nodeType

该属性返回一个整数值，表示节点的类型。

```js
document.nodeType		//9
```

另外，Node对象还定义了常量，对应这些类型值。

```js
document.nodeType === Node.DOCUMENT_NODE	//true
```

不同节点的nodeType属性值和对应的常量如下：

* 文档节点（document）：9，对应常量`Node.DOCUMENT_NODE`
* 元素节点（element）：1，对应常量`Node.ELEMENT_NODE`
* 属性节点（attr）：2，对应常量`Node.ATTRIBUTE_NODE`
* 文本节点（text）：3，对应常量`Node.TEXT_NODE`
* 文档片断节点（DocumentFragment）：11，对应常量`Node.DOCUMENT_FRAGMENT_NODE`
* 文档类型节点（DocumentType）：10，对应常量`Node.DOCUMENT_TYPE_NODE`
* 注释节点（Comment）：8，对应常量`Node.COMMENT_NODE`

该属性常常用于确定节点类型。

```js
document.querySelector('a').nodeType === 1					// true
document.querySelector('a').nodeType === Node.ELEMENT_NODE	// true，两种写法等价
```

##### Node.prototype.nodeName

该属性返回节点的名称，不同节点的nodeName属性值如下：

* 文档节点（document）：`#document`
* 元素节点（element）：大写的标签名
* 属性节点（attr）：属性的名称
* 文本节点（text）：`#text`
* 文档片断节点（DocumentFragment）：`#document-fragment`
* 文档类型节点（DocumentType）：文档的类型
* 注释节点（Comment）：`#comment`

##### Node.prototype.nodeValue

该属性返回一个字符串，表示当前节点本身的文本值，该属性可读写。

只有文本节点（text）和注释节点（comment）有文本值，因此这两类节点的`nodeValue`可以返回结果，其他类型的节点一律返回`null`。同样的，也只有这两类节点可以设置`nodeValue`属性的值，其他类型的节点设置无效。

```html
<div>hi</div>
<script>
	document.querySelector("div").firstChild.nodeValue	//"hi"
</script>
```

##### Node.prototype.textContent

该属性返回当前节点和它的所有后代节点的文本内容，它会自动忽略当前节点内部的HTML标签，返回所有文本内容。

```html
<div>hello<span>world</span></div>
<script>
	document.querySelector("div").textContent		//"helloworld"
</script>
```

该属性是可读写的，设置该属性的值，会用一个新的文本节点替换所有原来的子节点。此外，它还会对HTML标签转义，这很适合用于用户提供的内容。

```javascript
document.querySelector("div").textContent = "<p>hello</p>"	//p标签会被解释为文本而不是当做标签处理
```

对于Text和Comment节点，该属性与nodeValue属性返回值相同。但是对于其它类型的节点，该属性会将指定节点的每个子节点(不包括comment节点)的内容连接在一起返回。如果当前节点没有子节点，则返回空字符串。

此外，document节点和doctype节点的`textContent`属性为null。如果要读取整个文档的内容，需要在html节点上调用该属性。

```javascript
document.documentElement.textContent
```

##### Node.prototype.baseURI

该属性返回一个字符串，表示当前网页的绝对路径。浏览器根据这个属性，计算网页上的相对路径的 URL。该属性为只读。

```javascript
document.baseURI		//""http://javascript.ruanyifeng.com/dom/node.html#toc4"
```

如果无法读到网页的URL，该属性返回null。

不同节点调用这个属性返回值通常相同，属性值一般由当前文档的URL(即window.location)决定，但是可以使用base标签改变该属性的值。

```html
<base href="http://www.baidu.com">
<script>
	document.scripts[0].baseURI	//"http://www.baidu.com"
</script>
```

##### Node.prototype.ownerDocument

该属性返回当前节点所在的文档对象，即document对象。注意：document节点上的`ownerDocument`属性则返回null。

```javascript
document.body.ownerDocument === document	//true
document.ownerDocument						//null
```

##### Node.prototype.nextSibling

该属性返回当前节点的后一个同级节点。如果当前节点后面没有同级节点，则返回null。注意，该属性包括Text节点和Comment节点，因此如果当前节点后面有空格，该属性会返回一个文本节点，内容为空格。

```javascript
document.firstChild === document.childNodes[0];		//true
```

##### Node.prototype.previousSibling

该属性返回当前节点的前一个同级节点。如果当前节点前面没有同级节点，则返回null。注意：该属性和`Node.nextSibling`属性一样，同样包含文本节点和注释节点。

##### Node.prototype.parentNode

该属性返回当前节点的父节点。对于一个节点来说，它的父节点只可能是三种类型：element节点、document节点和documentfragment节点。而对于document节点和documentfragment节点来说，它们的父节点都是null。另外，对于那些生成后还未插入DOM Tree中的节点，其父节点也是null。

```javascript
document.parentNode							//null
document.createElement("div").parentNode	//null
```

##### Node.prototype.parentElement

该属性返回当前节点的父元素节点。如果当前节点没有父节点或者父节点类型不是element节点，则返回null。

```javascript
if(document.links[0].parentElement){
    document.links[0].parentElement.style.color = "red"		//只有元素节点才有style属性
}
```

由于父节点只可能是三种类型：元素节点、文档节点（document）和文档片段节点（documentfragment）。`parentElement`属性相当于把后两种父节点都排除了。

##### Node.prototype.childNodes

该属性返回一个由当前节点的所有子节点组成的动态NodeList对象实例。如果当前节点没有子节点，则返回一个空NodeList对象。注意：该属性返回的NodeList对象实例中包含Text节点和Comment节点。

```javascript
document.querySelector("div").childNodes[0] === document.querySelector("div").firstChild
```

注意：文档节点（document）就有两个子节点：文档类型节点（docType）和 HTML 根元素节点。

##### Node..prototype.firstChild、Node.prototype.lastChild

* 前者返回当前节点的第一个子节点，如果当前节点没有子节点，则返回null。
* 后者返回当前节点的最后一个子节点，如果当前节点没有子节点，则返回null。

注意：这两个属性的返回值都包含Text节点和Comment节点。

#### 方法

##### Node..prototype.appendChild()

该方法接受一个节点对象作为参数，将其作为最后一个子节点，插入到指定节点。注意：如果传入的节点参数是DOM Tree中已经存在的节点，那么该方法会将其从原来的位置移动到新位置。

```javascript
document.body.appendChild(document.createElement("p"))
```

如果`appendChild`方法的参数是`DocumentFragment`节点，那么插入的是`DocumentFragment`的所有子节点，而不是`DocumentFragment`节点本身。返回值是一个空的`DocumentFragment`节点。

##### Node.prototype.hasChildNodes()

该方法返回一个布尔值，表示当前节点是否有子节点。

```javascript
if(document.body.hasChildNodes()){
    document.body.removeChild(document.body.childNodes[0])
}
```

注意，子节点包括所有类型的节点，并不仅仅是元素节点。哪怕节点只包含一个空格，`hasChildNodes`方法也会返回`true`。

判断一个节点有没有子节点，有许多种方法，下面是其中的三种。

- `node.hasChildNodes()`
- `node.firstChild !== null`
- `node.childNodes && node.childNodes.length > 0`

##### Node.prototype.cloneNode()

该方法用于克隆一个节点。它接受一个布尔值作为参数，表示是否同时克隆子节点。它的返回值是一个克隆出来的新节点。

```html
<div class="demo">123</div>
<script>
	var div = document.querySelector("div")
    div.onclick = function(){
        alert(this.innerHTML)
    }
    var clonenode = div.cloneNode()		//<div class="demo"></div>，不复制子节点123
    document.body.appendChild(clonenode)//点击插入的克隆节点并不会弹出内容，说明没有复制onclick属性
</script>
```

该方法有一些使用注意点：

* 克隆一个节点，会拷贝该节点的所有属性，但是会丧失`addEventListener`方法和`on-`属性（即`node.onclick = fn`），添加在这个节点上的事件回调函数
* 该方法返回的节点不在文档之中，即没有任何父节点，必须使用诸如`Node.appendChild`这样的方法添加到文档之中
* 克隆一个节点之后，DOM 有可能出现两个有相同`id`属性（即`id="xxx"`）的网页元素，这时应该修改其中一个元素的`id`属性。如果原节点有`name`属性，可能也需要修改

##### Node..prototype.insertBefore()

该方法用于将某个节点插入当前节点内部的指定位置。该方法接受两个参数，第一个参数是所要插入的节点，第二个参数是当前节点内部的一个子节点，新的节点将插在这个子节点的前面。该方法返回被插入的节点。

```javascript
document.body.insertBefore(document.createElement("p"),document.body.lastChild)
//如果body节点中没有任何子节点，那么其lastChild属性就返回null
```

注意：如果第二个参数为null时，那么第一个参数节点将插在当前节点的最后位置，即变成最后一个子节点。而如果第一个参数节点是DOM Tree中已经存在的节点，那么该方法会将该节点从原来的位置移除，插入到新的位置。

如果要插入的节点是`DocumentFragment`类型，那么插入的将是`DocumentFragment`的所有子节点，而不是`DocumentFragment`节点本身。返回值将是一个空的`DocumentFragment`节点。

> 注意：并不存在insertAfter方法，如果新节点要插在父节点的某个子节点后面，可以用`insertBefore`方法结合`nextSibling`属性模拟。

##### Node.prototype.removeChild()

该方法接受一个子节点作为参数，用于从当前节点中移除该子节点。该方法返回被移除的子节点。如果这个参数节点不是当前节点的子节点，该方法会报错。

```javascript
if(document.body.firstChild){
    document.body.removeChild(document.body.childNodes[0])
}
```

注意：该方法时在父节点上调用，而不是在被移除的节点上调用。

另外被移除的节点依然存在于内存之中，但是不再是DOM的一部分。所以，一个节点移除之后，依然可以将其插入到另一个节点中。

##### Node.prototype.replaceChild()

该方法用于将一个新的节点，替换当前节点的某一个子节点。该方法接受两个参数，第一个参数是用来替换的新节点，第二个参数时将要被替换走的子节点。该方法返回被替换走的节点。

```javascript
document.body.replaceChild(document.createElement("p"),document.body.firstChild)
```

##### Node.prototype.contains()

该方法接受一个节点作为参数，判断这个参数节点是否是当前节点的后代节点。注意：如果当前节点作为参数传入该方法中，也会返回true。虽然从意义上说，一个节点不应该包含自身。

```javascript
document.body.contains(document.body)	//true
```

##### Node.prototype.compareDocumentPosition()

该方法的用法与contains方法完全一致，返回一个七个比特位的二进制值，表示参数节点与当前节点的关系。

| 二进制值 | 十进制值 | 含义                                               |
| -------- | -------- | -------------------------------------------------- |
| 000000   | 0        | 两个节点相同                                       |
| 000001   | 1        | 两个节点不在同一个文档（即有一个节点不在当前文档） |
| 000010   | 2        | 参数节点在当前节点的前面                           |
| 000100   | 4        | 参数节点在当前节点的后面                           |
| 001000   | 8        | 参数节点包含当前节点                               |
| 010000   | 16       | 当前节点包含参数节点                               |
| 100000   | 32       | 浏览器内部使用                                     |

```js
// HTML 代码如下
// <div id="mydiv">
//   <form><input id="test" /></form>
// </div>

var div = document.getElementById('mydiv');
var input = document.getElementById('test');

div.compareDocumentPosition(input) // 20
input.compareDocumentPosition(div) // 10
```

上面代码中，节点`div`包含节点`input`（二进制`010000`），而且节点`input`在节点`div`的后面（二进制`000100`），所以第一个`compareDocumentPosition`方法返回`20`（二进制`010100`，即`010000 + 000100`），第二个`compareDocumentPosition`方法返回`10`（二进制`001010`）。

由于`compareDocumentPosition`返回值的含义，定义在每一个比特位上，所以如果要检查某一种特定的含义，就需要使用比特位运算符。

```js
var head = document.head;
var body = document.body;
if (head.compareDocumentPosition(body) & 4) {
  console.log('文档结构正确');
} else {
  console.log('<body> 不能在 <head> 前面');
}
```

上面代码中，`compareDocumentPosition`的返回值与`4`（又称掩码）进行与运算（`&`），得到一个布尔值，表示`<head>`是否在`<body>`前面。

##### Node.prototype.isEqualNode()，Node.prototype.isSameNode()

前者返回一个布尔值，用于检查两个节点是否相等。所谓相等的节点，指的是两个节点的类型相同、属性相同、子节点相同。

```js
var p1 = document.createElement('p')
var p2 = document.createElement('p')

p1.isEqualNode(p2) // true
```

后者返回一个布尔值，表示两个节点是否为同一个节点。

```javascript
var p1 = document.createElement('p')
var p2 = document.createElement('p')

p1.isSameNode(p2) // false
p1.isSameNode(p1) // true
```

##### Node.normalize()

该方法用于清理当前节点内部的所有文本节点（text）。它会去除空的文本节点，并且将毗邻的文本节点合并成一个，也就是说不存在空的文本节点，以及毗邻的文本节点。

```
var wrapper = document.createElement('div');

wrapper.appendChild(document.createTextNode('Part 1 '));
wrapper.appendChild(document.createTextNode('Part 2 '));

wrapper.childNodes.length // 2
wrapper.normalize();
wrapper.childNodes.length // 1
```

上面代码使用`normalize`方法之前，`wrapper`节点有两个毗邻的文本子节点。使用`normalize`方法之后，两个文本子节点被合并成一个。

该方法是`Text.splitText`的逆方法，可以查看《Text 节点对象》一章，了解更多内容。

##### Node.prototype.getRootNode()

该方法返回当前节点所在文档的根节点，与`ownerDocument`属性的作用相同。

### NodeList接口和HTMLCollection接口

节点都是单个对象，有时需要一种数据结构，能够容纳多个节点。DOM 提供两种节点集合，用于容纳多个节点：`NodeList`和`HTMLCollection`。两者的主要区别如下：

* 前者可以包含各种类型的节点
* 后者只能包含HTML元素的节点

注意：这两个只读对象都可以当做构造函数使用，但是一般不这么使用，因为DOM标准中定义的属性和方法返回的都是这两个对象的实例，而不需要自己手动定义这两个对象的实例。

```javascript
typeof NodeList 		// "function"
typeof HTMLCollection 	// "function"
```

#### NodeList接口

##### 概述

NodeList实例是一个类似数组的对象，它的成员是节点对象。通过以下方法可以得到NodeList实例：

* Node.childNodes
* document.querySelectorAll()等节点搜索方法

```js
document.body.childNodes instanceof NodeList // true
```

注意：NodeList对象实例可能是动态集合，也可能是静态集合。所谓动态集合是指DOM Tree中新增或者删除一个相关节点，都会立即反映在这个对象中。其中`Node.childNodes`属性返回的就是动态集合，其他的 NodeList 都是静态集合，也就是说DOM Tree发生变化，不会实时反映在返回的对象中。

```javascript
var parent = document.getElementById('parent')
parent.childNodes.length 		// 2
parent.appendChild(document.createElement('div'))
parent.childNodes.length 		// 3
```

##### NodeList.prototype.length

`length`属性返回 NodeList 实例包含的节点数量。

```javascript
var list = document.querySelector("div").childNodes
list[0]			//获取第一个节点对象
var arr = Array.prototype.slice.call(document.querySelector("div").childNodes)	
//转换为真正的数组，这样返回arr数组就是一个静态集合，不再实时反映DOM变化。
//注意：在迭代实时NodeList对象实例或者HTMLCollection实例之前，一定要先转换为其静态副本
```

##### NodeList.prototype.forEach()

`forEach`方法用于遍历 NodeList 的所有成员。它接受一个回调函数作为参数，每一轮遍历就执行一次这个回调函数，用法与数组实例的`forEach`方法完全一致。

##### NodeList.prototype.item()

`item`方法接受一个整数值作为参数，表示成员的位置，返回该位置上的成员。

注意：如果参数值大于实际长度，或者索引不合法（比如负数），该方法会返回null。如果省略参数，该方法还会报错。

```javascript
document.getElementsByTagName("div").item(1) === document.getElementsByTagName("div")[1]  //true
//一般使用更简洁的第二种
```

##### NodeList.prototype.keys()，NodeList.prototype.values()，NodeList.prototype.entries()

这三个方法都返回一个 ES6 的遍历器对象，可以通过`for...of`循环遍历获取每一个成员的信息。区别在于，`keys()`返回键名的遍历器，`values()`返回键值的遍历器，`entries()`返回的遍历器同时包含键名和键值的信息。

```js
var children = document.body.childNodes;

for (var key of children.keys()) {
  console.log(key);
}
// 0
// 1
// 2
// ...

for (var value of children.values()) {
  console.log(value);
}
// #text
// <script>
// ...

for (var entry of children.entries()) {
  console.log(entry);
}
// Array [ 0, #text ]
// Array [ 1, <script> ]
// ...
```

#### HTMLCollection接口

##### 概述

`HTMLCollection`是一个节点对象的集合，只能包含元素节点（element），不能包含其他类型的节点。它的返回值是一个类似数组的对象，但是与`NodeList`接口不同，`HTMLCollection`没有`forEach`方法，只能使用`for`循环遍历。

返回`HTMLCollection`实例的，主要是一些`Document`对象的集合属性，比如`document.links`、`docuement.forms`、`document.images`等。

```js
document.links instanceof HTMLCollection // true
```

`HTMLCollection`实例都是动态集合，节点的变化会实时反映在集合中。

如果元素节点有`id`或`name`属性，那么`HTMLCollection`实例上面，可以使用`id`属性或`name`属性引用该节点元素。如果没有对应的节点，则返回`null`。

```js
// HTML 代码如下
// <img id="pic" src="http://example.com/foo.jpg">

var pic = document.getElementById('pic')
document.images.pic === pic // true
```

##### HTMLCollection.prototype.length

该属性返回`HTMLCollection`实例包含的成员数量。

##### HTMLCollection.prototype.item()

该方法接受一个整数值作为参数，表示成员的位置，返回该位置上的成员。

```js
var c = document.images
var img0 = c.item(0)		//当然，还是更建议使用第二种方法
```

如果参数值超出成员数量或者不合法（比如小于0），那么`item`方法返回`null`。

##### HTMLCollection.prototype.namedItem()

该方法的参数是一个字符串，表示`id`属性或`name`属性的值，返回对应的元素节点。如果没有对应的节点，则返回`null`。

```html
<form id="demo"></form>
<script>
	var f = document.forms;
  	f[0] === f["demo"] === f.item(0) === f.namedItem("demo");	//true
</script>
```

#### 总结

两种有如下区别：

- HTMLCollection对象实例的成员只能是`Element`节点(元素节点)，而NodeList实例对象的成员可以包含其它类型的节点
- HTMLCollection对象实例都是动态集合，而NodeList对象实例还可以是静态集合
- HTMLCollection对象实例和NodeList对象实例都可以使用数字和`item`方法索引成员，如果取不到成员或者数字索引不合法，都会返回null。此外，HTMLCollection对象实例还可以使用`id`或`name`属性索引成员和`nameItem`方法索引成员。注意：nameItem()方法是根据节点成员的`id`属性或`name`属性进行索引。

### ParentNode和ChildNode接口

节点对象除了继承Node接口之外，还会继承其他接口。`ParentNode`接口表示当前节点是一个父节点，提供一些处理子节点的方法。`ChildNode`接口表示当前节点是一个子节点，提供一些相关方法。

#### ParentNode接口

如果当前节点是父节点，就会继承`ParentNode`接口。由于只有元素节点（element）、文档节点（document）和文档片段节点（documentFragment）拥有子节点，因此只有这三类节点会继承`ParentNode`接口。

##### ParentNode.children

该属性返回一个`HTMLCollection`实例，成员是当前节点的所有元素子节点。该属性只读。

注意，`children`属性只包括元素子节点，不包括其他类型的子节点（比如文本子节点）。如果没有元素类型的子节点，返回值`HTMLCollection`实例的`length`属性为`0`。

另外，`HTMLCollection`是动态集合，会实时反映 DOM 的任何变化。

##### ParentNode.firstElementChild、ParentNode.lastElementChild

* 前者返回当前节点的第一个子元素节点，如果当前节点中不存在子元素节点，则返回null
* 后者返回当前节点的最后一个子元素节点。如果当前节点中不存在子元素节点，则返回null

##### ParentNode.childElementCount

该属性返回当前节点包含的子元素节点的个数，如果不包含任何元素子节点，则返回0。

```javascript
document.body.childElementCount === document.body.children.length	//true
```

##### ParentNode.append()，ParentNode.prepend()

前者可以为当前节点追加一个或多个子节点，位置是最后一个元素子节点的后面。

该方法不仅可以添加元素子节点，还可以添加文本子节点。

```js
var parent = document.body;

// 添加元素子节点
var p = document.createElement('p');
parent.append(p);

// 添加文本子节点
parent.append('Hello');
```

注意：该方法没有返回值。

`prepend`方法为当前节点追加一个或多个子节点，位置是第一个元素子节点的前面。它的用法与`append`方法完全一致，也是没有返回值。

#### ChildNode 接口

如果一个节点有父节点，那么该节点就继承了ChildNode接口。

##### ChildNode.remove()

`remove`方法用于移除当前节点。注意：该方法是移除节点本身。

```javascript
document.body.firstElementChild.remove()
```

##### ChildNode.before()、ChildNode.after()

前者用于在当前节点的前面，插入一个或多个同级节点。两种拥有相同的父节点。

注意：该方法不仅可以插入元素节点，还可以插入文本节点。

```js
var p = document.createElement('p')

// 插入元素节点
el.before(p)

// 插入文本节点
el.before('Hello')
```

后者用于在当前节点的后面，插入一个或多个同级节点，两者拥有相同的父节点。用法与`before`方法完全相同。

##### ChildNode.replaceWith()

该方法使用参数节点，替换当前节点。参数可以是元素节点，也可以是文本节点。

```javascript
document.links[0].replaceWith("hi")
```

### document节点

document节点对象代表整个文档，每张网页都有自己的document对象。window.document属性就指向这个对象。只要浏览器开始载入HTML文档，该对象就存在了，可以直接使用。

document对象有不同的方法获取：

* 正常的网页，直接使用`document`或`window.document`
* `iframe`框架里面的网页，使用`iframe`节点的`contentDocument`属性
* Ajax 操作返回的文档，使用`XMLHttpRequest`对象的`responseXML`属性
* 内部节点的`ownerDocument`属性

```js
document.querySelector("div").ownerDocument === document	//true
document.querySelector("iframe").contentDocument
```

document对象继承了EventTarget接口、Node接口、ParentNode接口。这意味着，这些接口的方法都可以在document对象上调用。除此之外，document对象还有很多自己的属性和方法。

#### 快捷方式属性

以下属性是指向文档内部的某个节点的快捷方式。

##### document.defaultView

该属性返回document对象所属的window对象。如果当前文档不属于`window`对象，该属性返回`null`。

```js
document.defaultView === window // true
```

##### document.doctype

对于 HTML 文档来说，`document`对象一般有两个子节点。第一个子节点是`document.doctype`，指向`<DOCTYPE>`节点，即文档类型（Document Type Declaration，简写DTD）节点。HTML 的文档类型节点，一般写成`<!DOCTYPE html>`。如果网页没有声明 DTD，该属性返回`null`。

```js
var doctype = document.doctype;
doctype // "<!DOCTYPE html>"
doctype.name // "html"
```

而该属性通常返回这个节点。

##### document.documentElement

该属性返回当前文档的根元素节点（root）。它通常是`document`节点的第二个子节点，紧跟在`document.doctype`节点后面。HTML网页的该属性，一般是`<html>`节点。

```javascript
document.documentElement instanceof HTMLElement 	//tru
```

##### document.body，document.head

* 前者返回当前文档中的head节点
* 后者返回当前文档中的body节点

```javascript
document.body.innerHTML = "<p>hello world</p>"	
//这两个属性是可读写的，对其重新赋值会导原有的所有子节点被移除
```

注意：这两个节点总是存在的，如果源码中省略了`<head>`和`<body>`标签，浏览器会自动创建这两个元素。

> 另外，这两个属性是可写的，如果改写它们的值，相当于移除所有子节点。

##### document.scrollingElement

该属性返回文档的滚动元素。也就是说，当文档整体滚动时，到底是哪个元素在滚动。

标准模式下，这个属性返回的文档的根元素`document.documentElement`（即`<html>`）。兼容（quirk）模式下，返回的是`<body>`元素，如果该元素不存在，返回`null`。

```js
// 页面滚动到浏览器顶部
document.scrollingElement.scrollTop = 0
```

##### document.activeElement

该属性返回获得当前焦点（focus）的 DOM 元素。通常，这个属性返回的是`<input>`、`<textarea>`、`<select>`等表单元素，如果当前没有焦点元素，返回`<body>`元素或`null`。

```javascript
document.activeElement		//<body>..</body>
```

##### document.fullscreenElement

该属性返回当前以全屏状态展示的DOM元素。如果不是全屏状态，该属性返回null。

```js
//判断video元素是否处于全屏状态
if (document.fullscreenElement.nodeName == 'VIDEO') {
  console.log('全屏播放视频');
}
```

#### 节点集合属性

以下属性返回一个`HTMLCollection`实例，表示文档内部特定元素的集合。这些集合都是动态的，原节点有任何变化，立刻会反映在集合中。

##### document.links

该属性返回当前文档所有设定了`href`属性的`<a>`及`<area>`节点。

##### document.forms

该属性返回所有`<form>`表单节点。

```js
var selectForm = document.forms[0]
```

除了使用位置序号，id属性和name属性也可以用来引用表单。

```js
/* HTML 代码如下
  <form name="foo" id="bar"></form>
*/
document.forms[0] === document.forms.foo // true
document.forms.bar === document.forms.foo // true
```

##### document.images

该属性返回页面所有img图片节点。

##### document.embeds，document.plugins

这两个属性都返回所有的`<embed>`节点。

##### document.scripts

该属性返回所有`<script>`节点。

##### document.styleSheets

该属性返回文档内嵌或引入的样式表集合，详细看下面讲解。

##### 总结

除了`document.styleSheets`，以上的集合属性返回的都是`HTMLCollection`实例。

```js
document.links instanceof HTMLCollection // true
document.images instanceof HTMLCollection // true
document.forms instanceof HTMLCollection // true
document.embeds instanceof HTMLCollection // true
document.scripts instanceof HTMLCollection // true
```

`HTMLCollection`实例是类似数组的对象，所以这些属性都有`length`属性，都可以使用方括号运算符引用成员。如果成员有`id`或`name`属性，还可以用这两个属性的值，在`HTMLCollection`实例上引用到这个成员。

```js
// HTML 代码如下
// <form name="myForm">
document.myForm === document.forms.myForm // true
```

#### 文档静态信息属性

##### document.documentURI，document.URL

`document.documentURI`属性和`document.URL`属性都返回一个字符串，表示当前文档的网址。不同之处是它们继承自不同的接口，`documentURI`继承自`Document`接口，可用于所有文档；`URL`继承自`HTMLDocument`接口，只能用于 HTML 文档。

```javascript
document.URL	//"file:///C:/Users/Administrator/Desktop/demo/demo.html"
document.URL === document.documentURI	//true
```

注意：如果文档的锚点发生变化，这两个属性也会随之变化。

##### document.domain

该属性返回当前文档的域名，不包含协议和接口。比如，网页的网址是`http://www.example.com:80/hello.html`，那么`domain`属性就等于`www.example.com`。如果无法获取域名，该属性返回`null`。

`document.domain`基本上是一个只读属性，只有一种情况除外。次级域名的网页，可以把`document.domain`设为对应的上级域名。比如，当前域名是`a.sub.example.com`，则`document.domain`属性可以设置为`sub.example.com`，也可以设为`example.com`。修改后，`document.domain`相同的两个网页，可以读取对方的资源，比如设置的 Cookie。

另外，设置`document.domain`会导致端口被改成`null`。因此，如果通过设置`document.domain`来进行通信，双方网页都必须设置这个值，才能保证端口相同。

```javascript
//如果当前网址是http://www.360doc.com/content/demo.shtml，那么域名就是www.360doc.com
document.domain						//"javascript.ruanyifeng.com"
document.domain = "ruanyifeng.com"	//设置成功
document.domain						//"javascript.ruanyifeng.com"
document.domain	= "www.baidu.com"	//报错
```

##### document.lastModified

该属性返回一个字符串，表示当前文档最后修改的时间。不同浏览器的返回值，日期格式是不一样的。

```javascript
document.lastModified	//"11/24/2017 13:41:00"
```

注意：该属性的值是字符串，所以不能直接用来比较。`Date.parse`方法将其转为`Date`实例，才能比较两个网页。

```js
var lastVisitedDate = Date.parse('01/01/2018');
if (Date.parse(document.lastModified) > lastVisitedDate) {
  console.log('网页已经变更');
}
```

如果页面上有JavaScript生成的内容，该属性返回的总是当前时间。

##### document.location

`Location`对象是浏览器提供的原生对象，提供 URL 相关的信息和操作方法。通过`window.location`和`document.location`属性，可以拿到这个对象。

```javascript
// 当前网址http://user:passwd@www.example.com:4097/path/a.html?x=111#part1
document.location.href 		// "http://user:passwd@www.example.com:4097/path/a.html?x=111#part1"
document.location.protocol 	// "http:"
document.location.host 		// "www.example.com:4097"
document.location.hostname 	// "www.example.com"
document.location.port 		// "4097"
document.location.pathname 	// "/path/a.html"
document.location.search 	// "?x=111"
document.location.hash 		// "#part1"
document.location.user 		// "user"
document.location.password 	// "passwd"
```

此外，该对象上还定义了一些方法来操作当前文档的URL信息。

```javascript
document.location.assign('http://www.google.com')	// 跳转到另一个网址
document.location.reload(true)						// 优先从服务器重新加载
document.location.reload(false)						// 优先从本地缓存重新加载(默认值)
//跳转到新网址，并将取代掉history对象中的当前记录
document.location.replace('http://www.google.com');	
//将location对象转换为字符串形式，等价于document.location.href
document.location.toString()
```

如果将新的网址赋值给location对象，网页就会自动跳转到新网址。

```javascript
document.location = 'http://www.baidu.com';
// 等同于document.location.href = "http://www.baidu.com"
document.location = 'page2.html';	//可以指定相对地址
document.location = "#top";			//可以指定锚点，此时浏览器会自动滚动到锚点处
```

注意：使用上面介绍的方法重置URL，跟用户点击链接跳转的效果是一样的。上一个网页依然保存在浏览器历史中，点击后退按钮便可回到前一个网页。而如果不希望用户回到前一个网页，可以使用`location.replace`方法，这样后退按钮就不会回到前一个网页。该方法的最佳实践就是判断用户是移动设备时，就立即跳转到移动版网页。

此外，`document.location`属性与`window.location`属性是等价的，但是建议使用`document.location`，这样语义性更好。 

```javascript
document.location === window.location // true
```

##### document.title

该属性返回当前文档的标题，默认情况下，返回title节点的值。但是该属性是可写的，一旦被修改，就返回修改后的值。

```js
document.title = '新标题';
document.title // "新标题"
```

##### document.characterSet

该属性返回当前文档的编码，比如`UTF-8`、`ISO-8859-1`等等。

```js
document.characterSet	//"UTF-8"	
```

##### document.referrer

该属性返回一个字符串，表示当前文档的访问者来自哪里。

```js
document.referrer
// "https://example.com/path"
```

如果无法获取来源，或者用户直接键入网址而不是从其他网页点击进入，`document.referrer`返回一个空字符串。

`document.referrer`的值，总是与 HTTP 头信息的`Referer`字段保持一致。但是，`document.referrer`的拼写有两个`r`，而头信息的`Referer`字段只有一个`r`。

##### document.dir

该属性返回一个字符串，表示文字方向。它只有两个可能的值：rtl表示文字从右到左，阿拉伯文是这种方式；ltr表示文字从左到右，包括英语和汉语在内的大多数文字采用这种方式。

##### document.compatMode

该属性返回浏览器处理文档的模式，可能的值为`BackCompat`（向后兼容模式）和`CSS1Compat`（严格模式）。

```js
document.compatMode		//"CSS1Compat"
```

一般来说，如果网页代码的第一行设置了明确的`DOCTYPE`（比如`<!doctype html>`），`document.compatMode`的值都为`CSS1Compat`。

#### 文档状态属性

##### document.hidden

该属性返回一个布尔值，表示当前页面是否可见。如果窗口最小化、浏览器切换了 Tab，都会导致导致页面不可见，使得`document.hidden`返回`true`。

这个属性是 Page Visibility API 引入的，一般都是配合这个 API 使用。

##### document.visibilityState

该属性返回文档的可见状态。

它的值有四种可能：

* visible：页面可见。注意，页面可能是部分可见，即不是焦点窗口，前面被其他窗口部分挡住了。
* hidden： 页面不可见，有可能窗口最小化，或者浏览器切换到了另一个 Tab。
* prerender：页面处于正在渲染状态，对于用于来说，该页面不可见。
* unloaded：页面从内存里面卸载了。

这个属性可以用在页面加载时，防止加载某些资源；或者页面不可见时，停掉一些页面功能。

##### document.readyState

`document.readyState`属性返回当前文档的状态，可能是如下三种值：

- loading，表示文档处于加载HTML代码阶段(尚未完成解析)
- interactive，表示文档处于加载外部资源阶段
- complete，表示文档已经加载完成

详细的属性变化过程如下：

1. 浏览器开始解析文档，此时属性值为loading
2. 浏览器遇到文档中的`script`元素，并且如果该元素上没有`async`或`defer`属性，就暂停解析文档，开始执行脚本，此时属性值仍然是loading
3. 文档解析完成后，属性值变为interactive
4. 文档中的图片，样式表，字体文件等外部资源全部加载完成后，属性值变为complete

```javascript
var interval = setInterval(function(){
    if(document.readyState === "complete"){
        clearInterval(interval);
      	//code
    }
},100)
```

##### document.designMode

`document.designMode`属性用来控制当前文档是否可编辑，利用该属性可以实现所见即所得编辑器。

```javascript
document.designMode = "on"		//页面变为可编辑，默认不可编辑，即off
```

##### document.implementation

该属性返回一个`DOMImplementation`对象。该对象有三个方法，主要用于创建独立于当前文档的新的 Document 对象。

- `DOMImplementation.createDocument()`：创建一个 XML 文档。
- `DOMImplementation.createHTMLDocument()`：创建一个 HTML 文档。
- `DOMImplementation.createDocumentType()`：创建一个 DocumentType 对象。



```javascript
var doc = document.implementation.createHTMLDocument('Title');
var p = doc.createElement('p');
p.innerHTML = 'hello world';
doc.body.appendChild(p);

document.replaceChild(
  doc.documentElement,
  document.documentElement
);
```

上述代码中，用生成的文档替换原来的文档，这会使得当前文档的内容全部消失，变成helloworld。

##### document.cookie

`document.cookie`属性用来操作浏览器Cookie，详细内容参考BOM章节。

#### 方法

##### document.open()，document.close()

* `document.open`方法清除当前文档所有内容，使得文档处于可写状态，供`document.write`方法写入内容
* `document.close`方法用来关闭`document.open()`打开的文档。

```javascript
document.open()
document.write("hello world")
document.close()
document.write("ni hao ma?")	
```

##### document.write()，document.writeln()

`document.write()`方法用于向当前文档写入内容。

在网页的首次渲染阶段，只要页面没有关闭写入（即没有执行document.close()方法），该方法写入的内容就会追加在已有内容的后面。

注意，`document.write`会当作 HTML 代码解析，不会转义。

```js
document.write('<p>hello world</p>')		//p会被当做标签解析
```

如果页面已经解析完成（`DOMContentLoaded`事件发生之后），再调用`write`方法，它会先调用`open`方法，擦除当前文档所有内容，然后再写入。

```js
document.addEventListener('DOMContentLoaded', function (event) {
  document.write('<p>Hello World!</p>');
});

// 等同于
document.addEventListener('DOMContentLoaded', function (event) {
  document.open();
  document.write('<p>Hello World!</p>');
  document.close();
});
```

如果在页面渲染过程中调用`write`方法，并不会自动调用`open`方法。（可以理解成，`open`方法已调用，但`close`方法还未调用。）

```html
<html>
<body>
hello
<script type="text/javascript">
  document.write("world")
</script>
</body>
</html>
```

在浏览器打开上面网页，将会显示`hello world`。

`document.write`是 JavaScript 语言标准化之前就存在的方法，现在完全有更符合标准的方法向文档写入内容（比如对`innerHTML`属性赋值）。所以，除了某些特殊情况，应该尽量避免使用`document.write`这个方法。

`document.writeln`方法与`write`方法完全一致，除了会在输出内容的尾部添加换行符。

```js
document.write(1);
document.write(2);
// 12

document.writeln(1);
document.writeln(2);
// 1
// 2
```

注意，`writeln`方法添加的是 ASCII 码的换行符，渲染成 HTML网页时不起作用，即在网页上显示不出换行。网页上的换行，必须显式写入`<br>`。

##### document.querySelector()，document.querySelectorAll()

* `document.querySelector`方法接受一个CSS选择器(包括CSS3选择器，但是IE7,8只支持CSS2选择器)作为参数，返回匹配该选择器的元素节点。如果有多个节点满足匹配条件，则返回第一个匹配的节点。如果没有发现匹配的节点，则返回null
* `document.querySelectorAll`方法法与`querySelector`用法类似，区别是该方法返回一个NodeList对象，包含所有匹配选择器的节点。注意：该方法的返回结果不是动态集合，不会实时反映元素节点的变化。

此外，这两个方法的参数，可以是逗号分隔的多个CSS选择器，返回匹配其中一个选择器的元素节点。并且这两个方法都支持复杂的CSS选择器，但是不支持CSS伪元素的选择器(比如`:first-line`和`:first-letter`)和伪类的选择器(比如`:link`和`:visited`)，即无法选中伪元素和伪类。

```javascript
document.querySelectorAll('.demo1,.demo2')		//返回类名是demo1或demo2的元素节点
document.querySelectorAll('DIV,A,SCRIPT')		//返回div，a，script三类元素节点
document.querySelectorAll("*")					//返回文档中所有元素节点
```

这两个方法除了定义在document节点上，还定义在Element节点上，也就是说这两个方法还可以在元素节点上调用。注意：当在元素节点上调用时，浏览器会先在全局范围内搜索给定的CSS选择器，然后过滤哪些属于当前元素的后代元素，这种机制可能会返回一些意料之外的结果。

```html
<div>
	<blockquote id="outer">
  		<p>Hello</p>
  		<div id="inner"><p>World</p></div>
	</blockquote>
</div>
<script>
	var outer = document.getElementById('outer').querySelector("div p");	
  	//返回的是第一个p元素，而不是第二个
</script>
```

##### document.getElementsByTagName()

该方法搜索HTML标签名，返回符合条件的元素。它的返回值是一个`HTMLCollection`对象实例，可以实时反映HTML文档的变化。如果没有匹配的元素，则返回一个空数组。

```javascript
document.getElementsByTagName("div") instanceof HTMLcollection			//true
document.getElementsByTagName("span")									//[]
document.getElementsByTagName("*")			//返回文档中所有HTML元素			
//由于HTML标签对大小写不敏感，所以该方法也对大小写不敏感	
document.getElementsByTagName("a") === document.getElementsByTagName("A")//true
```

HTML 标签名是大小写不敏感的，因此`getElementsByTagName`方法也是大小写不敏感的。另外，返回结果中，各个成员的顺序就是它们在文档中出现的顺序。

此外，元素节点本身也定义了该方法，返回该元素的后代元素中匹配指定标签的元素。也就是说，该方法不仅可以在document上调用，也可以在任何元素节点上调用。

```javascript
document.querySelector("div").getElementsByTageName("span")
```

##### document.getElementsByClassName()

该方法返回由匹配指定class属性的元素组成的HTMLCollection对象实例，元素的变化实时反映在结果中。注意：由于class在JavaScript中属于保留字，所以一律使用className代替class属性。

```javascript
document.getElementsByClassName("foo bar")	//匹配同时具有foo和bar类名的元素，顺序可以颠倒
```

该方法同样可以在元素节点上调用。

```javascript
var d = document.querySelector("div")
d.getElementsByClassName("DEMO") === d.getElementsByClassName("demo")	//false 
```

> 正常模式下，CSS的class是大小写敏感的，但是在quirks mode下，大小写不敏感。

##### document.getElementsByName()

该方法用于返回由匹配指定`name`属性的元素所组成的NodeList对象实例，因为name属性相同的元素不止一个。注意：HTML文档中只有`<form>`、`<radio>`、`<img>`、`<frame>`、`<embed>`和`<object>`等元素拥有`name`属性。 

```javascript
document.getElementsByName("demo")	
```

##### document.getElementById()

`document.getElementById`方法返回匹配指定`id`属性的元素节点，如果没有匹配的节点，则返回null。注意：该方法的参数对大小写敏感，并且该方法只能在document节点上使用，而不能在元素节点上使用。

```javascript
document.getElementById("d") === document.getElementById("D");	//false
document.querySelector("div").getElementById("d");				//TypeError
```

> 一般来说，通过id选择元素效率要比其他元素选择方法要高许多。

##### document.elementFromPoint()

该方法返回位于页面指定位置最上层的元素节点。

```js
document.elementFromPoint(50, 50)
```

上述代码选中在（50,50）这个坐标位置的最上层的那个HTML元素。

该方法的两个参数，依次是相对于当前视口左上角的横坐标和纵坐标，单位是像素。如果位于该位置的 HTML 元素不可返回（比如文本框的滚动条），则返回它的父元素（比如文本框）。如果坐标值无意义（比如负值或超过视口大小），则返回`null`。

##### document.elementsFromPoint()

该方法返回一个数组，成员是位于指定坐标（相对视口）的所有元素。

##### document.caretPositionFromPoint()

该方法返回一个 CaretPosition 对象，包含了指定坐标点在节点对象内部的位置信息。CaretPosition 对象就是光标插入点的概念，用于确定光标点在文本对象内部的具体位置。

```js
var range = document.caretPositionFromPoint(clientX, clientY);
```

上述代码中，range是指定坐标点的CarePosition对象。该对象有两个属性：

* CaretPosition.offsetNode：该位置的节点对象
* CaretPosition.offset：该位置在`offsetNode`对象内部，与起始位置相距的字符数

##### document.createElement()

该方法接受一个元素标签名作为参数，用于创建该标签名对应的元素节点。注意：由于HTML文档对元素标签大小写不敏感，所以该方法传入的参数同样对大小写不敏感。

```javascript
document.createElement('div').tagName	//"div"
document.createElement('div') === document.createElement('DIV')	//true
document.createElement('<div>')			//报错
```

注意：该方法的参数可以是自定义的标签名。

```js
document.createElement("foo")
```

##### document.createTextNode()

该方法接受一个文本作为参数，用于创建该文本对应的文本节点。注意：该方法不会将将传入的文本参数当做HTML代码解析，因此可以用来展示用户的输入，避免XSS攻击。

```javascript
document.createTextNode('<span>hello</span>')	//<span>hello</span>
document.createTextNode('"hi"')					//"hi"
```

##### document.createAttribute()

该方法接受一个属性名作为参数，用于创建该属性对应的属性节点。

```javascript
var attr = document.createAttribute("myAttr")
attr.value = "hi"
document.links[0].setAttribute(attr)
// 等同于
document.links[0].setAttribute("myAttr","hi")
```

##### document.createComment()

该方法生成一个新的注释节点，并返回该节点。

```js
var CommentNode = document.createComment(data)
```

该方法的参数是一个字符串，会成为注释节点的内容。

##### document.createDocumentFragment()

该方法用于创建DocumentFragment节点。

```javascript
document.createDocumentFragment()
```

`DocumentFragment`是一个存在于内存的 DOM 片段，不属于当前文档，常常用来生成一段较复杂的 DOM 结构，然后再插入当前文档。这样做的好处在于，因为`DocumentFragment`不属于当前文档，对它的任何改动，都不会引发网页的重新渲染，比直接修改当前文档的 DOM 有更好的性能表现。

##### document.createEvent()

该方法生成一个事件对象（Event实例），该对象可以被`element.dispatchEvent()`方法使用，触发指定事件。

```js
var event = document.createEvent(type)
```

该方法的参数是事件类型，比如UIEvents、MouseEvents、HTMLEvents等。

注意：自定义事件对象后，随之必须调用自身的init方法进行初始化。

```javascript
var event = document.createEvent("Event") 	//新建event事件
event.initEvent('build',true,true)			//初始化event事件
document.addEventListener('build',function(e){
    alert(1)
},false)				
document.dispatchEvent(event)				//触发event事件
```

##### document.addEventListener()，document.removeEventListener()，document.dispatchEvent()

* `document.addEventListener`方法用于绑定事件监听函数
* `document.removeEventListener`方法用于移除事件监听函数
* `document.dispatchEvent`方法用于触发事件

这三个方法与document节点的事件相关，并且都继承自EventTarget接口(参照EventTarget章节)。

```javascript
document.addEventListener('click', listener, false)	
document.removeEventListener('click', listener, false)	
var event = new Event('click')
document.dispatchEvent(event)
```

##### document.hasFocus()

该方法返回一个布尔值，表示当前文档之中是否有元素被激活或获得焦点。

注意：有焦点的文档必定被激活，而激活的文档未必有焦点。例如：从当前窗口跳出一个新窗口，这个新窗口就是激活的，但是不拥有焦点。

```html
<input type="text" autofocus />
<script>
	document.hasfocus()		//true
</script>
```

##### document.createNodeIterator()，document.createTreeWalker()

* `document.createNodeIterator`方法，该方法返回一个当前节点的子节点遍历器。它接受两个参数，第一个参数是要遍历的节点，第二个参数是要遍历的子节点类型。常见的节点类型有：元素节点(`NodeFilter.SHOW.ELEMENT`)、文本节点(`NodeFilter.SHOW_TEXT`)、注释节点(`NodeFilter.SHOW_COMMENT`)等。

  ```javascript
  document.createNodeIterator(document.body,NodeFilter.SHOW_ALL)	//遍历body节点下的所有子节点
  ```

  返回的遍历器对象上定义了`nextNode`方法和`previousNode`方法依次遍历所有子节点。

  ```html
  <body>
  	<div></div>
   	<p></p>
  </body>
  <script>
  	var it = document.createNodeIterator(document.body,NodeFilter.SHOW_ELEMENT)
      it.nextNode()	//<body></body>，遍历器返回的第一个节点总是当前节点
    	it.nextNode()	//<div></div>
    	it.nextNode()	//<p></p>
    	it.nextNode()	//null，遍历完后返回null
  </script>
  ```

* `document.createTreeWalker`方法，该方法和`document.createNodeIterator`方法用法一致，唯一的区别就是该方法返回一个当前节点的后代节点遍历器。

  ```javascript
  document.createTreeWalker(document.body,NodeFilter.SHOW_ALL)	//遍历body节点下的所有后代节点
  ```

##### document.adoptNode()，document.importNode()

`document.adoptNode`方法将某个节点及其子节点，从原来所在的文档或`DocumentFragment`里面移除，归属当前`document`对象，返回插入后的新节点。插入的节点对象的`ownerDocument`属性，会变成当前的`document`对象，而`parentNode`属性是`null`。

注意：该节点参数以及它的后代节点会从原文档中全部移除。

```js
var n = document.adoptNode(document.links[0])	//返回被移除的节点
n.parentNode		//null，还未插入到当前文档，所以父节点为null
n.ownerDocument		//返回当前文档的document节点，而不是原文档的document节点
```

注意，`document.adoptNode`方法只是改变了节点的归属，并没有将这个节点插入新的文档树。所以，还要再用`appendChild`方法或`insertBefore`方法，将新节点插入当前文档树。

`document.importNode`方法则是从原来所在的文档或`DocumentFragment`里面，拷贝某个节点及其子节点，让它们归属当前`document`对象。拷贝的节点对象的`ownerDocument`属性，会变成当前的`document`对象，而`parentNode`属性是`null`。

```js
var node = document.importNode(externalNode, deep);
```

`document.importNode`方法的第一个参数是外部节点，第二个参数是一个布尔值，表示对外部节点是深拷贝还是浅拷贝，默认是浅拷贝（false）。虽然第二个参数是可选的，但是建议总是保留这个参数，并设为`true`。

注意，`document.importNode`方法只是拷贝外部节点，这时该节点的父节点是`null`。下一步还必须将这个节点插入当前文档树。

```js
var iframe = document.getElementsByTagName('iframe')[0];
var oldNode = iframe.contentWindow.document.getElementById('myNode');
var newNode = document.importNode(oldNode, true);
document.getElementById("container").appendChild(newNode);
```

上面代码从`iframe`窗口，拷贝一个指定节点`myNode`，插入当前文档。

##### document.createNodeIterator()

该方法返回一个子节点遍历器。

```js
var nodeIterator = document.createNodeIterator(
  document.body,
  NodeFilter.SHOW_ELEMENT
);
```

上述代码返回body元素子节点的遍历器。

该方法的第一个参数为所要遍历的根节点，第二个参数为所要遍历的节点类型，这里指定为元素节点（NodeFilter.SHOW_ELEMENT）。几种主要的节点类型写法如下：

* 所有节点：NodeFilter.SHOW_ALL
* 元素节点：NodeFilter.SHOW_ELEMENT
* 文本节点：NodeFilter.SHOW_TEXT
* 评论节点：NodeFilter.SHOW_COMMENT

`document.createNodeIterator`方法返回一个“遍历器”对象（`NodeFilter`实例）。该实例的`nextNode()`方法和`previousNode()`方法，可以用来遍历所有子节点。

```js
var nodeIterator = document.createNodeIterator(document.body);
var pars = [];
var currentNode;

while (currentNode = nodeIterator.nextNode()) {
  pars.push(currentNode);
}
```

上面代码中，使用遍历器的`nextNode`方法，将根节点的所有子节点，依次读入一个数组。`nextNode`方法先返回遍历器的内部指针所在的节点，然后会将指针移向下一个节点。所有成员遍历完成后，返回`null`。`previousNode`方法则是先将指针移向上一个节点，然后返回该节点。

```js
var nodeIterator = document.createNodeIterator(
  document.body,
  NodeFilter.SHOW_ELEMENT
);

var currentNode = nodeIterator.nextNode();
var previousNode = nodeIterator.previousNode();

currentNode === previousNode // true
```

上面代码中，`currentNode`和`previousNode`都指向同一个的节点。

注意，遍历器返回的第一个节点，总是根节点。

```js
pars[0] === document.body // true
```

##### document.createTreeWalker()

`document.createTreeWalker`方法返回一个 DOM 的子树遍历器。它与`document.createNodeIterator`方法基本是类似的，区别在于它返回的是`TreeWalker`实例，后者返回的是`NodeIterator`实例。另外，它的第一个节点不是根节点。

`document.createTreeWalker`方法的第一个参数是所要遍历的根节点，第二个参数指定所要遍历的节点类型（与`document.createNodeIterator`方法的第二个参数相同）。

```js
var treeWalker = document.createTreeWalker(
  document.body,
  NodeFilter.SHOW_ELEMENT
);

var nodeList = [];

while(treeWalker.nextNode()) {
  nodeList.push(treeWalker.currentNode);
}
```

上面代码遍历`<body>`节点下属的所有元素节点，将它们插入`nodeList`数组。

##### document.execCommand()，document.queryCommandSupported()，document.queryCommandEnabled()

如果`document.designMode`属性设为`on`，那么整个文档用户可编辑；如果元素的`contenteditable`属性设为`true`，那么该元素可编辑。这两种情况下，可以使用`document.execCommand()`方法，改变内容的样式，比如`document.execCommand('bold')`会使得字体加粗。

```
document.execCommand(command, showDefaultUI, input)
```

该方法接受三个参数。

- `command`：字符串，表示所要实施的样式。
- `showDefaultUI`：布尔值，表示是否要使用默认的用户界面，建议总是设为`false`。
- `input`：字符串，表示该样式的辅助内容，比如生成超级链接时，这个参数就是所要链接的网址。如果第二个参数设为`true`，那么浏览器会弹出提示框，要求用户在提示框输入该参数。但是，不是所有浏览器都支持这样做，为了兼容性，还是需要自己部署获取这个参数的方式。

```js
var url = window.prompt('请输入网址');

if (url) {
  document.execCommand('createlink', false, url);
}
```

上面代码中，先提示用户输入所要链接的网址，然后手动生成超级链接。注意，第二个参数是`false`，表示此时不需要自动弹出提示框。

`document.execCommand()`的返回值是一个布尔值。如果为`false`，表示这个方法无法生效。

这个方法大部分情况下，只对选中的内容生效。如果有多个内容可编辑区域，那么只对当前焦点所在的元素生效。

`document.execCommand()`方法可以执行的样式改变有很多种，下面是其中的一些：bold、insertLineBreak、selectAll、createLink、insertOrderedList、subscript、delete、insertUnorderedList、superscript、formatBlock、insertParagraph、undo、forwardDelete、insertText、unlink、insertImage、italic、unselect、insertHTML、redo。这些值都可以用作第一个参数，它们的含义不难从字面上看出来。

`document.queryCommandEnabled()`方法返回一个布尔值，表示浏览器是否允许使用这个方法。

```js
if (document.queryCommandEnabled('SelectAll')) {
  // ...
}
```

`document.queryCommandSupported()`方法返回一个布尔值，表示当前是否可用某种样式改变。比如，加粗只有存在文本选中时才可用，如果没有选中文本，就不可用。

```js
if (document.queryCommandSupported('SelectAll')) {
  // ...
}
```

##### document.getSelection()

`document.getSelection`方法指向`window.getSelection`方法，详细参照window对象章节。

```javascript
document.getSelection() === window.getSelection()		//true
```

### element节点

Element节点对象对应网页的HTML元素，每一个HTML元素，在DOM树上都会转化成一个Element节点对象（元素节点）。

`Element`对象继承了`Node`接口，因此`Node`的属性和方法在`Element`对象都存在。此外，不同的 HTML 元素对应的元素节点是不一样的，浏览器使用不同的构造函数，生成不同的元素节点，比如`<a>`元素的节点对象由`HTMLAnchorElement`构造函数生成，`<button>`元素的节点对象由`HTMLButtonElement`构造函数生成。因此，元素节点不是一种对象，而是一组对象，这些对象除了继承`Element`的属性和方法，还有各自构造函数的属性和方法。

```javascript
new HTMLAnchorElement()		//创建一个a元素节点
new HTMLButtonElement()		//创建一个button元素节点
```

#### 元素特性的相关实例属性

##### Element.id，Element.tagName

* `Element.id`属性，该可读写属性返回当前元素节点的`id`属性
* `Element.tagName`属性，该属性返回当前元素节点所对应的HTML元素的大写标签名

```javascript
document.querySelector("div").tagName === document.querySelector("div").nodeName	//true
document.querySelector("div").tagName		//"DIV"
```

##### Element.dir

该属性用于读写当前元素的文字方向，可能是从左到右（“ltr”），也可能是从右到左（“rtl”）。

##### Element.accessKey

该属性用于读写分配给当前元素的快捷键。

```js
// HTML 代码如下
// <button accesskey="h" id="btn">点击</button>
var btn = document.getElementById('btn');
btn.accessKey // "h"
```

上面代码中，`btn`元素的快捷键是`h`，按下`Alt + h`就能将焦点转移到它上面。

##### Element.draggable

该属性返回一个布尔值，表示当前元素是否可拖动。该属性可读写。

##### Element.lang

该属性返回当前元素的语言设置，该属性可读写。

```js
// HTML 代码如下
// <html lang="en">
document.documentElement.lang // "en"
```

##### Element.tabIndex

该属性返回一个整数，表示当前元素在Tab键遍历时的顺序。该属性可读写。

`tabIndex`属性值如果是负值（通常是`-1`），则 Tab 键不会遍历到该元素。如果是正整数，则按照顺序，从小到大遍历。如果两个元素的`tabIndex`属性的正整数值相同，则按照出现的顺序遍历。遍历完所有`tabIndex`为正整数的元素以后，再遍历所有`tabIndex`等于`0`、或者属性值是非法值、或者没有`tabIndex`属性的元素，顺序为它们在网页中出现的顺序。

##### Element.title

该属性用来读写当前元素的HTML属性title。该属性通常用来指定，鼠标悬浮时弹出的文字提示框。

#### 元素状态的相关实例属性

##### Element.hidden

该属性返回一个布尔值，表示当前元素的hidden属性，用来控制当前元素是否可见。该属性可读写。

```js
var btn = document.getElementById('btn');
var mydiv = document.getElementById('mydiv');

btn.addEventListener('click', function () {
  mydiv.hidden = !mydiv.hidden;
}, false);
```

注意，该属性与 CSS 设置是互相独立的。CSS 对这个元素可见性的设置，`Element.hidden`并不能反映出来。也就是说，这个属性并不能用来判断当前元素的实际可见性。

CSS的设置高于该属性，如果CSS设置了该元素不可见（display:none）或可见，那么该属性并不能改变该元素实际的可见性。换言之，这个属性只在css没有明确设定当前元素的可见性时才有效。

##### Element.contentEditable，Element.isContentEditable

HTML元素可以设置contentEditable属性，使得元素的内容可以编辑。

```html
<div contenteditable>123</div>	<!--可以在网页编辑这个区块的内容-->
```

`Element.contentEditable`属性返回一个字符串，表示是否设置了`contenteditable`属性，有三种可能的值。该属性可写。

- `"true"`：元素内容可编辑
- `"false"`：元素内容不可编辑
- `"inherit"`：元素是否可编辑，继承了父元素的设置

`Element.isContentEditable`属性返回一个布尔值，同样表示是否设置了`contenteditable`属性。该属性只读。

##### Element.attributes

该属性返回一个类似数组的对象，成员是当前元素节点的所有属性节点。具体查看属性操作章节。

##### Element.dataset

网页元素可以自定义`data-`属性，用来添加数据。

```html
<div data-timestamp="1522907809292"></div>
```

上面代码中，`<div>`元素有一个自定义的`data-timestamp`属性，用来为该元素添加一个时间戳。

`Element.dataset`属性返回一个对象，可以从这个对象读写`data-`属性。

```js
// <article
//   id="foo"
//   data-columns="3"
//   data-index-number="12314"
//   data-parent="cars">
//   ...
// </article>
var article = document.getElementById('foo');
foo.dataset.columns // "3"
foo.dataset.indexNumber // "12314"
foo.dataset.parent // "cars"
```

注意，`dataset`上面的各个属性返回都是字符串。

HTML 代码中，`data-`属性的属性名，只能包含英文字母、数字、连词线（`-`）、点（`.`）、冒号（`:`）和下划线（`_`）。它们转成 JavaScript 对应的`dataset`属性名，规则如下。

- 开头的`data-`会省略。
- 如果连词线后面跟了一个英文字母，那么连词线会取消，该字母变成大写。
- 其他字符不变。

因此，`data-abc-def`对应`dataset.abcDef`，`data-abc-1`对应`dataset["abc-1"]`。

除了使用`dataset`读写`data-`属性，也可以使用`Element.getAttribute()`和`Element.setAttribute()`，通过完整的属性名读写这些属性。

```js
var mydiv = document.getElementById('mydiv');

mydiv.dataset.foo = 'bar';
mydiv.getAttribute('data-foo') // "bar"
```

##### Element.innerHTML

`Element.innerHTML`属性，该可读写属性用于设置或返回当前元素节点包含的HTML代码。注意：当用于获取该属性值时，如果当前元素节点中的文本节点包含`&`、`<`和`>`，那么该属性会分别将其转换为`&amp;`、`&lt;`和`&gt;`再返回。 当用于设置该属性值时，如果要设置的文本中包含HTML标签，会尝试将其解析为对应的节点对象。

```html
<p><span>hi</span>,kat&sid</p>
<script>
  	var p = document.querySelector("p")
	p.innerHTML						//"<span>hi</span>,kat&amp;sid"
  	p.innerHTML = ""				//清除当前元素节点中的所有后代节点
  	p.innerHTML = "<div>hi</div>"	//解析成div节点
</script>
```

如果要设置的文本包含`<script>`标签，虽然会被解析成script节点，但是其中的代码不会执行。

```javascript
p.innerHTML = "<script>alert(1)</script>" //不弹出1
p.innerHTML = "<img onerror=alert(1) />"  //弹出1，有风险。所以如果是设置文本最好使用textContent属性
```

##### Element.outerHTML

`Element.outerHTML`属性，该可读写属性用于设置或返回当前元素节点包含的HTML代码。和`Element.innerHTML` 属性用法一致，唯一的不同的是该属性返回的HTML代码包含当前元素节点自身。

```html
<div><p>h<span>ell</span>o</p></div>
<script>
	document.querySelector("div").outerHTML	 //"<div><p>h<span>ell</span>o</p></div>"
  	document.querySelector("div").outerHTML	= ""
</script>
```

##### Element.className，Element.classList

* `Element.className`属性，该可读写属性用于设置或返回当前元素节点的所有`class`属性的字符串形式，每个类之间用空格分隔。
* `Element.classList`属性，该属性返回一个由当前元素节点的所有`class`属性组成的类数组对象。注意：这个类数组对象上定义如下六个方法用来操作类。
  * `add`方法，该方法用于增加一个类
  * `remove`方法，该方法用于移除一个类
  * `contains`方法，该方法用于判断当前元素节点是否包含某个类
  * `toggle`方法，该方法用于切换类。该方法接受一个布尔值作为它的第二个可选参数，如果是true，则表示添加类。如果是false，则表示移除类。
  * `item`方法，该方法用于索引类
  * `toString`方法，该方法用于将返回的类数组对象转换为字符串形式 

```html
<div class="one two three"></div>
<script>
	var d = document.querySelector("div")
  	d.className						//"one two three"
  	d.className = "one two" 
  	d.className += "three"
  	d.className 					//"one two three"
  	d.classList.remove("three")		//之所以定义这些方法使用了简化使用className属性操作类
  	d.classList 					//{0:"one",1:"two",length:2}
  	d.classList.item(0)				//"one"
  	d.classList.toString()			//"one trwo"
</script>
```

注意：`toggle`方法可以接受一个布尔值，作为第二个参数。如果为`true`，则添加该属性；如果为`false`，则去除该属性。

```
el.classList.toggle('abc', boolValue);

// 等同于
if (boolValue) {
  el.classList.add('abc');
} else {
  el.classList.remove('abc');
}
```

##### Element.clientHeight，Element.clientWidth

`Element.clientHeight`属性返回一个整数值，表示元素节点的 CSS 高度（单位像素），只对块级元素生效，对于行内元素返回`0`。如果块级元素没有设置 CSS 高度，则返回实际高度。

除了元素本身的高度，它还包括`padding`部分，但是不包括`border`、`margin`。如果有水平滚动条，还要减去水平滚动条的高度。注意，这个值始终是整数，如果是小数会被四舍五入。

`Element.clientWidth`属性返回元素节点的 CSS 宽度，同样只对块级元素有效，也是只包括元素本身的宽度和`padding`，如果有垂直滚动条，还要减去垂直滚动条的宽度。

`document.documentElement`的`clientHeight`属性，返回当前视口的高度（即浏览器窗口的高度），等同于`window.innerHeight`属性减去水平滚动条的高度（如果有的话）。

```js
//不存在浏览器滚动条
document.documentElement.clientHeight === window.innerHeight	//true
document.documentElement.clientwidth === window.innerwidth		//true
//存在浏览器滚动条
document.documentElement.clientHeight === window.innerHeight - 水平滚动条高度	//true
document.documentElement.clientwidth === window.innerWidth - 垂直滚动条宽度	//true	
```

`document.body`的高度则是网页的实际高度。一般来说，`document.body.clientHeight`大于`document.documentElement.clientHeight`。

```js
// 视口高度
document.documentElement.clientHeight

// 网页总高度
document.body.clientHeight
```

##### Element.clientLeft，Element.clientTop

`Element.clientLeft`属性等于元素节点左边框（left border）的宽度（单位像素），不包括左侧的`padding`和`margin`。如果没有设置左边框，或者是行内元素（`display: inline`），该属性返回`0`。该属性总是返回整数值，如果是小数，会四舍五入。

`Element.clientTop`属性等于网页元素顶部边框的宽度（单位像素），其他特点都与`clientLeft`相同。

```html
<div style="border: 2px solid red"></div>
<span style="border: 2px solid red"></span>	
<script>
	document.querySelector("div").clientLeft	//2
  	document.querySelector("span").clientLeft	//0
</script>
```

注意：这两个属性包括滚动条的宽度，但是通常来说，只有排版方向从右到左并且发生高度溢出，才会出现左侧滚动条，而顶部滚动条更是不可能存在。

##### Element.scrollHeight，Element.scrollWidth

`Element.scrollHeight`属性返回一个整数值（小数会四舍五入），表示当前元素的总高度（单位像素），包括溢出容器、当前不可见的部分。它包括`padding`，但是不包括`border`、`margin`以及水平滚动条的高度（如果有水平滚动条的话），还包括伪元素（`::before`或`::after`）的高度。

`Element.scrollWidth`属性表示当前元素的总宽度（单位像素），其他地方都与`scrollHeight`属性类似。这两个属性只读。

整张网页的总高度可以从document.documentElement或document.body上读取。

```js
//返回网页的总高度
document.documentElement.scollHeight
document.body.scrollHeight
```

注意，如果元素节点的内容出现溢出，即使溢出的内容是隐藏的，`scrollHeight`属性仍然返回元素的总高度。

```js
// HTML 代码如下
// <div id="myDiv" style="height: 200px; overflow: hidden;">...<div>
document.getElementById('myDiv').scrollHeight // 356
```

上面代码中，即使`myDiv`元素的 CSS 高度只有200像素，且溢出部分不可见，但是`scrollHeight`仍然会返回该元素的原始高度。

##### Element.scrollLeft，Element.scrollTop

`Element.scrollLeft`属性表示当前元素的水平滚动条向右侧滚动的像素数量，`Element.scrollTop`属性表示当前元素的垂直滚动条向下滚动的像素数量。对于那些没有滚动条的网页元素，这两个属性总是等于0。

如果要查看整张网页的水平的和垂直的滚动距离，要从`document.documentElement`元素上读取。

```
document.documentElement.scrollLeft
document.documentElement.scrollTop
```

这两个属性都可读写，设置该属性的值，会导致浏览器将当前元素自动滚动到相应的位置。

##### Element.offsetParent

`Element.offsetParent`属性返回最靠近当前元素的、并且 CSS 的`position`属性不等于`static`的上层元素。

```html
<div style="position: absolute;">
  <p>
    <span>Hello</span>
  </p>
</div>
```

上面代码中，`span`元素的`offsetParent`属性就是`div`元素。

该属性主要用于确定子元素位置偏移的计算基准，`Element.offsetTop`和`Element.offsetLeft`就是`offsetParent`元素计算的。

如果该元素是不可见的（`display`属性为`none`），或者位置是固定的（`position`属性为`fixed`），则`offsetParent`属性返回`null`。

```html
<div style="position: absolute;">
  <p>
    <span style="display: none;">Hello</span>
  </p>
</div>
```

上面代码中，`span`元素的`offsetParent`属性是`null`。

如果某个元素的所有上层节点的`position`属性都是`static`，则`Element.offsetParent`属性指向`<body>`元素。

##### Element.offsetHeight，Element.offsetWidth

`Element.offsetHeight`属性返回一个整数，表示元素的 CSS 垂直高度（单位像素），包括元素本身的高度、padding 和 border，以及水平滚动条的高度（如果存在滚动条）。

`Element.offsetWidth`属性表示元素的 CSS 水平宽度（单位像素），其他都与`Element.offsetHeight`一致。

这两个属性都是只读属性，只比`Element.clientHeight`和`Element.clientWidth`多了边框的高度或宽度。如果元素的 CSS 设为不可见（比如`display: none;`），则返回`0`。

##### Element.offsetLeft，Element.offsetTop

`Element.offsetLeft`返回当前元素左上角相对于`Element.offsetParent`节点的水平位移，`Element.offsetTop`返回垂直位移，单位为像素。通常，这两个值是指相对于父节点的位移。

##### Element.style

`Element.style`属性，该可读写属性用于设置或获取当前元素节点的`style`属性，即该元素节点的行内样式。 详细参照CSS操作章节。

##### Element.children，Element.childElementCount

`Element.children`属性返回一个类似数组的对象（`HTMLCollection`实例），包括当前元素节点的所有子元素。如果当前元素没有子元素，则返回的对象包含零个成员。

```
if (para.children.length) {
  var children = para.children;
    for (var i = 0; i < children.length; i++) {
      // ...
    }
}
```

上面代码遍历了`para`元素的所有子元素。

这个属性与`Node.childNodes`属性的区别是，它只包括元素类型的子节点，不包括其他类型的子节点。

`Element.childElementCount`属性返回当前元素节点包含的子元素节点的个数，与`Element.children.length`的值相同。

##### Element.firstElementChild，Element.lastElementChild

`Element.firstElementChild`属性返回当前元素的第一个元素子节点，`Element.lastElementChild`返回最后一个元素子节点。

如果没有元素子节点，这两个属性返回`null`。

##### Element.nextElementSibling，Element.previousElementSibling

`Element.nextElementSibling`属性返回当前元素节点的后一个同级元素节点，如果没有则返回`null`。

```js
// HTML 代码如下
// <div id="div-01">Here is div-01</div>
// <div id="div-02">Here is div-02</div>
var el = document.getElementById('div-01');
el.nextElementSibling
// <div id="div-02">Here is div-02</div>
```

`Element.previousElementSibling`属性返回当前元素节点的前一个同级元素节点，如果没有则返回`null`。

#### 属性相关方法

元素节点提供六个方法，用来操作属性。

* `getAttribute()`：读取某个属性的值
* `getAttributeNames()`：返回当前元素的所有属性名
* `setAttribute()`：写入属性值
* `hasAttribute()`：某个属性是否存在
* `hasAttributes()`：当前元素是否有属性
* `removeAttribute()`：删除属性

##### Element.querySelector()

`Element.querySelector`方法接受 CSS 选择器作为参数，返回父元素的第一个匹配的子元素。如果没有找到匹配的子元素，就返回`null`。

```
var content = document.getElementById('content');
var el = content.querySelector('p');
```

上面代码返回`content`节点的第一个`p`元素。

`Element.querySelector`方法可以接受任何复杂的 CSS 选择器。

```
document.body.querySelector("style[type='text/css'], style:not([type])");
```

注意，这个方法无法选中伪元素。

它可以接受多个选择器，它们之间使用逗号分隔。

```
element.querySelector('div, p')
```

上面代码返回`element`的第一个`div`或`p`子元素。

需要注意的是，浏览器执行`querySelector`方法时，是先在全局范围内搜索给定的 CSS 选择器，然后过滤出哪些属于当前元素的子元素。因此，会有一些违反直觉的结果，下面是一段 HTML 代码。

```
<div>
<blockquote id="outer">
  <p>Hello</p>
  <div id="inner">
    <p>World</p>
  </div>
</blockquote>
</div>
```

那么，像下面这样查询的话，实际上返回的是第一个`p`元素，而不是第二个。

```
var outer = document.getElementById('outer');
outer.querySelector('div p')
// <p>Hello</p>
```

##### Element.querySelectorAll()

`Element.querySelectorAll`方法接受 CSS 选择器作为参数，返回一个`NodeList`实例，包含所有匹配的子元素。

```
var el = document.querySelector('#test');
var matches = el.querySelectorAll('div.highlighted > p');
```

该方法的执行机制与`querySelector`方法相同，也是先在全局范围内查找，再过滤出当前元素的子元素。因此，选择器实际上针对整个文档的。

它也可以接受多个 CSS 选择器，它们之间使用逗号分隔。如果选择器里面有伪元素的选择器，则总是返回一个空的`NodeList`实例。

##### Element.getElementsByClassName() 

`Element.getElementsByClassName`方法返回一个`HTMLCollection`实例，成员是当前元素节点的所有具有指定 class 的子元素节点。该方法与`document.getElementsByClassName`方法的用法类似，只是搜索范围不是整个文档，而是当前元素节点。

```
element.getElementsByClassName('red test');
```

注意，该方法的参数大小写敏感。

由于`HTMLCollection`实例是一个活的集合，`document`对象的任何变化会立刻反应到实例，下面的代码不会生效。

```
// HTML 代码如下
// <div id="example">
//   <p class="foo"></p>
//   <p class="foo"></p>
// </div>
var element = document.getElementById('example');
var matches = element.getElementsByClassName('foo');

for (var i = 0; i< matches.length; i++) {
  matches[i].classList.remove('foo');
  matches.item(i).classList.add('bar');
}
// 执行后，HTML 代码如下
// <div id="example">
//   <p></p>
//   <p class="foo bar"></p>
// </div>
```

上面代码中，`matches`集合的第一个成员，一旦被拿掉 class 里面的`foo`，就会立刻从`matches`里面消失，导致出现上面的结果。

##### Element.getElementsByTagName()

`Element.getElementsByTagName`方法返回一个`HTMLCollection`实例，成员是当前节点的所有匹配指定标签名的子元素节点。该方法与`document.getElementsByClassName`方法的用法类似，只是搜索范围不是整个文档，而是当前元素节点。

```
var table = document.getElementById('forecast-table');
var cells = table.getElementsByTagName('td');
```

注意，该方法的参数是大小写不敏感的。

##### Element.closest()

该方法接受一个CSS选择器作为参数，返回匹配该选择器的、最接近当前节点的一个祖先节点（包括当前节点本身）。如果没有任何节点匹配CSS选择器，则返回null。

```js
// HTML 代码如下
// <article>
//   <div id="div-01">Here is div-01
//     <div id="div-02">Here is div-02
//       <div id="div-03">Here is div-03</div>
//     </div>
//   </div>
// </article>

var div03 = document.getElementById('div-03');

// div-03 最近的祖先节点
div03.closest("#div-02") // div-02
div03.closest("div div") // div-03
div03.closest("article > div") //div-01
div03.closest(":not(div)") // article
```

上述代码中，由于closest方法将当前节点也考虑在内，所以第二个closest方法返回`div-03`。

##### Element.matches()

该方法返回一个布尔值，表示当前元素是否匹配给定的CSS选择器。

```js
if(el.matches('.someclass')){
    console.log("matches")
}
```

#### 事件相关方法

以下三个方法与Element节点的事件相关。这些方法都继承自EventTarget接口：

* Element.addEventListener()：添加事件的回调函数
* Element.removeEventListener()：移除事件监听函数
* Element.dispatchEvent()：触发事件

```js
element.addEventListener('click', listener, false);
element.removeEventListener('click', listener, false);

var event = new Event('click');
element.dispatchEvent(event);
```

##### Element.scrollIntoView()

该方法滚动当前元素，进入浏览器的可见预期，类似于设置window.location.hash的效果。

```js
el.scrollIntoView(); // 等同于el.scrollIntoView(true)
el.scrollIntoView(false);
```

该方法可以接受一个布尔值作为参数。如果为true，表示元素的顶部与当前区域的课件部分的顶部对齐（前提是当前区域可滚动）；如果为false，表示元素的底部与当前区域的可见部分的尾部对齐（前提是当前区域可滚动）。如果没有提供该参数，默认为true。

##### Element.getBoundingClientRect()

`Element.getBoundingClientRect`方法返回一个对象，提供当前元素节点的大小、位置等信息，基本上就是 CSS 盒状模型的所有信息。

```
var rect = obj.getBoundingClientRect();
```

上面代码中，`getBoundingClientRect`方法返回的`rect`对象，具有以下属性（全部为只读）。

- `x`：元素左上角相对于视口的横坐标
- `y`：元素左上角相对于视口的纵坐标
- `height`：元素高度
- `width`：元素宽度
- `left`：元素左上角相对于视口的横坐标，与`x`属性相等
- `right`：元素右边界相对于视口的横坐标（等于`x + width`）
- `top`：元素顶部相对于视口的纵坐标，与`y`属性相等
- `bottom`：元素底部相对于视口的纵坐标（等于`y + height`）

由于元素相对于视口（viewport）的位置，会随着页面滚动变化，因此表示位置的四个属性值，都不是固定不变的。如果想得到绝对位置，可以将`left`属性加上`window.scrollX`，`top`属性加上`window.scrollY`。

注意，`getBoundingClientRect`方法的所有属性，都把边框（`border`属性）算作元素的一部分。也就是说，都是从边框外缘的各个点来计算。因此，`width`和`height`包括了元素本身 + `padding` + `border`。

另外，上面的这些属性，都是继承自原型的属性，`Object.keys`会返回一个空数组，这一点也需要注意。

```
var rect = document.body.getBoundingClientRect();
Object.keys(rect) // []
```

上面代码中，`rect`对象没有自身属性，而`Object.keys`方法只返回对象自身的属性，所以返回了一个空数组。

##### Element.getClientRects()

`Element.getClientRects`方法返回一个类似数组的对象，里面是当前元素在页面上形成的所有矩形（所以方法名中的`Rect`用的是复数）。每个矩形都有`bottom`、`height`、`left`、`right`、`top`和`width`六个属性，表示它们相对于视口的四个坐标，以及本身的高度和宽度。

对于盒状元素（比如`<div>`和`<p>`），该方法返回的对象中只有该元素一个成员。对于行内元素（比如`<span>`、`<a>`、`<em>`），该方法返回的对象有多少个成员，取决于该元素在页面上占据多少行。这是它和`Element.getBoundingClientRect()`方法的主要区别，后者对于行内元素总是返回一个矩形。

```
<span id="inline">Hello World Hello World Hello World</span>
```

上面代码是一个行内元素`<span>`，如果它在页面上占据三行，`getClientRects`方法返回的对象就有三个成员，如果它在页面上占据一行，`getClientRects`方法返回的对象就只有一个成员。

```
var el = document.getElementById('inline');
el.getClientRects().length // 3
el.getClientRects()[0].left // 8
el.getClientRects()[0].right // 113.908203125
el.getClientRects()[0].bottom // 31.200000762939453
el.getClientRects()[0].height // 23.200000762939453
el.getClientRects()[0].width // 105.908203125
```

这个方法主要用于判断行内元素是否换行，以及行内元素的每一行的位置偏移。

注意，如果行内元素包括换行符，那么该方法会把换行符考虑在内。

```
<span id="inline">
  Hello World
  Hello World
  Hello World
</span>
```

上面代码中，`<span>`节点内部有三个换行符，即使 HTML 语言忽略换行符，将它们显示为一行，`getClientRects()`方法依然会返回三个成员。如果行宽设置得特别窄，上面的`<span>`元素显示为6行，那么就会返回六个成员。

##### Element.insertAdjacentElement()

`Element.insertAdjacentElement`方法在相对于当前元素的指定位置，插入一个新的节点。该方法返回被插入的节点，如果插入失败，返回`null`。

```
element.insertAdjacentElement(position, element);
```

`Element.insertAdjacentElement`方法一共可以接受两个参数，第一个参数是一个字符串，表示插入的位置，第二个参数是将要插入的节点。第一个参数只可以取如下的值。

- `beforebegin`：当前元素之前
- `afterbegin`：当前元素内部的第一个子节点前面
- `beforeend`：当前元素内部的最后一个子节点后面
- `afterend`：当前元素之后

注意，`beforebegin`和`afterend`这两个值，只在当前节点有父节点时才会生效。如果当前节点是由脚本创建的，没有父节点，那么插入会失败。

```
var p1 = document.createElement('p')
var p2 = document.createElement('p')
p1.insertAdjacentElement('afterend', p2) // null
```

上面代码中，`p1`没有父节点，所以插入`p2`到它后面就失败了。

如果插入的节点是一个文档里现有的节点，它会从原有位置删除，放置到新的位置。

##### Element.insertAdjacentHTML()，Element.insertAdjacentText()

`Element.insertAdjacentHTML`方法用于将一个 HTML 字符串，解析生成 DOM 结构，插入相对于当前节点的指定位置。

```
element.insertAdjacentHTML(position, text);
```

该方法接受两个参数，第一个是一个表示指定位置的字符串，第二个是待解析的 HTML 字符串。第一个参数只能设置下面四个值之一。

- `beforebegin`：当前元素之前
- `afterbegin`：当前元素内部的第一个子节点前面
- `beforeend`：当前元素内部的最后一个子节点后面
- `afterend`：当前元素之后

```
// HTML 代码：<div id="one">one</div>
var d1 = document.getElementById('one');
d1.insertAdjacentHTML('afterend', '<div id="two">two</div>');
// 执行后的 HTML 代码：
// <div id="one">one</div><div id="two">two</div>
```

该方法只是在现有的 DOM 结构里面插入节点，这使得它的执行速度比`innerHTML`方法快得多。

注意，该方法不会转义 HTML 字符串，这导致它不能用来插入用户输入的内容，否则会有安全风险。

`Element.insertAdjacentText`方法在相对于当前节点的指定位置，插入一个文本节点，用法与`Element.insertAdjacentHTML`方法完全一致。

```
// HTML 代码：<div id="one">one</div>
var d1 = document.getElementById('one');
d1.insertAdjacentText('afterend', 'two');
// 执行后的 HTML 代码：
// <div id="one">one</div>two
```

##### Element.remove()

该方法集成自ChildNode接口，用于将当前元素节点从它的父节点移除。

```js
var el = document.getElementById('mydiv');
el.remove();
```

##### Element.focus()，Element.blur()

`Element.focus`方法用于将当前页面的焦点，转移到指定元素上。

```
document.getElementById('my-span').focus();
```

该方法可以接受一个对象作为参数。参数对象的`preventScroll`属性是一个布尔值，指定是否将当前元素停留在原始位置，而不是滚动到可见区域。

```
function getFocus() {
  document.getElementById('btn').focus({preventScroll:false});
}
```

上面代码会让`btn`元素获得焦点，并滚动到可见区域。

最后，从`document.activeElement`属性可以得到当前获得焦点的元素。

`Element.blur`方法用于将焦点从当前元素移除。

##### Element.click()

该方法用于在当前元素上模拟一次鼠标点击，相当于触发了click事件。

### 属性操作

HTML元素包括标签名和若干个键值对，这个键值对就称为属性（attribute）。

属性本身是一个对象（Attr对象），但是实际上，这个对象极少使用。一般都是通过元素节点对象（HTMLElement对象）来操作属性。

#### Element.attributes 属性

元素对象有一个`attributes`属性，返回一个类似数组的动态对象，成员是该元素标签的所有属性节点对象，属性的实时变化都会反映在这个节点对象上。其他类型的节点对象，虽然也有`attributes`属性，但返回的都是`null`，因此可以把这个属性视为元素对象独有的。

单个属性可以通过序号引用，也可以通过属性名引用。

```
// HTML 代码如下
// <body bgcolor="yellow" onload="">
document.body.attributes[0]
document.body.attributes.bgcolor
document.body.attributes['ONLOAD']
```

注意，上面代码的三种方法，返回的都是属性节点对象，而不是属性值。

属性节点对象有`name`和`value`属性，对应该属性的属性名和属性值，等同于`nodeName`属性和`nodeValue`属性。

```
// HTML代码为
// <div id="mydiv">
var n = document.getElementById('mydiv');

n.attributes[0].name // "id"
n.attributes[0].nodeName // "id"

n.attributes[0].value // "mydiv"
n.attributes[0].nodeValue // "mydiv"
```

下面代码可以遍历一个元素节点的所有属性。

```js
var para = document.getElementsByTagName('p')[0];
var result = document.getElementById('result');

if (para.hasAttributes()) {
  var attrs = para.attributes;
  var output = '';
  for(var i = attrs.length - 1; i >= 0; i--) {
    output += attrs[i].name + '->' + attrs[i].value;
  }
  result.textContent = output;
} else {
  result.textContent = 'No attributes to show';
}
```

#### 元素的标准属性

HTML 元素的标准属性（即在标准中定义的属性），会自动成为元素节点对象的属性。

```
var a = document.getElementById('test');
a.id // "test"
a.href // "http://www.example.com/"
```

上面代码中，`a`元素标签的属性`id`和`href`，自动成为节点对象的属性。

这些属性都是可写的。

```
var img = document.getElementById('myImage');
img.src = 'http://www.example.com/image.jpg';
```

上面的写法，会立刻替换掉`img`对象的`src`属性，即会显示另外一张图片。

这种修改属性的方法，常常用于添加表单的属性。

```
var f = document.forms[0];
f.action = 'submit.php';
f.method = 'POST';
```

上面代码为表单添加提交网址和提交方法。

注意，这种用法虽然可以读写属性，但是无法删除属性，`delete`运算符在这里不会生效。

HTML 元素的属性名是大小写不敏感的，但是 JavaScript 对象的属性名是大小写敏感的。转换规则是，转为 JavaScript 属性名时，一律采用小写。如果属性名包括多个单词，则采用骆驼拼写法，即从第二个单词开始，每个单词的首字母采用大写，比如`onClick`。

有些 HTML 属性名是 JavaScript 的保留字，转为 JavaScript 属性时，必须改名。主要是以下两个。

- `for`属性改为`htmlFor`
- `class`属性改为`className`

另外，HTML 属性值一般都是字符串，但是 JavaScript 属性会自动转换类型。比如，将字符串`true`转为布尔值，将`onClick`的值转为一个函数，将`style`属性的值转为一个`CSSStyleDeclaration`对象。因此，可以对这些属性赋予各种类型的值。

#### 属性操作的标准方法

元素节点提供六个方法，用来操作属性。

* `getAttribute()`
* `getAttributeNames()`
* `setAttribute()`
* `hasAttribute()`
* `hasAttributes()`
* `removeAttribute()`

这有几点注意：

* 适用性：这六个方法对所有属性（包括用户自定义的属性）都适用
* 返回值：`getAttribute`只返回字符串，不会返回其他类型的值
* 属性名：这些方法只接受属性的标准名称，不用改写保留字，比如for和class都可以直接使用。另外，这些方法对于属性名是大小写敏感的

##### Element.getAttribute()

`Element.getAttribute`方法返回当前元素节点的指定属性。如果指定属性不存在，则返回`null`。

```
// HTML 代码为
// <div id="div1" align="left">
var div = document.getElementById('div1');
div.getAttribute('align') // "left"
```

##### Element.getAttributeNames()

`Element.getAttributeNames()`返回一个数组，成员是当前元素的所有属性的名字。如果当前元素没有任何属性，则返回一个空数组。使用`Element.attributes`属性，也可以拿到同样的结果，唯一的区别是它返回的是类似数组的对象。

```
var mydiv = document.getElementById('mydiv');

mydiv.getAttributeNames().forEach(function (key) {
  var value = mydiv.getAttribute(key);
  console.log(key, value);
})
```

上面代码用于遍历某个节点的所有属性。

##### Element.setAttribute()

`Element.setAttribute`方法用于为当前元素节点新增属性。如果同名属性已存在，则相当于编辑已存在的属性。该方法没有返回值。

```
// HTML 代码为
// <button>Hello World</button>
var b = document.querySelector('button');
b.setAttribute('name', 'myButton');
b.setAttribute('disabled', true);
```

上面代码中，`button`元素的`name`属性被设成`myButton`，`disabled`属性被设成`true`。

这里有两个地方需要注意，首先，属性值总是字符串，其他类型的值会自动转成字符串，比如布尔值`true`就会变成字符串`true`；其次，上例的`disable`属性是一个布尔属性，对于`<button>`元素来说，这个属性不需要属性值，只要设置了就总是会生效，因此`setAttribute`方法里面可以将`disabled`属性设成任意值。

##### Element.hasAttribute()

`Element.hasAttribute`方法返回一个布尔值，表示当前元素节点是否包含指定属性。

```
var d = document.getElementById('div1');

if (d.hasAttribute('align')) {
  d.setAttribute('align', 'center');
}
```

上面代码检查`div`节点是否含有`align`属性。如果有，则设置为居中对齐。

##### Element.hasAttributes()

`Element.hasAttributes`方法返回一个布尔值，表示当前元素是否有属性，如果没有任何属性，就返回`false`，否则返回`true`。

```
var foo = document.getElementById('foo');
foo.hasAttributes() // true
```

##### Element.removeAttribute()

`Element.removeAttribute`方法移除指定属性。该方法没有返回值。

```
// HTML 代码为
// <div id="div1" align="left" width="200px">
document.getElementById('div1').removeAttribute('align');
// 现在的HTML代码为
// <div id="div1" width="200px">
```

#### dataset 属性

有时，需要在HTML元素上附加数据，供 JavaScript 脚本使用。一种解决方法是自定义属性。

```
<div id="mydiv" foo="bar">
```

上面代码为`div`元素自定义了`foo`属性，然后可以用`getAttribute()`和`setAttribute()`读写这个属性。

```
var n = document.getElementById('mydiv');
n.getAttribute('foo') // bar
n.setAttribute('foo', 'baz')
```

这种方法虽然可以达到目的，但是会使得 HTML 元素的属性不符合标准，导致网页代码通不过校验。

更好的解决方法是，使用标准提供的`data-*`属性。

```
<div id="mydiv" data-foo="bar">
```

然后，使用元素节点对象的`dataset`属性，它指向一个对象，可以用来操作 HTML 元素标签的`data-*`属性。

```
var n = document.getElementById('mydiv');
n.dataset.foo // bar
n.dataset.foo = 'baz'
```

上面代码中，通过`dataset.foo`读写`data-foo`属性。

删除一个`data-*`属性，可以直接使用`delete`命令。

```
delete document.getElementById('myDiv').dataset.foo;
```

除了`dataset`属性，也可以用`getAttribute('data-foo')`、`removeAttribute('data-foo')`、`setAttribute('data-foo')`、`hasAttribute('data-foo')`等方法操作`data-*`属性。

注意，`data-`后面的属性名有限制，只能包含字母、数字、连词线（`-`）、点（`.`）、冒号（`:`）和下划线（`_`)。而且，属性名不应该使用`A`到`Z`的大写字母，比如不能有`data-helloWorld`这样的属性名，而要写成`data-hello-world`。

转成`dataset`的键名时，连词线后面如果跟着一个小写字母，那么连词线会被移除，该小写字母转为大写字母，其他字符不变。反过来，`dataset`的键名转成属性名时，所有大写字母都会被转成连词线+该字母的小写形式，其他字符不变。比如，`dataset.helloWorld`会转成`data-hello-world`。

### Text节点

文本节点（Text）代表元素节点（Element）和属性节点（Attribute）的文本操作。如果一个节点只包含一段文本，那么它就有一个文本子节点，代表该节点的文本内容。

通常我们使用父节点的`firstChild`、`nextSibling`等属性获取文本节点，或者使用`Document`节点的`createTextNode`方法创造一个文本节点。

浏览器原生提供一个Text构造函数，它返回一个文本节点实例。它的参数就是该文本节点的文本内容。

```js
new Text("This is a text node")
new Text(" ")			//空格也是一个字符，因此也会形成文本节点
```

文本节点除了继承Node节点，还继承了CharacterData接口。Node接口的属性和方法参考前面的内容，下面会介绍来自CharacterData接口的属性和方法。

#### Text节点的属性

##### CharacterData.property.data

该属性用于设置或读取文本节点的内容，等同于nodeValue属性。

```javascript
new Text("hi").data			//"hi"
new Text("hi").data === new Text("hi").nodeValue === new Text("hi").textContent	//true
var t = new Text("")
t.data = "lee"
t.data						//"lee"
```

##### CharacterData.property.wholeText

该属性将当前文本节点与毗邻的文本节点作为一个整体返回。大多数情况下，该属性的返回值与data属性和textContent属性相同，某些情况下会有差异。

```html
<p>A C</p>
<script>
	var p = document.querySelector("p")
  	p.firstChild.wholeText		//"A c"
  	p.firstChild.data			//"A "
</script>
```

##### CharacterData.property.length

该属性返回当前文本节点的文本长度。

```javascript
(new Text('hi')).length 	//2
```

##### CharacterData.property.nextElementSibling，CharacterData.property.previousElementSibling

* 前者返回当前文本节点的后一个同级元素节点，如果没有则返回null
* 后者返回当前文本节点的前一个同级元素节点，如果没有，则返回null

#### Text节点的方法

##### CharacterData.propertyappendData()，CharacterData.property.deleteData()，CharacterData.property.insertData()，CharacterData.property.replaceData()，CharacterData.property.subStringData()

这五个方法都是编辑Text文本节点内容的方法：

* `CharacterData.property.appendData`方法用于在当前文本节点尾部追加文本内容。
* `CharacterData.property.deleteData`方法用于删除当前文本节点的文本内容。该方法接受两个参数，第一个参数是要删除文本的位置，第二个参数是要删除文本的长度。
* `CharacterData.property.insertData`方法用于在当前文本节点插入文本内容。该方法接受两个参数，第一个参数是要插入文本的位置，第二个参数是要插入文本。
* `CharacterData.property.replaceData`方法用于替换当前文本节点的文本内容。该方法接受三个参数，第一个参数是要替换文本的位置，第二个参数是要替换文本的长度，第三个参数是要替换文本。
* `CharacterData.property.subStringData`方法用于截取当前文本节点的子文本内容 。该方法接受两个参数，第一个参数是要截取文本的位置，第二个参数是要截取文本的长度。

```javascript
var t = new Text("hello")
t.appendData("world")
t.data					//"helloworld"
t.deleteData(5,5)
t.data					//"hello"
t.insertData(5,"lee")	
t.data					//"hellolee"
t.replaceData(5,3,"yan")
t.data					//"helloyan"
t.subStringData(5,3)	//"yan"
t.data					//"hello"
```

##### CharacterData.property.remove()

该方法用于移除当前Text节点。

##### CharacterData.property.splitText()，CharacterData.property.normalize()

前者用于将当前文本节点分割成两个毗邻的文本节点，它接受一个参数表示分割位置，如果指定的分割位置不存在，则报错。

注意：该方法返回分割位置后方的字符串，而原Text节点变成只包含分割位置前方的字符串。

父元素节点的normalize方法可以将毗邻的两个Text节点合并。

```html
<p>leesin</p>
<script>
	var p = document.querySelector("p")
    var t = p.firstChild
    t.splitText(3)			//"sin"
  	t.data					//"lee"
  	p.childNodes.length		//2
  	p.normalize()	
  	p.childNodes.length		//1
</script>
```

### DocumentFragment节点

DocumentFragment节点表示文档中的一个片段，它是存在于内存之中，而不属于当前文档。所以该节点没有父节点，它的`Element.parentNode`属性返回null。但是它又和元素节点一样，可以使用`Element.appendChild`、`Element.insertBefore`、`Element.replaceChild`等方法插入任意多的子节点。由于DocumentFragment节点不属于当前文档，所以对它的任何改动都不会引发网页的重新渲染，这比直接修改文档DOM结构更有效率。

该节点常常用于生产复杂的DOM结构，然后插入当前文档。注意：DocumentFragment节点本身不能插入当前文档，而是将它所有的子节点插入当前文档。一旦DocumentFragment节点"插入"到当前文档，它自身就变成了空节点，可以被再次调用。如果想要保存DocumentFragment节点的内容，需要使用`Node.cloneNode`方法克隆该节点。

可以使用如下两种方法创建DocumentFragment节点。注意：DocumentFragment节点对象自身没有属性和方法，全部继承自Node节点和ParentNode接口。

```javascript
var docFrag = document.createDocumentFragment()
var docFrag = new DocumentFragment()
```

### CSS操作

#### HTML元素的style属性

操作CSS样式最简单的方法就是直接使用`Element.getAttribute`、`Element.setAttribute`和`Element.removeAttribute`方法读写元素节点的`Element.style`属性。

```javascript
div.setAttribute(
  'style',
  'background-color:red;' + 'border:1px solid black;'
);
```

style不仅可以使用字符串读写，它本身还是一个对象，部署了CSSStyleDeclaration接口，可以直接读写个别属性。

#### CSSStyleDeclaration接口

该接口用来操作元素的样式，三个地方部署了这个接口：

* 元素节点的style属性
* CSSStyle实例的style属性
* window.getComputed()的返回值

CSSStyleDeclartion接口可以直接读写CSS的样式属性。

```js
div.style.color = "red"
div.style.backgroundColor = "blue"
div.style.cssFloat = "left"
```

通过操作`Element.style`属性，可以读写行内CSS样式。该属性返回值是一个Style对象，这个对象包含的属性名与CSS属性名一一对应，但是名字需要部分改写。例如：`background-color`需要改写成`backgroundColor`。 

具体的改写规则是将`-`号从CSS属性名中去除，然后将`-`号后的第一个字母大写。如果CSS属性名是JavaScript保留字，则需要在其前面加上css字符。例如：`float`需要改写成`cssFloat`。注意：style对象的属性值都是字符串，设置时必须包括单位，但是不包含结尾的分号。

style对象上定义了`Style.cssText`属性用来读写整个CSS样式，但是该属性不需要改写CSS属性名。

```javascript
div.style.cssText = "background-color: red;" + "float: left"
```

删除一个元素的所有行内样式，最简便的方法就是设置cssText为空字符串。

#### CSSStyleDeclaration实例属性

##### CSSStyleDeclaration.property.cssText

该属性用来读写当前规则的所有样式声明文本。详细见上面。

##### CSSStyleDeclaration.property.length

该属性返回一个整数值，表示当前规则包含多少条样式声明。

##### CSSStyleDeclaration.property.parentRule

该属性返回当前规则所属的那个样式块（CSSRule实例）。如果不存在所属的样式块，该属性返回null。

该属性只读，且只在使用CSSRule接口时有意义。

##### CSSStyleDeclaration实例方法

##### CSSStyleDeclaration.property.getPropertyPriority()

该方法接受CSS样式的属性作为参数，返回一个字符串，表示有没有设置important优先级。如果有就返回important，否则返回空字符串。

##### CSSStyleDeclaration.property.getPropertyValue()

该方法接口CSS样式属性名作为参数，返回一个字符串，表示该属性的属性值。

##### CSSStyleDeclaration.property.item()

该方法接受一个整数值作为参数，返回该位置的CSS属性名。

```js
// HTML 代码为
// <div id="myDiv" style="color: red; background-color: white;"/>
var style = document.getElementById('myDiv').style;
style.item(0) // "color"
style.item(1) // "background-color"
```

如果没有提供参数，这个方法会报错。如果参数值超过实际的属性数目，这个方法返回一个空字符串。

##### CSSStyleDeclaration.property.removeProperty()

该方法接受一个属性名作为参数，在CSS规则里面移除这个属性，返回这个属性原来的值。

##### CSSStyleDeclaration.property.setProperty()

该方法用来设置新的CSS属性，该方法没有返回值。

它接受三个参数：

* 第一个参数：属性名，该参数是必须的
* 第二个参数：属性值，该参数可选。如果省略，则参数默认为空字符串
* 第三个参数：优先级，该参数可选，唯一的合法值是important，表示CSS规则里面的`!important`

#### CSS模块侦测

CSS模块发展迅速，新的模块层出不穷，所以不同浏览器的不同版本对CSS模块的支持情况不同。有时，需要知道当前浏览器是否支持某个模块，这个过程就称为CSS模块检测。

其中一种普遍适用的方法是判断style对象中的某个属性是否为字符串，因为如果该属性在当前有定义，那么即使未设置属性值，也会返回一个空字符串。而如果该属性不存在，则返回undefined。注意：无论CSS属性名带不带`-`号，只要有定义，就会返回空字符串。

```javascript
typeof div.style.transform === "string"
div.style["maxWidth"]	//""
div.style.maximumWidth	//undefined
document.body.style['backgroundColor'] 	// ""
document.body.style['background-color'] // ""
//所有浏览器都能使用这种方法，但是有些属性需要将浏览器私有前缀写上去
typeof content.style['webkitAnimation'] === 'string'
```

此外，部分浏览器(Firefox 22+, Chrome 28+, Opera 12.1+)部署了supports API，可以返回一个布尔值，表示是否支持某条CSS规则。注意：这个API还没有成为标准。

```javascript
CSS.supports('transform-origin', '5px')
CSS.supports('(display: table-cell) and (display: list-item)')
```

#### CSS对象

浏览器原生提供CSS对象，为JavaScript操作CSS提供一些工具方法。这个对象目前有两个静态方法。





#### window.getComputedStyle()

行内样式具有最高的优先级，改变行内样式，通常会立即反映出来。但是，网页元素最终的样式是综合各种CSS规则计算出来的。因此，如果想得到元素现有的样式，只读取行内样式是不够的，而是需要得到浏览器最终计算出来的那个样式规则。

使用`window.getComputedStyle`方法就可以得到某个DOM元素的计算样式，这个计算样式是指CSS规则叠加后的结果。该方法接受两个参数，其中第一个参数表示指定的DOM节点，第二个可选参数表示指定DOM节点的伪元素，比如`:before`，`:after`，`:first-line`等。

```javascript
window.getComputedStyle(div).color
window.getComputedStyle(div,":before").backgroundColor
window.getComputedStyle(div,null).fontSize
```

使用该方法还有如下几点需要补充：

* 该方法返回的样式对象是只读的，如果想要设置样式，应该使用style属性


* 该方法返回的样式对象和style对象一样，同样拥有cssText属性，返回计算样式的字符串形式。但是不能使用该属性设置样式，因为该样式对象是只读
* 该方法返回的样式对象也同样拥有setProperty()，getPropertyValue()和removeProperty()这三个方法，但是不能使用其中setProperty()和removeProperty()方法，因为该对象属性是只读
* 该方法返回的样式对象中的CSS值都是绝对单位。例如：长度都是以像素为单位(包括px)，颜色都是`rgb`或`rgba`
* CSS规则的简写形式无效。比如：想读取margin属性的值，不能直接读，只能读`marginLeft`、`marginTop`等属性

#### CSS伪元素

CSS伪元素是通过CSS向DOM添加的元素，主要是通过`:before`和`:after`选择器生成伪元素，然后用`content`属性指定伪元素的内容。但是DOM节点的style对象无法获取伪元素的样式，此时就需要使用`window.getComputedStyle`方法。

```html
<style>
  div:after {
      content: "world";
      color: red
  }
</style>
<div>hello</div>
<script>
	window.getComputedStyle(document.querySelector("div"),":after").content;
  	window.getComputedStyle(document.querySelector("div"),":after").getPropertyValue("color")
</script>
```

#### StyleSheet接口

##### 概述

StyleSheet接口代表网页的一张样式表，包括`<link>`元素加载的样式表和`<style>`元素内嵌的样式表。

`document`对象的`styleSheet`属性，可以返回当前页面的所有`StyleSheet`实例（即所有样式表）。它是一个类似数组的对象。

```js
var sheets = document.styleSheets;
var sheet = document.styleSheets[0];
sheet instanceof StyleSheet // true
```

如果是`style`元素嵌入的样式表，还有另一种获取`StyleSheet`实例的方法，就是这个节点元素的`sheet`属性。

严格地说，`StyleSheet`接口不仅包括网页样式表，还包括 XML 文档的样式表。所以，它有一个子类`CSSStyleSheet`表示网页的 CSS 样式表。我们在网页里面拿到的样式表实例，实际上是`CSSStyleSheet`的实例。这个子接口继承了`StyleSheet`的所有属性和方法，并且定义了几个自己的属性，下面把这两个接口放在一起介绍.

##### 实例属性

##### StyleSheet.prototype.disabled

该属性返回一个布尔值，表示该样式表是否处于禁用状态。手动设置disable属性为true，等同于在link元素里面讲这张样式表设置为`alternate stylesheet`，即该样式表将不会生效。

注意：该属性只能在JavaScript脚本中设置，不能再HTML语句中设置。

##### StyleSheet.prototype.href

该属性返回样式表的网址。对于内嵌样式表，该属性返回null。该属性只读。

##### StyleSheet.prototype.media

`StyleSheet.media`属性返回一个类似数组的对象（`MediaList`实例），成员是表示适用媒介的字符串。表示当前样式表是用于屏幕（screen），还是用于打印（print）或手持设备（handheld），或各种媒介都适用（all）。该属性只读，默认值是`screen`。

```
document.styleSheets[0].media.mediaText
// "all"
```

`MediaList`实例的`appendMedium`方法，用于增加媒介；`deleteMedium`方法用于删除媒介。

```
document.styleSheets[0].media.appendMedium('handheld');
document.styleSheets[0].media.deleteMedium('print');
```

##### StyleSheet.prototype.title

该属性返回样式表的title属性。

##### StyleSheet.prototype.type

该属性返回样式表的type属性，通常是`text/css`。

##### StyleSheet.prototype.parentStyleSheet

CSS 的`@import`命令允许在样式表中加载其他样式表。`StyleSheet.parentStyleSheet`属性返回包含了当前样式表的那张样式表。如果当前样式表是顶层样式表，则该属性返回`null`。

```
if (stylesheet.parentStyleSheet) {
  sheet = stylesheet.parentStyleSheet;
} else {
  sheet = stylesheet;
}
```

##### StyleSheet.prototype.ownerNode

该属性返回`StyleSheet`对象所在的DOM节点，通常是`<link>`或`<style>`。对于那些由其它样式引用的样式表，该属性为null。

```js
// HTML代码为
// <link rel="StyleSheet" href="example.css" type="text/css" />
document.styleSheets[0].ownerNode // [object HTMLLinkElement]
```

##### StyleSheet.prototype.cssRules

该属性指向一个类似数组的对象（CSSRuleList实例），里面每一个成员都是当前样式表的一条CSS规则。使用该规则的cssText属性，可以得到CSS规则对应的字符串。

```js
var sheet = document.querySelector('#styleElement').sheet;

sheet.cssRules[0].cssText
// "body { background-color: red; margin: 20px; }"

sheet.cssRules[1].cssText
// "p { line-height: 1.4em; color: blue; }"
```

每条CSS规则还有一个style属性，指向一个对象，用来读写具体的CSS命令。

```js
cssStyleSheet.cssRules[0].style.color = 'red';
cssStyleSheet.cssRules[1].style.color = 'purple';
```

##### StyleSheet.prototype.ownerRule

有些样式表是通过`@important`规则输入的，它的`ownerRule`属性会返回一个`CSSRule`实例，代表那行`@import`规则。如果当前样式表不是通过`@import`引入的，该属性返回null。

##### 实例方法

##### StyleSheet.prototype.insertRule()

该方法用于在当前样式表插入一个新的CSS规则。它接受两个参数，第一个参数是表示CSS规则的字符串，这里只能有一条规则，否则会报错。第二个参数是该规则在样式表的插入位置（从0开始），该参数可选，默认为0（即默认插在样式表的头部）。

注意：如果插入位置大于现有规则的数目，会报错。

该方法的返回值是新插入规则的位置序号。

> 浏览器对脚本在样式表里面插入规则有很多[限制](https://drafts.csswg.org/cssom/#insert-a-css-rule)。所以，这个方法最好放在`try...catch`里使用。

##### StyleSheet.prototype.deleteRule()

该方法用来在样式表里面移除一条规则，它的参数是该条规则在cssRules对象中的位置。该方法没有返回值。

```js
var s = document.querySelector("style").sheet;	//返回CSSStyleSheet对象
//相当于document.styleSheets[0].ownerNode
s.insertRule("p{color:red}",0);					//在样式表最顶处插入新规则
s.deleteRule(1);
```

#### 添加样式表

添加样式表有两种方式，其中一种是通过style节点添加内联样式表，另一种是通过link节点添加外联样式表。

```javascript
//添加内联样式表
var style = document.createElement('style');
style.setAttribute("media", "@media only screen and (max-width : 1024px)");
// 或者 style.setAttribute('media', 'screen');
style.innerHTML = 'body{color:red}';
// 或者 style.sheet.insertRule("header { float: left; opacity: 0.8; }", 1);
document.head.appendChild(style);
//添加外联样式表
var link = document.createElement("link")
link.setAttribute('rel',"stylesheet")
link.setAttrbute("type","text/css")
link.setAttribute("href","reset.css")
document.head.appendChild(link)
```

#### CSSRuleList接口

CSSRuleList接口是一个类似数组的对象，表示一组CSS规则，成员都是CSSRule实例。

获取CSSRuleList实例，一般是通过StyleSheet.cssRules属性。

```js
// HTML 代码如下
// <style id="myStyle">
//   h1 { color: red; }
//   p { color: blue; }
// </style>
var myStyleSheet = document.getElementById('myStyle').sheet;
var crl = myStyleSheet.cssRules;
crl instanceof CSSRuleList // true
```

CSSRuleList 实例里面，每一条规则（CSSRule 实例）可以通过`rules.item(index)`或者`rules[index]`拿到。CSS 规则的条数通过`rules.length`拿到。还是用上面的例子。

```
crl[0] instanceof CSSRule // true
crl.length // 2
```

注意，添加规则和删除规则不能在 CSSRuleList 实例操作，而要在它的父元素 StyleSheet 实例上，通过`StyleSheet.insertRule()`和`StyleSheet.deleteRule()`操作。

#### CSSRule接口

##### 概述

一条CSS规则包括两个部分：CSS选择器和样式声明。JavaScript通过CSSRule接口操作CSS规则，一般通过CSSRuleList接口（StyleSheet.cssRule）获取CSSRule实例。

##### CSSRule实例属性

##### CSSRule.prototype.cssText

该属性返回当前规则的文本。如果规则是加载（`@import`）其他样式表，`cssText`属性返回`@import 'url'`。

##### CSSRule.prototype.parentStyleSheet

该属性返回当前规则所在的样式表对象（StyleSheet实例）。

##### CSSRule.prototype.parentRule

该属性返回包含当前规则的父规则，如果不存在父规则（即当前规则是顶层规则），则返回null。

父规则最常见的情况是，当前规则包含在`@media`规则代码块之中。

```js
// HTML 代码如下
// <style id="myStyle">
//   @supports (display: flex) {
//     @media screen and (min-width: 900px) {
//       article {
//         display: flex;
//       }
//     }
//  }
// </style>
var myStyleSheet = document.getElementById('myStyle').sheet;
var ruleList = myStyleSheet.cssRules;

var rule0 = ruleList[0];
rule0.cssText
// "@supports (display: flex) {
//    @media screen and (min-width: 900px) {
//      article { display: flex; }
//    }
// }"

// 由于这条规则内嵌其他规则，
// 所以它有 cssRules 属性，且该属性是 CSSRuleList 实例
rule0.cssRules instanceof CSSRuleList // true

var rule1 = rule0.cssRules[0];
rule1.cssText
// "@media screen and (min-width: 900px) {
//   article { display: flex; }
// }"

var rule2 = rule1.cssRules[0];
rule2.cssText
// "article { display: flex; }"

rule1.parentRule === rule0 // true
rule2.parentRule === rule1 // true
```

##### CSSRule.prototype.type

该属性返回一个整数值，表示当前规则的类型。

最常见的类型有以下几种。

* 1：普通样式规则（CSSStyleRule 实例）
* 3：`@import`规则
* 4：`@media`规则（CSSMediaRule 实例）
* 5：`@font-face`规则

##### CSSStyleRule接口

如果一条CSS规则是普通的样式规则（不含特殊的CSS命令），那么除了CSSRule接口，它还部署了CSSStyleRule接口。

##### 实例属性

##### CSSStyleRule.prototype.selectorText

该属性返回当前规则的选择器。注意：该属性是可写的。

##### CSSStyleRule.prototype.style

该属性返回一个对象（CSSStyleDeclartion实例），代表当前规则的样式声明，也就是选择器后面的大括号里面的部分。

```
// HTML 代码为
// <style id="myStyle">
//   p { color: red; }
// </style>
var styleSheet = document.getElementById('myStyle').sheet;
styleSheet.cssRules[0].style instanceof CSSStyleDeclaration
// true
```

CSSStyleDeclaration 实例的`cssText`属性，可以返回所有样式声明，格式为字符串。

```
styleSheet.cssRules[0].style.cssText
// "color: red;"
styleSheet.cssRules[0].selectorText
// "p"
```

##### CSSMediaRule 接口

如果一条 CSS 规则是`@media`代码块，那么它除了 CSSRule 接口，还部署了 CSSMediaRule 接口。

该接口主要提供`media`属性和`conditionText`属性。前者返回代表`@media`规则的一个对象（MediaList 实例），后者返回`@media`规则的生效条件。

```
// HTML 代码如下
// <style id="myStyle">
//   @media screen and (min-width: 900px) {
//     article { display: flex; }
//   }
// </style>
var styleSheet = document.getElementById('myStyle').sheet;
styleSheet.cssRules[0] instanceof CSSMediaRule
// true

styleSheet.cssRules[0].media
//  {
//    0: "screen and (min-width: 900px)",
//    appendMedium: function,
//    deleteMedium: function,
//    item: function,
//    length: 1,
//    mediaText: "screen and (min-width: 900px)"
// }

styleSheet.cssRules[0].conditionText
// "screen and (min-width: 900px)"
```

#### window.matchMedia()

##### 基本用法

`window.matchMedia`方法用来将 CSS 的[`MediaQuery`](https://developer.mozilla.org/en-US/docs/DOM/Using_media_queries_from_code)条件语句，转换成一个 MediaQueryList 实例。

```
var mdl = window.matchMedia('(min-width: 400px)');
mdl instanceof MediaQueryList // true
```

注意，如果参数不是有效的`MediaQuery`条件语句，`window.matchMedia`不会报错，依然返回的一个 MediaQueryList 实例。

```
window.matchMedia('bad string') instanceof MediaQueryList // true
```

##### MediaQueryList 接口的实例属性

##### MediaQueryList.media.prototype.media

该属性返回一个字符串，表示对应的MediaQuery条件语句。

##### MediaQueryList.media.prototype.matches

该属性返回一个布尔值，表示当前页面是否符合指定的MediaQuery条件语句。

```js
if (window.matchMedia('(min-width: 400px)').matches) {
  /* 当前视口不小于 400 像素 */
} else {
  /* 当前视口小于 400 像素 */
}
```

下面的例子根据`mediaQuery`是否匹配当前环境，加载相应的 CSS 样式表。

```
var result = window.matchMedia("(max-width: 700px)");

if (result.matches){
  var linkElm = document.createElement('link');
  linkElm.setAttribute('rel', 'stylesheet');
  linkElm.setAttribute('type', 'text/css');
  linkElm.setAttribute('href', 'small.css');

  document.head.appendChild(linkElm);
}
```

##### MediaQueryList.media.prototype.onchange

如果 MediaQuery 条件语句的适配环境发生变化，会触发`change`事件。`MediaQueryList.onchange`属性用来指定`change`事件的监听函数。该函数的参数是`change`事件对象（MediaQueryListEvent 实例），该对象与 MediaQueryList 实例类似，也有`media`和`matches`属性。

```
var mql = window.matchMedia('(max-width: 600px)');

mql.onchange = function(e) {
  if (e.matches) {
    /* 视口不超过 600 像素 */
  } else {
    /* 视口超过 600 像素 */
  }
}
```

上面代码中，`change`事件发生后，存在两种可能。一种是显示宽度从700像素以上变为以下，另一种是从700像素以下变为以上，所以在监听函数内部要判断一下当前是哪一种情况。

##### MediaQueryList接口的实例方法

MediaQueryList 实例有两个方法`MediaQueryList.addListener()`和`MediaQueryList.removeListener()`，用来为`change`事件添加或撤销监听函数。

```
var mql = window.matchMedia('(max-width: 600px)');

// 指定监听函数
mql.addListener(mqCallback);

// 撤销监听函数
mql.removeListener(mqCallback);

function mqCallback(e) {
  if (e.matches) {
    /* 视口不超过 600 像素 */
  } else {
    /* 视口超过 600 像素 */
  }
}
```

#### CSS事件

##### transitionEnd事件

CSS的过渡效果(transition)结束后，会触发transitionEnd事件。该事件的事件对象上定义了如下属性：

* `Event.propertyName`属性：该属性表示发生过渡效果的CSS属性名
* `Event.elapsedTime`属性：该属性表示过渡效果持续的秒数，不包含`transition-delay`属性时间 
* `Event.pseudoElement`属性：该属性表示如果过渡效果发生在伪元素上，就会返回该伪元素的名称(以`::`开头)，如果没有发生在伪元素上，则返回一个空字符串。

##### animationstart事件，animationend事件，animationiteration事件

CSS动画过程中触发以下三个事件：

- animationstart事件，该事件会在动画开始时触发。
- animationend事件，该事件会在动画结束时触发。
- animationiteration事件，该事件会在开始新一轮动画循环时触发。如果`animation-iteration-count`属性等于1，即只播放一轮的CSS动画，那么就不会触发animationiteration事件。

这三个事件的事件对象上都定义了以下两个属性：

* `Event.animationName`属性：该属性返回产生动画效果的CSS属性名
* `Event.elapsedTime`属性：该属性返回动画已经运行的秒数。注意：对于animationstart事件来说，该属性的属性值为0，除非`animation-delay`属性为负值。

#### Mutation Observer

Mutation Observer API 用来监视 DOM 变动。DOM 的任何变动，比如节点的增减、属性的变动、文本内容的变动，这个 API 都可以得到通知。

概念上，它很接近事件，可以理解为 DOM 发生变动就会触发 Mutation Observer 事件。但是，它与事件有一个本质不同：事件是同步触发，也就是说，DOM 的变动立刻会触发相应的事件；Mutation Observer 则是异步触发，DOM 的变动并不会马上触发，而是要等到当前所有 DOM 操作都结束才触发。

这样设计是为了应付 DOM 变动频繁的特点。举例来说，如果文档中连续插入1000个`<p>`元素，就会连续触发1000个插入事件，执行每个事件的回调函数，这很可能造成浏览器的卡顿；而 Mutation Observer 完全不同，只在1000个段落都插入结束后才会触发，而且只触发一次。

Mutation Observer 有以下特点。

- 它等待所有脚本任务完成后，才会运行（即异步触发方式）。
- 它把 DOM 变动记录封装成一个数组进行处理，而不是一条条个别处理 DOM 变动。
- 它既可以观察 DOM 的所有类型变动，也可以指定只观察某一类变动。Mutation Observer API 用来监视 DOM 变动。DOM 的任何变动，比如节点的增减、属性的变动、文本内容的变动，这个 API 都可以得到通知。

##### 构造函数

使用时，首先使用`MutationObserver`构造函数，新建一个观察器实例，同时指定这个实例的回调函数。

```
var observer = new MutationObserver(callback);
```

上面代码中的回调函数，会在每次 DOM 变动后调用。该回调函数接受两个参数，第一个是变动数组，第二个是观察器实例，下面是一个例子。

```
var observer = new MutationObserver(function (mutations, observer) {
  mutations.forEach(function(mutation) {
    console.log(mutation);
  });
});
```

##### 实例方法

##### observe()

该方法用来启动监听，它接受两个参数。

* 第一个参数：所要观察的DOM节点
* 第二个参数：一个配置对象，指定所要观察的特定变动

```js
var article = document.querySelector('article');

var  options = {
  'childList': true,
  'attributes':true
} ;

observer.observe(article, options);
```

上面代码中，`observe`方法接受两个参数，第一个是所要观察的DOM元素是`article`，第二个是所要观察的变动类型（子节点变动和属性变动）。

观察器所能观察的 DOM 变动类型（即上面代码的`options`对象），有以下几种。

- **childList**：子节点的变动（指新增，删除或者更改）。
- **attributes**：属性的变动。
- **characterData**：节点内容或节点文本的变动。

想要观察哪一种变动类型，就在`option`对象中指定它的值为`true`。需要注意的是，必须同时指定`childList`、`attributes`和`characterData`中的一种或多种，若未均指定将报错。

除了变动类型，`options`对象还可以设定以下属性：

- `subtree`：布尔值，表示是否将该观察器应用于该节点的所有后代节点。
- `attributeOldValue`：布尔值，表示观察`attributes`变动时，是否需要记录变动前的属性值。
- `characterDataOldValue`：布尔值，表示观察`characterData`变动时，是否需要记录变动前的值。
- `attributeFilter`：数组，表示需要观察的特定属性（比如`['class','src']`）。

```
// 开始监听文档根节点（即<html>标签）的变动
mutationObserver.observe(document.documentElement, {
  attributes: true,
  characterData: true,
  childList: true,
  subtree: true,
  attributeOldValue: true,
  characterDataOldValue: true
});
```

对一个节点添加观察器，就像使用`addEventListener`方法一样，多次添加同一个观察器是无效的，回调函数依然只会触发一次。但是，如果指定不同的`options`对象，就会被当作两个不同的观察器。

下面的例子是观察新增的子节点。

```
var insertedNodes = [];
var observer = new MutationObserver(function(mutations) {
  mutations.forEach(function(mutation) {
    for (var i = 0; i < mutation.addedNodes.length; i++)
      insertedNodes.push(mutation.addedNodes[i]);
  })
});
observer.observe(document, { childList: true });
console.log(insertedNodes);
```

##### disconnect()，takeRecords()

`disconnect`方法用来停止观察。调用该方法后，DOM 再发生变动，也不会触发观察器。

```
observer.disconnect();
```

`takeRecords`方法用来清除变动记录，即不再处理未处理的变动。该方法返回变动记录的数组。

```
observer.takeRecords();
```

下面是一个例子。

```
// 保存所有没有被观察器处理的变动
var changes = mutationObserver.takeRecords();

// 停止观察
mutationObserver.disconnect();
```

##### MutationRecord对象

DOM 每次发生变化，就会生成一条变动记录（MutationRecord 实例）。该实例包含了与变动相关的所有信息。Mutation Observer 处理的就是一个个`MutationRecord`实例所组成的数组。

`MutationRecord`对象包含了DOM的相关信息，有如下属性：

- `type`：观察的变动类型（`attribute`、`characterData`或者`childList`）。
- `target`：发生变动的DOM节点。
- `addedNodes`：新增的DOM节点。
- `removedNodes`：删除的DOM节点。
- `previousSibling`：前一个同级节点，如果没有则返回`null`。
- `nextSibling`：下一个同级节点，如果没有则返回`null`。
- `attributeName`：发生变动的属性。如果设置了`attributeFilter`，则只返回预先指定的属性。
- `oldValue`：变动前的值。这个属性只对`attribute`和`characterData`变动有效，如果发生`childList`变动，则返回`null`。

##### 应用实例

##### 子元素的变动

下面的例子说明如何读取变动记录。

```
var callback = function (records){
  records.map(function(record){
    console.log('Mutation type: ' + record.type);
    console.log('Mutation target: ' + record.target);
  });
};

var mo = new MutationObserver(callback);

var option = {
  'childList': true,
  'subtree': true
};

mo.observe(document.body, option);
```

上面代码的观察器，观察`<body>`的所有下级节点（`childList`表示观察子节点，`subtree`表示观察后代节点）的变动。回调函数会在控制台显示所有变动的类型和目标节点。

##### 属性的变动

下面的例子说明如何追踪属性的变动。

```
var callback = function (records) {
  records.map(function (record) {
    console.log('Previous attribute value: ' + record.oldValue);
  });
};

var mo = new MutationObserver(callback);

var element = document.getElementById('#my_element');

var options = {
  'attributes': true,
  'attributeOldValue': true
}

mo.observe(element, options);
```

上面代码先设定追踪属性变动（`'attributes': true`），然后设定记录变动前的值。实际发生变动时，会将变动前的值显示在控制台。

##### 取代DOMContentLoaded事件

网页加载的时候，DOM 节点的生成会产生变动记录，因此只要观察 DOM 的变动，就能在第一时间触发相关事件，因此也就没有必要使用`DOMContentLoaded`事件。

```
var observer = new MutationObserver(callback);
observer.observe(document.documentElement, {
  childList: true,
  subtree: true
});
```

上面代码中，监听`document.documentElement`（即HTML节点）的子节点的变动，`subtree`属性指定监听还包括后代节点。因此，任意一个网页元素一旦生成，就能立刻被监听到。

下面的代码，使用`MutationObserver`对象封装一个监听 DOM 生成的函数。

```
(function(win){
  'use strict';

  var listeners = [];
  var doc = win.document;
  var MutationObserver = win.MutationObserver || win.WebKitMutationObserver;
  var observer;

  function ready(selector, fn){
    // 储存选择器和回调函数
    listeners.push({
      selector: selector,
      fn: fn
    });
    if(!observer){
      // 监听document变化
      observer = new MutationObserver(check);
      observer.observe(doc.documentElement, {
        childList: true,
        subtree: true
      });
    }
    // 检查该节点是否已经在DOM中
    check();
  }

  function check(){
  // 检查是否匹配已储存的节点
    for(var i = 0; i < listeners.length; i++){
      var listener = listeners[i];
      // 检查指定节点是否有匹配
      var elements = doc.querySelectorAll(listener.selector);
      for(var j = 0; j < elements.length; j++){
        var element = elements[j];
        // 确保回调函数只会对该元素调用一次
        if(!element.ready){
          element.ready = true;
          // 对该节点调用回调函数
          listener.fn.call(element, element);
        }
      }
    }
  }

  // 对外暴露ready
  win.ready = ready;

})(this);

ready('.foo', function(element){
  // ...
});
```