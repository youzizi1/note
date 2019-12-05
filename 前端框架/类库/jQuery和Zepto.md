## jQuery

### 概述

jQuery是一个JavaScript功能库，相较于原生JavaScript有如下优点：

* 强大的选择器
* 统一的API接口，不用担心兼容问题
* 轻易而又简洁的操作DOM
* Write less，Do more

另外，jQuery从2.xx版本开始部分支持IE8，不再支持IE6,7，所以更加精巧。注意：jQuery 3.x版本完全不支持IE8及其以下版本，并且提供不包含Ajax和动画API的版本。

### 起步

```html
<script src="jquery-3.2.1.min.js"></script>		<!--或者使用CDN引入库文件-->
<script>
	//入口函数1	
  	$(function(){
        
    });
  	//入口函数2
    $(document).ready(function(){
        
    })
</script>
```
jQuery入口函数可以出现多次，不会存在覆盖问题。而原生JavaScript入口函数只能出现一次，若出现多次，后面的入口函数会覆盖前面的入口函数。

### jQuery函数和jQuery对象

下面是从jQuery中摘录的一段源码：

```javascript
(function(window){
  var jQuery = function(){
  	return new jQuery.fn.init( selector, context, rootjQuery );
  }
  window.jQuery = window.$ = jQuery
})(window)
```

从源码中可以看出`jQuery`/`$`本质上是函数，执行这个函数返回`jQuery.fn.init`实例对象，这个实例对象又称为jQuery对象。也就是说，jQuery对象是通过执行jQuery函数产生。

```javascript
typeof $	//"function"
$("body") instanceof jQuery.fn.init	//true
$("body") instanceof jQuery			//true
$("body").__proto__ === jQuery.fn.init.prototype	//true
$("body").__proto__ === jQuery.prototype			//true
jQuery === jQuery.fn.init			//false,说明这两个函数并不相等，至于为什么jQuery对象的原型会同时是jQuery.fn.init和jQuery这两个函数的prototype属性，其实是jQuery库的内部实现
```

jQuery函数会根据参数不同，返回不同的值，但是返回的值都是jQuery对象。具体如下：

* 参数为函数：表示当DOM加载完成后，执行此回调函数。注意：将函数传入jQuery函数中，执行并返回的仍然是jQuery对象，只不过这个对象很少使用而已。

- 参数为选择器字符串：表示查找所有符合条件的DOM对象，并将这些DOM对象一起封装成jQuery对象返回。

  ```javascript
  $(".demo")[0]		//获取其中第一个DOM对象
  $(".demo").get(1)	//获取其中第二个DOM对象
  $(".demo").eq(2) instanceof jQuery.fn.init	
  //true，如果想要将jQuery对象中的某个位置的DOM对象封装成jQuery对象返回，则需要这样使用
  ```

- 参数为DOM对象：表示将DOM对象转换为封装成jQuery对象。

  ```javascript
  $("#demo").click(function(){
    alert($(this).html())		//this指向DOM对象，将DOM对象传入jQuery函数中，便将其转换为jQuery对象
  })
  ```

- 参数为html标签字符串：表示标签对应的DOM对象，并封装成jQuery对象

  ```javascript
  $("<span></span>").appendTo("div")		//将span标签插入所有div元素内部
  ```

注意：所有的jQuery对象都是类数组，包含了所有被选中的DOM元素(可能一个或者多个)。通过该对象的length属性可以获取选中了多少元素。例如：`$("div")`返回的就是一个jQuery对象，如果不存在div元素，那么就返回`[]`，而不像原生选择器那样返回undefined或null，这样设计的目的是为了让开发者不用再下一行判断该元素是否存在。

```javascript
$("div").length		
//或者
$("div").size()
```

### 释放$

绝大多数情况下，都是直接使用更加简洁的`$`，但是如果它不幸地被占用了，那么就需要让jQuery库把`$`控制权交出来，然后就只能使用`jQuery`全局变量。

```javascript
$ in window			// true
jQuery.noConflict()
$					// undefined
```

### jQuery对象方法

#### each()和map()

```javascript
$("li").each(function(i,ele){
  //ele === this
  console.log($(this).innerHTML)
})
$("li").map(function(i,ele){
  console.log(ele.innerHTML)
}).get()		//get()方法将返回的jQuery对象转换为数组
```

注意：jQuery会默认对当前结果集进行循环处理，所以`each()`和`map()`方法有时是不必要的 。

```javascript
$("div").addClass("demo")		//所有div元素都添加demo类名
```

#### index()

```html
<div>1</div>
<div id="demo">2</div>
<div>3</div>
<script>
	$("#demo").index()	//1
  	//index()方法得到该元素所在兄弟元素中的下标
</script>
```

#### is()方法

`is()`方法返回一个布尔值，表示选中的结果是否符合某个条件。这个用来验证的条件，可以是CSS选择器，也可以是一个函数，或者是DOM元素和jQuery实例。 

```javascript
$('li').is('li') 				// true
$('li').is($('.item')) 
$('li').is(document.querySelector('li'))
$('li').is(function() {
      return $("strong", this).length === 0;
});
```

#### closest()方法

`closest()`方法用于返回当前元素以及当前元素的所有上级元素中，第一个符合条件的元素。 

```javascript
$("li").closest("div")
```

#### add()，addBack()和end()方法

* `add()`方法用于为结果集添加元素
* `addBack()`方法用于将当前元素加回原始的结果集 
* `end()`方法用于返回原始的结果集

```javascript
$('li').add('p')
$('li').parent().addBack()
$('li').first().end()
```

#### serialize()和serializeArray()方法

* `serialize()`方法用于将表单元素的值，转为url使用的查询字符串
* `serializeArray()`方法用于将表单元素的值转为数组

```javascript
$( "form" ).on( "submit", function( event ) {
  event.preventDefault();
  console.log( $( this ).serialize() );
});
// single=Single&multiple=Multiple&check=check2&radio=radio1
$("form").submit(function (event){
  console.log($(this).serializeArray());
  event.preventDefault();
});
//	[
//		{name : 'field1', value : 123},
//		{name : 'field2', value : 'hello world'}
//	]
```

### 链式操作

jQuery最方便的一点就是，库中的大部分方法返回的都是jQuery对象，因此可以链式操作。

### jQuery选择器

#### 基本选择器

```javascript
// id选择器
$("#demo")
// class选择器
$(".demo")
$(".lee.sin")	//返回类名就是lee又是sin的元素
// 标签选择器
$("div")
// 通配符选择器
$("*")			//返回所有元素
// 并集选择器
$("div,span")	//返回所有div和span元素
// 交集选择器
$("div.demo")	//返回所有类名为demo的div元素
```

#### 层级选择器

```javascript
// 子代选择器
$("div>span")		//返回所有div中所有子span元素
// 后代选择器
$("div span")		//返回所有div中所有后代span元素
// 紧邻选择器
$("div+span")		//返回所有div的下一个兄弟span元素
// 兄弟选择器
$("div~span")		//返回所有div的后面所有兄弟span元素
```

#### 过滤选择器

```javascript
// 奇数选择器
$("li:odd")			//返回所有索引号为奇数的li元素
// 偶数选择器
$("li:even")		
// 索引选择器
$("li.eq(2)")		//返回索引号为2的li元素
// 大于选择器
$("li:gt(2)")		//返回所有索引号大于2的li元素
// 小于选择器
$("li:lt(2)")	
$("li:gt(1):lt(4)")	
//将索引号大于1的li元素重新拿出来编排，在其中找出新的索引号小于4的所有li元素。也就是说，jQuery选择器是从左往右解析的，而不是同时解析
// 首元素选择器
$("li:first")		//返回第一个li元素
// 尾元素选择器
$("li:last")	
```

#### 筛选选择器

```javascript
// 后代查找选择器
$("div").find("p")		//返回所有div元素中的所有后代p元素
//等价于
$("div p")
//等价于
$("p","div")			//选择器从右向左解析，所以优于上一种写法
// 子代查找选择器
$("div").children("p")
// 父代查找选择器
$("p").parent("div")
// 祖代查找选择器
$("p").parents("div")	//返回所有p元素的所有祖先p元素，直至根元素html
// 兄弟查找选择器
$("div").siblings()		//返回所有div元素的所有兄弟元素，不包括自身
// 索引查找选择器
$("li").eq(2)
// 前兄弟查找选择器
$("div").prev()			//返回所有div元素的前一个兄弟元素
$("div").prev("p")		//返回所有div元素的前一个兄弟p元素
// 后兄弟查找选择器
$("div").next()
$("div").next("p")
// 前所有兄弟查找选择器
$("div").prevAll()		//返回所有div元素的前面所有兄弟元素
$("div").prevAll("p")	//返回所有div元素的前面所有兄弟p元素
// 后所有兄弟查找选择器
$("div").nextAll()		
$("div").nextAll("div")	
// 首元素选择器
$("div").first()		//返回第一个div元素
// 尾元素选择器
$("div").last()
// 过滤选择器
$("div").filter(".demo")//返回所有类名为demo的div元素
$("div").filter(function(){
  return this.innerHTML.indexOf("s") === 0	//返回所有以s开头的div元素
})
// 排除选择器
$("div").not(".demo")	//返回所有类名不是demo的div元素，包含没有class属性的元素
```

#### 属性选择器

当时使用属性选择器选择元素时，如果属性值包含空格等特殊字符，那么就必须使用引号。

```javascript
$("a[href]")					//返回定义href属性的所有a元素
$("a[href=demo]")				//返回href属性为demo的所有a元素
$("a[href!='demo']")			//返回href属性不为demo的所有a元素
$("a[href^='demo']")			//返回href属性以demo开头的所有a元素
$("a[href$='demo']")			//返回href属性以demo结尾的所有a元素
$("a[href*='demo']")			//返回href属性包含demo的所有a元素
$("a[href='demo'][id='lee']")	//返回href属性为demo，id属性为lee的所有a元素
```

#### 表单选择器

```javascript
$(":input")				//返回所有input，textarea，select和button元素
$(":text")				//返回type属性为text的所有input元素
$(":passworld")			//返回type属性为password的所有input元素
$(":radio")				//返回type属性为radio的所有input元素
$(":checkbox")			//返回type属性为checkbox的所有input元素
$(":submit")			//返回type属性为submit的所有input元素
$(":reset")				//返回type属性为reset的所有input元素
$(":image")				//返回type属性为image的所有input元素
$(":file")				//返回type属性为file的所有input元素
$(":enabled")			//返回所有激活的input元素
$(":disabled")			//返回所有禁用的input元素
$(":checked")			//返回所有被选中的单复选框元素
$(":focus")				//返回当前获得焦点的元素
```

#### 其他选择器

```javascript
$(":header")			//返回所有标题元素
$(":animated")			//返回所有动画元素
$(":contains('demo')")	//返回包含demo文本的所有元素
$(":empty")				//返回所有无子元素的元素
$(":hidden")			//返回所有隐藏的元素
$("p:hidden")			//返回所有隐藏的p元素
$(":visible")			//返回所有可见的元素
```

#### 总结

通过jQuery选择的元素是按照它们在文档中出现的顺序排列的，并且不会有重复元素。例如：`<p class="red green"></p>`不会被`$("p.red,p.green")`选择两次。

### 样式和类操作

#### 样式操作

* 设置单个样式

  ```javascript
  $(selector).css("color","red")
  $(selector).css("backgroundColor","red")
  $(selector).css("background-color","red")
  ```

* 设置多个样式

  ```javascript
  $(selector).css({
      "color":"red",
    	"font-soze": "20px"	//jQuery为了追求简洁，允许不加px单位，即"font-size":20
  })
  ```


* 获取样式

  ```javascript
  $(selector).css("color")
  ```

另外，通过`css()`方法改变的样式是定义元素的`style`属性中，具有最高优先级。

#### 类操作

* 添加类

  ```javascript
  $(selector).addClass("demo")	//注意：和attr()方法一样，该方法不会覆盖原有的值
  ```


* 移除类

  ```javascript
  $(selector).removeClass("demo")
  $(selector).removeClass()			//移除所有类
  ```

* 包含类

  ```javascript
  $(selector).hasClass("demo")		//包含demo类，则返回true
  ```

* 切换类

  ```javascript
  $(selector).toggleClass("demo")
  ```

关于类和样式操作可以总结为如下两点：

* 如果操作样式很少，可以通过`$(selector).css()`方法操作
* 如果操作样式很多，建议通过类操作

### 动画

jQuery提供一个方法，可以很容易地显示网页动画效果。但是它不如CSS动画强大和节约资源，所以优先考虑使用CSS动画。另外，如果将jQuery.fx.off设为true，就可以将所有动画效果关闭，使得网页元素的各种变化一步到位，没有中间过渡的动画效果。

#### 显示隐藏

通过`css()`方法改变`display`属性可以实现显式和隐藏一个DOM元素，这种方式有一个缺点是：在显示元素时，需要记住该元素的原始`display`属性值。 而`show()`方法和`hide()`方法总是能显示和隐藏元素，并且不用关心它原有的`display`属性值。

* 显示元素

  ```javascript
  $(selector).show()						//通过改变宽高和opacity属性实现
  $(selector).show("fast")				//fast=200ms normal=400ms slow=600ms
  $(selector).show(600)					//单位是ms
  $(selector).show("normal",function(){	//第一个参数是动画完成时间
      console.log("animate finished")		//第二个可选参数是回调函数
  })
  ```

* 隐藏元素

  ```javascript
  $(selector).hide()
  $(selector).hide(2000)
  $(selector).hide("fast")
  $(selector).hide("slow",function(){})
  ```

#### 滑动动画

* 向下滑动元素

  ```javascript
  $(selector).slideDown()						//通过改变height属性实现
  $(selector).slideDown(2000)					//默认是400ms
  $(selector).slideDown("fast")
  $(selector).slideDown("slow",function(){})
  ```

* 向上滑动元素

  ```javascript
  $(selector).slideUp()
  ```


* 切换滑动效果

  ```javascript
  $(selector).slideToggle()
  ```

#### 淡入淡出

* 淡入元素

  ```javascript
  $(selector).fadeIn()					//通过改变opacity属性实现
  $(selector).fadeIn(2000)				//默认是400ms
  $(selector).fadeIn("fast")
  $(selector).fadeIn("slow",function(){})
  ```

* 淡出元素

  ```javascript
  $(selector).fadeOut()
  ```

* 切换淡入淡出

  ```javascript
  $(selector).fadeToggle()
  ```

* 透明到某个程度

  ```javascript
  $(selector).fadeTo(1000,0.5,function(){		//第一个参数表示动画执行事件
      console.log("animate finished")			//第二个参数表示0-1之间的透明度
  })											//第三个可选参数表示回调函数
  ```

#### 自定义动画

通过`$(selector).animate()`方法可以执行 CSS 属性集的自定义动画，但是并不是所有的CSS属性都可以做动画，详细参考W3C官网查看。

```javascript
//写法1
$(selector).animate({				//第一个参数是规定产生动画效果的 CSS 样式和值
    "height":"200px"				//属性必须和CSS属性一致，如果有连字符，需要使用驼峰命名
},"normal",function(){				//第二个可选参数是动画的执行时间
    console.log("animate finished")	//第三个可选参数是回调函数
})
//写法2
$(selector).animate(styles,options)	//参考W3C官网	
// case2
$("div").animate({	
  left: 100,
  top: 100
},1000)				//移动到指定位置
$("div").animate({
  left: "+=100",
  top: "+=100"
})					//移动了指定距离
```

#### 延迟动画

`delay()`方法接受一个时间参数，表示暂停多少毫秒后继续执行。

#### 停止动画

如果元素上定义了多个动画效果，那么其中还未执行的所有动画会加入到动画队列，直到当前动画执行完后，动画队列中的动画才会执行。而`$(selector).stop()`方法可以当前停止正在执行的动画，包括自定义动画，该方法接受两个参数：

* 第一个参数表示动画队列中的动画要执行，true表示不执行，false表示执行
* 第一个参数表示当前动画是否执行完，true表示立即执行完，false表示立即停止执行

#### 自定义关键字

jQuery预定义的关键字定义在jQuery.fx.speeds对象上，所以通过该对象可以修改或自定义关键字。

```javascript
jQuery.fx.speeds.fast = 50;
jQuery.fx.speeds.slow = 3000;
jQuery.fx.speeds.normal = 1000;
jQuery.fx.speeds.blazing = 30;			//自定义blazing关键字
```

#### 总结

在实际开发中，可能会遇到某些元素设置动画没有任何效果，这是因为jQuery动画都是改变CSS属性值来实现的，例如：width，height属性等。而这些元素本身就是行内元素，设置宽高根本不会起作用。

另外，jQuery也没有实现对`background-color`属性的动画效果，这种情况下需要使用CSS3的`transition`属性实现。

### DOM操作

#### 创建元素

```javascript
$("<span>helloworld</span>")		
```

#### 清空元素

* `empty()`方法用于清空当前元素的内部元素，包括当前元素中的文本
* `remove()`方法用于移除当前元素，同时也会清空其内部元素，包括移除当前元素绑定的事件
* `detach()`方法和`remove()`方法用法一样，但是它不会取消该元素上所有事件的绑定
* `replaceWith()`方法用于将参数元素替换并返回当前元素，它同样会取消当前元素的所有事件绑定 
* `replaceAll()`方法和上面方法的区别

```javascript
// 保留自身
$(selector).empty()
$(selector).html("")		//推荐使用
// 包括自身
$(selector).remove()
```
#### 添加元素

除了使用`html()`方法添加DOM元素，jQuery还提供如下一系列方法添加DOM元素。这些方法中的参数可以是HTML片段，原生DOM对象，jQuery对象和函数。另外，如果要添加的DOM节点已经存在于文档中，那么它会先从文档中移除，然后再添加。也就是说，这些方法可以移动一个DOM节点。

* `append()`和`appendTo()`方法。其中前者是将参数中的元素插入当前元素的尾部，而后者是将当前元素插入参数元素尾部。

  ```javascript
  $(selector).append("<span>hello</span>")
  $(selector).append(document.forms[0])
  $(selector).append($(selector),$(selector))		//可以添加多个DOM元素
  $(selector).append(function(index,html){
      return '<span>' + index + '</span>'		
     //当传入函数时，必须返回一个HTML片段，DOM对象或者jQuery对象
  })
  $("<p>World</p>").appendTo("div")
  ```

* `prepend()`和`prependTo()`方法。前者将参数元素变为当前元素的第一个子元素，而后者将当前元素变为参数元素的第一个子元素。

* `after()`和`insertAfter()`方法。前者会将参数元素插在当前元素后面，而后者将当前元素插在参数元素后面。

* `before()`和`insertBefore()`方法。前者将参数元素插在当前元素的前面，而后者将当前元素插在参数元素的前面。

* `wrap()`，`wrapAll()`，`unwrap()`和`wrapInner()`方法。其中`wrap()`方法用于将参数元素变成当前元素的父元素；`wrapAll()`方法为结果集的所有元素添加一个共同的父元素；`unwrap()`方法用于移除当前元素的父元素；`wrapInner()`方法为当前元素的所有子元素添加一个父元素。 

#### 克隆元素

通过`clone()`方法可以克隆元素。注意：对于那些有id属性的节点，该方法会连同id属性也一起克隆。所以克隆后，务必修改或移除id属性。

```javascript
$(selector).clone()
```

### 属性操作

#### 设置和获取属性

通过`attr()`方法可以设置或获取元素的属性。

```javascript
$(selector).attr("id","demo")		//注意：如果原来就有id属性，那么会覆盖原来的值
$(selector).attr("class")			//如果未定义该属性，则返回undefined
```

#### 删除属性

通过`removeAttr()`方法可以删除元素属性。

```javascript
$(selector).removeAttr("id")
```

#### prop()和is()

对于处理HTML5中的布尔类型属性(checked，selected)，通常使用`prop()`方法而不是`attr()`方法。当然对于这类属性，更好的方法是使用`is()`方法。

```html
<input type="radio" checked />	
<script>
  	$(function(){
        $("input").prop("checked")		//true，返回值更加合理
      	$("input").attr("checked")		//"checked"
      	$("input").is(":checked")		//true
    })
</script>
```

#### 自定义属性

通过`data()`方法可以在一个DOM对象上存储数据。 

```javascript
$("body").data("foo", 52);		//存储数据
$("body").data("foo");			//读取数据
```

### 文本操作

* 通过`text()`方法可以设置或获取元素的纯文本内容
* 通过`html()`方法可以设置或获取元素的内容，包括HTML标记

```html
<ul>
  <li>1</li>
  <li>2</li>
</ul>
<script>
	$(function(){
       $("ul li").text()				//12，将匹配元素的文本内容拼接后返回
       //这是因为如果一个jQuery对象包含多个DOM对象，那么它的方法会作用于每个元素
       $("ul li").text("jQuery")		//设置每个li元素内容为jQuery文本
       $("ul li").html("<a>hi</a>")		//设置每个li元素内容为a标签
       $("ul li").html()				//"<a>hi</a>"，不需要拼接后返回
    })
</script>
```

另外，这两个方法还可以接受一个函数作为参数，函数的返回值就是网页元素所有包含的新的代码和文本。这个函数参数接受两个参数，第一个参数表示网页元素在集合中的位置，第二个参数表示网页元素原来的代码或文本。

```javascript
$("li").html(function(i,v){
    return (i + ':' + v)
})
/*
	0:1
	1:2
*/
```

### 表单操作

通过`val()`方法可以设置或获取表单元素的`value`属性值。

```javascript
$(selector).val()
$(selector).val("button")
```

### 尺寸和位置

#### 尺寸操作

* `height()`方法用于设置或获取内容高度
* `width()`方法用于设置或获取宽度 
* `innerHeight()`:height+padding
* `innerWidth()`:width+padding
* `outerWidth()`:width+padding+border
* `outerHeight()`:height+padding+border

注意：对于`outerWidth()`和`outerHeight()`方法来说，它们可以传递参数。如果传递true，那么尺寸就还要加上margin；如果不传参数或者传递false，那么就不会加上margin

```javascript
$(window).width()			//获取浏览器可视窗口宽度，不带单位
$(window).height()			//获取浏览器可视窗口高度，不带单位

$("#demo").width(200)		//设置匹配元素的宽度，是否生效取决于CSS是否生效
$("#demo").width("200px")	//设置匹配元素的宽度，是否生效取决于CSS是否生效
```

注意：通过`css()`方法获取的尺寸带有单位。

#### 坐标值操作

`offset()`方法用于设置或获取元素相对于文档的位置。注意：如果当前元素没有定位，那么设置该方法后，该元素会被设置为相对定位。另外，`position()`方法返回的相对于父元素的位置，但是该方法不能加参数。

```javascript
$(selector).offset()
$(selector).offset({left:100,top:100})
```

还有滚动方法`scrollTop()`和`scrollLeft()`方法，其中第二个方法很少使用。

```javascript
//某个元素的滚动
$("div").scrollTop()		//获取某个元素内部滚动条滚动的高度
$("div").scrollTop(100)		//设置该元素内部滚动条滚动的高度
//浏览器的滚动
$("body").scrollTop()		//获取浏览器滚动的高度
$("body").scrollTop(100)	//设置浏览器滚动的高度
//注意：在IE浏览器需要使用document.documentElement来获取滚动条的位置。所以兼容写法如下
$("body").scrollTop()+$("html").scrollTop()
//而设置浏览器滚动条的位置的兼容写法如下：
$("html,body").scrollTop(100)	//两个元素总有一个生效
```

### jQuery事件

对于简单事件来说，可以直接使用事件函数来绑定回调函数。

```javascript
$(selector).click(function(){
    alert("click me");
})
```

当然，更好的等价方式是使用`on`方法来绑定事件回调函数。该方法允许一次为多个事件指定相同的回调函数。注意：第一种方式不会覆盖已有的回调函数。

```javascript
$(selector).on("click hover",function(){
    alert("click or hover me")
})
```

另外，`on()`方法还可以为当前元素的某一个子元素添加回调函数。 这种写法有两个好处。首先click事件还是在ul元素上触发，但是会检查event对象的target属性是否为li子元素，如果是true，再调用回调函数。这样就比为li元素一一绑定回调函数，节省了内存空间。其次，这种绑定的回调函数，对于在绑定后生成的li元素依然有效。

```javascript
$("ul").on('click','li',function(e){		//子元素作为第二个参数
    console.log(this);
})
```

`on()`方法还允许向回调函数中传入数据。

```javascript
$("ul" ).on("click", {name: "张三"}, function (event){
	console.log(event.data.name);
});
```

常见的事件如下：

* 鼠标事件
  * click：鼠标单击时触发

  * dbclick：鼠标双击时触发

  * mouseenter：鼠标进入时触发

  * mouseleave：鼠标移出时触发

  * mousemove：鼠标元素内部移动时触发，也就是说移入内部子元素也触发

  * hover：鼠标进入和退出时会触发，相当于mouseenter加上mouseleave

    ```javascript
    $(selector).hover(handlerIn, handlerOut)
    // 等同于
    $(selector).mouseenter(handlerIn).mouseleave(handlerOut)
    $("div").hover(function(){
        $(this).addClass("demo")		//hover事件开始时，添加demo类
    },function(){
        $(this.removeClass("demo"))		//hover事件结束时，移除demo类
    })
    //简化
    $("div").hover(function(){
        $(this).toggleClass("demo")
    })
    ```
* 键盘事件：键盘事件仅作用在当前焦点的DOM上，通常是input和textarea元素
  * keydown：键盘按下时触发
  * keyup：键盘松开时触发
  * keypress：按一次键后触发
* 其它事件
  * foucs：当DOM获得焦点时触发
  * blur：当DOM失去焦点时触发
  * change：当input，select和textarea元素的内容发生改变时触发
  * submit：当form表单提交时触发
  * ready：当页面被载入并且DOM树初始化后触发。该事件只作用于document对象。

通过`off()`方法可以解除已绑定的事件。

```javascript
function fn(){
    console.log("hi")
}
$(selector).click(fn)
$(selector).off('click',fn)			//注意：一定要是具名函数，匿名函数无法解绑
$(selector).off('click')			//这样就可以移除所有已绑定的事件
//事件对象
e.offsetX	//以元素左上角为准
e.pagesX	//以页面左上角为准
e.clientX	//以视口左上角为准
```

#### 事件触发

事件的触发总是由用户操作引起的，如果是开发者通过JavaScript代码导致DOM状态发生改变是不会触发相应的事件。但是有时候，希望用代码触发事件，这时就需要使用无参数的事件方法来触发该事件。注意：使用无参数的事件方法触发该事件和使用`trigger()`方法触发该事件都不会引发浏览器对该事件的默认行为。

```javascript
$("#input-name").val("change it").change()		
//属性值发生改变但是不会触发change事件，除非调用change()方法
$("#input-name").val("change it").trigger("change")		//等价于trigger()方法
```

#### 事件命名空间

同一个事件有时绑定了多个回调函数，这时如果想移除其中的一个回调函数，可以采用名称空间的方式，即为每一个回调函数指定一个二级事件名，然后再用`off()`方法移除这个二级事件的回调函数。

```javascript
$('li').on('click.logging', function (){
  console.log('click.logging callback removed');
});

$('li').off('click.logging');
$('li').trigger('click.logging')		//对trigger()方法也适用
```

#### 事件委托

子元素的事件交于父元素代为处理，称为事件委托或者事件代理。事件委托的另一用处就是动态创建的元素无法绑定事件，而直接在其父元素上可以代为处理。注意：回调函数中的this是发生事件的元素，即e.target元素

移除事件委托使用undelegate

### Ajax

jQuery在全局对象`jQuery`上绑定了`ajax()`方法，用来处理AJAX请求。该方法接受两个参数，第一个参数是URL，第二个参数是可选的 配置对象(当然第一个url参数也可以写在第二个参数中)，具体配置选项如下：

* async：是否异步请求AJAX请求，默认是true
* method：请求的方式，默认是`"GET"`
* contentType：发送POST请求的格式，默认是`'application/x-www-form-urlencoded; charset=UTF-8'`。还可以指定为`text/plain`、`application/json`。 
* data：发送的数据，可以是字符串、数组和对象。如果是GET请求，数据将被转换成查询字符串附加到URL上。如果是POST请求，会根据contentType把数据序列化成合适的格式。
* headers：发送的额外的HTTP头信息，必须是一个对象
* dataType：接受的数据格式，可以指定为`"html"`、`"xml"`、`"json"`、`"text"`等，缺省情况下会根据响应的Content-Type猜测。

```javascript
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
}).done(function (data) {
    console.log('成功, 收到的数据: ' + JSON.stringify(data));		//处理返回的数据
}).fail(function (xhr, status) {
    console.log('失败: ' + xhr.status + ', 原因: ' + status);	   //处理请求失败
}).always(function () {
    console.log('请求完成: 无论成功或失败都会调用');
});
```

#### get请求

对于常见的AJAX请求，jQuery提供了一些辅助方法。

```javascript
var jqxhr = $.get("/path/to/resource",{
    name: "Bob Lee",
  	check: 1
});
/*
get()方法的第二个参数如果是对象，会被自动转换为query string附加在URL后面，
这个过程会自动进行转码，所以实际URL是
/path/to/resource?name=Bob%20Lee&check=1
*/
```

#### post请求

`post()`方法和`get()`方法类似，但是传入的第二个参数默认被序列化为`application/x-www-form-urlencoded`。 

```javascript
var jqxhr = $.post('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
})
//实际构造的数据nameBob%20Lee&check=1会作为POST的body被发送
```

#### getJSON

由于JSON被普遍适用，所以jQuery还定义了`getJSON()`方法来快速发送GET请求获取一个JSON对象。

```javascript
var jqxhr = $.getJSON("/path/to/resource",{
    name: "Bob Lee",
  	check: 1
}).done(function(data)){
 	//data已经被解析成JSON对象了         
 }
```

#### 安全限制

jQuery的`ajax()`方法完全是封装原生JavaScript的AJAX操作，所以安全限制和发送原生AJAX请求一样。另外，如果需要使用JSONP，可以在`ajax()`方法中设置`jsonp:"callback"`，实现JSONP跨域加载数据。

### jQuery插件

插件就是用户自己新增的jQuery实例对象的方法。由于该方法要被所有jQuery对象实例共享，所以只能定义在jQuery构造函数的原型对象(prototype)上，而这个原型对象被简写成`jQuery.fn`对象，所以只需要在`jQuery.fn`或`$.fn`上绑定一个新方法便可实现一个jQuery插件。

```js
$.fn.hg = function() {
    this.css({
        color: 'red'
    })
    return this
}

$(function() {
    $('.app').hg()
});
```

当然你还可以传递一些默认配置：

```js
(function($, window) {
    $.fn.hg = function(cssOptions) {
        const defaultCssOptions = {
            color: "red"
        };
        const options = $.extend({}, defaultCssOptions, cssOptions);
        this.css(options);
        return this;
    };
})(jQuery, window);

$(function() {
    $(".app").hg();
});
```

#### 自定义方法

通过`extend()`可以自定义工具方法。

```js
$.extend({
  min:function(a,b){
    return a < b ? a : b
  }
})
$.min(1,2)		// 1
```

当然该方法还可以自定义实例方法。

```js
$.fn.extend({
  getText:function(){
    return this.html()
  }
})
$("div").getText()
```

#### 针对特定元素

jQuery中的一些方法只能作用于特定的DOM元素，例如`submit()`只能作用于form元素，因此需要使用`filter()`方法来实现针对特定元素的扩展插件。

```javascript
$.fn.plugins = function(){
    // 只有this指向下的span标签会生效
    this.filter("span").css("color","red");
}
```

### jQuery工具方法

jQuery本质上是函数，而函数也是一种特殊对象，所以jQuery函数本身定义了一些方法，这些方法一般具有类型通用工具的性质，所以称为工具方法。

```javascript
//$.trim()：该方法用于移除字符串头部和尾部多余的空格
//$.each()：该方法用于遍历数组和对象，然后返回原始数组或对象。它接受两个参数，分别是数据集合和回调函数
$.each([1,2],function(i,ele){
  console.log(this,i,ele)
})
// Number {1} 0 1
// Number {2} 1 2
// [1,2]
var obj = {
  p1: "hello",
  p2: "world"
};
$.each( obj, function( key, value ) {
  console.log( key + ": " + value );
});
// p1: hello
// p2: world
// obj
```

#### contains()方法

`$.contains()`方法返回一个布尔值，表示第二个参数DOM元素是否是第一个参数DOM元素的下级元素。

```javascript
$.contains(document.documentElement,document.body)	//true
```

`$.map()`方法也是用来遍历数组和对象，但是会返回一个新对象。

```javascript
var a = ["a", "b", "c", "d", "e"];
a = $.map(a, function (n, i){
  return (n.toUpperCase() + i);
});
// ["A0", "B1", "C2", "D3", "E4"]
```

#### inArray()

`$.inArray()`方法返回一个值在数组中的位置。如果该值不在数组中，则返回-1。

```javascript
var a = [1,2,3,4];
$.inArray(4,a) 		// 3
```

#### extend()方法

`$.extend()`方法用于将多个对象合并进第一个对象。

```javascript
var o1 = {p1:'a',p2:'b'};
var o2 = {p1:'c'};

$.extend(o1,o2);
o1.p1 // "c"
```

$.extend的另一种用法是生成一个新对象，用来继承原有对象。这时，它的第一个参数应该是一个空对象。

```javascript
var o1 = {p1:'a',p2:'b'};
var o2 = {p1:'c'};

var o = $.extend({},o1,o2);
o
// Object {p1: "c", p2: "b"}
```

默认情况下，extend方法生成的对象是“浅拷贝”，也就是说，如果某个属性是对象或数组，那么只会生成指向这个对象或数组的指针，而不会复制值。如果想要“深拷贝”，可以在extend方法的第一个参数传入布尔值true。

```javascript
var o1 = {p1:['a','b']};

var o2 = $.extend({},o1);
var o3 = $.extend(true,{},o1);

o1.p1[0]='c';

o2.p1 // ["c", "b"]
o3.p1 // ["a", "b"] 
```

#### proxy()方法

`$.proxy()`方法类似于ECMAScript 5的bind方法，可以绑定函数的上下文this和参数，返回一个新函数。

```javascript
var o = {
	type: "object",
	test: function(event) {
		console.log(this.type);
	}
};

$("#button")
  .on("click", o.test) // 无输出
  .on("click", $.proxy(o.test, o)) // object
//等价于
$("#button").on("click",$.proxy(o,test))	//将o的test方法与o绑定
```

上述结果表明，该方法的写法主要有如下两种。第一种是函数指定上下文对象，而第二种写法是指定上下文对象和它的某个方法名。

这个例子表明，proxy方法的写法主要有两种。

```javascript
jQuery.proxy(function, context)
// or
jQuery.proxy(context, name)
//例子
$('#myElement').click(function() {
    setTimeout(function() {
        $(this).addClass('aNewClass');		
    }, 1000);
});
//setTimeout使得回调函数在全局环境中允许，this指向全局，导致出错。所以使用该方法改进为如下
$('#myElement').click(function() {
    setTimeout($.proxy(function() {
        $(this).addClass('aNewClass'); 
    }, this), 1000);
});
```

#### data()和remove()方法

`$.data()`方法可以用来在DOM节点上储存数据。

```javascript
// 存入数据
$.data(document.body, "foo", 52 );

// 读取数据
$.data(document.body, "foo");

// 读取所有数据
$.data(document.body);
```

而`$.removeData()`方法用于移除由`$.data()`方法存储的数据。 

```javascript
$.data(div, "test1", "VALUE-1");
$.removeData(div, "test1");
```

#### parseHTML()，parseJSON()和parseDOM()方法

`$.parseHTML()`方法用于将字符串解析为DOM对象。`$.parseJSON()`方法用于将JSON字符串解析为JavaScript对象，作用与原生的JSON.parse()类似。但是，jQuery没有提供类似JSON.stringify()的方法，即不提供将JavaScript对象转为JSON对象的方法。而`$.parseXML()`方法用于将字符串解析为XML对象。注意：`$.parseJSON()`方法在3.x版本不建议使用，建议改用原生的`JSON.parse()`方法。

#### makeArray()方法

`$.makeArray()`方法将一个类似数组的对象，转化为真正的数组。

#### merge()方法

`$.merge()`方法用于将第二个参数数组合并到第一个参数数组中。

```javascript
var a1 = [0,1,2];
var a2 = [2,3,4];
$.merge(a1, a2);

a1
// [0, 1, 2, 2, 3, 4]
```

#### now()方法

`$.now()`方法返回当前时间距离1970年1月1日00:00:00 UTC对应的毫秒数，等同于(new Date).getTime()。

```javascript
$.now()			// 1388212221489
```

#### 判断数据类型的方法

jQuery提供一系列工具方法，用来判断数据类型，以弥补原生JavaScript的typeof运算符的不足。

- jQuery.isArray()：是否为数组。
- jQuery.isEmptyObject()：是否为空对象（不含可枚举的属性）。
- jQuery.isFunction()：是否为函数。
- jQuery.isNumeric()：是否为数值（整数或浮点数）。
- jQuery.isPlainObject()：是否为使用“{}”或“new Object”生成的对象，而不是浏览器原生提供的对象。
- jQuery.isWindow()：是否为window对象。
- jQuery.isXMLDoc()：判断一个DOM节点是否处于XML文档之中。

另外，还有一个`$.type()`方法，可以返回一个变量的数据类型，返回结果等同于使用`Object.prototype.toString()`方法读取对象内部的[[Class]]属性。

```js
$.type(/test/) // "regexp"
```

### deferred

deferred对象是在jQuery1.5版本中引入，是通过调用jQuery.Deferred()方法创建一个可链式调用的工具对象。 它可以注册多个回调到回调队列， 调用回调队列，准备代替任何同步或异步函数的成功或失败状态。 

简单来说，deferred对象就是jQuery的回调函数解决方案。

### 主要功能

**Ajax操作的链式写法**

```js
$.ajax('/test.json')
	.done(() => console.log('success'))
	.fail(() => console.log('fail'))
```

在1.5之前的版本`$.ajax()`方法返回的是XHR对象，无法进行链式操作；而高于1.5版本的则是返回jqXHR对象，deferred对象实例，可以进行链式操作。其中，done相当于`success`方法，fail相当于`error`方法。

注意：1.5版本返回的jqXHR实现了Promise接口，所以你可以使用Promise的所有属性和方法。但是jQuery回了让回调函数统一，也提供了done，fail和always方法。

```js
$.ajax('/test.json')
	.then(res => console.log(res))		// 等价于done
```

**同一个操作多个回调**

你可以添加多个回调函数。

```js
$.ajax('/test.json')
	.done(() => console.log('success'))
	.fail(() => console.log('fail'))
	.done(() => console.log('第二个回调'))
```

**多个操作指定一个回调**

```js
$.when($('/test1.json'), $.ajax('/test2.json'))
	.done(() => console.log('success'))
	.fail(() => console.log('fail'))
```

都成功了才会指定done回调，否则就会进入error回调。

**会同操作的回调函数接口**

deferred对象的最大优点是——无论是同步还是异步操作，都可以任意的添加回调函数。

```js
const add = (a, b) => {
    console.log(a + b);
};

$.when(add(1, 2)).then(() => {
    console.log("回调");
});
```

当然deferred对象上还有很多方法，自行查看API文档。

## Zepto

### 基本概念

zepto是轻量级的JavaScript库，专门为移动端定制的框架，并且与jQuery有着类似的API。

zepto的特点：

- 针对移动端
- 轻量级，压缩后只有8kb左右
- 语法、API和jQuery类似，学习成本低

和jQuery的区别：

- jQuery针对的更多是PC端
- jQuery体积较重
- jQueryAPI更加完善

### 触摸事件

- tap事件

  ```javascript
  $("div").tap(function(){
    console.log("demo元素tap事件委托给document元素处理")
  })
  ```

- singleTap事件：表示单击事件

- doubleTap事件：表示双击事件

  ```javascript
  $("div").singleTap(function(){
    console.log("单击事件")
  })
  $("div").doubleTap(function(){
    console.log("双击事件")
  })
  ```

- longTap事件：表示长按事件

  ```javascript
  $("div").longTap(function(){
    console.log("长按事件")
  })
  ```

- swipe事件：表示滑动事件。另外，该事件还有四个方向的派生事件，swipeLeft，swipeRight，swipeDown和swipeUp。注意：当手指往一个方向滑行超过30px才算滑动，否则算点击。

  ```javascript
  $("div").swipe(function(){
    console.log("滑动事件")
    /*
    	注意：swipe事件的默认行为是翻页，所以需要选中该元素去掉其默认的翻页行为
      div{
      	touch-action:none
      }
    */
  })
  ```

















