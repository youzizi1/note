## 视口

视口（ViewPort）就是浏览器显示页面内容的屏幕区域，视口可以分为布局视口，视觉视口和理想视口。

* 布局视口（layout viewport）：一般移动设备的浏览器都默认设置了一个布局视口，用于解决早期的PC端页面在手机上的显示问题。IOS和安卓基本都将这个视口分辨率设置为980px（与浏览器有关），所以PC上的网页大多都能在手机上显示，只不过元素看起来很小，但是默认都可以通过手势去缩放页面

* 视觉视口（visual viewport）：用户的可视区域。我们可以通过缩放去操作视觉视口，但是不会影响布局视口，布局视口仍然会保持原来的宽度

* 理想视口（ideal viewport）：由苹果公司提出，目的是为了使网站在移动端上有理想的浏览和阅读宽度而设定。你需要通过手动设置meta标签去设置布局视口等于理想视口的宽度。简单来说就是：设备多宽，布局视口就多宽

  ```html
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  ```

## 二倍图

* 物理像素：显示器最小的物理显示单位，由硬件决定
* CSS像素：又称逻辑像素，是我们通过CSS设置的像素
* 设备像素比（device pixel radio，dpr）：物理像素/逻辑像素

一般来说，在PC端页面中，dpr为1，也就是说1px等于1物理像素。但是在移动端上，苹果公司从iphone4开始推出的Retina屏幕（又称视网膜屏），分辨率提高了一倍，但是屏幕尺寸没变。例如：iphone7的分辨率是`750x1334`，但是屏幕尺寸却是4.7英寸，也就说同样尺寸大小的屏幕，物理像素多了一倍。

也就是说对于dpr为2的屏幕，一个CSS像素是等于两个物理像素的，这样就会造成一个问题——对于`50×50`的图片放到dpr为1的屏幕显示是清晰的，放到dpr为2的屏幕上显示是模糊的。所以，我们需要使用二倍图`100×100`，然后再通过CSS缩放成`50×50`，从而解决模糊问题。如果dpr更高，你可能还需要使用三倍图，甚至多倍图。

对于通过img标签插入的图片，你可以通过设置width属性来控制大小；对于通过background插入的图片，你可以通过`background-size`来控制大小。

当然，你也可以写成一个sass函数：

```scss
@mixin bg-image($url) {
  background-image: url($url + "@2x.png");
  @media (-webkit-min-device-pixel-ratio:3),(min-device-pixel-ratio:3){
    background-image: url($url + "@3x.png")
  }
}
```

## 移动端布局

* 流式布局
* 弹性布局
* rem布局
* vw布局

> hotcss也是基于rem的移动端布局解决方案。

### 流式布局

流式布局，又称百分比布局，也就是通过百分比进行布局，一般需要配合max-width和min-width一起使用。

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }
    body {
        width: 100%;
        max-width: 750px;
        min-width: 320px;
        margin: 0 auto;
    }
    .content {
        background-color: #ccc;
    }
</style>
<body>
  <div class="content">
    hello world
  </div>
</body>
```

### 弹性布局

弹性布局，又称为伸缩布局，基于flex技术。

- 主轴：Flex容器的主轴主要用来配置Flex项目，默认是水平方向
- 侧轴：与主轴垂直的轴称作侧轴，默认是垂直方向的
- 方向：默认主轴从左向右，侧轴默认从上到下

主轴和侧轴并不是固定不变的，通过flex-direction可以互换。

#### 必要元素

1. 指定一个盒子为伸缩盒子 display: flex
2. 设置属性来调整此盒的子元素的布局方式 例如 flex-direction
3. 明确主侧轴及方向
4. 可互换主侧轴，也可改变方向

#### 属性详解

- flex-direction：调整主轴方向(默认是水平方向)
- justify-content：调整主轴对齐
- align-items：调整侧轴对齐
- flex-wrap：控制是否换行
- align-content：堆栈（由flex-wrap产生的独立行）对齐
- flex-flow是flex-direction、flex-wrap的简写形式
- flex子项目在主轴的缩放比例，不指定flex属性，则不参与伸缩分配
- order控制子项目的排列顺序，正序方式排序，从小到大

注意：当父元素设置flex之后，子元素的float，clear以及vertical属性将会失效。

```html
<style>
    * {
        margin: 0;
        padding: 0;
    }
    body {
        width: 100%;
        max-width: 750px;
        min-width: 320px;
        margin: 0 auto;
    }
    .content {
        display: flex;
        background-color: #ccc;
    }
</style>
<body>
  <div class="content">
    hello world
  </div>
</body>
```

### rem布局

rem布局的本质就是基于根元素的font-size对页面进行等比缩放，自动适配不同尺寸的屏幕，所以我们只需要动态改变不同屏幕尺寸的font-size大小即可。

第一种就是通过媒体查询动态设置不同尺寸的屏幕的根元素的字体大小。那么如何进行动态设置根元素的字体大小呢？原理如下：

1. 假设设计稿是750px，我们将其划分15等分（划分标准不一，可以是10或20），这样每一份当作html元素的字体大小，即50px
2. 如果此时设备屏幕是320px宽，那么根元素的大小就变成320/15，即21.33px，所以你需要根据不同屏幕尺寸手动计算并设置根元素的字体大小
3. 这样在750px下的`100*100`的元素大小，即`2rem*2rem`，在320px下就是`42.66px*42.66px`，这样就实现元素等比例缩放了

```scss
html {
    /* 在pc端打开页面，写死成50px */
    /* pc端总是大于750px，不需要除以15 */
    font-size: 50px;
}

body {
    width: 100%;
    min-width: 320px;
    max-width: 750px;
    margin: 0 auto;
    /* 设置阅读字体大小，因为字体大小不应该等比例缩放大小 */
    font-size: 16px;
}

$no: 15;

@media screen and (min-width: 320px) {
  html {
      font-size: 320px/$no
  }
  /* 设置不同尺寸下阅读字体大小 */
  body {
      font-size: 12px
  }
}

@media screen and (min-width: 360px) {
  html {
      font-size: 360px/$no
  }

  body {
      font-size: 12px
  }
}

@media screen and (min-width: 375px) {
  html {
      font-size: 375px/$no
  }

  body {
      font-size: 12px
  }
}

@media screen and (min-width: 400px) {
  html {
      font-size: 400px/$no
  }

  body {
      font-size: 14px
  }
}

@media screen and (min-width: 480px) {
  html {
      font-size: 480px/$no
  }

  body {
      font-size: 15.36px
  }
}

@media screen and (min-width: 540px) {
  html {
      font-size: 540px/$no
  }

  body {
      font-size: 17.28px
  }
}

@media screen and (min-width: 720px) {
  html {
      font-size: 720px/$no
  }

  body {
      font-size: 23.04px
  }
}

@media screen and (min-width: 750px) {
  html {
      font-size: 750px/$no
  }

  body {
      font-size: 24px
  }
}
```

接下来你只需要动态设置元素的rem值即可。

```scss
$base: 750 / 15;

@function pxTorem($px) {
    @return $px / $base * 1rem;
}
```

例如设计稿中的某个元素是100*100像素：

```scss
@import './util.scss';

.box {
    width: pxTorem(100);
    height: pxTorem(100);
}
```

第二种就是通过手淘开源的flexible.js来自动计算并设置不同尺寸下的html元素的字体大小。其原理如下：

```js
(function flexible(window, document){
    // 动态设置字体大小
    function setRemUnit() {
        const htmlDOM = document.documentElement
        var rem = htmlDOM.clientWidth / 10
        htmlDOM.style.fontSize = rem + 'px'
    }
    setRemUnit()
    
    window.addEventListener('rezie', setRemUnit)
    
    // 动态设置阅读字体大小，继承自body
    var dpr = window.devicePixelRatio || 1
    
    function setBodyFont() {
        if(document.body) {
            document.body.style.fontSize = (12*dpr)+'px'
        }else {
            document.addEventListener('DOMContentLoaded', setBodyFont)
        }
    }
    setBodyFont()
})(window, document)
```

但是rem布局不是十全十美的，有如下缺点：

* 可能造成页面抖动
* 1px问题没有解决，需要自己手动解决。lib-flexible已经解决了这个问题

> 对于border-width不建议等比缩放。

### vw布局

```css
.box {
    /* 假设可视区域为375px，那么10vw就是375/100*10=37.5 */
    width: 10vw;				/* 37.5px */
    height: calc(10vw/2);		/* 18.75px */
}
```

## 响应式开发

上面都是单独制作移动端页面，而有的时候我们可能需要响应式开发，即一个页面兼容不同尺寸的设备，包括PC端和移动端。

响应式开发可以减少我们的工作量，但是缺点也很明显——兼容代码过多，影响加载速度。

### 媒体查询

响应式开发是基于媒体查询的，通过CSS3媒体查询`@media`属性可以针对不同的屏幕尺寸，设置不同的样式。

```css
@media mediaType and|not|only (media feature) {
    /* css code */
}
```

其中媒体类型有：

* screen：设备屏幕
* print：打印机
* all，所有设备，默认值

注意：如果设置了操作符就一定要设置媒体类型。

操作符用来链接媒体类型和媒体特性，具体有：

* and：且
* or：或
* not：非
* only：只有

注意：逗号等于or。

媒体特性有：

* max-width
* min-width

### 具体实现

**外联**

你可以通过媒体查询，针对不同的屏幕尺寸引入不同的样式文件。

```html
<link rel="stylesheet" href="./mobile.css" media="only screen and (max-width:750px)">
<link rel="stylesheet" href="./pc.css" media="only screen and (min-width:750px)">
```

**内联**

把屏幕分为超小屏幕，小屏幕，中等屏幕以及大屏幕，分别定义不同的CSS样式。

```css
/* 简单的例子 */
@media screen and (max-width: 400px) {
    .box {
        background-color: blue;
    }
}
@media screen and (max-width: 800px) {
    .box {
        background-color: red;
    }
}
```

### bootstrap

如果我们手动通过媒体查看来进行页面响应式开发，开发量不会小，所以你可以借助第三方框架——bootstrap。它是twitter开发的CSS框架，可以帮助开发者快速开发响应式页面。

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <!--如果在IE浏览器下，则使用最新的标准渲染当前页面-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap 101 Template</title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <!--[if lt IE 9]>
      <script src="https://cdn.bootcss.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script>
    <![endif]-->
    
    <!--其中HTML5 shiv and Respond.js for IE8 support of HTML5 elements and media queries -->

    <link href="css/demo.css" rel="stylesheet">    <!--引入自己的css文件-->
  </head>
  <body>
    <h1>你好，世界！</h1>

    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="js/bootstrap.min.js"></script>
    <script src="js/demo.js"></script>
    <!--引入自己的js文件-->
  </body>
</html>
```

上面模板中的`[if lt IE 9]`是HTML条件注释，还有如下值：

* `lte`：小于等于
* `gte`：大于等于
* `lt`：小于
* `gt`：大于

## 解决方案

* 禁止移动端手机号码识别

  ```html
  <meta name="format-detection" content="telephone=no">
  ```

* 清除点击高亮

  ```css
  a {
      -webkit-tap-highlight-color: transparent;
  }
  ```

* 移除原生控件样式

  ```css
  input[type="button"], input[type="submit"], input[type="reset"] {
  	-webkit-appearance: none;
  }
  ```

* 禁止长按页面弹出菜单

  ```css
  img, a {
      -webkit-touch-callout: none;
  }
  ```

* 1px问题。造成1px的原因是所有移动设备为了布局都统一设置了理想视口，而在retina屏幕的iphone上，dpr为2或3，CSS设置的1px会映射成物理像素2px或3px。解决办法：

  ```css
  /* 伪元素 */
  .box {
      position: relative;
      border: none;
  }
  .box::after {
      content: '';
  	position: absolute;
      bottom: 0;
      top: 0;
      width: 100%;
      height: 1px;
      transform: scaleY(0.5);
  }
  ```

  另一种解决办法就是把viewport的宽度设置为实际的设备物理宽度，这也是flexible的做法：

  ```js
  metaEl = doc.createElement('meta');
  metaEl.setAttribute('name', 'viewport');
  metaEl.setAttribute('content', 'initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
  ```

* 为了防止和双击缩放有冲突，移动端click事件300ms延迟。

  你可以设置meta标签的禁止缩放，这种方法不是很优雅。因为有的时候我们需要客户进行缩放查看，并且这种方式对Apple Safari浏览器不起作用。

  另一种解决办法是使用CSS的`touch-action`属性，这个属性指定了相应元素上能够触发的用户代理（也就是浏览器）的默认行为。如果设置为none，那么就不会触发用户代理的任何默认行为，但是这种方式兼容性不是很好。

  ```css
  button {
      touch-action: none;
  }
  ```

  还有一种解决办法是使用成熟的第三方库，例如fastClick。其原理就是在检测到touchend事件的时候，会通过DOM自定义事件立即模拟一个click事件，并把浏览器在300ms之后的click事件阻止掉。

* 移动端点击穿透问题。你可能会想到为什么不直接使用touchstart来代替click事件，这样不就解决了300ms延迟问题了吗？但是使用touchstart有两个问题：

  * touchstart手指触摸就触发，很多情况下用户只是想滑动屏幕
  * touchstart在某些场景下会出现点击穿透问题

  例如B元素在A元素之上，此时B元素绑定了touchstart事件回调，该回调函数会隐藏B元素。此时你触发B元素touchstart事件，你会发现A元素触发了Click事件。这是因为在移动端浏览器，事件执行的顺序是touchstart->touchend->click。而click事件有300ms的延迟，当touchstart事件把B元素隐藏之后，隔了300ms，浏览器触发了click事件，但是此时B元素不见了，所以该事件被派发到了A元素身上。如果A元素是一个链接，那此时页面就会意外地跳转。

  解决这个问题的方法就是不要混用touchstart和click事件。

* 下拉刷新和上拉加载

上面只列举了部分，更多请自行搜索。

## 移动端调试

* Chrome DevTools模拟调试
* 局域网内真机调试
* vconsole

## Hybird开发

### 简介

Hybird App，俗称混合应用，即融合了native和web技术进行开发的移动应用。下面三种技术主要在UI渲染机制上不同：

1. 基于Webview UI的基础方案。市面上主流，例如微信SDK，通过JsBridge完成H5和Native的双向通讯，从而赋予H5一定的原生能力。
2. 基于Native UI的方案，例如React-native、Weex等。在赋予H5原生API能力的基础上，进一步通过JsBridge将js解析成虚拟dom传递到native，并使用原生渲染。
3. 小程序方案。通过定制化的JsBridge，使用双Webview和双线程的模式隔离了JS逻辑和UI渲染，形成特殊的开发模式，加强了H5和原生的混合程度，提高了页面性能和开发体验。

### 优势

Hybird方案能利用H5强大的开发和迭代能力，又能赋予H5强大的底层能力和用户体验，同时能复用现有的成熟Native组件。

### 原理

Hybird App的本质，是在原生App中，使用Webview作为容器直接承载web页面。最核心的点就是Native和H5之间的双向通讯层，其实这里也可以理解为我们需要一套跨语言通讯方案，来完成Native（Java/Objective-c）与JavaScript的通讯。这个方案就是JsBridge。

**JavaScript通知Native**

基于Webview的机制和开放的API，实现这个功能有三种常见的方案：

- API注入，Native获取JavaScript环境上下文，并直接在上面挂载对象挥着方案，使js可以直接调用，但IOS和Android需要分别挂载。
- Webview中的prompt、console、alert拦截。
- Webview URL Scheme 跳转拦截。但是又限制长度，因此需要制定新的参数传递规则，使用函数调用的方式。原理是，Native可以直接调用JS方法并直接获取函数的返回值。

**Native通知JavaScript**

由于Native可以算作H5的宿主，因此拥有更大的权限，Native可以直接通过Webview API直接执行JS代码。

**App中H5的接入方式**

- 在线H5，客户端在Webview中直接打开URL。

好处：

1. 独立性强，有独立的开发、调试、更新、上线能力。
2. 资源通过服务端引入，不影响客户端体积。
3. 接入成本低，完全的热更新机制。

劣势：

1. 完全依赖网络，离线无法使用
2. 首屏加载依赖网络。

- 客户端内置H5。

优势：

1. 本地化，首屏加载快，体验接近原生。
2. 不依赖网络，离线运行。

劣势：

1. 开发流程、更新机制复杂，需要客户端和服务端共同合作
2. 增加App包体积