## HTML

### XML，HTML，XHTML和HTML5

* XML文档常常作为数据传输的载体，其语法规则如下：
  * 标签必须闭合
  * 标签区分大小写
  * 标签必须正确嵌套
  * 文档中必须有根元素
  * 属性值必须加引号


* HTML(HyperText Markup Language，超文本标记语言)是构成网页结构的主要语言，通常所说的HTML是指HTML4.01，是从IE6开始兼容。

  HTML在语法定义上十分松散，这样设计的目的是为了便于开发者书写代码，但是不利于后期维护工作，因此引入了语法更加严格的XHTML。

* XHTML(Extensible HyperText Markup Language，可扩展超文本标记语言)是XML风格的HTML，可以认为是HTML4.01的过渡版本。XHTML相较于HTML语法更加严格，具体如下：

  * 标签必须闭合
  * 标签和属性小写
  * 属性值加引号
  * id属性代替name属性

* HTML5是HTML的下一个版本，除了新增标签之外，还增加了创建现代应用的一组技术，例如：canvas，web存储等。但是从语法角度上，HTML5有如下特点：

  * 简化了文档声明
  * 标签区分大小写
  * 属性可以不加引号
  * 部分属性值可以省略

  其中可以省略的属性如下表：

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

  另外，HTML5还废弃了一些样式和行为标签，因为不符合W3C推崇的样式，结构和行为分离的原则，这些标签有：basefont，rb(ruby替代)，big，center，font，xmp(code替代)，strike，s，u，dir(ul替代)，acronym(abbr替代)，applet，listing(pre替代)，isindex(form配合input替代)，frame，frameset，noframes，bgsound，blink和marquee等。

> HTML并不是编程语言，而是一种用于定义内容结构的标记语言。

### HTML属性

#### lang属性

html标签的lang属性用来设置页面语言。注意：该属性对页面渲染没有任何影响。

```html
<html lang="en"></html>
```

#### id属性和class属性

* id属性具有唯一性，因此一个页面中相同的id只能出现一次，并且一般只有页面关键结构才使用该属性
* class属性通常用于抽取公共样式以减少代码重复

注意：id属性会被搜索引擎识别，因此该属性不能滥用。

### 编码和字符集

将字符转换为计算机能识别的二进制码的过程称为编码，而字符集规定了如何将字符转换为编码，常见的字符集有ASCII，SIO8859-1，GBK，UTF-8等。

当保存文件时的编码和浏览器读取页面使用的编码不同时，就会出现乱码。解决办法就是让其编码统一。

### doctype声明

浏览器有如下两种渲染方式：

* BackCompat：怪异模式，浏览器会使用自己的怪异模式去渲染页面
* CSS1Compat：标准模式，浏览器会使用W3C的标准去渲染页面

如果你的页面没有进行文档声明，那么浏览器就会采用怪异模式去渲染页面，而不同的浏览器的怪异模式不同，渲染出来的页面也就不尽相同。

### HTML标签

#### meta标签

meta标签可以设置文档编码，常见的编码有UTF-8，GB2312(简体中文)，GBK(所有中文字符)，BIG5(繁体中文)等。注意：UTF-8编码中存储一个汉字需要三个字节，而GB2312需要两个，因为采用UTF-8编码的页面加载会相对慢些。

```html
<meta charset="UTF-8">
```

meta标签可以设置页面关键字，页面描述和页面作者等信息，这些信息会被搜索引擎识别，有利于SEO优化。

```html
<meta name="Keywords" content="网易,游戏,新闻" />
<meta name="Description" content="网易是中国领先的互联网技术公司" />
<meta name="Author" content="youzi">
```

#### a标签

a标签的target属性默认`_self`，即在当前窗口打开连接。另外，该标签还可以用来下载文件，但是不建议这样用。

```html
<a href="web.txt">右击另存为即可下载文件</a>
```

#### img标签

有些元素并不包含内容，它们被称为空元素，例如img元素，由于它主要是向其所在的位置嵌入一个图像，所以又被称为替换元素。

img标签可以显示不同格式的图片，具体如下：

* 如果图片需要色彩还原度高，并且拥有复杂颜色，推荐使用JPG格式
* 如果图片需要支持透明和动画，以及压缩比例要高，推荐使用GIF格式
* 如果图片需要无损压缩，以及渐变透明，推荐使用PNG格式

另外，PNG图片在传输图像的过程中，会先将整个图像轮廓显示出来，然后在逐步提高图片的分辨率，这种从模糊到清晰的过程具有良好的用户体验。

img标签定义了alt属性用于图片描述，当图片因为某种原因无法显示时，就会显示alt属性中的文本。注意：该属性会被搜索引擎赋予一定的权重，因此alt属性必须添加。

img标签还定义了title属性也用于图片描述，但是该属性的文本只有在鼠标移入图片时才会显示。另外，HTML5中还新增了figure标签和figcaption标签用于指定图注。

```html
<figure>
	<img src="" alt="" />
  	<figcaption></figcaption>
</figure>	
```

#### textarea标签

textarea标签是多行文本输入域，该标签上定义了cols和rows属性，用来设置每行和每列中的字符数。但是，在不同的浏览器中这两个属性值会有细微差别，并且在IE中还默认带有滚动条。因此，在实际开发中，通常使用width和height属性指定textarea元素的尺寸，并且使用`overflow:auto`指定它的滚动自适应。 

#### button标签

使用button标签定可以实现多种复杂的按钮，而input标签由于是自闭合标签，所以无法实现复杂按钮。

```html
<button><img src="demo.png" /></button>
<!--这是一个图片按钮-->
```

#### label标签

label标签用于将表单成员和其说明性文本关联起来，该标签上的for属性是所关联的表单成员的id属性。一旦关联之后，label标签就具有如下特点：

- 绑定label元素和表单成员
- 增强鼠标易用性

```html
<label for="man"><input type="radio" name="sex" id="man">man</label>
<label for="woman"><input type="radio" name="sex" id="woman">woman</label>
<!--name属性要设置一样，这样才能保证只能单选其中一个-->
```

默认情况下，label标签中的文本和单选框以及复选框不是垂直对齐，这是因为单复选框默认和文字以基线对齐。需要需要垂直对齐，可以使用vertical-align属性来解决。具体如下：

- 如果字体大小为12px，那么设置对应input的`vertical-align:-3px`
- 如果字体大小为14px，那么设置对应input的`vertical-align:-2px`

如果是其它字体大小，可以通过浏览器不断调整到满意为止。

#### submit标签

提交按钮必须定义在form标签内，才能将表单中的数据提交到服务器中。

#### dl标签

dl标签用来自定义列表，其中dt标签用来定义列表标题，dd标签用来定义列表描述。另外，一个dt标签可以配合多个dd标签使用。

```html
<dl>
	<dt>南京</dt>
  	<dd>江苏省的省会，六朝古都。</dd>
  	<dd>发展均衡，但是信息产业不够发达。</dd>
</dl>
```

#### br标签

W3C规定br标签只能用于p标签中文本的换行，不能在其它地方使用。

#### del标签和ins标签

del标签和ins标签分别定义被删除的文本和被更新以及修正的文本，并且带有默认样式。

```html
<del>原价：3000</del>	
<ins>现价：2000</ins>	
```

#### noscript标签

noscript标签可以用于识别`<script>`标签但无法支持其中的脚本的浏览器。

```html
<noscript>Your browser does not support JavaScript</noscript>
```

### 语义化

#### 标题语义化

对于标题标签来说，搜索引擎会赋予h1~h4标签一定的权重，而h5，h6标签权重和普通标签差不多，因此很少使用这两个标签。另外，使用标题标签还要注意如下几点：

* 一个页面中只能有一个h1标签
* h1~h6标签在使用过程中不能断层
* 不要使用标题标签来定义样式
* 不要使用div标签代替标题标签，否则页面会丢失大量权重

#### 图片语义化

如果图片需要作为文档结构的一部分，并且想被搜索引擎识别，推荐使用img标签显示图片。如果图片仅仅起到修饰作用，推荐使用背景图片的形式显示。

#### 表格语义化

caption标签用于定义表格标题，而thead标签，tbody标签和tfoot标签分别定义表头，表身和表脚。这些标签不一定全部使用，其中caption标签和tfoot标签就不经常使用。注意：如果不写tbody标签，table标签会自动生成，所以建议将tr标签写在tbody标签中。

#### 表单语义化

fieldset标签用来给表单元素分组，其中legend标签定义被分组表单的标题。分组的好处是可以通过fieldset标签的disabled属性来禁用整组的表单成员。

```html
<fieldset>
	<legend>表单组标题</legend>
  	......
</fieldset>
```

#### 语义化意义

* 网页结构合理
* 利于SEO优化
* 对视障用户友好

关于语义化标签还有如下几点需要注意：

* 尽量少使用无语义的div和span标签
* 不要使用样式标签定义样式
* 使用strong标签和em标签强调文本，而不是b标签和i标签

一般来说，去掉样式表可以判断当前文档是否具有语义化。

## CSS

### 简介

CSS是层叠样式表(Cascading Style Sheet)的缩写，用于控制网页样式并允许将样式信息与网页内容分离的一种标记性语言。CSS不需要编译，可以直接由浏览器执行，所以又称为浏览器解释型语言。

### 历史

- CSS 最早被提议是在1994年
- 最早被浏览器支持是1996年
- 1996年12月，W3C 正式推出了CSS1。该版本中定义了颜色，字体，外边距，边框等基本样式
- 1998年5月， W3C 正式推出了CSS2。该版本中新增了绝对定位等高级特性
- 不久，W3C推出CSS2.1版本。该版本澄清并更正了CSS2，删除了一些浏览器厂商从未实现的功能
- 目前，所有主流浏览器都支持CSS2.1(IE8可能还有一些遗漏)，并且也是W3C现在正在推荐使用的
- CSS3现在还处于开发中，它被分为多个专业化模块来分别通过标准进程

另外，浏览器厂商会在属性前添加私有前缀来实现非标准CSS属性，甚至用这种方式来实现将来可能标准化的属性，直到该属性成为标准后才移除前缀。

### 样式引入

常见的样式引入有如下四种方式：

* 行内样式
* 内联样式
* 外联样式
* 导入样式

其中，通过`@import`导入样式并不常用，因为导入样式表在浏览器渲染过程中先加载HTML后加载CSS，这会带来极差的用户体验，而外联样式表不会出现这种情况。

### CSS规范

#### 命名规范

如果类名有多个字符，建议使用`-`连接，并且为了避免类名的重复，一般使用父元素的类型作为前缀。

```html
<div class="columns">
	<h3 class="columns-title"></h3>
    <div class="columns-content"></div>
</div>
```

#### 书写规范

推荐按照如下顺序书写CSS属性：

1. 布局属性：display，position，float，clear等属性
2. 盒模型属性：width，height，padding，margin，border，overflow等属性
3. 文本属性：font，line-height，text-align，text-indent，vertical-align等属性
4. 装饰属性：color，background-color，opacity，cursor等属性
5. 其它属性：content，list-style，quotes等属性

但是对于CSS功能代码不需要按照这个顺序书写，相反应该写在一起，有利于代码的可读性。

#### 注释规范

CSS中，使用`/**/`书写注释。注意：在项目上线阶段，通常使用CSS压缩工具将开发阶段的CSS注释删除，以减少文件提高性能，但是对于`/*!comment*/`形式的注释是不会被删除的。

### CSS特性

CSS拥有两大特性，分别是继承性和层叠性。

#### 继承性

CSS中并非所有属性都具有继承性，能够继承的属性可以分为如下三类：

* 文本属性，包括font-family，font-size，font-style，font-weight，font，line-height，text-align，text-indent，word-spacing等属性
* 列表属性，包括list-style-image，list-style-position，list-style-type，list-style等属性
* 颜色属性，只包括color属性

其中，a标签自身定义了color属性，并且优先级比继承的color属性要高。因此如果想要改变a标签的字体颜色，需要选中a标签后再重新设置color属性。

#### 层叠性

说到层叠性就必须提到优先级，因此只有通过优先级才能判断元素到底应该应用何种属性。

##### 引用方式冲突

常用的CSS样式引入分别是行内式、内联式和外联式，并且这三种方式具有一定的优先级。其中，行内样式的优先级最高，内联样式和外联样式的优先级相同，如果它们同时存在，那么后出现的优先级高。

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

* 行内样式：1000
* id选择器：100
* class选择器，伪类选择器，属性选择器：10
* 元素选择器，伪元素选择器：1
* 通配符选择器：0

注意：权重计算只有在选择器选中该元素时才会生效，对于继承而来的属性权重为0。

##### !important

`!important`可以用来提高样式规则的权重，使其权重变为无穷大。此时，如果想要层叠这条样式，可以使用如下两种方式：

* 在目标选择器后使用相同的选择器和样式规则，并且添加`!important`。这是因为选择器权重一样，后出现的优先级高。
* 使用权重更高的选择器，并且在样式规则后添加`!important`。 

#### 总结

关于优先级的比较可以总结为如下两条准则：

* 若选择器选中元素，那么权重高的优先级高。如果权重相同。那么后出现的优先级高。
* 若选择器未选中元素，那么继承而来的属性权重为0，所以选择器离目标元素近的优先级高。

### CSSReset

浏览器会为大多数元素定义默认样式，例如：

* p元素有上下边距
* strong元素有字体加粗样式
* em元素有字体倾斜样式
* ul元素有缩进样式

但是不同浏览器定义的默认样式也有所不同，为了消除显示差异性，往往需要样式重置(CSSReset)。在使用CSSReset的过程中有如下两点需要注意：

* 重置样式表必须放在所有样式表最前面
* 重置样式表中的规则需要根据需求不同而重新定义

### CSS单位

CSS单位可以分为绝对单位和相对单位。

#### 绝对单位

绝对单位是指可以使用物理度量的单位，这些单位定义的尺寸是固定的，不会受到外界影响。常见的绝对单位有：

* cm：表示厘米
* mm：表示毫米
* in：表示英寸
* pt：表示印刷的点数
* pc：表示派卡，1pc=12pt

#### 相对单位

相对于很少使用的绝对单位，相对单位定义的元素尺寸是相对于其它尺寸。常见的相对单位有：

* px：表示像素
* %：表示百分比
* em：表示当前元素字体大小
* rem：表示当前根元素字体大小
* ex：表示当前字体环境中**x**字母的高度
* vw：表示1%视口(浏览器可视区域)的宽度
* vh：表示1%视口的高度

##### 像素

像素(pixel)是指一张图片中最小的点或者计算机屏幕中最小的点。例如：当前计算机屏幕的分辨率是1024*768，则表示该屏幕宽度为1024个像素点，高度为768个像素点。

另外，平常所说的像素通常被当做绝对单位，但是严格来讲，由于屏幕分辨率不同，一像素大小也是有所不同，因此像素实际上是相对单位。

##### 百分比

CSS中的一些属性还支持百分比作为单位，它是一种相对于包含块的计量单位，这些属性有：

* width，height和font-size属性的百分比是相对父元素的相同属性的值来计算
* line-height属性的百分比是相对于当前元素的font-size属性来计算
* vertical-align属性的百分比是相对于当前元素的line-height来计算

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

##### em和rem

* em单位表示当前元素字体大小。在实际开发中，使用它作为统一单位可以增加页面的扩展性，因为只需要改变一处字体大小，就能作用于整个页面。

  ```css
  body {font-size: 62.5%;}		/* 62.5%*16px=10px */
  ```

* rem单位是CSS3中新增的单位，表示当前根元素字体大小，而根元素的默认字体大小是16px。另外，在chrome浏览器下，中文的最小字体大小是12px，英文是10px，当然你可以通过chrome的设置来解除这一个限制。

### CSS选择器

常见的层次选择器如下：

* M N：后代选择器，即选择所有后代元素
* M > N：子代选择器，即选择所有子代元素
* M~N：兄弟选择器，即选择M元素后的所有同级N元素
* M + N：相邻选择器，即选择M元素的后一个同级N元素

### 文档流

W3C规定处于文档流(normal flow，译为正常流或常规流好点)中的元素具有如下特点：

> Boxes in the normal flow belong to a formatting context, which may be block or inline, but not both simultaneously.  Block-level boxes participate in a block formatting context.  Inline-level boxes participate in an inline formatting context. 

翻译过来就是文档流中的盒子属于块级或者行内格式化上下文，块级盒子参与块格式化上下文的计算，行内盒子参与行内格式化上下文的计算。从实际工作经验中可以总结为如下：

* 空白折叠现象
* 高矮不齐，底边对齐。例如：图片和文字或图片和图片默认是基线对齐
* 自动换行

### display属性

display属性可以显式设置元素的表现形式，取值如下：

* inline：以行内元素形式显示
* block：以块级元素形式显示
* inline-block：以行内块元素形式显示，例如：img元素和input元素
* table：以表格形式显示，类似table元素
* table-row：以表格行形式显示，类似tr元素
* table-cell：以单元格形式显示，类似td元素
* none：隐藏元素

#### 行内元素

行内元素具有如下特性：

- 设置宽高无效
- 设置上下外边距无效，左右外边距有效
- 设置上下左右内边距有效

#### 行内块元素

`display:inline-block`属性可以将元素显式设置为行内块元素，但是对于块级元素来说，设置该属性在IE6,7中无法被识别，需要使用如下CSShack。对于行内元素来说，设置该属性被所有浏览器支持。

```css
div {
  *display: inline;
  *zoom: 1;
}
```

另外一个常见问题是带有空白字符相隔的行内块元素之间会有间隙，有如下方法解决：

* 设置父元素字体大小为0，但是唯一的不同是要重新设置当前元素的字体大小
* 设置负margin
* 设置letter-spacing和word-spacing属性，通过这两个属性减少空白字符和其它字符之间的间距来消除行内块元素之间的间距。
* 格式化html文档

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

在使用inline-block进行布局时，你需要注意：

* vertical-align属性会影响到inline-block元素，你可能会把它的值设置为top
* 你需要设置每一列的宽度
* 如果HTML源码中元素之间有空格，那么列与列之间会产生空隙

#### 隐藏元素

有如下三种方式隐藏元素：

* 设置`display:none`属性完全隐藏元素，并且该元素不会占据原来的空间。注意：不推荐使用这种方式隐藏页面中SEO关键部分。
* 设置`visibility:hidden`属性完全隐藏元素，但是该元素还会占据原来位置。
* 设置`opacity:0`属性间接隐藏元素，该元素同样会占据原来位置。

另外，一些特殊元素的display属性默认是none，例如script元素。

#### 单元格元素

`display:table-cell`属性可以显式设置元素为单元格元素，表现和td元素类似，其外边距会失效，但是内边距不会失效。注意：设置该属性的元素会被浮动和绝对定位破坏（因为同一个元素同时设置`display:block`和`display:table-cell`会发生冲突），所以不要将它们混合使用。 

IE8之前的浏览器不支持该属性，但是单元格元素在实际开发中应用十分广泛。例如：

* 图片垂直居中于某个不固定尺寸元素中

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

* 自适应等高布局中，原理是同一行的td元素高度始终相等，由td元素的最大高度所决定。

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

* 自动平均划分元素宽度

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

还有很多更有意思的display值，例如list-item，run-in等等，具体请看[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display)。

### 盒子模型

CSS中，页面中所有元素都可以看作是一个由外边距，边框，内边距和内容组成的盒子模型。另外，盒子模型可以分为IE盒模型和W3C盒模型，其中W3C盒模型规定width和height属性是内容的尺寸，而IE盒模型的width和height属性是内容+内边距+边框的尺寸。

CSS3中，新增box-sizing属性来指定元素使用哪种盒模型进行渲染。（IE8+）

```css
div {
    box-sizing: content-box;	/*内容盒模型，W3C盒模型*/
  	box-sizing: border-box;		/*边框盒模型，IE盒模型*/
  	/*其中，content-box是默认值*/
}
```

通过`JavaScript`获取盒模型对应的宽高：

* dom.style.width/height。缺点：只能取到通过行内样式设置的width和height属性，并且带有单位。
* dom.currentStyle.width/height || window.getComputedStyle(dom).width/height。前者是IE支持的方法，后者是w3c支持的方法。注意：该方法不仅能取到通过行内样式设置的宽高，还能拿到通过外联和内联样式设置的宽高。
* dom.getBoundingClientRect().width/height

### CSS属性

#### 盒模型属性

##### 宽高

```css
div{
    width: 600px;
    margin: 0 auto;
}
```

你会发现在小窗口的情况下会出现横线滚动条，当然你可以使用max-width来解决这个问题，当视口小于600px，那么会显示当前视口的100%宽度。（IE7+）

##### 边框

设置`border:0`和`border:none`样式都可以让元素在显示效果上没有边框，它们之间的区别如下：

* 性能差异：前者会让浏览器渲染出边框宽度为0的边框，也就是说依旧占据内存。而后者对于浏览器来说不会渲染边框，也就是说不会占据内存。
* 兼容差异：前者会在所有浏览器中都不显示边框，而后者对于IE6,7中的`<input type="button">`元素无效。

##### 外边距

当处于文档流中的上下两个元素的垂直外边距相遇时，会发生外边距叠加现象，叠加后的外边距等于这两个元素外边距的最大值。这个现象会发生在同级元素，父子元素和空元素上，具体如下：

* 对于同级元素来说：当一个元素在另一个元素上面时，上面元素的下边距会和下面元素的上边距叠加。
* 对于父子元素来说：当父元素没有设置边框或内边距时，父元素的垂直外边距会和子元素的垂直外边距叠加。

对于外边距叠加还有如下两点补充：

* 水平外边距不会叠加
* 只有块级元素和行内块元素才会发生叠加现象

注意：当父元素空间不足会导致子元素右外边距失效，因为渲染引擎会优先保护保护左外边距。

另外，外边距是为数几个不多的可以设置负值的属性(这些属性有margin，letter-spacing，word-spacing和vertical-align等)，这就是常用的负外边距技术。常见的应用场景如下：

* 带竖线分隔的横向列表。注意：分隔线最好使用边框模拟，而不是使用`|`字符。

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

* 自适应两列布局。

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

##### 浮动

浮动常用于页面布局，设置浮动的元素有如下特性：

* 浮动元素脱离文档流，也就是不再在文档中占用位置
* 浮动元素会一直向上飘浮，直到遇到父元素的边界或者其它浮动元素
* 浮动元素有字围效果
* 浮动元素若没有设置宽度，那么将是内容宽度
* 浮动元素以元素顶部对齐
* 浮动元素只影响后面元素

注意：当一个元素浮动以后将会自动变为块级元素，尽管其原来是行内元素。

但是浮动给布局带来便利的同时，还会带来一些副作用。例如：父元素高度塌陷问题。此时，需要通过清除浮动来消除影响。常见的清除浮动方式如下：

* 给父元素设置高度。这种方式的缺点是把高度写死，不利于页面的可扩展性

* 给父元素设置浮动。这种方式的缺点会影响布局。

* 给紧跟浮动元素的后面元素设置`clear:both`属性。这种方式的缺点是会导致margin失效或者增加一些额外标签。

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

##### 定位

position属性可以设置元素的定位类型，常见的定位类型有静态定位，相对定位，绝对定位和固定定位，分别对应如下属性值：

* static：静态定位，表示元素按照文档流进行定位。注意：该属性值是默认值。
* relative：相对定位，表示元素按照其原来位置进行定位，并且原位置空间依然保留在文档流中，也就是说其他的元素的位置不会受该元素的影响发生位置改变来弥补它偏移后剩下的空隙。注意：设置相对定位的元素不会脱离文档流，并且可以使用left，top等定位属性进行定位。
* absolute：绝对定位，表示元素相对于最近已定位(除了静态定位)的祖先元素(会忽略其内边距)进行定位，如果不存在这样的元素，则相对于初始包含块（可以理解为视口）进行定位。注意：设置绝对定位的元素会脱离文档流，并且可以使用定位属性进行定位。
* fixed：固定定位，表示元素相对于初始包含块（可以理解为视口）进行定位。注意：设置固定定位的元素不会保留它原本在页面应有的位置，也就是脱离文档流，并且可以使用定位属性进行定位。
* sticky：粘性定位。从表现上可以看出，当元素在屏幕内，表现为relative，就要滚出显示器屏幕的时候，表现为fixed。

注意：其中相对定位，绝对定位和固定定位既可以使用定位属性来进行位置移动，也可以使用margin来进行位置移动，但是不建议使用margin。另外，对于绝对定位和固定定位来说，必须设置定位属性，否则上面叙述的结论不成立。

对于绝对定位元素，可以使用width属性设置宽度，也可以同时使用left和right属性设置宽度，但是如果这两种方式同时存在，那么以width属性为准，并且right属性失效。同理，对于高度也是如此。

既然绝对定位和固定定位会脱离文档流，那么如果其父元素没有设置高度，也会造成父元素高度塌陷。但是此时高度塌陷不能使用`overflow:hidden`来解决。

另外，相对定位的width和height属性百分比是相对于去父元素的对应属性，而绝对定位是相对于其最近的已定位的祖先元素（没有则相对于初始包含块）的对应属性，固定定位则相对于初始包含块的对应的属性。

##### z-index

z-index属性可以设置当元素重叠时的显示顺序，浏览器会将属性值高的元素显示在上面。如果属性值相等，那么后出现的元素会显示在上面。注意：只有定位元素才拥有z-index属性，其中静态定位元素的属性值默认为0，其余定位元素的属性值默认为1。

另外，该属性的显示顺序规则只应用于兄弟元素，对于非兄弟元素，则是由其父元素的z-index属性值决定显示顺序。

#### 文本属性

##### text-indent

text-indent属性可以设置文本缩进，常用于文档中logo图片的优化。

##### text-align

text-align属性可以设置文本水平对齐方式。注意：该属性只对文本，行内元素，行内块元素有效，并且在其父元素上定义。

另外，设置`margin: 0 auto`属性可以实现块级元素水平居中对齐，前提是该元素设置了width属性并且处于文档流中。注意：该属性在当前元素上定义。

##### word-spacing

word-spacing和letter-spacing属性是两个非常容易混淆的文本属性，具体区别如下：

* 前者表示单词之间的空白距离
* 后者表示每个字符之间的空白距离

##### line-height

在块级元素中，行高指定了该块级元素所有的行盒（line-box）的最小高度；在非替换元素中，行高指定了该元素行盒（line-box）的高度；而替换元素（img、button等）没有行高。

拓展知识：

* 行盒（line-box）：元素的每一行称为一条line-box，它是由这一行的许多inline-box组成
* 替换元素：替换元素（行内块元素）是浏览器根据其标签的元素与数学来判断显示具体的内容。例如：`<input type="text">`文本输入框，输入新值时，浏览器显示就不一样。常见的替换元素还有：img，input，textarea，select，object等，这些元素都没有实际的内容。
* 非替换元素：HTML中的大多数元素都是非替换元素，也就是说内容直接被浏览器显示出来。

line-heigth属性可以设置元素中文本行高，该可继承属性可以取值为如下关键字：

- top：表示文字顶线
- middle：表示文字中线
- baseline：表示文字基线，该属性值是默认值
- bottom：表示文字底线

其中，行高是两条基线之间的距离，行距是上一行文字底线和下一行文字顶线之间的距离，半行距就是行距的一半，内容区域是一行文字的顶线到底线之间的距离。

另外，line-height属性还可以取值为具体数值，不同单位表示还不同的含义，具体如下：

* 如果单位是px，那么行高就是指定值
* 如果单位是em，那么行高等于当前元素字体大小*设置值
* 如果单位是%，那么行高等于当前元素字体大小*设置值
* 如果没有单位，那么行高等于当前元素字体大小*设置值
* 如果单位是normal，那么行高是取决于浏览器，与元素字体有关

注意：虽然`em`，`%`以及纯数字在计算上没有差别，但是在应用元素上是有差别的。对于没有数字来说，所有可继承子元素会根据font-size重新计算行高，而对于`em`和`%`来说，当前元素根据font-size计算行高，然后继承给子元素。

另外，line-height属性常常用于单行文本垂直居中，如果是多行文本垂直居中如果使用内边距调整。

注意：行高是会影响元素高度的。如果行高为指定具体长度（px），那么字体大小的改变会影响行高，此时：

* 如果块元素（可以设置宽高的元素）未指定高度，其高度等于行内元素（该行内元素是块元素的子元素）的行高，如果是多行就是多行行高的累加
* 行内元素（inline）的行高虽然无法撑起该行内元素的高度，但却影响当前行的行高，进而影响父元素的高度。

##### vertical-align

关于vertical-align属性可以总结为如下几点：

* vertical-align属性用于定义周围的文本，行内元素和行内块元素相对于当前元素(设置该属性的元素)基线的垂直对齐方式。
* vertical-align属性可以设置单元格元素中内容的垂直对齐方式，其中单元格元素是指设置了`display:table-cell`的元素。
* vertical-align属性对行内元素，行内块元素和单元格元素有效，对块元素无效，也就是说该元素不能与浮动和绝对定位同时使用。注意：IE8以上浏览器，input，button，img元素是行内块元素。但是在IE8以下浏览器中，input和button元素时行内元素。

该属性允许指定如下关键字作为属性值：

* top：表示相对于顶线对齐
* middle：表示相对于中线对齐
* baseline：表示相对于基线对齐
* bottom：表示相对于底线对齐

还允许使用百分比值和负值，具体如下：

* `vertical-align:-2px`表示相对于当前元素基线向下偏移2像素。也就是说，如果是正值，则表示相对于基线向上偏移对应的像素对齐。如果是负数，则表示相对于基线向下偏移的像素对齐。


* `vertical-align:50%`表示相对于当前元素的行高的50%对齐。也就是说，如果元素行高是20像素，那么该属性值就是10像素，则表示相对于基线向上偏移10像素对齐。

vertical-align属性的应用场景有许多，具体如下：

* 清除图片底部3px空白

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

* 在未知尺寸元素内部垂直居中

  ```html
  <style>
  div {
  	display: table-cell;
  	vertical-align: middle;
  	width: 300px;
  	height: 300px;
  	border: 1px solid red;
  }
  img {
  	width: 100px;
  	vertical-align: middle;
  }
  </style>
  <div>
    <img src="demo.jpg" alt="">
  </div>
  ```

#### 其它属性

##### overflow

前面提到使用`display:none`和`visibility:hidden`属性可以完全隐藏元素，而overflow属性可以隐藏部分元素。该属性指定当内容超出容器元素大小时该如何显示，取值如下：

* visible：表示内容可以溢出并绘制在元素边框的外面，该属性值是默认值
* hidden：表示裁剪掉和隐藏溢出的内容
* scroll：表示元素一直显示水平和垂直滚动条。如果内容超出元素尺寸，则允许通过滚动查看额外的内容
* auto：表示滚动条只在内容超出元素尺寸时显示，而非一直显示

#### clip

clip属性指定了元素的裁剪区域，也用来只显示部分元素。在CSS2中，裁剪区域是矩形的，但是在未来CSS标准中可能允许其它形状的裁剪。另外，该属性不允许使用百分比作为单位，但是允许指定负值，表示裁剪区域超过元素指定的边框尺寸。如果指定auto关键字，则表示裁剪区域是元素边框。

```css
div {
    clip: rect(0px 100px 100px auto);	/*括号中数字表示裁剪矩形的边距，以距离上右下左为参考点*/
  	/*必须带上单位，即便值为0*/
}
```

##### opacity

大部分元素的背景是完全透明的，对于那些背景不是完全透明的元素，可以设置`background-color:transparent`属性来设置背景完全透明。而opacity属性可以设置元素背景的透明度，而不是全透明。注意：IE不支持这个属性，使用filter属性来代替。

另外，opacity属性除了会使元素背景透明之外，元素中内容也会跟着透明，因此CSS3中引入RGBA属性来只让元素背景透明。

##### cursor

cursor属性用来指定鼠标样式，取值如下：

* pointer：小手
* default：小白
* move：移动
* text：文本输入

另外，当button元素设置宽高，默认的pointer就会变成default。

### 性能优化

#### 属性缩写

复合属性可以通过缩写来进行性能优化，这些属性有color，border，margin，padding，font，background等。注意：字体缩写时，要将英文字体放在前面，中文字体放在后面，并且中文字体最好使用如下的unicode编码(通过全局函数escape()查看编码)表示，以取得良好的兼容性。

* 宋体(SimSun)：`\5B8B\4F53`
* 新宋体(NSimSun)：`\65B0\5B8B\4F53`
* 黑体(SimHei)：  `\9ED1\4F53`
* 微软雅黑(Microsoft yahei[YaHei])：`\5FAE\8F6F\96C5\9ED1`
* 隶书(LiSu)：  `\96B6\4E66`
* 幼圆(YouYuan)：  `\5E7C\5706`
* 华文细黑(STXhei)：`\534E\6587\7eC6\9ED1`
* 细明体(MingLiu)：  `\7EC6\660E\4F53`
* 新细明体(PMingLiu)：  `\65B0\7EC6\660E\4F53`

```css
div {
  font: "Times New Roman","微软雅黑","宋体" 12px/1.5 bold;
  /*字体连写时，行高必须写在字体大小后面。并且font-weight最好不要使用关键字，而是使用700代替bold*/
  background: #ccc url(../images/logo.png) no-repeat 40px 40px;
}
```
#### 语法压缩

在CSS中，还可以通过如下语法压缩进行性能优化：

* 省略结尾分号
* 省略URL路径中引号
* 省略值为0的单位
* 省略小数中整数为0的部分
* 多利用继承性

当然在实际开发中，常常使用构建工具压缩代码和图片。

#### 合理选择器

浏览器通常是从右向左解析选择器，因此合理的选择器对于性能优化也存在存在一定的影响。可以按照匹配效率将选择器从高到低进行排列，具体如下：

1. id选择器
2. class选择器
3. 元素选择器
4. 相邻选择器
5. 子代选择器
6. 后代选择器
7. 通配符选择器
8. 属性选择器
9. 伪类选择器

另外，对于使用高性能的选择器还有如下几点需要注意：

* 禁止使用通配符选择器
* 禁止在id选择器和class选择器之前加上元素名
* 选择器最好不要超过三层，并且靠右的选择器尽量精确
* 避免使用后代选择器，少用子代选择器

### 兼容性问题

正常显示的页面在IE6浏览器中或多或少会存在一些BUG，这些经典的BUG以及对应的解决方法如下：

* IE6不支持小于12像素大小的元素，解决办法是将元素的字体大小设置小于该元素的高度。

  ```css
  div {
      height: 4px;
    	_font-size: 0;		/*带下划线属性只有IE6能识别*/
  }
  ```

* IE6不支持使用`overflow:hidden`来清除浮动，解决方法是使用如下CSShack。

  ```css
  div {
    overflow: hidden;
    _zoom: 1;		 /*该属性会触发IE浏览器的hasLayout*/
  }
  ```

* IE6中，如果出现连续浮动的元素，并且浮动方向和外边距方法一致时，那么第一个浮动元素会具有双倍外边距。解决方法是设置浮动方向和外边距方法相反或者单独设置第一个浮动元素为一半外边距。

* IE6中，如果子元素设置了右外边距，那么离父元素的距离会多出3像素。解决方法是使用内边距代替外边距。

### CSSsprite

合理的CSSsprite可以减少http请求次数，提高页面访问速度。在使用雪碧图还有如下几点需要注意：

* 开发后期使用雪碧图，而不是前期。因为，小背景图的位置可能经常改变
* 有条理的使用雪碧图，将小背景图按照风格和大小进行分类
* 控制雪碧图的大小，最好不超过200kb，如果超出，可以再将其分为多个小的雪碧图

### 包含块

包含块（containing block）是视觉格式化模型的一个重要概念，可以把它理解为矩形，为其内部的元素提供一个参考，元素的尺寸和位置往往都是由该元素所在的包含块绝对。所以有时也会将其称为定位参考框或者定位坐标参考系。

一个元素的包含块定义如下：

* 浏览器选择根元素（html）作为初始包含块。注意：初始包含块和视口等大
* 文档流以及浮动的元素，其包含块是由最近的块级祖先元素盒子的内容边界（content-box）组成
* 绝对定位的元素，其包含块是由最近的非static（absolute/relative）定位的祖先元素的padding-box建立，默认是基于初始包含块
* 固定定位的元素，其包含块是由视口建立

### 层叠上下文

层叠上下文(stacking context)和层叠级别(stacking level)是CSS中两个重要概念。其中层叠上下文是HTML文档中一个三维概念。从前面提到到的z-index属性可以看出，虽然一个网页是平面的，但实际上是三维结构，除了X，Y轴之外，还有Z轴，而Z轴往往都是用来设定层的先后顺序。

层叠上下文和BFC类似，是可以创建出来的。也就是说，为元素设置特定的属性会创建层叠上下文出来。一般来说，只要元素具有如下其中一个条件都会创建一个新的层叠上下文：

* 根元素，并且由它创建的层叠上下文称之为根层叠上下文
* z-index不为auto的定位元素

简单来说就是，使用z-index属性可以为一个定位元素创建一个新的层叠上下文。有了层叠上下文之后，就可以根据层叠级别来决定其内部元素或背景的显示顺序。在同一个层叠上下文中，层叠级别从低到高排列如下：

* 背景和边框：也就是当前层叠上下文的背景和边框
* 负z-index：z-index为负值的内部元素
* 块盒子：文档流中的块级盒子(block-level box)
* 浮动盒子：非定位的浮动元素，也就是排除了相对定位的浮动元素
* 行内盒子：文档流中的行内盒子(inline-level box)
* z-index为0：z-index为0的内部元素
* 正z-index：z-index为正值的内部元素

另外，关于层叠性还有如下几点需要注意：

* 除了背景和边框是针对当前层叠上下文之外，其它都是针对当前层叠上下文内部元素
* inline-block元素不是块盒子，而是行内盒子
* 当前层叠上下文内部定位元素也可以设置z-index创建新的层叠上下文，也就是说层叠上下文可以嵌套，但是内部元素创建的层叠上下文依然受制于当前层叠上下文
* 背景和边框往往起装饰效果，所以层叠级别最低。浮动元素和块元素一般用于布局，而行内元素一般用于显示内容，所以行内元素层叠级别较高。

有了层叠上下文和层叠级别，就可以判断一个元素在Z轴上的堆叠顺序，具体如下：

* 同一个层叠上下文中，层叠级别大的内部元素显示在上面
* 同一个层叠上下文中，如果内部元素的层叠级别相同，那么后面的元素显示在上面
* 不同的层叠上下文中，由父元素的层叠级别决定显示顺序，与自身层叠级别无关。

### 格式上下文

在CSS中，页面中任何一个元素都可以看成一个盒子。并且在文档流中，盒子会参与一种格式上下文(formatting context)。这个盒子可能是块盒子(block-level box)，也可能使行内盒子(inline-level box)，但是不可能两者皆是。其中，块盒子参与BFC，行内盒子参与IFC。

格式上下文是指页面中的一块渲染区域，并且这个格式上下文有一套自身的渲染规则。格式上下文决定了其内部元素将如何定位，以及和其它元素之间的关系。另外，格式上下文可以分为两种：

* 块级格式化上下文(Block Formatting Context，BFC)
* 行级格式化上下文(Inline Formatting Context，IFC)

#### 盒子

盒子，又称CSS盒子，它是布局的基本单位。简单来说，一个页面是由多个盒子组成(参考前面提到的盒子模型)。而元素的类型是由display属性决定，不同类型的盒子也会参与不同的格式上下文。例如：设置`display:inline-block`属性的元素是行内块元素，这个元素会生成行内盒子，参与IFC。更具体的情况如下：

* 块盒子：display属性为block，table，list-item的元素会生成块盒子
* 行内盒子：display属性为inline，inline-block，inline-table的元素会生成行内盒子

另外，除了块盒子和行内盒子，CSS3还新增了run-in box盒子。这里不多作介绍。

#### BFC

块级格式化上下文(Block Formatting Content，BFC)是一个独立的渲染区域，只有块盒子参与。BFC规定内部的块盒子如何布局，并且这个渲染区域与外界区域无关。另外，W3C规定，满足如下其中一个条件就会创建一个新的BFC：

* 根元素，也就是说默认情况下，一个页面中的所有元素都处在由根元素创建的BFC中
* float属性除了none之外的值
* position属性除了static和relative之外的值
* overflow属性除了visible之外的值
* display属性是inline-block，table-caption，table-cell，flex和inline-flex
* 触发IE的hasLayout属性

W3C规范还规定，BFC的特性可以分为如下两条：

* 在一个BFC中，盒子从顶端开始垂直一个接着一个地排列，两个相邻盒子之间的垂直间距由margin属性决定。在同一个BFC中，两个相邻块盒子之间垂直方向上的外边距会叠加
* 在一个BFC中，每一个盒子的左边界(margin-left)会紧贴容器的左边(border-left)，即使浮动元素也是如下。

将这两条特性可以分开来理解，具体如下：

* 在一个BFC内部，盒子会在垂直方向上一个接着一个地排列
* 在一个BFC内部，相邻的margin-top和margin-bottom会叠加
* 在一个BFC内部，每一个元素的左外边界会紧贴着包含盒子的左边，即使存在浮动也是如此
* 在一个BFC内部，如果存在内部元素是一个新的BFC，并且存在内部元素是浮动元素。则该BFC的区域不会与float元素的区域重叠
* BFC就是页面上的一个隔离的盒子，该盒子内部的子元素不会影响到外面的元素
* 计算一个BFC的高度时，其内部浮动元素的高度也会参与计算

BFC的用途很多，具体如下：

* 创建BFC来避免垂直外边距叠加

* 创建BFC来清除浮动

* 创建BFC来避免因为浮动而引起的文字环绕

* 创建BFC来实现自适应两列布局

  ```html
  <style>
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

### hasLayout

IE6,7的显示引擎使用的是一个称谓布局(layout)的内部概念，它用来控制元素自身及其子元素的尺寸设置和定位。这些特性和BFC类似，因此可以简单将hasLayout看做是IE5.5/6/7中的BFC。注意：这个显示引擎存在很多缺陷，因此会导致很多显示BUG。

当hasLayout为true时，就是所谓的拥有布局时，相当于元素产生新的BFC，块级元素自己对自身内容进行组织和尺寸计算。

而当hasLayout为false时，就是所谓的不拥有布局时，相当于元素不产生新BFC，元素由其所属的包含块进行组织和尺寸计算。

和产生新的BFC的特性一样，hasLayout无法通过CSS属性直接设置，而是通过某些CSS属性间接开启这一特性。不同的是某些CSS属性是以不可逆方式间接开启。具体如下：

触发hasLayout的条件：

* position：absolute
* float：left|right
* display：inline-block
* width：除auto外的任意值
* height：除auto外的任意值
* zoom：除normal外的任意值

另外IE7中，如下条件也会触发hasLayout：

* overflow：（除visible外任意值，仅用于块级元素）
* overflow-x：（除visible外任意值，仅用于块级元素）
* overflow-y：（除visible外任意值，仅用于块级元素）
* min-height：（任意值）
* min-width：（任意值）
* max-width：（除none外任意值）
* max-height：（除none外任意值）
* position：fixed
* 等等

另外，IE8使用了全新的渲染引擎，删除hasLayout原本的功能，因此消除了很多深恶痛绝的BUG。但 IE8~11通过`document.documentElement.currentStyle.hasLayout`依然可以获得 hasLayout 的标志。

由于BFC和hasLayout的特性，所以都可以用来清除浮动。具体如下：

* 在支持BFC的浏览器中，通过创建新的BFC闭合浮动
* 在不支持BFC的浏览器中，通过触发hasLayout闭合浮动


### 布局

* PC端布局特点：pc网页设置一个默认宽度，倾向于像素单位（px）
* 移动端布局特点：基于视口作为网页宽度，更倾向于使用相对单位（rem，%，vh，vw等）

#### 圣杯布局（淘宝搜索栏）

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		* {
			margin: 0;
			padding: 0;
		}
		body {
			min-width: 700px; /*缩放到一定程度布局会乱*/
		}
		.container {
			height: 300px;
			padding: 0 200px;
			word-break: break-all;
		}
		.main,.left,.right {
			float: left;
			height: 300px;
		}
		.main {
			width: 100%;
			background-color: red;
		}
		.left {
			position: relative;
			left: -200px;
			width: 200px;
			margin-left: -100%;
			background-color: blue;
		}
		.right {
			position: relative;
			right: -200px;
			width: 200px;
			margin-left: -200px;
			background-color: green;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="main">1111111111111111111111112222222222222222222222333333333333333333333334444444444444444444444555555555555555555555555666666666666666666666777777777777777777777777778888888888888888899999999999999999</div>
		<div class="left"></div>
		<div class="right"></div>
	</div>
</body>
</html>
<!--先浮动，在padding，在定位-->
```

#### 双飞翼布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		* {
			margin: 0;
			padding: 0;
		}
		.container {
			height: 300px;
			word-break: break-all;
		}
		.main,.left,.right {
			float: left;
			height: 300px;
		}
		.main {
			width: 100%;
			background-color: red;
		}
		.main .inner {
			padding: 0 200px;
			/*使用margin也是可以的，但是不推荐*/
		}
		.left {
			width: 200px;
			margin-left: -100%;
			background-color: blue;
		}
		.right {
			width: 200px;
			margin-left: -200px;
			background-color: green;
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="main">
			<div class="inner">
				1111111111111111111111112222222222222222222222333333333333333333333334444444444444444444444555555555555555555555555666666666666666666666777777777777777777777777778888888888888888899999999999999999
			</div>
		</div>
		<div class="left"></div>
		<div class="right"></div>
	</div>
</body>
</html>
<!--先浮动，在加inner，在padding/margin-->
```

#### 块级元素居中

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





