## HTML

### 区别

* XML
* HTML
* XHTML
* HTML5

**XML**

常作为数据传输的载体，语法规则如下：

* 标签必须闭合
* 标签区分大小写
* 标签必须正确嵌套
* 必须有根元素
* 属性值必须加引号

**HTML**

超文本标记语言，通常是指`4.01`版本，从IE6开始兼容。

**XHTML**

`HTML`在语法定义上松散，为了代码维护，引入了语法更加严格的XHTML，具体如下：

* 标签必须闭合
* 标签和属性必须小写
* 属性值加引号
* `id`属性替代`name`属性

**HTML5**

`HTML`的下一个版本，在语法上有如下变化：

* 简化了文档声明
* 标签区分大小写
* 属性可以不加引号
* 部分属性值可以省略

这些属性值可以省略的属性如下：

| 省略形式     | 等价于                  |
| -------- | -------------------- |
| checked  | checked="true"       |
| readonly | readonly="readonly"  |
| defer    | defer="defer"        |
| ismap    | ismap="ismap"        |
| nohref   | nohref="nohref"      |
| noshade  | noshade="noshade"    |
| nowrap   | nowrap="nowrap"      |
| selected | selected="selected"  |
| multiple | multiple="multipled" |
| disabled | disabled="disabled"  |
| noresize | noresize="disabled"  |

`HTML5`还新增了一些标签，如何`header`，`footer`等。

> 当然还废弃了一些样式和行为标签，因为不符合结构和样式与行为分离原则，这些标签有：`big`，`center`，`s`，`u`等等。

当然，最让人兴奋的是新增了创建现代应用的一些技术，如`Canvas`，`Web`存储等。

> `HTML`不是编程语言，而是一种声明式的定义内容结构的标记语言。

### 文档声明

浏览器有如下两种渲染方式：

* `BackCompat`：怪异模式，浏览器不会按照`W3C`标准去渲染页面
* `CSS1Compat`：标准模式，浏览器会按照标准去渲染页面

如果你的页面没有进行文档声明，浏览器就会按照怪异模式去渲染，每个浏览器的怪异模式不同，渲染出来的页面也就不同。

### HTML属性

**lang属性**

用于设置页面语言，对页面渲染没有任何影响。

```html
<html lang="en"></html>
```

**id和class属性**

* `id`属性具有唯一性，一般页面关键结构才使用
* `class`属性常常用于抽取公共样式来减少代码重复

注意：`id`属性会被搜索引擎识别，不能滥用。

### HTML标签

**meta**

`meta`标签可以用来设置文档编码。

```html
<meta charset="UTF-8">
```

常见的编码有：

* `UTF-8`：国际通用
* `GB2312`：简体中文
* `BIG5`：繁体中文
* `GBK`：所有中文字符

`meta`标签还可以设置一些`SEO`信息。

```html
<meta name="Keywords" content="网易,游戏,新闻" />
<meta name="Description" content="网易是中国领先的互联网技术公司" />
<meta name="Author" content="ugu">
```

**a**

`a`标签的target属性默认为`_self`，即在当前窗口打开链接。

```html
<a href="_target"></a>
```

该标签还可以用来下载文件，但是不建议这样使用。

```html
<a href="web.txt">右击另存为即可下载文件</a>
```

**img**

`img`标签用来显示不同格式的图片，区别如下：

* 如果需要色彩还原度高，建议使用`JPG`
* 如果需要支持动画，建议使用`GIF`
* 如果需要支持透明和无损压缩，建议使用`PNG`

> `PNG`格式的图片在传输图像的过程中，会先将整个图像轮廓显示出来，然后逐步提高分辨率，这种从模糊到清晰的过程具有良好的用户体验。

`img`标签的`alt`属性用于图片描述，当图片因为某种原因无法显示，就会显示`alt`属性的文本。

> `alt`属性会被搜索引擎赋予一定权重，所以必须定义该属性。

`img`标签还可以使用`title`属性进行图片描述，当鼠标移入会显示定义的图片描述。

```html
<img src='1.png' title="图片1" alt="图片1" />
```

`HTML5`中还新增了`figure`标签和`figcaption`标签用于指定图注。

```html
<figure>
	<img src="" alt="" />
  	<figcaption></figcaption>
</figure>	
```

**textarea**

`textarea`标签用来定义多行文本输入域，它有`cols`和`rows`属性，分别用来设置每行和每列的字符数。

注意：在不同的浏览器，这个属性有细微差别，所以在实际开发中，通常使用`width`和`height`属性指定它的尺寸，并且使用`overflow:auto`让其滚动自适应。

**button**

`button`标签可以用来实现复杂按钮，而`input`标签是自闭合标签则无法实现。

```html
<button><img src="demo.png" /></button>
<!--这是一个图片按钮-->
```

**label**

`label`标签用于将表单成员和说明下文本关联，该标签上的`for`属性和表单成员的`id`属性一旦关联，那么`label`标签就有如下特点：

* 增强鼠标易用性
* 绑定`label`和表单成员

```html
<label for="man"><input type="radio" name="sex" id="man">man</label>
<label for="woman"><input type="radio" name="sex" id="woman">woman</label>
<!--name属性要设置一样，这样才能保证只能单选其中一个-->
```

默认情况下，`label`标签中的文本和表单元素不是垂直对齐的，而是以基线对齐，可以通过`vertial-align`来调整对齐。

**submit**

`submit`按钮必须定义在`form`标签内，才能将表单数据提交到服务器上。

> 该标签有默认行为。

**dl**

`dl`标签用来自定义列表，`dt`标签用来定义列表标题，`dd`标签用来定义列表描述。

```html
<dl>
	<dt>南京</dt>
  	<dd>江苏省的省会，六朝古都。</dd>
  	<dd>发展均衡，但是信息产业不够发达。</dd>
</dl>
```

**br**

`W3C`规定`br`标签只能用于`p`标签中文本的换行，不能在其它地方使用。

**del和ins**

`del`和`ins`标签分别定义被删除的文本和被更新的文本。

```html
<del>原价：3000</del>	
<ins>现价：2000</ins>	
```

> 这个两个标签有默认样式。

**noscript**

用于识别`script`标签但是无法支持其中的脚本的浏览器。

```html
<noscript>Your browser does not support JavaScript</noscript>
```

### 语义化

#### 标题语义化

搜索引擎会赋予`h1~h4`标签一定的权重，`h5`和`h6`标签和普通标签差不多，所以使用不是很多。

使用标题标签有如下注意点：

* 一个页面只能有一个`h1`标签
* `h1~h6`不要断层使用
* 不要用来定义样式
* 不要使用`div`来替代标题标签，会丢失权重

#### 图片语义化

* 如果图片需要被搜索引擎识别，推荐使用`img`标签
* 如果图片仅仅起到修饰作用，推荐使用背景图片

#### 表格语义化

* `caption`：用于定义表格标题
* `thead`：用于定义表头
* `tbody`：用于定义表身
* `tfoot`：用于定义表脚

这些标签不一样全部使用，像`caption`和`tfoot`就很少使用。

> 如果不写`tbody`标签，`table`会自动生成。

#### 表单语义化

* `fieldset`：用于定义表单元素分组
* `legend`：用于定义分组表单的标题

```html
<fieldset>
	<legend>表单组标题</legend>
  	......
</fieldset>
```

分组的好处是可以通过`fieldset`标签的`disabled`属性来禁用整组的表单成员。

#### 语义化意义

* 网页结构合理
* 利于`SEO`优化
* 对视障用户友好
* 代码可读性好

总结来说：对开发者和机器都友好。

关于语义化标签还有如下几点需要注意：

* 尽量少使用无语义的`div`和`span`标签
* 不要使用样式标签定义样式
* 使用`strong`标签和`em`标签强调文本，而不是`b`标签和`i`标签

一般来说，去掉样式表可以判断当前文档是否具有语义化。

## CSS

### 简介

`CSS`，又称层叠样式表是一种定义网页样式的标记语言，它不需要编译，直接由浏览器解释执行。

### 历史

- `CSS`最早被提议是在`1994`年
- 最早被浏览器支持是`1996`年
- `1996`年`12`月，`W3C`正式推出了`CSS1`。该版本中定义了颜色，字体，外边距，边框等基本样式
- `1998`年`5`月， `W3C`正式推出了`CSS2`。该版本中新增了绝对定位等高级特性
- 不久，W3C推出`CSS2.1`版本。该版本澄清并更正了`CSS2`，删除了一些浏览器厂商从未实现的功能
- 目前，所有主流浏览器都支持`CSS2.1`(`IE8`可能还有一些遗漏)，并且也是`W3C`现在正在推荐使用的
- `CSS3`现在还处于开发中，它被分为多个专业化模块来分别通过标准进程

另外，浏览器厂商会在属性前添加私有前缀来实现非标准`CSS`属性，甚至用这种方式来实现将来可能标准化的属性，直到该属性成为标准后才移除前缀。

### 样式引入

可以通过如下四种方式引入样式：

* 行内样式
* 内联样式
* 外联样式
* 导入样式

其中，通过`@import`导入样式并不常用，因为导入样式表在浏览器渲染过程中先加载`HTML`后加载`CSS`，这会带来极差的用户体验。

### 规范

**命名规范**

如果类名有多个字符，建议使用`-`连接，并且为了避免类名的重复，一般使用父元素的类作为前缀。

```html
<div class="columns">
	<h3 class="columns-title"></h3>
    <div class="columns-content"></div>
</div>
```

**书写规范**

推荐按照如下顺序书写`CSS`属性：

1. 布局属性：`display`，`position`，`float`，`clear`等属性
2. 盒模型属性：`width`，`height`，`padding`，`margin`，`border`，`overflow`等属性
3. 文本属性：`font`，`line-height`，`text-align`，`text-indent`，`vertical-align`等属性
4. 装饰属性：`color`，`background-color`，`opacity`，`cursor`等属性
5. 其它属性：`content`，`list-style`，`quotes`等属性

对于`CSS`功能代码不需要按照这个顺序书写，相反应该写在一起，增强代码的可读性。

**注释规范**

在开发环境可以通过`/**/`来书写注释，但是在线上环境，应该删除`CSS`注释以减少文件大小。

### 特性

#### 继承性

`CSS`中并非所有属性都具有继承性，能够继承的属性可以分为如下三类：

* 文本属性，如`font-family`，`font-size`，`font-style`，`font-weight`，`font`，`line-height`，`text-align`，`text-indent`，`word-spacing`等
* 列表属性，如`list-style-image`，`list-style-position`，`list-style-type`，`list-style`等

* 颜色属性，只有`color`属性

> `a`标签自身定义了`color`属性，并且优先级比继承的`color`属性要高。如果需要改变`a`标签的字体颜色，需要选中`a`标签后重新设置`color`属性。

#### 层叠性

说到层叠性就必须提到优先级，因此只有通过优先级才能判断元素到底应该应用何种属性。

##### 引用方式冲突

行内样式的优先级最高，内联样式和外联样式的优先级相同，如果它们同时存在，那么后出现的优先级高。

##### 继承方式冲突

对于继承来的属性，选择器离目标元素近的优先级高。

```html
<style>
	.one {
    	color: red;
	}
  	.two {
    	color: blue;
  	}
</style>
<div class="one">
    <div class="two">
    	<div>我是什么颜色</div>
      	<!--蓝色-->
    </div>  
</div>
```

##### 指定样式冲突

如果不是继承属性，也就是说选择器选中该目标元素，那么此时就需要比较选择器权重，权重高的优先级高、如果权重相同，那么后出现的优先级高。具体选择器权重如下：

* 行内样式：`1000`
* `id`选择器：`100`
* `class`选择器，伪类选择器，属性选择器：`10`
* 元素选择器，伪元素选择器：`1`
* 通配符选择器：`0`

注意：权重计算只有在选择器选中该元素时才会生效，对于继承而来的属性权重为`0`。

##### !important

`!important`可以用来提高样式规则的权重，使其权重变为无穷大。此时，如果想要层叠这条样式，可以使用如下两种方式：

* 在目标选择器后使用相同的选择器和样式规则，并且添加`!important`。这是因为选择器权重一样，后出现的优先级高。
* 使用权重更高的选择器，并且在样式规则后添加`!important`。 

#### 总结

关于优先级的比较可以总结为如下两条准则：

* 若选择器选中元素，那么权重高的优先级高。如果权重相同。那么后出现的优先级高。
* 若选择器未选中元素，那么继承而来的属性权重为`0`，所以选择器离目标元素近的优先级高。

### 样式重置

浏览器会为大多数元素定义默认样式，例如：

* `p`元素有上下边距
* `strong`元素有字体加粗样式
* `em`元素有字体倾斜样式
* `ul`元素有缩进样式

但是不同浏览器定义的默认样式也有所不同，为了消除显示差异性，往往需要样式重置(`CSSReset`)。在使用`CSSRese`t的过程中有如下两点需要注意：

* 重置样式表必须放在所有样式表最前面
* 重置样式表中的规则需要根据需求不同而重新定义

### 单位

`CSS`单位可以分为绝对单位和相对单位，而`CSS`更多的是使用相对单位来定义尺寸，常见的相对单位有：

* `px`：表示像素
* `%`：表示百分比
* `em`：表示当前元素字体大小
* `rem`：表示当前根元素字体大小
* `vw`：表示`1%`视口(浏览器可视区域)的宽度
* `vh`：表示`1%`视口的高度

**像素**

像素是指一张图片中最小的点或者计算机屏幕中最小的点。例如：当前计算机屏幕的分辨率是`1024*768`，则表示该屏幕宽度为`1024`个像素点，高度为`768`个像素点。

> 像素通常被当做绝对单位，但是严格来讲，由于屏幕材质不同，`1px`大小也是有所不同，因此像素实际上是相对单位。

**百分比**

`CSS`中的一些属性还支持百分比作为单位，它是一种相对于包含块的计量单位，这些属性有：

* `width`，`height`和`font-size`属性的百分比是相对包含块的相同属性的值来计算
* `line-height`属性的百分比是相对于当前元素的`font-size`属性来计算
* `vertical-align`属性的百分比是相对于当前元素的`line-height`来计算

>  当`margin`/`padding`取形式为`百分比`的值时，无论是`left`/`right`，还是`top/bottom`，都是以包含块的`width`为参照物的， 但这只发生在默认的`writing-mode: horizontal-tb;`和 `direction: ltr;`的情况下。 

```html
<style>
  div{
    width:200px;
    height:200px;
    font-size:30px;
  }
  p{
    width:50%;
    height:50%;
    font-size:50%;
    line-height:50%;
  }
</style>
<div id="app">
  <p id="content">hello</p>
</div>
<script>
	let content = document.getElementById("content")
    console.log(window.getComputedStyle(p).width)			//"100px"
    console.log(window.getComputedStyle(p).height)			//"100px"
    console.log(window.getComputedStyle(p).fontSize)		//"15px"
    console.log(window.getComputedStyle(p).lineHeight)		//"7.5px"
  	console.log(window.getComputedStyle(p).verticalAlign)	//"50%"
</script>
```

**em**

`em`单位表示当前元素字体大小。在实际开发中，使用它作为统一单位可以增加页面的扩展性，因为只需要改变一处字体大小，就能作用于整个页面。

```css
body {font-size: 62.5%;}		/* 62.5%*16px=10px */
```

**rem**

`rem`单位是`CSS3`中新增的单位，表示当前根元素字体大小，大部分浏览器的根元素的默认字体大小是`16px`。

> 在chrome浏览器下，中文的最小字体大小是`12px`，英文是`10px`，当然你可以通过chrome的设置来解除这一个限制。

### 选择器

常见的层次选择器如下：

* M N：后代选择器，即选择所有后代元素
* M > N：子代选择器，即选择所有子代元素
* M~N：兄弟选择器，即选择M元素后的所有同级N元素
* M + N：相邻选择器，即选择M元素的后一个同级N元素

使用选择器有如下注意事项：

* 禁止使用通配符选择器
* 禁止在id选择器和class选择器之前加上元素名
* 选择器最好不要超过三层，并且靠右的选择器尽量精确
* 避免使用后代选择器，少用子代选择器

### 文档流

`W3C`规定处于文档流(`normal flow`，译为正常流或常规流好点)中的元素具有如下特点：

> Boxes in the normal flow belong to a formatting context, which may be block or inline, but not both simultaneously.  Block-level boxes participate in a block formatting context.  Inline-level boxes participate in an inline formatting context. 

翻译过来就是文档流中的盒子属于块级或者行内格式化上下文，块级盒子参与块格式化上下文的计算，行内盒子参与行内格式化上下文的计算。

从实际工作经验中可以总结为如下：

* 空白折叠现象
* 高矮不齐，底边对齐。例如：图片和文字或图片和图片默认是基线对齐
* 自动换行

### display

`display`属性可以显式设置元素的表现形式，取值如下：

* `inline`：以行内元素形式显示
* `block`：以块级元素形式显示
* `inline-block`：以行内块元素形式显示，例如：`img`元素和`input`元素
* `table`：以表格形式显示，类似table元素
* `table-row`：以表格行形式显示，类似tr元素
* `table-cell`：以单元格形式显示，类似td元素
* `none`：隐藏元素

#### 行内元素

行内元素具有如下特性：

- 设置宽高无效
- 设置上下边距无效，左右边距有效

#### 行内块元素

`display:inline-block`属性可以将元素显式设置为行内块元素。

一个常见问题是带有空白字符相隔的行内块元素之间会有间隙，有如下方法解决：

* 设置父元素字体大小为`0`，当然子元素要重新设置字体大小
* 设置负`margin`
* 设置`letter-spacing`和`word-spacing`属性，通过这两个属性减少空白字符和其它字符之间的间距来消除行内块元素之间的间距
* 格式化`html`文档

```html
<style>
div {
	letter-spacing: -5px;
  	/*或者word-spacing: -5px;*/
}
span {
	display: inline-block;
	width: 100px;
	height: 50px;
	text-align: center;
	line-height: 50px;
	letter-spacing: 0;
    /*子元素重新设置letter-spacing属性，而不是从父元素继承*/
  	/*或者word-spacing: 0;*/
}
span:first-child {background-color: red;}
span:last-child {background-color: blue;}
</style>
<div>
	<span>1</span>
	<span>2</span>
</div>
```

在使用`inline-block`进行布局时，你需要注意：

* `vertical-align`属性会影响到`inline-block`元素
* 你需要设置每一列的宽度

#### 隐藏元素

有如下三种方式隐藏元素：

* 设置`display:none`属性完全隐藏元素，并且该元素不会占据原来的空间。注意：不推荐使用这种方式隐藏页面中`SEO`关键部分。
* 设置`visibility:hidden`属性完全隐藏元素，但是该元素还会占据原来位置。
* 设置`opacity:0`属性间接隐藏元素，该元素同样会占据原来位置。

另外，一些特殊元素的`display`属性默认是`none`，例如`script`元素。

#### 单元格元素

`display:table-cell`属性可以显式设置元素为单元格元素，表现和`td`元素类似，其外边距会失效，但是内边距不会失效。

> 设置该属性的元素会被浮动和绝对定位破坏（因为同一个元素同时设置`display:block`和`display:table-cell`会发生冲突），所以不要将它们混合使用。 

`IE8`之前的浏览器不支持该属性，但是单元格元素在实际开发中应用十分广泛。例如：

**元素垂直居中于某个不固定尺寸元素中**

```css
div {
  display: table-cell;
  width: 500px;
  height: 500px;
  border: 1px solid red;
  vertical-align: middle;
}
img {
  vertical-align: middle; 
  /*去掉img下面的3px留白，使用负margin也可以*/
}
```

**自适应等高布局**

其原理是同一行的`td`元素高度始终相等，由`td`元素的最大高度所决定。

```html
<style>
	.wrapper {
		display: table-row;
	}
	.left {
		display: table-cell;
		width: 400px;
		vertical-align: middle;
		text-align: center;
		border: 1px solid red;
	}
	.left img {vertical-align: middle;}
	.right {
		display: table-cell;
		width: 600px;
		border: 1px solid red;
		padding: 20px;
	}
</style>
<div class="wrapper">
	<div class="left">
		<img src="avatar.jpg" alt="">
	</div>
	<div class="right">我是文字文字我是文字文字是文字文字我是文字文字我是文字文字我是文字文字是文字文字我是文字文字我是文字文字我是文字文字是文字文字我是文字文字我是文字文字我是文字文字是文字文字我是文字</div>
</div>
```

**自动平均划分元素宽度**

```html
<style>
	ul {
		display: table;
		width: 300px;
		list-style: none;
		margin: 100px auto;
	}
	ul li {
		display: table-cell;
		height: 50px;
		line-height: 50px;
		text-align: center;
		color: white;
	}
	ul li:nth-child(odd) {background-color: red;}
	ul li:nth-child(even) {background-color: blue;}
</style>
<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
</ul>
```

#### 总结

还有很多更有意思的`display`值，例如`list-item`，`run-in`等等，具体请看[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display)。

### 盒子模型

`CSS`中，页面中所有元素都可以看作是一个由外边距，边框，内边距和内容组成的盒子模型。

盒子模型可以分为`IE`盒模型和`W3C`盒模型，其中`W3C`盒模型规定`width`和`height`属性是内容的尺寸，而`IE`盒模型的`width`和`height`属性是内容+内边距+边框的尺寸。

`CSS3`中，新增`box-sizing`属性来指定元素使用哪种盒模型进行渲染。

```css
div {
    box-sizing: content-box;	/*内容盒模型，W3C盒模型*/
  	box-sizing: border-box;		/*边框盒模型，IE盒模型*/
  	/*其中，content-box是默认值*/
}
```

通过`JavaScript`获取盒模型对应的宽高：

* `dom.style.width/height`。缺点：只能取到通过行内样式设置的`width`和`height`属性，并且带有单位
* `dom.currentStyle.width/height || window.getComputedStyle(dom).width/height`。前者是`IE`支持的方法，后者是`W3C`支持的方法。注意：该方法不仅能取到通过行内样式设置的宽高，还能拿到通过外联和内联样式设置的宽高
* `dom.getBoundingClientRect().width/height`

### CSS属性

#### 盒模型属性

**宽高**

```css
div{
    width: 600px;
    height: 100px;
    margin: 0 auto;
}
```

上述代码在小窗口下会出现横向滚动条，当然你可以通过`max-width`来设置当视口宽度小于`600px`显示当前视口宽度的`100%`。

**边框**

设置`border:0`和`border:none`样式都可以让元素在显示效果上没有边框，它们之间的区别如下：

* 性能差异：前者会让浏览器渲染出边框宽度为`0`的边框，也就是说依旧占据内存。而后者对于浏览器来说不会渲染边框，也就是说不会占据内存
* 兼容差异：前者会在所有浏览器中都不显示边框，而后者对于`IE6,7`中的`<input type="button">`元素无效

**外边距**

当处于**文档流**中的上下两个元素的垂直外边距相遇时，会发生外边距叠加现象，叠加后的外边距等于这两个元素外边距的最大值。这个现象会发生在同级元素，父子元素和空元素上，具体如下：

* 对于同级元素来说：当一个元素在另一个元素上面时，上面元素的下边距会和下面元素的上边距叠加
* 对于父子元素来说：当父元素没有设置边框或内边距时，父元素的垂直外边距会和子元素的垂直外边距叠加

```css
/* 父子元素 */
.container {
    margin-top: 100px;
    width: 300px;
    height: 300px;
    background-color: skyblue;
}

.box {
    margin-top: 100px;		/* 无效 */
    width: 100px;
    height: 100px;
    background-color: green;
}
```

对于外边距叠加还有如下两点补充：

* 水平外边距不会叠加
* 只有块级元素和行内块元素才会发生叠加现象

> 如果父元素空间不足会导致子元素右外边距失效，因为渲染引擎会优先保护左外边距。

另外，外边距是为数几个不多的可以设置负值的属性，这些属性还有`margin`，`letter-spacing`，`word-spacing`和`vertical-align`等。

负外边距技术常见的应用场景如下：

**带竖线分隔的横向列表**

分隔线最好使用边框模拟，而不是使用`|`字符。

```css
*{margin: 0;padding: 0;}
ul{
  list-style-type: none;
  margin: 100px 0 0 200px;
  width: 243px;
  overflow: hidden;
}
ul li {
  float: left;
  width: 60px;
  height: 40px;
  margin-left: -1px;
  border-left: 1px solid red;
  line-height: 40px;
  text-align: center;
}
ul li:hover{background-color: #ccc;}
```

**自适应两列布局**

```css
.left,.right {
	float: left;
  	height: 500px;
}
.left {
	width: 200px;
	background-color: red;
}
.right {
	width: 100%;
	margin-right: -200px;
	background-color: blue;
} 
```

#### 布局属性

**浮动**

浮动常用于页面布局，设置浮动的元素有如下特性：

* 浮动元素脱离文档流，也就是不再在文档中占用位置
* 浮动元素会一直向上飘浮，直到遇到父元素的边界或者其它浮动元素
* 浮动元素有字围效果
* 浮动元素若没有设置宽度，那么将是内容宽度
* 浮动元素以元素顶部对齐
* 浮动元素只影响后面元素

> 当一个元素浮动以后将会自动变为块级元素，尽管其原来是行内元素。
>

浮动给布局带来便利的同时，还会带来一些副作用。例如：父元素高度塌陷问题。此时，需要通过清除浮动来消除影响。常见的清除浮动方式如下：

* 给父元素设置高度。这种方式的缺点是把高度写死，不利于页面的可扩展性
* 给父元素设置浮动。这种方式的缺点会影响布局
* 给紧跟浮动元素的后面元素设置`clear:both`属性。这种方式的缺点是会导致`margin`失效或者增加一些额外标签
* 给父元素设置`overflow:hidden`属性，让其触发BFC。这种方式的缺点是存在一定的兼容性
* 给父元素设置`display:table`属性。这种方式的缺点是会造成盒模型发生变化，得不偿失。
* 使用伪元素清除浮动。这种方式的缺点是复用不当会造成代码量增加。补充：[那些年我们清除过的浮动](http://www.iyunlu.com/view/css-xhtml/55.html)。 

```css
.clearfix:after {
	content: "";
    /*使生成的元素以块级元素显示占满剩余空间*/
	display: block;	
    /*避免生成内容破坏原有布局的高度*/
	height: 0;
	/*使生成的内容不可见，并允许可能被生成内容盖住的内容可以进行点击和交互*/	
    visibility: hidden;
	overflow: hidden;
	clear: both
}
.clearfix {
  	*zoom: 1; 			/*IE6,7不支持:after，需要使用zoom:1触发hasLayout*/
}
.clearfix:before,.clearfix:after {
  content: "";			/*content属性无法通过&nbsp;添加空格，可以通过unicode编码添加*/
  display: table;		/*:before伪元素为了防止外边距叠加*/
}
.clearfix:after {
  clear: both;			/*For IE6,7 trigger hasLayout*/
}
.clearfix {
  zoom: 1;
}
/*display: table本身无法触发BFC，但是它会产生匿名框(anonymous boxes)，而匿名框中的display: table-cell可以触发BFC。简单来说，触发块级格式化上下文的是匿名框而不是display: table。所以通过display:table和display:table-cell创建的BFC效果是不一样的*/
```

**定位**

`position`属性可以设置元素的定位类型，常见的定位类型有静态定位，相对定位，绝对定位和固定定位，分别对应如下属性值：

* `static`：静态定位，表示元素按照文档流进行定位。注意：该属性值是默认值。
* `relative`：相对定位，表示元素按照其原来位置进行定位，并且原位置空间依然保留在文档流中，也就是说其他的元素的位置不会受该元素的影响发生位置改变来弥补它偏移后剩下的空隙。注意：设置相对定位的元素不会脱离文档流，并且可以使用`left`，`top`等定位属性进行定位。
* `absolute`：绝对定位，表示元素相对于最近已定位(除了静态定位)的祖先元素(会忽略其内边距)进行定位，如果不存在这样的元素，则相对于初始包含块（可以理解为视口）进行定位。注意：设置绝对定位的元素会脱离文档流，并且可以使用定位属性进行定位。
* `fixed`：固定定位，表示元素相对于初始包含块（可以理解为视口）进行定位。注意：设置固定定位的元素不会保留它原本在页面应有的位置，也就是脱离文档流，并且可以使用定位属性进行定位。
* `sticky`：粘性定位。从表现上可以看出，当元素在屏幕内，表现为`relativ`e，就要滚出显示器屏幕的时候，表现为`fixed`。

其中相对定位，绝对定位和固定定位既可以使用定位属性来进行位置移动，也可以使用`margin`来进行位置移动，但是不建议使用`margin`。另外，对于绝对定位和固定定位来说，必须设置定位属性，否则上面叙述的结论不成立。

对于绝对定位元素，可以使用`width`属性设置宽度，也可以同时使用`left`和`right`属性设置宽度，但是如果这两种方式同时存在，那么以`width`属性为准，并且`right`属性失效。同理，对于高度也是如此。

既然绝对定位和固定定位会脱离文档流，那么如果其父元素没有设置高度，也会造成父元素高度塌陷。但是此时高度塌陷不能使用`overflow:hidden`来解决。

另外，相对定位的`width`和`height`属性百分比是相对于去父元素的对应属性，而绝对定位是相对于其最近的已定位的祖先元素（没有则相对于初始包含块）的对应属性，固定定位则相对于初始包含块的对应的属性。

**z-index**

z-index属性可以设置当元素重叠时的显示顺序，浏览器会将属性值高的元素显示在上面。如果属性值相等，那么后出现的元素会显示在上面。

> 只有定位元素才拥有z-index属性，其中静态定位元素的属性值默认为`0`，其余定位元素的属性值默认为`1`。

另外，该属性的显示顺序规则只应用于兄弟元素，对于非兄弟元素，则是由其父元素的`z-index`属性值决定显示顺序。

#### 文本属性

**text-indent**

用于设置文本缩进，常用于文档中`logo`图片的优化。

**text-align**

用于设置文本水平对齐方式。注意：该属性只对文本，行内元素，行内块元素有效，并且在其父元素上定义。

另外，设置`margin: 0 auto`属性可以实现块级元素水平居中对齐，前提是该元素设置了width属性并且处于文档流中，并且在当前元素上定义。

**word-spacing**

`word-spacing`和`letter-spacing`属性是两个非常容易混淆的文本属性，具体区别如下：

* 前者表示单词之间的空白距离
* 后者表示每个字符之间的空白距离

**line-height**

该属性用于设置多行元素的空间量，如多行文本的间距。对于块级元素，它指定元素行盒`line boxes`的最小高度，对于非替代的`inline`元素，它用于计算行盒的高度。而替换元素没有行高。

> * 行盒：元素的每一行称为一条`line-box`，它是由这一行的许多`inline-box`组成
> * 替换元素和非替换元素：内容不受当前文档的样式的影响，例如`img`元素是根据图片具体内容进行渲染，常见的替换元素有`img`，`input`等；大部分元素都是非替换元素。

简单来说，行高是两条基线之间的距离，行距是上一行文字底线和下一行文字顶线之间的距离，半行距就是行距的一半，内容区域是一行文字的顶线到底线之间的距离。

`line-heigth`属性的不同单位表示还不同的含义，具体如下：

* 如果单位是`px`，那么行高就是指定值
* 如果单位是`em`，那么行高等于当前元素字体大小`*`设置值
* 如果单位是`%`，那么行高等于当前元素字体大小`*`设置值
* 如果没有单位，那么行高等于当前元素字体大小`*`设置值
* 如果单位是`normal`，那么行高是取决于浏览器，与元素字体有关

注意：虽然`em`，`%`以及纯数字在计算上没有差别，但是在应用元素上是有差别的。对于没有数字来说，所有可继承子元素会根据`font-siz`e重新计算行高，而对于`em`和`%`来说，当前元素根据`font-size`计算行高，然后继承给子元素。

> `line-height`常常用来设置单行文本垂直居中，如果是多行文本居中需要使用内边距调整。

另外，行高是会影响元素高度的，而字体大小的改变会影响行高。

**vertical-align**

* 用于定义周围的文本，行内元素和行内块元素相对于设置该属性的元素基线的垂直对齐方式
* 可以设置单元格元素中内容的垂直对齐方式，其中单元格元素是指设置了`display:table-cell`的元素。
* 仅对行内元素，行内块元素和单元格元素有效，对块元素无效，也就是说该元素不能与浮动和绝对定位同时使用

该属性允许指定如下关键字作为属性值：

* `top`：表示相对于顶线对齐
* `middle`：表示相对于中线对齐
* `baseline`：表示相对于基线对齐
* `bottom`：表示相对于底线对齐

还允许使用百分比值和负值，具体如下：

* `vertical-align:-2px`表示相对于当前元素基线向下偏移`2`像素。也就是说，如果是正值，则表示相对于基线向上偏移对应的像素对齐。如果是负数，则表示相对于基线向下偏移的像素对齐。


* `vertical-align: 50%`表示相对于当前元素的行高的`50%`对齐。也就是说，如果元素行高是`20`像素，那么该属性值就是`10`像素，则表示相对于基线向上偏移`10`像素对齐。

`vertical-align`属性有如下常见的应用场景，具体如下：

**清除图片底部`3px`空白**

```css
/*method 1*/
img {
  display:block; /*vertical-align不作用于块级元素*/
}
/*method 2*/
img {
  vertical-align: top/middle/bottom;
}
/*method 3*/
fu {
  line-height: 0;
}
/*method 4*/
fu {
  line-height: 1.5;
  font-size: 0; 
}
```

#### 其它属性

**overflow**

不同于`display:none`和`visibility:hidden`属性完全隐藏元素，`overflow`属性可以隐藏部分元素。该属性指定当内容超出容器元素大小时该如何显示，取值如下：

* `visible`：表示内容可以溢出并绘制在元素边框的外面，该属性值是默认值
* `hidden`：表示裁剪掉和隐藏溢出的内容
* `scroll`：表示元素一直显示水平和垂直滚动条。如果内容超出元素尺寸，则允许通过滚动查看额外的内容
* `auto`：表示滚动条只在内容超出元素尺寸时显示，而非一直显示

**clip**

`clip`属性指定了元素的裁剪区域，也用来只显示部分元素。在`CSS2`中，裁剪区域是矩形的，但是在未来`CSS`标准中可能允许其它形状的裁剪。

该属性不允许使用百分比作为单位，但是允许指定负值，表示裁剪区域超过元素指定的边框尺寸。如果指定auto关键字，则表示裁剪区域是元素边框。

```css
div {
    clip: rect(0px 100px 100px auto);	/*括号中数字表示裁剪矩形的边距，以距离上右下左为参考点*/
  	/*必须带上单位，即便值为0*/
}
```

**opacity**

大部分元素的背景是完全透明的，对于那些背景不是完全透明的元素，可以设置`background-color:transparent`属性来设置背景完全透明。而`opacity`属性可以设置元素背景的透明度，而不是全透明。

> 老版本`IE`不支持这个属性，使用`filter`属性来代替。

`opacity`属性除了会使元素背景透明之外，元素中内容也会跟着透明，因此`CSS3`中引入`RGBA`颜色来只让元素背景透明。

**cursor**

用来指定鼠标样式，取值如下：

* `pointer`：小手
* `default`：小白
* `move`：移动
* `text`：文本输入

> 当`button`元素设置宽高，默认的`pointer`就会变成`default`。
>

### CSSsprite

合理的`CSSsprite`可以减少`http`请求次数，提高页面访问速度。在使用雪碧图还有如下几点需要注意：

* 开发后期使用雪碧图，而不是前期。因为，小背景图的位置可能经常改变
* 有条理的使用雪碧图，将小背景图按照风格和大小进行分类
* 控制雪碧图的大小，最好不超过`200kb`，如果超出，可以再将其分为多个小的雪碧图

### 包含块

包含块(`containing block`)是视觉格式化模型的一个重要概念，可以把它理解为矩形，为其内部的元素提供一个参考，元素的尺寸和位置往往都是由该元素所在的包含块绝对。所以有时也会将其称为定位参考框或者定位坐标参考系。

一个元素的包含块定义如下：

* 浏览器选择根元素(`html`)作为初始包含块。注意：初始包含块和视口等大
* 文档流以及浮动的元素，其包含块是由最近的块级祖先元素盒子的内容边界(`content-box`)组成
* 绝对定位的元素，其包含块是由最近的非`static`定位的祖先元素的`padding-box`建立，默认是基于初始包含块
* 固定定位的元素，其包含块是由视口建立

### 层叠上下文

层叠上下文(`stacking context`)和层叠级别(`stacking level`)是`CSS`中两个重要概念。其中层叠上下文是`HTML`文档中一个三维概念。从前面提到到的`z-index`属性可以看出，虽然一个网页是平面的，但实际上是三维结构，除了`X`，`Y`轴之外，还有`Z`轴，而`Z`轴往往都是用来设定层的先后顺序。

层叠上下文和`BFC`类似，是可以创建出来的。也就是说，为元素设置特定的属性会创建层叠上下文出来。一般来说，只要元素具有如下其中一个条件都会创建一个新的层叠上下文：

* 根元素，并且由它创建的层叠上下文称之为根层叠上下文
* `z-index`不为`auto`的定位元素

简单来说就是，使用`z-index`属性可以为一个定位元素创建一个新的层叠上下文。有了层叠上下文之后，就可以根据层叠级别来决定其内部元素或背景的显示顺序。在同一个层叠上下文中，层叠级别从低到高排列如下：

* 背景和边框：也就是当前层叠上下文的背景和边框
* 负`z-index`：`z-index`为负值的内部元素
* 块盒子：文档流中的块级盒子(`block-level box`)
* 浮动盒子：非定位的浮动元素，也就是排除了相对定位的浮动元素
* 行内盒子：文档流中的行内盒子(`inline-level box`)
* `z-index`为`0`：`z-index`为0的内部元素
* 正`z-index`：`z-index`为正值的内部元素

另外，关于层叠性还有如下几点需要注意：

* 除了背景和边框是针对当前层叠上下文之外，其它都是针对当前层叠上下文内部元素
* `inline-block`元素不是块盒子，而是行内盒子
* 当前层叠上下文内部定位元素也可以设置`z-index`创建新的层叠上下文，也就是说层叠上下文可以嵌套，但是内部元素创建的层叠上下文依然受制于当前层叠上下文
* 背景和边框往往起装饰效果，所以层叠级别最低。浮动元素和块元素一般用于布局，而行内元素一般用于显示内容，所以行内元素层叠级别较高。

有了层叠上下文和层叠级别，就可以判断一个元素在Z轴上的堆叠顺序，具体如下：

* 同一个层叠上下文中，层叠级别大的内部元素显示在上面
* 同一个层叠上下文中，如果内部元素的层叠级别相同，那么后面的元素显示在上面
* 不同的层叠上下文中，由父元素的层叠级别决定显示顺序，与自身层叠级别无关。

### 格式上下文

在`CSS`中，页面中任何一个元素都可以看成一个盒子。并且在文档流中，盒子会参与一种格式上下文(`formatting context`)。这个盒子可能是块盒子(`block-level box`)，也可能使行内盒子(`inline-level box`)，但是不可能两者皆是。其中，块盒子参与`BFC`，行内盒子参与`IFC`。

格式上下文是指页面中的一块渲染区域，并且这个格式上下文有一套自身的渲染规则。格式上下文决定了其内部元素将如何定位，以及和其它元素之间的关系。另外，格式上下文可以分为两种：

* 块级格式化上下文(`Block Formatting Context`，`BFC`)
* 行级格式化上下文(`Inline Formatting Context`，`IFC`)

#### 盒子

盒子，又称`CSS`盒子，它是布局的基本单位。简单来说，一个页面是由多个盒子组成(参考前面提到的盒子模型)。而元素的类型是由`display`属性决定，不同类型的盒子也会参与不同的格式上下文。例如：设置`display:inline-block`属性的元素是行内块元素，这个元素会生成行内盒子，参与`IFC`。更具体的情况如下：

* 块盒子：`display`属性为`block`，`table`，`list-item`的元素会生成块盒子
* 行内盒子：`display`属性为`inline`，`inline-block`，`inline-table`的元素会生成行内盒子

另外，除了块盒子和行内盒子，CSS3还新增了`run-in box`盒子。这里不做介绍。

#### BFC

块级格式化上下文是一个独立的渲染区域，只有块盒子参与。`BFC`规定内部的块盒子如何布局，并且这个渲染区域与外界区域无关。另外，`W3C`规定，满足如下其中一个条件就会创建一个新的`BFC`：

* 根元素，也就是说默认情况下，一个页面中的所有元素都处在由根元素创建的`BFC`中
* `float`属性除了`none`之外的值
* `position`属性除了`static`和`relative`之外的值
* `overflow`属性除了`visible`之外的值
* `display`属性是`inline-block`，`table-caption`，`table-cell`，`flex`和`inline-flex`
* 触发`IE`的`hasLayout`属性

`W3C`规范还规定，`BFC`的特性可以分为如下两条：

* 在一个`BFC`中，盒子从顶端开始垂直一个接着一个地排列，两个相邻盒子之间的垂直间距由`margin`属性决定。在同一个`BFC`中，两个相邻块盒子之间垂直方向上的外边距会叠加
* 在一个`BFC`中，每一个盒子的左边界(`margin-left`)会紧贴容器的左边(`border-left`)，即使浮动元素也是如此

将这两条特性可以分开来理解，具体如下：

* 在一个`BFC`内部，盒子会在垂直方向上一个接着一个地排列
* 在一个`BFC`内部，相邻的`margin-top`和`margin-bottom`会叠加
* 在一个`BFC`内部，每一个元素的左外边界会紧贴着包含盒子的左边，即使存在浮动也是如此
* 在一个`BFC`内部，如果存在内部元素是一个新的`BFC`，并且存在内部元素是浮动元素。则该`BFC`的区域不会与`float`元素的区域重叠
* `BFC`就是页面上的一个隔离的盒子，该盒子内部的子元素不会影响到外面的元素
* 计算一个`BFC`的高度时，其内部浮动元素的高度也会参与计算

`BFC`的用途很多，具体如下：

* 创建`BFC`来避免垂直外边距叠加

* 创建`BFC`来清除浮动

* 创建`BFC`来避免因为浮动而引起的文字环绕

* 创建`BFC`来实现自适应两列布局


### hasLayout

`IE6,7`的显示引擎使用的是一个称谓布局(`layout`)的内部概念，它用来控制元素自身及其子元素的尺寸设置和定位。这些特性和`BFC`类似，因此可以简单将`hasLayout`看做是`IE5.5/6/7`中的`BFC`。

> 这个显示引擎存在很多缺陷，因此会导致很多显示`BUG`。

* 当`hasLayout`为`true`时，就是所谓的拥有布局时，相当于元素产生新的`BFC`，块级元素自己对自身内容进行组织和尺寸计算
* 当`hasLayout`为`false`时，就是所谓的不拥有布局时，相当于元素不产生新`BFC`，元素由其所属的包含块进行组织和尺寸计算

和产生新的`BFC`的特性一样，`hasLayout`无法通过`CSS`属性直接设置，而是通过某些`CSS`属性间接开启这一特性。不同的是某些`CSS`属性是以不可逆方式间接开启。具体如下：

触发`hasLayout`的条件：

* `position`：`absolute`
* `float`：`left|right`
* `display`：`inline-block`
* `width`：除`auto`外的任意值
* `height`：除`auto`外的任意值
* `zoom`：除`normal`外的任意值

另外`IE7`中，如下条件也会触发`hasLayout`：

* `overflow`：（除`visible`外任意值，仅用于块级元素）
* `overflow-x`：（除`visible`外任意值，仅用于块级元素）
* `overflow-y`：（除`visible`外任意值，仅用于块级元素）
* `min-height`：（任意值）
* `min-width`：（任意值）
* `max-width`：（除`none`外任意值）
* `max-height`：（除`none`外任意值）
* `position`：`fixed`
* 等等

幸运的是，`IE8`使用了全新的渲染引擎，删除`hasLayout`原本的功能，因此消除了很多深恶痛绝的`BUG`。但` IE8~11`通过`document.documentElement.currentStyle.hasLayout`依然可以获得`hasLayout`的标志。

由于`BFC`和`hasLayout`的特性，所以都可以用来清除浮动。具体如下：

* 在支持`BFC`的浏览器中，通过创建新的`BFC`闭合浮动
* 在不支持`BFC`的浏览器中，通过触发`hasLayout`闭合浮动

### 布局

* `PC`端布局特点：一般会设置一个默认宽度，更多的是使用像素单位
* 移动端布局特点：基于视口作为网页宽度，更多的是使用相对单位

**左侧固定，右侧自适应**

* `flex`
* 负`margin`
* 创建`BFC`

```css
<style>
	/* 创建BFC */
	.left {
		width: 200px;
		height: 400px;
		float: left;
		background-color: red;
	}
	.right {
		/*
      	margin-right: -200px;
        width：100%;
        */
		overflow: hidden;
		height: 400px;
		background-color: blue;
	}
</style>
<div class="left"></div>
<div class="right"></div>
```

**中间自适应，两侧固定**

衍生出来的有圣杯布局和双飞翼布局。

```css
* {
    margin: 0;
    padding: 0;
}
html,
body {
    width: 100%;
    height: 100%;
}

body {
    min-width: 550px;
}
.container {
    display: flex;
    height: 300px;
}

.container > div {
    height: 100%;
}

.left {
    width: 200px;
    background-color: red;
}

.right {
    width: 200px;
    background-color: green;
}

.main {
    flex: 1;
    background-color: blue;
}
```

**元素居中**

```css
/*
	1.position:absolute/fixed + 负margin
	优点：兼容性好
    缺点：元素必须给定宽高
*/
/*  2.*/
.box {
	position: absolute;
	top: 0;
	left: 0;
	bottom: 0;
	right: 0;
	width: 100px;
	height: 100px;
	margin: auto;
	background-color: red;
}
/*  3.*/
.fu {
	display:table-cell
	vertical-align: middle;
}
.son {
	vertical-align: middle;
	margin: 0 auto; 
	/*如果是子元素是行内元素或者行内块元素，只需要在父元素设置text-align：center*/
}
/*	4.使用flex实现*/
/*  5.使用transform:translate(-50%,-50%)*/
/*  6.*/
.box {
	display: table-cell;
	width: 300px;
	height: 300px;
	vertical-align: middle;
	border: 1px solid red;
	font-size: 0;
}
.box:before {
	content: "";
	display: inline-block;
	height: 100%;
	vertical-align: middle;
}
.content {
	display: inline-block;
	width: 100px;
	height: 100px;
	vertical-align: middle;
	background-color: blue;
}
```
