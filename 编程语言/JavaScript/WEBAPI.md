### HTML网页元素

#### id属性和name属性

由于历史原因，HTML元素的`Element.id`属性会自动成为全局变量，指向该元素本身，此时对该全局变量赋值，就会覆盖掉原有的值。由于这种原因，默认的全局变量名(例如history，location等)最好不要设置为`Element.id`属性名。注意：如果已有同名全局变量，那么该属性名不会自动成为全局变量。

同样是因为历史原因，部分HTML元素的`Element.name`属性也会自动成为全局变量，指向该元素本身。这些元素有applet元素，area元素，embed元素，form元素，frame元素，frameset元素，iframe元素，img元素和object元素。注意：如果元素的`Element.name`属性同名，或者某个元素的`Element.id`属性与另一个元素的`Element.name`属性同名，此时全局变量会指向一个类数组对象。

另外，iframe元素的`Element.name`属性和`Element.id`属性不是指向该元素本身，而是指向这个iframe窗口的window对象。

```html
<div id="lee"></div>
<form name="sin"></form>
<img name="sin" />	
<script>
	lee		//<div></div>
    sin[0]	//<form>
  	sin[1]	//<img />
</script>
```

另外，这些元素的`Element.name`属性名也会成为document对象的属性。如果有多个该属性相同的元素，那么document对象的该属性返回一个NodeList对象实例。这样设计的目的是为了通过document对象获取该元素，但是因为这些属性名会自动生成全局变量，既不规范也不利于出错，所以不建议使用它们。

```html
<form name="lee"></form>
<form name="hi"></form>
<img name='hi' />
<script>
	document.lee === lee		//true
  	document.hi === hi			//false，左边是NodeList对象，右边是一个普通的类数组对象，虽然成员一样，但是仍然返回false
  	document.hi[0] === hi[0]	//true
</script>	
```

除了自动成为window和document对象的属性，这些两个属性还会自动成为集合对象的属性，指向该元素本身。

```html
<form name='lee'></form>
<script>
	document.forms['lee'] === document.forms.lee	//true
</script>
```

#### form元素

表单元素主要用于收集用户的输入，送到服务器或者在前端处理。通过`document.forms`属性可以获取页面中的表单元素，该属性返回一个HTMLCollection对象实例，包含了页面中所有的表单元素。通过表单元素的`Element.id`或`Element.name`属性或者索引可以获取对应的表单元素。

另外，表单元素上还定义了`HTMLFormElement.elements`属性，包含了当前表单的所有子元素 。

```html
<form>
	<input name="lee" type="text" />  
  	<input name="sin" type="text" />
  	<input name="sin" type="text" />
</form>	
<script>
	document.forms[0].elements[0] === document.forms[0].elements.lee	//true
  	document.forms[0].elements.sin.length		//2
  	//如果有多个同名name属性，那么返回的就是类数组对象
</script>
```

##### Form对象

Form对象上除了定义`Form.elements`属性之外，还定义了如下属性和方法：

* `Form.action`属性
* `Form.encoding`属性
* `Form.method`属性
* `Form.target`属性
* `Form.onsubmit`属性
* `Form.onreset`属性
* `Form.submit`方法
* `Form.reset`方法

##### 表单控件对象

表单包含了各种控件，每个控件都是一个对象。它们都包含了以下四个属性。

- `type`：表示控件的类型，对于`<input>`元素、`<button>`元素等于这些标签的`type`属性，对于其他控件，`<select>`为`select-one`，`<select multiple>`为`select-multiple`，`<textarea>`为`textarea`。该属性只读。
- `form`：指向包含该控件的表单对象，如果该控件不包含在表单之中，则返回`null`。该属性只读。
- `name`：返回控件标签的`name`属性。该属性只读。
- `value`：返回或设置该控件的值，这个值会被表单提交到服务器。该属性可读写。 才会 `form`属性有一个特别的应用，就是在控件的事件回调函数里面，`this`指向事件所在的控件对象，所以`this.form`就指向控件所在的表单，`this.form.x`就指向其他控件元素，里面的`x`就是该控件的`name`属性或`id`属性的值。

表单控件之中，只要是按钮，都有`onclick`属性，用来指定用户点击按钮时的回调函数；其他的控件一般都有`onchange`属性，控件值发生变化，并且该控件失去焦点时调用。单选框（Radio控件）和多选框（Checkbox控件）可以同时设置`onchange`和`onclick`属性。

表单控件还有以下两个事件。

- `focus`：得到焦点时触发
- `blur`：失去焦点时触发

##### Select元素

`<select>`元素用来生成下拉列表。默认情况下，浏览器只显示一条选项，其他选项需要点击下拉按钮才会显示。`size`选项如果大于1，那么浏览器就会默认显示多个选项。

```
<select size="3">

```

上面代码指定默认显示三个选项，更多的选项需要点击下拉按钮才会显示。

`<select>`元素默认只能选中一个选项，如果想选中多个选项，必须指定`multiple`属性。

```
<select multiple>

```

用户选中或者取消一个下拉选项时，会触发`Select`对象的`change`事件，从而自动执行`onchange`监听函数。

`<select>`元素有一个`options`属性，返回一个类似数组的对象，包含了所有的`<option>`元素。

```
// HTML 代码为
// <select id="example">
//   <option>1</option>
//   <option>2</option>
//   <option>3</option>
// </select>

var element = document.querySelector('#example');
element.options.length
// 3

```

上面代码中，`<select>`元素的`options`属性包含了三个`<option>`元素。

`options`属性可读写，可以通过设置`options.length`，控制向用户显示的下拉选项的值。将`options.length`设为0，可以不再显示任何下拉属性。将`options`里面某个位置的`Option`对象设为`null`，将等于移除这个选项，后面的`Option`对象会自动递补这个位置。

`Select`对象的`selectedIndex`属性，返回用户选中的第一个下拉选项的位置（从0开始）。如果返回`-1`，则表示用户没有选中任何选项。该属性可读写。对于单选的下拉列表，这个属性就可以得知用户的选择；对于多选的下拉列表，这个属性还不够，必须逐个轮询`options`属性，判断每个`Option`对象的`selected`属性是否为`true`。

##### Option元素

`<option>`元素用于在下拉列表（`<select>`）中生成下拉选项。每个下拉选项就是一个`Option`对象，它有以下属性。

- `selected`：返回一个布尔值，表示用户是否选中该选项。
- `text`：返回该下拉选项的显示的文本。该属性可读写，可用来显示向用户显示的文本。
- `value`：返回该下拉选项的值，即向服务器发送的那个值。该属性可读写。
- `defaultSelected`：返回一个布尔值，表示这个下拉选项是否默认选中。

浏览器提供`Option`构造函数，用来生成下拉列表的选项对象。利用这个函数，可以用脚本生成下拉选项，然后放入`Select.options`对象里面，从而自动生成下拉列表。

```
var item = new Option(
  'Hello World',  // 显示的文本，即 text 属性
  'myValue',  // 向服务器发送的值，即 value 属性
  false,    // 是否为默认选项，即 defaultSelected 属性
  false   // 是否已经选中，即 selected 属性
);

// 获取 Selector 对象
var mySelector = document.forms.myForm.mySelector;
mySelector.options[mySelector.options.length] = item;

```

上面代码在下拉列表的末尾添加了一个选项。从中可以看到，`Option`构造函数可以接受四个选项，对应`<Option>`对象的四个属性。

注意，用脚本插入下拉选项完全可以用标准的DOM操作方法实现，比如`Document.create Element()`、`Node.insertBefore()`和`Node.removeChild()`等等。

#### image元素

##### alt属性，src属性

`alt`属性返回`image`元素的HTML标签的`alt`属性值，`src`属性返回`image`元素的HTML标签的`src`属性值。

```
// 方法一：HTML5构造函数Image
var img1 = new Image();
img1.src = 'image1.png';
img1.alt = 'alt';
document.body.appendChild(img1);

// 方法二：DOM HTMLImageElement
var img2 = document.createElement('img');
img2.src = 'image2.jpg';
img2.alt = 'alt text';
document.body.appendChild(img2);

document.images[0].src
// image1.png

```

##### complete属性

complete属性返回一个布尔值，true表示当前图像属于浏览器支持的图形类型，并且加载完成，解码过程没有出错，否则就返回false。

##### height属性，width属性

这两个属性返回image元素被浏览器渲染后的高度和宽度。

##### naturalWidth属性，naturalHeight属性

这两个属性只读，表示image对象真实的宽度和高度。

```
myImage.addEventListener('onload', function() {
	console.log('My width is: ', this.naturalWidth);
	console.log('My height is: ', this.naturalHeight);
});


```

#### table元素

表格有一些特殊的DOM操作方法。

- **insertRow()**：在指定位置插入一个新行（tr）。
- **deleteRow()**：在指定位置删除一行（tr）。
- **insertCell()**：在指定位置插入一个单元格（td）。
- **deleteCell()**：在指定位置删除一个单元格（td）。
- **createCaption()**：插入标题。
- **deleteCaption()**：删除标题。
- **createTHead()**：插入表头。
- **deleteTHead()**：删除表头。

下面是使用JavaScript生成表格的一个例子。

```
var table = document.createElement('table');
var tbody = document.createElement('tbody');
table.appendChild(tbody);

for (var i = 0; i <= 9; i++) {
  var rowcount = i + 1;
  tbody.insertRow(i);
  tbody.rows[i].insertCell(0);
  tbody.rows[i].insertCell(1);
  tbody.rows[i].insertCell(2);
  tbody.rows[i].cells[0].appendChild(document.createTextNode('Row ' + rowcount + ', Cell 1'));
  tbody.rows[i].cells[1].appendChild(document.createTextNode('Row ' + rowcount + ', Cell 2'));
  tbody.rows[i].cells[2].appendChild(document.createTextNode('Row ' + rowcount + ', Cell 3'));
}

table.createCaption();
table.caption.appendChild(document.createTextNode('A DOM-Generated Table'));

document.body.appendChild(table);

```

这些代码相当易读，其中需要注意的就是`insertRow`和`insertCell`方法，接受一个表示位置的参数（从0开始的整数）。

`table`元素有以下属性：

- **caption**：标题。
- **tHead**：表头。
- **tFoot**：表尾。
- **rows**：行元素对象，该属性只读。
- **rows.cells**：每一行的单元格对象，该属性只读。
- **tBodies**：表体，该属性只读。

#### audio元素，video元素

audio元素和video元素加载音频和视频时，以下事件按次序发生。

- loadstart：开始加载音频和视频。
- durationchange：音频和视频的duration属性（时长）发生变化时触发，即已经知道媒体文件的长度。如果没有指定音频和视频文件，duration属性等于NaN。如果播放流媒体文件，没有明确的结束时间，duration属性等于Inf（Infinity）。
- loadedmetadata：媒体文件的元数据加载完毕时触发，元数据包括duration（时长）、dimensions（大小，视频独有）和文字轨。
- loadeddata：媒体文件的第一帧加载完毕时触发，此时整个文件还没有加载完。
- progress：浏览器正在下载媒体文件，周期性触发。下载信息保存在元素的buffered属性中。
- canplay：浏览器准备好播放，即使只有几帧，readyState属性变为CAN_PLAY。
- canplaythrough：浏览器认为可以不缓冲（buffering）播放时触发，即当前下载速度保持不低于播放速度，readyState属性变为CAN_PLAY_THROUGH。

除了上面这些事件，audio元素和video元素还支持以下事件。

| 事件           | 触发条件                                  |
| ------------ | ------------------------------------- |
| abort        | 播放中断                                  |
| emptied      | 媒体文件加载后又被清空，比如加载后又调用load方法重新加载。       |
| ended        | 播放结束                                  |
| error        | 发生错误。该元素的error属性包含更多信息。               |
| pause        | 播放暂停                                  |
| play         | 暂停后重新开始播放                             |
| playing      | 开始播放，包括第一次播放、暂停后播放、结束后重新播放。           |
| ratechange   | 播放速率改变                                |
| seeked       | 搜索操作结束                                |
| seeking      | 搜索操作开始                                |
| stalled      | 浏览器开始尝试读取媒体文件，但是没有如预期那样获取数据           |
| suspend      | 加载文件停止，有可能是播放结束，也有可能是其他原因的暂停          |
| timeupdate   | 网页元素的currentTime属性改变时触发。              |
| volumechange | 音量改变时触发（包括静音）。                        |
| waiting      | 由于另一个操作（比如搜索）还没有结束，导致当前操作（比如播放）不得不等待。 |

#### tabindex属性

`tabindex`属性用来指定，当前HTML元素节点是否被tab键遍历，以及遍历的优先级。

```
var b1 = document.getElementById("button1");

b1.tabIndex = 1;

```

如果`tabindex = -1`，tab键跳过当前元素。

如果`tabindex = 0`，表示tab键将遍历当前元素。如果一个元素没有设置tabindex，默认值就是0。

如果`tabindex > 0`，表示tab键优先遍历。值越大，就表示优先级越大。

### 表单

#### 表单元素

input、textarea，password和select等元素都可以通过`value`属性获取到它们的值。

##### select标签

select标签用于定义一组下拉列表，该标签及其子标签上定义了如下属性：

- `HTMLSelectElement.size`属性取值为正整数，指定可见选项数
- `HTMLSelectElement.multiple`属性用来指定是否具有多选功能
- `HTMLOptionElement.selected`属性用来设置默认选项

```html
<form>
  	<!--普通下拉列表-->
  	<select multiple size="3">
		<option value="item1">item1</option>
		<option value="item2" selected>item2</option>
		<option value="item3">item3</option>
  		<option value="item4">item4</option>
	</select>
  	<!--带分组的下拉列表-->
 	<select name="os" id="os">
    	<option>Choose</option>
    	<optgroup label="Windows">
        	<option value="win 95">win 95</option>
        	<option value="win 97">win 97</option>
        	<option value="win 98">win 98</option>
        	<option value="win xp">win xp</option>
        	<option value="win 7">win 7</option>
    	</optgroup>
	<select>
</form>	
<script>
	var s = document.querySelectorAll("select");
  	s[0].value					//"item2"
  	s[1].selectedIndex = 1		//设置选中索引值为1的选项
</script>	
```

当下拉列表设置为多选时，`value`属性只返回选中的第一个选项的`value`属性值。如果需要获取每一个被选中选中的`value`属性值，需要遍历每一个选项的`selected`属性。

```javascript
var s = document.querySelector("select")
var options = s.options				//获取所有选项
var arr = []
for(var i = 0; i < options.length; i++){
    if(options[i].selected){
        arr.push(options[i].value)
    }
}
```

##### checkbox标签

checkbox标签用于定义复选框控件，并且每个复选框都只由选中和不选中两种状态。

```html
<form>
  <label>football: <input value="football" type="checkbox" checked /></label>
  <label>basketball: <input value="basketball" type="checkbox" /></label>
  <label>baseball: <input value="baseball" type="checkbox" /></label>
  <label>handball: <input value="handball" type="checkbox" /></label>
</form>
<script>
   var inputs = document.querySelectorAll("input")
   inputs[0].checked			//true，判断用户是否选中某个选项
   inputs[1].checked			//false
   inputs[2].checked = true		//该属性可写，设置选中指定选项
   for(var i = 0; i < inputs.length; i++){
       if(inputs[i].checked){
           console.log(inputs[i].value)		//获取被选中选项的value属性值
       }
   }
</script>
```

##### radio标签

radio标签用于定义单选框控件，同一组选择框只能选中一个。

```html
<form>
	<label>male: <input type="radio" value="male" name="sex" checked /></label>
  	<label>female: <input type="radio" value="female" name="sex" /></label>	
</form>
<script>
	var inputs = document.querySelector("input")
    inputs[0].checked			//true
  	inputs[0].value				//"male"
  	inputs[1].value				//"female"
</script>
```

#### 表单的验证

##### HTML 5表单验证

所谓“表单验证”，指的是检查用户提供的数据是否符合要求，比如Email地址的格式。

检查用户是否在`input`输入框之中填入值。

```
if (inputElem.value === inputElem.defaultValue) {
  // 用户没有填入内容
}

```

HTML 5原生支持表单验证，不需要JavaScript。

```
<input type="date" >

```

上面代码指定该input输入框只能填入日期，否则浏览器会报错。

但有时，原生的表单验证不完全符合需要，而且出错信息无法指定样式。这时，可能需要使用表单对象的noValidate属性，将原生的表单验证关闭。

```
var form = document.getElementById("myform");
form.noValidate = true;

form.onsubmit = validateForm;

```

上面代码先关闭原生的表单验证，然后指定submit事件时，让JavaScript接管表单验证。

此外，还可以只针对单个的input输入框，关闭表单验证。

```
form.field.willValidate = false;

```

每个input输入框都有willValidate属性，表示是否开启表单验证。对于那些不支持的浏览器（比如IE8），该属性等于undefined。

麻烦的地方在于，即使willValidate属性为true，也不足以表示浏览器支持所有种类的表单验证。比如，Firefox 29不支持date类型的输入框，会自动将其改为text类型，而此时它的willValidate属性为true。为了解决这个问题，必须确认input输入框的类型（type）未被浏览器改变。

```
if (field.nodeName === "INPUT" && field.type !== field.getAttribute("type")) {
    // 浏览器不支持该种表单验证，需自行部署JavaScript验证
}

```

##### checkValidity方法，setCustomValidity方法，validity对象

checkValidity方法表示执行原生的表单验证，如果验证通过返回true。如果验证失败，则会触发一个invalid事件。使用该方法以后，会设置validity对象的值。

每一个表单元素都有一个validity对象，它有以下属性。

- valid：如果该元素通过验证，则返回true。
- valueMissing：如果用户没填必填项，则返回true。
- typeMismatch：如果填入的格式不正确（比如Email地址），则返回true。
- patternMismatch：如果不匹配指定的正则表达式，则返回true。
- tooLong：如果超过最大长度，则返回true。
- tooShort：如果小于最短长度，则返回true。
- rangeUnderFlow：如果小于最小值，则返回true。
- rangeOverflow：如果大于最大值，则返回true。
- stepMismatch：如果不匹配步长（step），则返回true。
- badInput：如果不能转为值，则返回true。
- customError：如果该栏有自定义错误，则返回true。

setCustomValidity方法用于自定义错误信息，该提示信息也反映在该输入框的validationMessage属性中。如果将setCustomValidity设为空字符串，则意味该项目验证通过。

### 文件与二进制操作

历史上，JavaScript无法处理二进制数据。如果一定要处理的话，只能使用charCodeAt()方法，一个个字节地从文字编码转成二进制数据，还有一种办法是将二进制数据转成Base64编码，再进行处理。这两种方法不仅速度慢，而且容易出错。ECMAScript 5引入了Blob对象，允许直接操作二进制数据。

Blob对象是一个代表二进制数据的基本对象，在它的基础上，又衍生出一系列相关的API，用来操作文件。

- File对象：负责处理那些以文件形式存在的二进制数据，也就是操作本地文件；
- FileList对象：File对象的网页表单接口；
- FileReader对象：负责将二进制数据读入内存内容；
- URL对象：用于对二进制数据生成URL。

#### Blob对象

Blob（Binary Large Object）对象代表了一段二进制数据，提供了一系列操作接口。其他操作二进制数据的API（比如File对象），都是建立在Blob对象基础上的，继承了它的属性和方法。

生成Blob对象有两种方法：一种是使用Blob构造函数，另一种是对现有的Blob对象使用slice方法切出一部分。

（1）Blob构造函数，接受两个参数。第一个参数是一个包含实际数据的数组，第二个参数是数据的类型，这两个参数都不是必需的。

```
var htmlParts = ["<a id=\"a\"><b id=\"b\">hey!<\/b><\/a>"];
var myBlob = new Blob(htmlParts, { "type" : "text\/xml" });

```

下面是一个利用Blob对象，生成可下载文件的例子。

```
var blob = new Blob(["Hello World"]);

var a = document.createElement("a");
a.href = window.URL.createObjectURL(blob);
a.download = "hello-world.txt";
a.textContent = "Download Hello World!";

body.appendChild(a);

```

上面的代码生成了一个超级链接，点击后提示下载文本文件hello-world.txt，文件内容为“Hello World”。

（2）Blob对象的slice方法，将二进制数据按照字节分块，返回一个新的Blob对象。

```
var newBlob = oldBlob.slice(startingByte, endindByte);

```

下面是一个使用XMLHttpRequest对象，将大文件分割上传的例子。

```
function upload(blobOrFile) {
  var xhr = new XMLHttpRequest();
  xhr.open('POST', '/server', true);
  xhr.onload = function(e) { ... };
  xhr.send(blobOrFile);
}

document.querySelector('input[type="file"]').addEventListener('change', function(e) {
  var blob = this.files[0];

  const BYTES_PER_CHUNK = 1024 * 1024; // 1MB chunk sizes.
  const SIZE = blob.size;

  var start = 0;
  var end = BYTES_PER_CHUNK;

  while(start < SIZE) {
    upload(blob.slice(start, end));

    start = end;
    end = start + BYTES_PER_CHUNK;
  }
}, false);

})();

```

（3）Blob对象有两个只读属性：

- size：二进制数据的大小，单位为字节。
- type：二进制数据的MIME类型，全部为小写，如果类型未知，则该值为空字符串。

在Ajax操作中，如果xhr.responseType设为blob，接收的就是二进制数据。

#### FileList对象

FileList对象针对表单的file控件。当用户通过file控件选取文件后，这个控件的files属性值就是FileList对象。它在结构上类似于数组，包含用户选取的多个文件。

```
<input type="file" id="input" onchange="console.log(this.files.length)" multiple />

```

当用户选取文件后，就可以读取该文件。

```
var selected_file = document.getElementById('input').files[0];

```

采用拖放方式，也可以得到FileList对象。

```
var dropZone = document.getElementById('drop_zone');
dropZone.addEventListener('drop', handleFileSelect, false);

function handleFileSelect(evt) {
    evt.stopPropagation();
    evt.preventDefault();

    var files = evt.dataTransfer.files; // FileList object.

    // ...
}

```

上面代码的 handleFileSelect 是拖放事件的回调函数，它的参数evt是一个事件对象，该参数的dataTransfer.files属性就是一个FileList对象，里面包含了拖放的文件。

#### File API

File API提供`File`对象，它是`FileList`对象的成员，包含了文件的一些元信息，比如文件名、上次改动时间、文件大小和文件类型。

```
var selected_file = document.getElementById('input').files[0];

var fileName = selected_file.name;
var fileSize = selected_file.size;
var fileType = selected_file.type;

```

`File`对象的属性值如下。

- `name`：文件名，该属性只读。
- `size`：文件大小，单位为字节，该属性只读。
- `type`：文件的MIME类型，如果分辨不出类型，则为空字符串，该属性只读。
- `lastModified`：文件的上次修改时间，格式为时间戳。
- `lastModifiedDate`：文件的上次修改时间，格式为`Date`对象实例。

```
$('#upload-file')[0].files[0]
// {
//   lastModified: 1449370355682,
//   lastModifiedDate: Sun Dec 06 2015 10:52:35 GMT+0800 (CST),
//   name: "HTTP 2 is here Goodbye SPDY Not quite yet.png",
//   size: 17044,
//   type: "image/png"
// }

```

#### FileReader API

FileReader API用于读取文件，即把文件内容读入内存。它的参数是`File`对象或`Blob`对象。

对于不同类型的文件，FileReader提供不同的方法读取文件。

- `readAsBinaryString(Blob|File)`：返回二进制字符串，该字符串每个字节包含一个0到255之间的整数。
- `readAsText(Blob|File, opt_encoding)`：返回文本字符串。默认情况下，文本编码格式是’UTF-8’，可以通过可选的格式参数，指定其他编码格式的文本。
- `readAsDataURL(Blob|File)`：返回一个基于Base64编码的data-uri对象。
- `readAsArrayBuffer(Blob|File)`：返回一个ArrayBuffer对象。

`readAsText`方法用于读取文本文件，它的第一个参数是`File`或`Blob`对象，第二个参数是前一个参数的编码方法，如果省略就默认为`UTF-8`编码。该方法是异步方法，一般监听`onload`件，用来确定文件是否加载结束，方法是判断`FileReader`实例的`result`属性是否有值。其他三种读取方法，用法与`readAsText`方法类似。

```
var reader = new FileReader();
reader.onload = function(e) {
  var text = reader.result;
}

reader.readAsText(file, encoding);

```

`readAsDataURL`方法返回一个data URL，它的作用基本上是将文件数据进行Base64编码。你可以将返回值设为图像的`src`属性。

```
var file = document.getElementById('destination').files[0];
if(file.type.indexOf('image') !== -1) {
  var reader = new FileReader();
  reader.onload = function (e) {
    var dataURL = reader.result;
  }
  reader.readAsDataURL(file);
}

```

`readAsBinaryString`方法可以读取任意类型的文件，而不仅仅是文本文件，返回文件的原始的二进制内容。这个方法与XMLHttpRequest.sendAsBinary方法结合使用，就可以使用JavaScript上传任意文件到服务器。

```
var reader = new FileReader();
reader.onload = function(e) {
  var rawData = reader.result;
}
reader.readAsBinaryString(file);

```

`readAsArrayBuffer`方法读取文件，返回一个类型化数组（ArrayBuffer），即固定长度的二进制缓存数据。在文件操作时（比如将JPEG图像转为PNG图像），这个方法非常方便。

```
var reader = new FileReader();
reader.onload = function(e) {
  var arrayBuffer = reader.result;
}

reader.readAsArrayBuffer(file);

```

除了以上四种不同的读取文件方法，FileReader API还有一个`abort`方法，用于中止文件上传。

```
var reader = new FileReader();
reader.abort();

```

FileReader对象采用异步方式读取文件，可以为一系列事件指定回调函数。

- onabort方法：读取中断或调用reader.abort()方法时触发。
- onerror方法：读取出错时触发。
- onload方法：读取成功后触发。
- onloadend方法：读取完成后触发，不管是否成功。触发顺序排在 onload 或 onerror 后面。
- onloadstart方法：读取将要开始时触发。
- onprogress方法：读取过程中周期性触发。

下面的代码是如何展示文本文件的内容。

```
var reader = new FileReader();
reader.onload = function(e) {
  console.log(e.target.result);
}
reader.readAsText(blob);

```

`onload`事件的回调函数接受一个事件对象，该对象的`target.result`就是文件的内容。

下面是一个使用`readAsDataURL`方法，为`img`元素添加`src`属性的例子。

```
var reader = new FileReader();
reader.onload = function(e) {
  document.createElement('img').src = e.target.result;
};
reader.readAsDataURL(f);

```

下面是一个`onerror`事件回调函数的例子。

```
var reader = new FileReader();
reader.onerror = errorHandler;

function errorHandler(evt) {
  switch(evt.target.error.code) {
    case evt.target.error.NOT_FOUND_ERR:
      alert('File Not Found!');
      break;
    case evt.target.error.NOT_READABLE_ERR:
      alert('File is not readable');
      break;
    case evt.target.error.ABORT_ERR:
      break;
    default:
      alert('An error occurred reading this file.');
  };
}

```

下面是一个`onprogress`事件回调函数的例子，主要用来显示读取进度。

```
var reader = new FileReader();
reader.onprogress = updateProgress;

function updateProgress(evt) {
  if (evt.lengthComputable) {
    var percentLoaded = Math.round((evt.loaded / evt.totalEric Bidelman) * 100);
    var progress = document.querySelector('.percent');
    if (percentLoaded < 100) {
      progress.style.width = percentLoaded + '%';
      progress.textContent = percentLoaded + '%';
    }
  }
}

```

读取大文件的时候，可以利用`Blob`对象的`slice`方法，将大文件分成小段，逐一读取，这样可以加快处理速度。

#### 综合实例：显示用户选取的本地图片

假设有一个表单，用于用户选取图片。

```
<input type="file" name="picture" accept="image/png, image/jpeg"/>
```

一旦用户选中图片，将其显示在canvas的函数可以这样写：

```
document.querySelector('input[name=picture]').onchange = function(e){
     readFile(e.target.files[0]);
}

function readFile(file){

  var reader = new FileReader();

  reader.onload = function(e){
    applyDataUrlToCanvas( reader.result );
  };

  reader.readAsDataURL(file);
}
```

还可以在canvas上面定义拖放事件，允许用户直接拖放图片到上面。

```
// stop FireFox from replacing the whole page with the file.
canvas.ondragover = function () { return false; };

// Add drop handler
canvas.ondrop = function (e) {
  e.stopPropagation();
  e.preventDefault(); 
  e = e || window.event;
  var files = e.dataTransfer.files;
  if(files){
    readFile(files[0]);
  }
};
```

所有的拖放事件都有一个dataTransfer属性，它包含拖放过程涉及的二进制数据。

还可以让canvas显示剪贴板中的图片。

```
document.onpaste = function(e){
  e.preventDefault();
  if(e.clipboardData&&e.clipboardData.items){
    // pasted image
    for(var i=0, items = e.clipboardData.items;i<items.length;i++){
      if( items[i].kind==='file' && items[i].type.match(/^image/) ){
        readFile(items[i].getAsFile());
        break;
      }
    }
  }
  return false;
};
```

#### URL对象

URL对象用于生成指向File对象或Blob对象的URL。

```
var objecturl =  window.URL.createObjectURL(blob);
```

上面的代码会对二进制数据生成一个URL，类似于“blob:http%3A//test.com/666e6730-f45c-47c1-8012-ccc706f17191”。这个URL可以放置于任何通常可以放置URL的地方，比如img标签的src属性。需要注意的是，即使是同样的二进制数据，每调用一次URL.createObjectURL方法，就会得到一个不一样的URL。

这个URL的存在时间，等同于网页的存在时间，一旦网页刷新或卸载，这个URL就失效。除此之外，也可以手动调用URL.revokeObjectURL方法，使URL失效。

```
window.URL.revokeObjectURL(objectURL);
```

下面是一个利用URL对象，在网页插入图片的例子。

```
var img = document.createElement("img");

img.src = window.URL.createObjectURL(files[0]);

img.height = 60;

img.onload = function(e) {
    window.URL.revokeObjectURL(this.src);
}

body.appendChild(img);

var info = document.createElement("span");

info.innerHTML = files[i].name + ": " + files[i].size + " bytes";

document.querySelector('body').appendChild(info);
```

还有一个本机视频预览的例子。

```
var video = document.getElementById('video');
var obj_url = window.URL.createObjectURL(blob);
video.src = obj_url;
video.play()
window.URL.revokeObjectURL(obj_url);
```

### Web Worker

Web Workers是HTML5提供的一个JavaScript多线程解决方案，我们可以将一些大计算量的代码交由web worker运行而不冻结用户界面。但是创建子线程还是受主线程控制，并且不得操作DOM，所以这个新标准并没有改变JavaScript单线程的本质。



Worker线程分成好几种。

- 普通的Worker：只能与创造它们的主进程通信。
- Shared Worker：能被所有同源的进程获取（比如来自不同的浏览器窗口、iframe窗口和其他Shared worker），它们必须通过一个端口通信。
- ServiceWorker：实际上是一个在网络应用与浏览器或网络层之间的代理层。它可以拦截网络请求，使得离线访问成为可能。

Web Worker有以下几个特点：

- **同域限制**。子线程加载的脚本文件，必须与主线程的脚本文件在同一个域。
- **DOM限制**。子线程所在的全局对象，与主进程不一样，它无法读取网页的DOM对象，即`document`、`window`、`parent`这些对象，子线程都无法得到。（但是，`navigator`对象和`location`对象可以获得。）
- **脚本限制**。子线程无法读取网页的全局变量和函数，也不能执行alert和confirm方法，不过可以执行setInterval和setTimeout，以及使用XMLHttpRequest对象发出AJAX请求。
- **文件限制**。子线程无法读取本地文件，即子线程无法打开本机的文件系统（file://），它所加载的脚本，必须来自网络。

使用之前，检查浏览器是否支持这个API。

```
if (window.Worker) {
  // 支持
} else {
  // 不支持
}

```

#### 新建和启动子线程

主线程采用`new`命令，调用`Worker`构造函数，可以新建一个子线程。

```
var worker = new Worker('work.js');

```

Worker构造函数的参数是一个脚本文件，这个文件就是子线程所要完成的任务，上面代码中是`work.js`。由于子线程不能读取本地文件系统，所以这个脚本文件必须来自网络端。如果下载没有成功，比如出现404错误，这个子线程就会默默地失败。

子线程新建之后，并没有启动，必需等待主线程调用`postMessage`方法，即发出信号之后才会启动。`postMessage`方法的参数，就是主线程传给子线程的信号。它可以是一个字符串，也可以是一个对象。

```
worker.postMessage("Hello World");
worker.postMessage({method: 'echo', args: ['Work']});

```

只要符合父线程的同源政策，Worker线程自己也能新建Worker线程。Worker线程可以使用XMLHttpRequest进行网络I/O，但是`XMLHttpRequest`对象的`responseXML`和`channel`属性总是返回`null`。

#### 子线程的事件监听

在子线程内，必须有一个回调函数，监听message事件。

```
/* File: work.js */

self.addEventListener('message', function(e) {
  self.postMessage('You said: ' + e.data);
}, false);

```

self代表子线程自身，self.addEventListener表示对子线程的message事件指定回调函数（直接指定onmessage属性的值也可）。回调函数的参数是一个事件对象，它的data属性包含主线程发来的信号。self.postMessage则表示，子线程向主线程发送一个信号。

根据主线程发来的不同的信号值，子线程可以调用不同的方法。

```
/* File: work.js */

self.onmessage = function(event) {
  var method = event.data.method;
  var args = event.data.args;

  var reply = doSomething(args);
  self.postMessage({method: method, reply: reply});
};

```

#### 主线程的事件监听

主线程也必须指定message事件的回调函数，监听子线程发来的信号。

```
/* File: main.js */

worker.addEventListener('message', function(e) {
	console.log(e.data);
}, false);

```

#### 错误处理

主线程可以监听子线程是否发生错误。如果发生错误，会触发主线程的error事件。

```
worker.onerror(function(event) {
  console.log(event);
});

// or

worker.addEventListener('error', function(event) {
  console.log(event);
});

```

#### 关闭子线程

使用完毕之后，为了节省系统资源，我们必须在主线程调用terminate方法，手动关闭子线程。

```
worker.terminate();

```

也可以子线程内部关闭自身。

```
self.close();

```

#### 主线程与子线程的数据通信

前面说过，主线程与子线程之间的通信内容，可以是文本，也可以是对象。需要注意的是，这种通信是拷贝关系，即是传值而不是传址，子线程对通信内容的修改，不会影响到主线程。事实上，浏览器内部的运行机制是，先将通信内容串行化，然后把串行化后的字符串发给子线程，后者再将它还原。

主线程与子线程之间也可以交换二进制数据，比如File、Blob、ArrayBuffer等对象，也可以在线程之间发送。

但是，用拷贝方式发送二进制数据，会造成性能问题。比如，主线程向子线程发送一个500MB文件，默认情况下浏览器会生成一个原文件的拷贝。为了解决这个问题，JavaScript允许主线程把二进制数据直接转移给子线程，但是一旦转移，主线程就无法再使用这些二进制数据了，这是为了防止出现多个线程同时修改数据的麻烦局面。这种转移数据的方法，叫做[Transferable Objects](http://www.w3.org/html/wg/drafts/html/master/infrastructure.html#transferable-objects)。

如果要使用该方法，postMessage方法的最后一个参数必须是一个数组，用来指定前面发送的哪些值可以被转移给子线程。

```
worker.postMessage(arrayBuffer, [arrayBuffer]);
window.postMessage(arrayBuffer, targetOrigin, [arrayBuffer]);

```

#### 同页面的Web Worker

通常情况下，子线程载入的是一个单独的JavaScript文件，但是也可以载入与主线程在同一个网页的代码。假设网页代码如下：

```
<!DOCTYPE html>
    <body>
        <script id="worker" type="app/worker">

            addEventListener('message', function() {
                postMessage('Im reading Tech.pro');
            }, false);
        </script>
    </body>
</html>

```

我们可以读取页面中的script，用worker来处理。

```
var blob = new Blob([document.querySelector('#worker').textContent]);

```

这里需要把代码当作二进制对象读取，所以使用Blob接口。然后，这个二进制对象转为URL，再通过这个URL创建worker。

```
var url = window.URL.createObjectURL(blob);
var worker = new Worker(url);

```

部署事件监听代码。

```
worker.addEventListener('message', function(e) {
   console.log(e.data);
}, false);

```

最后，启动worker。

```
worker.postMessage('');

```

整个页面的代码如下：

```
<!DOCTYPE html>
<body>
  <script id="worker" type="app/worker">
    addEventListener('message', function() {
      postMessage('Work done!');
    }, false);
   </script>

  <script>
    (function() {
      var blob = new Blob([document.querySelector('#worker').textContent]);
      var url = window.URL.createObjectURL(blob);
      var worker = new Worker(url);

      worker.addEventListener('message', function(e) {
        console.log(e.data);
      }, false);

      worker.postMessage('');
    })();
  </script>
</body>
</html>

```

可以看到，主线程和子线程的代码都在同一个网页上面。

上面所讲的Web Worker都是专属于某个网页的，当该网页关闭，worker就自动结束。除此之外，还有一种共享式的Web Worker，允许多个浏览器窗口共享同一个worker，只有当所有网口关闭，它才会结束。这种共享式的Worker用SharedWorker对象来建立，因为适用场合不多，这里就省略了。

#### 实例：Worker 进程完成论询

有时，浏览器需要论询服务器状态，以便第一时间得知状态改变。这个工作可以放在 Worker 进程里面。

```
var pollingWorker = createWorker(function (e) {
  var cache;

  function compare(new, old) { ... };

  var myRequest = new Request('/my-api-endpoint');

  setInterval(function () {
    fetch('/my-api-endpoint').then(function (res) {
      var data = res.json();

      if(!compare(res.json(), cache)) {
        cache = data;

        self.postMessage(data);
      }
    })
  }, 1000)
});

pollingWorker.onmessage = function () {
  // render data
}

pollingWorker.postMessage('init');

```

#### Service Worker

Service worker是一个在浏览器后台运行的脚本，与网页不相干，专注于那些不需要网页或用户互动就能完成的功能。它主要用于操作离线缓存。

Service Worker有以下特点。

- 属于JavaScript Worker，不能直接接触DOM，通过`postMessage`接口与页面通信。
- 不需要任何页面，就能执行。
- 不用的时候会终止执行，需要的时候又重新执行，即它是事件驱动的。
- 有一个精心定义的升级策略。
- 只在HTTPs协议下可用，这是因为它能拦截网络请求，所以必须保证请求是安全的。
- 可以拦截发出的网络请求，从而控制页面的网路通信。
- 内部大量使用Promise。

Service worker的常见用途。

- 通过拦截网络请求，使得网站运行得更快，或者在离线情况下，依然可以执行。
- 作为其他后台功能的基础，比如消息推送和背景同步。

使用Service Worker有以下步骤。

首先，需要向浏览器登记Service Worker。

```
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js')
    .then(function(registration) {
    // 登记成功
    console.log('ServiceWorker登记成功，范围为', registration.scope);
    }).catch(function(err) {
    // 登记失败
      console.log('ServiceWorker登记失败：', err);
    });
}

```

上面代码向浏览器登记`sw.js`脚本，实质就是浏览器加载`sw.js`。这段代码可以多次调用，浏览器会自行判断`sw.js`是否登记过，如果已经登记过，就不再重复执行了。注意，Service worker脚本必须与页面在同一个域，且必须在HTTPs协议下正常运行。

`sw.js`位于域名的根目录下，这表明这个Service worker的范围（scope）是整个域，即会接收整个域下面的`fetch`事件。如果脚本的路径是`/example/sw.js`，那么Service worker只对`/example/`开头的URL有效（比如`/example/page1/`、`/example/page2/`）。如果脚本不在根目录下，但是希望对整个域都有效，可以指定`scope`属性。

```
navigator.serviceWorker.register('/path/to/serviceworker.js', {
  scope: '/'
});

```

一旦登记完成，这段脚本就会用户的浏览器之中长期存在，不会随着用户离开你的网站而消失。

`.register`方法返回一个Promise对象。

登记成功后，浏览器执行下面步骤。

1. 下载资源（Download）
2. 安装（Install）
3. 激活（Activate）

安装和激活，主要通过事件来判断。

```
self.addEventListener('install', function(event) {
  event.waitUntil(
    fetchStuffAndInitDatabases()
  );
});

self.addEventListener('activate', function(event) {
  // You're good to go!
});

```

Service worker一旦激活，就开始控制页面。网页加载的时候，可以选择一个Service worker作为自己的控制器。不过，页面第一次加载的时候，它不受Service worker控制，因为这时还没有一个Service worker在运行。只有重新加载页面后，Service worker才会生效，控制加载它的页面。

你可以查看`navigator.serviceWorker.controller`，了解当前哪个ServiceWorker掌握控制权。如果后台没有任何Service worker，`navigator.serviceWorker.controller`返回`null`。

Service worker激活以后，就能监听`fetch`事件。

```
self.addEventListener('fetch', function(event) {
  console.log(event.request);
});

```

`fetch`事件会在两种情况下触发。

- 用户访问Service worker范围内的网页。
- 这些网页发出的任何网络请求（页面本身、CSS、JS、图像、XHR等等），即使这些请求是发向另一个域。但是，`iframe`和`<object>`标签发出的请求不会被拦截。

`fetch`事件的`event`对象的`request`属性，返回一个对象，包含了所拦截的网络请求的所有信息，比如URL、请求方法和HTTP头信息。

Service worker的强大之处，在于它会拦截请求，并会返回一个全新的回应。

```
self.addEventListener('fetch', function(event) {
  event.respondWith(new Response("Hello world!"));
});

```

`respondWith`方法的参数是一个Response对象实例，或者一个Promise对象（resolved以后返回一个Response实例）。上面代码手动创造一个Response实例。

下面是完整的[代码](https://github.com/jakearchibald/isserviceworkerready/tree/gh-pages/demos/manual-response)。

先看网页代码`index.html`。

```
<!DOCTYPE html>
<html>
<head>
  <style>
    body {
      white-space: pre-line;
      font-family: monospace;
      font-size: 14px;
    }
  </style>
</head>
<body><script>
    function log() {
      document.body.appendChild(document.createTextNode(Array.prototype.join.call(arguments, ", ") + '\n'));
      console.log.apply(console, arguments);
    }
    window.onerror = function(err) {
      log("Error", err);
    };
    navigator.serviceWorker.register('sw.js', {
      scope: './'
    }).then(function(sw) {
      log("Registered!", sw);
      log("You should get a different response when you refresh");
    }).catch(function(err) {
      log("Error", err);
    });
  </script></body>
</html>

```

然后是Service worker脚本`sw.js`。

```
// The SW will be shutdown when not in use to save memory,
// be aware that any global state is likely to disappear
console.log("SW startup");

self.addEventListener('install', function(event) {
  console.log("SW installed");
});

self.addEventListener('activate', function(event) {
  console.log("SW activated");
});

self.addEventListener('fetch', function(event) {
  console.log("Caught a fetch!");
  event.respondWith(new Response("Hello world!"));
});

```

每一次浏览器向服务器要求一个文件的时候，就会触发`fetch`事件。Service worker可以在发出这个请求之前，前拦截它。

```
self.addEventListener('fetch', function (event) {
  var request = event.request;
  ...
});

```

实际应用中，我们使用`fetch`方法去抓取资源，该方法返回一个Promise对象。

```
self.addEventListener('fetch', function(event) {
  if (/\.jpg$/.test(event.request.url)) {
    event.respondWith(
      fetch('//www.google.co.uk/logos/example.gif', {
        mode: 'no-cors'
      })
    );
  }
});

```

上面代码中，如果网页请求JPG文件，就会被Service worker拦截，转而返回一个Google的Logo图像。`fetch`方法默认会加上CORS信息头，，上面设置了取消这个头。

下面的代码是一个将所有JPG、PNG图片请求，改成WebP格式返回的例子。

```
"use strict";

// Listen to fetch events
self.addEventListener('fetch', function(event) {

  // Check if the image is a jpeg
  if (/\.jpg$|.png$/.test(event.request.url)) {
    // Inspect the accept header for WebP support
    var supportsWebp = false;
    if (event.request.headers.has('accept')){
      supportsWebp = event.request.headers.get('accept').includes('webp');
    }

    // If we support WebP
    if (supportsWebp) {
      // Clone the request
      var req = event.request.clone();
      // Build the return URL
      var returnUrl = req.url.substr(0, req.url.lastIndexOf(".")) + ".webp";
      event.respondWith(fetch(returnUrl, {
        mode: 'no-cors'
      }));
    }
  }
});

```

如果请求失败，可以通过Promise的`catch`方法处理。

```
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request).catch(function() {
      return new Response("Request failed!");
    })
  );
});

```

登记成功后，可以在Chrome浏览器访问`chrome://inspect/#service-workers`，查看整个浏览器目前正在运行的Service worker。访问`chrome://serviceworker-internals`，可以查看浏览器目前安装的所有Service worker。

一个已经登记过的Service worker脚本，如果发生改动，浏览器就会重新安装，这被称为“升级”。

Service worker有一个Cache API，用来缓存外部资源。

```
self.addEventListener('install', function(event) {
  // pre cache a load of stuff:
  event.waitUntil(
    caches.open('myapp-static-v1').then(function(cache) {
      return cache.addAll([
        '/',
        '/styles/all.css',
        '/styles/imgs/bg.png',
        '/scripts/all.js'
      ]);
    })
  )
});

self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request).then(function(response) {
      return response || fetch(event.request);
    })
  );
});

```

上面代码中，`caches.open`方法用来建立缓存，然后使用`addAll`方法添加资源。`caches.match`方法则用来建立缓存以后，匹配当前请求是否在缓存之中，如果命中就取出缓存，否则就正常发出这个请求。一旦一个资源进入缓存，它原来指定是否过期的HTTP信息头，就会被忽略。缓存之中的资源，只在你移除它们的时候，才会被移除。

单个资源可以使用`cache.put(request, response)`方法添加。

下面是一个在安装阶段缓存资源的例子。

```
var staticCacheName = 'static';
var version = 'v1::';

self.addEventListener('install', function (event) {
  event.waitUntil(updateStaticCache());
});

function updateStaticCache() {
  return caches.open(version + staticCacheName)
    .then(function (cache) {
      return cache.addAll([
        '/path/to/javascript.js',
        '/path/to/stylesheet.css',
        '/path/to/someimage.png',
        '/path/to/someotherimage.png',
        '/',
        '/offline'
      ]);
    });
};

```

上面代码将JavaScript脚本、CSS样式表、图像文件、网站首页、离线页面，存入浏览器缓存。这些资源都要等全部进入缓存之后，才会安装。

安装以后，就需要激活。

```
self.addEventListener('activate', function (event) {
  event.waitUntil(
    caches.keys()
      .then(function (keys) {
        return Promise.all(keys
          .filter(function (key) {
            return key.indexOf(version) !== 0;
          })
          .map(function (key) {
            return caches.delete(key);
          })
        );
      })
  );
});
```

### Server-Sent Events

#### 简介

服务器向客户端推送数据，有很多解决方案。除了“轮询” 和 WebSocket，HTML 5 还提供了 Server-Sent Events（以下简称 SSE）。

一般来说，HTTP 协议只能客户端向服务器发起请求，服务器不能主动向客户端推送。但是有一种特殊情况，就是服务器向客户端声明，接下来要发送的是流信息（streaming）。也就是说，发送的不是一次性的数据包，而是一个数据流，会连续不断地发送过来。这时，客户端不会关闭连接，会一直等着服务器发过来的新的数据流。本质上，这种通信就是以流信息的方式，完成一次用时很长的下载。

SSE 就是利用这种机制，使用流信息向浏览器推送信息。它基于 HTTP 协议，目前除了 IE/Edge，其他浏览器都支持。

#### 与 WebSocket 的比较

SSE 与 WebSocket 作用相似，都是建立浏览器与服务器之间的通信渠道，然后服务器向浏览器推送信息。

总体来说，WebSocket 更强大和灵活。因为它是全双工通道，可以双向通信；SSE 是单向通道，只能服务器向浏览器发送，因为 streaming 本质上就是下载。如果浏览器向服务器发送信息，就变成了另一次 HTTP 请求。

但是，SSE 也有自己的优点。

- SSE 使用 HTTP 协议，现有的服务器软件都支持。WebSocket 是一个独立协议。
- SSE 属于轻量级，使用简单；WebSocket 协议相对复杂。
- SSE 默认支持断线重连，WebSocket 需要自己实现。
- SSE 一般只用来传送文本，二进制数据需要编码后传送，WebSocket 默认支持传送二进制数据。
- SSE 支持自定义发送的消息类型。

因此，两者各有特点，适合不同的场合。

#### 客户端 API

##### EventSource 对象

SSE 的客户端 API 部署在`EventSource`对象上。下面的代码可以检测浏览器是否支持 SSE。

```
if ('EventSource' in window) {
  // ...
}

```

使用 SSE 时，浏览器首先生成一个`EventSource`实例，向服务器发起连接。

```
var source = new EventSource(url);

```

上面的`url`可以与当前网址同域，也可以跨域。跨域时，可以指定第二个参数，打开`withCredentials`属性，表示是否一起发送 Cookie。

```
var source = new EventSource(url, { withCredentials: true });

```

##### readyState 属性

`EventSource`实例的`readyState`属性，表明连接的当前状态。该属性只读，可以取以下值。

- 0：相当于常量`EventSource.CONNECTING`，表示连接还未建立，或者断线正在重连。
- 1：相当于常量`EventSource.OPEN`，表示连接已经建立，可以接受数据。
- 2：相当于常量`EventSource.CLOSED`，表示连接已断，且不会重连。

```
var source = new EventSource(url);
console.log(source.readyState);

```

##### url 属性

`EventSource`实例的`url`属性返回连接的网址，该属性只读。

##### withCredentials 属性

`EventSource`实例的`withCredentials`属性返回一个布尔值，表示当前实例是否开启 CORS 的`withCredentials`。该属性只读，默认是`false`。

##### onopen 属性

连接一旦建立，就会触发`open`事件，可以在`onopen`属性定义回调函数。

```
source.onopen = function (event) {
  // ...
};

// 另一种写法
source.addEventListener('open', function (event) {
  // ...
}, false);

```

##### onmessage 属性

客户端收到服务器发来的数据，就会触发`message`事件，可以在`onmessage`属性定义回调函数。

```
source.onmessage = function (event) {
  var data = event.data;
  var origin = event.origin;
  var lastEventId = event.lastEventId;
  // handle message
};

// 另一种写法
source.addEventListener('message', function (event) {
  var data = event.data;
  var origin = event.origin;
  var lastEventId = event.lastEventId;
  // handle message
}, false);

```

上面代码中，参数对象`event`有如下属性。

- `data`：服务器端传回的数据（文本格式）。
- `origin`： 服务器 URL 的域名部分，即协议、域名和端口，表示消息的来源。
- `lastEventId`：数据的编号，由服务器端发送。如果没有编号，这个属性为空。

##### onerror 属性

如果发生通信错误（比如连接中断），就会触发`error`事件，可以在`onerror`属性定义回调函数。

```
source.onerror = function (event) {
  // handle error event
};

// 另一种写法
source.addEventListener('error', function (event) {
  // handle error event
}, false);

```

##### 自定义事件

默认情况下，服务器发来的数据，总是触发浏览器`EventSource`实例的`message`事件。开发者还可以自定义 SSE 事件，这种情况下，发送回来的数据不会触发`message`事件。

```
source.addEventListener('foo', function (event) {
  var data = event.data;
  var origin = event.origin;
  var lastEventId = event.lastEventId;
  // handle message
}, false);

```

上面代码中，浏览器对 SSE 的`foo`事件进行监听。如何实现服务器发送`foo`事件，请看下文。

##### close() 方法

`close`方法用于关闭 SSE 连接。

```
source.close();

```

#### 服务器实现

##### 数据格式

服务器向浏览器发送的 SSE 数据，必须是 UTF-8 编码的文本，具有如下的 HTTP 头信息。

```
Content-Type: text/event-stream
Cache-Control: no-cache
Connection: keep-alive

```

上面三行之中，第一行的`Content-Type`必须指定 MIME 类型为`event-steam`。

每一次发送的信息，由若干个`message`组成，每个`message`之间用`\n\n`分隔。每个`message`内部由若干行组成，每一行都是如下格式。

```
[field]: value\n

```

上面的`field`可以取四个值。

- data
- event
- id
- retry

此外，还可以有冒号开头的行，表示注释。通常，服务器每隔一段时间就会向浏览器发送一个注释，保持连接不中断。

```
: This is a comment

```

下面是一个例子。

```
: this is a test stream\n\n

data: some text\n\n

data: another message\n
data: with two lines \n\n

```

##### data 字段

数据内容用`data`字段表示。

```
data:  message\n\n

```

如果数据很长，可以分成多行，最后一行用`\n\n`结尾，前面行都用`\n`结尾。

```
data: begin message\n
data: continue message\n\n

```

下面是一个发送 JSON 数据的例子。

```
data: {\n
data: "foo": "bar",\n
data: "baz", 555\n
data: }\n\n

```

##### id 字段

数据标识符用`id`字段表示，相当于每一条数据的编号。

```
id: msg1\n
data: message\n\n

```

浏览器用`lastEventId`属性读取这个值。一旦连接断线，浏览器会发送一个 HTTP 头，里面包含一个特殊的`Last-Event-ID`头信息，将这个值发送回来，用来帮助服务器端重建连接。因此，这个头信息可以被视为一种同步机制。

##### event 字段

`event`字段表示自定义的事件类型，默认是`message`事件。浏览器可以用`addEventListener()`监听该事件。

```
event: foo\n
data: a foo event\n\n

data: an unnamed event\n\n

event: bar\n
data: a bar event\n\n

```

上面的代码创造了三条信息。第一条的名字是`foo`，触发浏览器的`foo`事件；第二条未取名，表示默认类型，触发浏览器的`message`事件；第三条是`bar`，触发浏览器的`bar`事件。

下面是另一个例子。

```
event: userconnect
data: {"username": "bobby", "time": "02:33:48"}

event: usermessage
data: {"username": "bobby", "time": "02:34:11", "text": "Hi everyone."}

event: userdisconnect
data: {"username": "bobby", "time": "02:34:23"}

event: usermessage
data: {"username": "sean", "time": "02:34:36", "text": "Bye, bobby."}

```

##### retry 字段

服务器可以用`retry`字段，指定浏览器重新发起连接的时间间隔。

```
retry: 10000\n

```

两种情况会导致浏览器重新发起连接：一种是时间间隔到期，二是由于网络错误等原因，导致连接出错。

#### Node 服务器实例

SSE 要求服务器与浏览器保持连接。对于不同的服务器软件来说，所消耗的资源是不一样的。Apache 服务器，每个连接就是一个线程，如果要维持大量连接，势必要消耗大量资源。Node 则是所有连接都使用同一个线程，因此消耗的资源会小得多，但是这要求每个连接不能包含很耗时的操作，比如磁盘的 IO 读写。

下面是 Node 的 SSE 服务器[实例](http://cjihrig.com/blog/server-sent-events-in-node-js/)。

```
var http = require("http");

http.createServer(function (req, res) {
  var fileName = "." + req.url;

  if (fileName === "./stream") {
    res.writeHead(200, {
      "Content-Type":"text/event-stream",
      "Cache-Control":"no-cache",
      "Connection":"keep-alive",
      "Access-Control-Allow-Origin": '*',
    });
    res.write("retry: 10000\n");
    res.write("event: connecttime\n");
    res.write("data: " + (new Date()) + "\n\n");
    res.write("data: " + (new Date()) + "\n\n");

    interval = setInterval(function () {
      res.write("data: " + (new Date()) + "\n\n");
    }, 1000);

    req.connection.addListener("close", function () {
      clearInterval(interval);
    }, false);
  }
}).listen(8844, "127.0.0.1");
```

### Page Visibility API

#### 简介

通常，我们使用各种事件，判断用户是否正在离开当前页面。

- visibilityState
- pageshow
- pagehide
- beforeunload
- unload

但是，手机浏览器往往不会触发这些事件，原因是浏览器进程会被突然关闭或者切换到后台，从而没有机会触发这些事件。常见的场景有以下这些。

- 用户点击了一条系统通知，切换到另一个 App。
- 用户进入任务切换窗口，切换到另一个 App。
- 用户点击了 Home 按钮，切换回主屏幕。
- 操作系统自动切换到另一个 App（比如，收到一个打入的电话）。

上面这些情况，都会导致浏览器进程切换到后台，也很可能会被操作系统自动终止，以便回收资源。 这使得`pagehide`、`beforeunload`、`unload`等事件根本不会触发。

这种情况下面，我们就要使用 Page Visibility API，判断页面是否可见。它可以保证会在手机浏览器得到执行。

```
// 页面的 visibility 属性可能返回三种状态
// prerender，visible 和 hidden
let pageVisibility = document.visibilityState;

// 监听 visibility change 事件
document.addEventListener('visibilitychange', function() {
  // 页面变为不可见时触发
  if (document.visibilityState == 'hidden') { ... }

  // 页面变为可见时触发
  if (document.visibilityState == 'visible') { ... }
});

```

其他的用途还包括根据用户行为调整程序。比如，在轮询的情况下，如果页面处于当前窗口，可以每隔15秒向服务器请求数据；如果不处于当前窗口，则每隔几分钟请求一次数据。

实际开发中，为了防止某些浏览器不支持这个 API，最好同时监听`pagehide`事件，这样会比较保险。

#### 属性

这个 API 部署在`document`对象上，提供以下两个属性。

- `document.hidden`：返回一个布尔值，表示当前是否被隐藏。

- ```
  document.visibilityState
  ```

  ：表示页面当前的状态，可以取三个值。

  - visibile：页面可见
  - hidden：页面不可见
  - prerender：页面即将或正在渲染，不可见

#### VisibilityChange 事件

页面的可见状态发生变化时，会触发`VisibilityChange`事件。

```
document.addEventListener('visibilitychange', function () {
  console.log(document.visibilityState);
});
```

### Fullscreen API：全屏操作

全屏API可以控制浏览器的全屏显示，让一个Element节点（以及子节点）占满用户的整个屏幕。目前各大浏览器的最新版本都支持这个API（包括IE11），但是使用的时候需要加上浏览器前缀。

#### 方法

##### requestFullscreen()

Element节点的requestFullscreen方法，可以使得这个节点全屏。

```
function launchFullscreen(element) {
  if(element.requestFullscreen) {
    element.requestFullscreen();
  } else if(element.mozRequestFullScreen) {
    element.mozRequestFullScreen();
  } else if(element.msRequestFullscreen){
    element.msRequestFullscreen();
  } else if(element.webkitRequestFullscreen) {
    element.webkitRequestFullScreen();
  }
}

launchFullscreen(document.documentElement);
launchFullscreen(document.getElementById("videoElement"));

```

放大一个节点时，Firefox和Chrome在行为上略有不同。Firefox自动为该节点增加一条CSS规则，将该元素放大至全屏状态，`width: 100%; height: 100%`，而Chrome则是将该节点放在屏幕的中央，保持原来大小，其他部分变黑。为了让Chrome的行为与Firefox保持一致，可以自定义一条CSS规则。

```
:-webkit-full-screen #myvideo {
  width: 100%;
  height: 100%;
}

```

##### exitFullscreen()

document对象的exitFullscreen方法用于取消全屏。该方法也带有浏览器前缀。

```
function exitFullscreen() {
  if (document.exitFullscreen) {
    document.exitFullscreen();
  } else if (document.msExitFullscreen) {
    document.msExitFullscreen();
  } else if (document.mozCancelFullScreen) {
    document.mozCancelFullScreen();
  } else if (document.webkitExitFullscreen) {
    document.webkitExitFullscreen();
  }
}

exitFullscreen();

```

用户手动按下ESC键或F11键，也可以退出全屏键。此外，加载新的页面，或者切换tab，或者从浏览器转向其他应用（按下Alt-Tab），也会导致退出全屏状态。

#### 属性

##### document.fullscreenElement

fullscreenElement属性返回正处于全屏状态的Element节点，如果当前没有节点处于全屏状态，则返回null。

```
var fullscreenElement =
  document.fullscreenElement ||
  document.mozFullScreenElement ||
  document.webkitFullscreenElement;

```

##### document.fullscreenEnabled

fullscreenEnabled属性返回一个布尔值，表示当前文档是否可以切换到全屏状态。

```
var fullscreenEnabled =
  document.fullscreenEnabled ||
  document.mozFullScreenEnabled ||
  document.webkitFullscreenEnabled ||
  document.msFullscreenEnabled;

if (fullscreenEnabled) {
  videoElement.requestFullScreen();
} else {
  console.log('浏览器当前不能全屏');
}

```

#### 全屏事件

以下事件与全屏操作有关。

- fullscreenchange事件：浏览器进入或离开全屏时触发。
- fullscreenerror事件：浏览器无法进入全屏时触发，可能是技术原因，也可能是用户拒绝。

```
document.addEventListener("fullscreenchange", function( event ) {
  if (document.fullscreenElement) {
    console.log('进入全屏');
  } else {
    console.log('退出全屏');
  }
});

```

上面代码在发生fullscreenchange事件时，通过fullscreenElement属性判断，到底是进入全屏还是退出全屏。

#### 全屏状态的CSS

全屏状态下，大多数浏览器的CSS支持`:full-screen`伪类，只有IE11支持`:fullscreen`伪类。使用这个伪类，可以对全屏状态设置单独的CSS属性。

```
:-webkit-full-screen {
  /* properties */
}

:-moz-full-screen {
  /* properties */
}

:-ms-fullscreen {
  /* properties */
}

:full-screen { /*pre-spec */
  /* properties */
}

:fullscreen { /* spec */
  /* properties */
}

/* deeper elements */
:-webkit-full-screen video {
  width: 100%;
  height: 100%;
}
```

### Web Speech

#### 概述

这个API用于浏览器接受语言输入，最早是由Google提出，目的是让用户直接进行语音搜索，即对着麦克风说出需要搜索的词，搜索结果就会自动出现。

Google首先部署的是input元素的speech属性(需要加上浏览器前缀x-webkit)，之后，输入框的右端会出现一个麦克风标志。点击该标志，就会跳出语音输入窗口。注意：目前只有Chrome浏览器支持该API。

```html
<input type="text" x-webkit-speech />
```

由于操作过于简单，Google又在它的基础上提出了Web Speech API，这使得JavaScript可以操作语音输入。

#### SpeechRecognition对象

这个API部署在SpeechRecognition对象上。

```javascript
var SpeechRecognition = window.SpeechRecognition || 
                        window.webkitSpeechRecognition || 
                        window.mozSpeechRecognition || 
                        window.oSpeechRecognition || 
                        window.msSpeechRecognition;
if(SpeechRecognition){
    var sr = new SpeechRecognition()		//实例化SpeechRecognition对象
    sr.maxAlternatives = 5					//表示最多返回5个语音匹配结果
}
```

#### 事件

目前，该API部署了11个事件，下面就介绍其中几个事件。

```javascript
var speak = document.querySelector("input")		//获取语音输入框
sr.onaudiostart = function(){}
sr.onnomatch = function(){}
sr.onerror = function(){}
```

首先，浏览器会询问用户是否允许浏览器获取麦克风数据。如果用户许可，就会触发audiostart事件，准备接收语音输入。如果找不到与语音匹配的值，就会触发nomatch事件。如果发生错误，就会触发error事件。

而如果得到与语音匹配的值，就会触发result事件。另外，result事件回调函数的参数，是一个SpeechRecognitionEvent对象，它的results属性就是一个语音匹配的结果数组，按照匹配度排序，最匹配的结果排在第一位。该数组的每个成员都是SpeechRecognitionResult对象，该对象的transcript属性是实际匹配的文本，confidence属性是可信度，在0~1之间。

```javascript
sr.onresult = function(e){
  if(e.results.length > 0){
    console.log("最匹配的文本：" + e.results[0].transcript+" 可信度："+ e.results[0].confidence)
  }
}
```

### requestAnimationFrame

#### 概述

requestAnimationFrame是浏览器用于定时循环操作的一个接口，类似于setTimeout，主要用途是按帧对网页进行重绘。

设置这个API的目的是为了让各种网页动画效果（DOM动画、Canvas动画、SVG动画、WebGL动画）能够有一个统一的刷新机制，从而节省系统资源，提高系统性能，改善视觉效果。代码中使用这个API，就是告诉浏览器希望执行一个动画，让浏览器在下一个动画帧安排一次网页重绘。

requestAnimationFrame的优势，在于充分利用显示器的刷新机制，比较节省系统资源。显示器有固定的刷新频率（60Hz或75Hz），也就是说，每秒最多只能重绘60次或75次，requestAnimationFrame的基本思想就是与这个刷新频率保持同步，利用这个刷新频率进行页面重绘。此外，使用这个API，一旦页面不处于浏览器的当前标签，就会自动停止刷新。这就节省了CPU、GPU和电力。

不过有一点需要注意，requestAnimationFrame是在主线程上完成。这意味着，如果主线程非常繁忙，requestAnimationFrame的动画效果会大打折扣。

requestAnimationFrame使用一个回调函数作为参数。这个回调函数会在浏览器重绘之前调用。

```
requestID = window.requestAnimationFrame(callback); 
```

目前，主要浏览器Firefox 23 / IE 10 / Chrome / Safari）都支持这个方法。可以用下面的方法，检查浏览器是否支持这个API。如果不支持，则自行模拟部署该方法。

```
 window.requestAnimFrame = (function(){
      return  window.requestAnimationFrame       || 
              window.webkitRequestAnimationFrame || 
              window.mozRequestAnimationFrame    || 
              window.oRequestAnimationFrame      || 
              window.msRequestAnimationFrame     || 
              function( callback ){
                window.setTimeout(callback, 1000 / 60);
              };
    })();
```

上面的代码按照1秒钟60次（大约每16.7毫秒一次），来模拟requestAnimationFrame。

使用requestAnimationFrame的时候，只需反复调用它即可。

```
function repeatOften() {
  // Do whatever
  requestAnimationFrame(repeatOften);
}

requestAnimationFrame(repeatOften);
```

#### cancelAnimationFrame方法

cancelAnimationFrame方法用于取消重绘。

```
window.cancelAnimationFrame(requestID);
```

它的参数是requestAnimationFrame返回的一个代表任务ID的整数值。

```
var globalID;

function repeatOften() {
  $("<div />").appendTo("body");
  globalID = requestAnimationFrame(repeatOften);
}

$("#start").on("click", function() {
  globalID = requestAnimationFrame(repeatOften);
});

$("#stop").on("click", function() {
  cancelAnimationFrame(globalID);
});
```

上面代码持续在body元素下添加div元素，直到用户点击stop按钮为止。

#### 实例

下面，举一个实例。

假定网页中有一个动画区块。

```
<div id="anim">点击运行动画</div> 
```

然后，定义动画效果。

```
var elem = document.getElementById("anim");

var startTime = undefined;
 
function render(time) {
 
  if (time === undefined)
    time = Date.now();
  if (startTime === undefined)
    startTime = time;
 
  elem.style.left = ((time - startTime)/10 % 500) + "px";
}
```

最后，定义click事件。

```
elem.onclick = function() {

    (function animloop(){
      render();
      requestAnimFrame(animloop);
    })();

};
```

运行效果可查看[jsfiddle](http://jsfiddle.net/paul/rjbGw/3/)。

### WebRTC

#### 概述

WebRTC是“网络实时通信”（Web Real Time Communication）的缩写。它最初是为了解决浏览器上视频通话而提出的，即两个浏览器之间直接进行视频和音频的通信，不经过服务器。后来发展到除了音频和视频，还可以传输文字和其他数据。

Google是WebRTC的主要支持者和开发者，它最初在Gmail上推出了视频聊天，后来在2011年推出了Hangouts，语序在浏览器中打电话。它推动了WebRTC标准的确立。

WebRTC主要让浏览器具备三个作用。

- 获取音频和视频
- 进行音频和视频通信
- 进行任意数据的通信

WebRTC共分成三个API，分别对应上面三个作用。

- MediaStream （又称getUserMedia）
- RTCPeerConnection
- RTCDataChannel

#### getUserMedia

##### 概述

navigator.getUserMedia方法目前主要用于，在浏览器中获取音频（通过麦克风）和视频（通过摄像头），将来可以用于获取任意数据流，比如光盘和传感器。

下面的代码用于检查浏览器是否支持getUserMedia方法。

```
navigator.getUserMedia  = navigator.getUserMedia ||
                          navigator.webkitGetUserMedia ||
                          navigator.mozGetUserMedia ||
                          navigator.msGetUserMedia;

if (navigator.getUserMedia) {
    // 支持
} else {
    // 不支持
}
```

Chrome 21, Opera 18和Firefox 17，支持该方法。目前，IE还不支持，上面代码中的msGetUserMedia，只是为了确保将来的兼容。

getUserMedia方法接受三个参数。

```
navigator.getUserMedia({
    video: true, 
    audio: true
}, onSuccess, onError);
```

getUserMedia的第一个参数是一个对象，表示要获取哪些多媒体设备，上面的代码表示获取摄像头和麦克风;onSuccess是一个回调函数，在获取多媒体设备成功时调用；onError也是一个回调函数，在取多媒体设备失败时调用。

下面是一个例子。

```
var constraints = {video: true};

function onSuccess(stream) {
  var video = document.querySelector("video");
  video.src = window.URL.createObjectURL(stream);
}

function onError(error) {
  console.log("navigator.getUserMedia error: ", error);
}

navigator.getUserMedia(constraints, onSuccess, onError);


```

如果网页使用了getUserMedia方法，浏览器就会询问用户，是否同意浏览器调用麦克风或摄像头。如果用户同意，就调用回调函数onSuccess；如果用户拒绝，就调用回调函数onError。

onSuccess回调函数的参数是一个数据流对象stream。stream.getAudioTracks方法和stream.getVideoTracks方法，分别返回一个数组，其成员是数据流包含的音轨和视轨（track）。使用的声音源和摄影头的数量，决定音轨和视轨的数量。比如，如果只使用一个摄像头获取视频，且不获取音频，那么视轨的数量为1，音轨的数量为0。每个音轨和视轨，有一个kind属性，表示种类（video或者audio），和一个label属性（比如FaceTime HD Camera (Built-in)）。

onError回调函数接受一个Error对象作为参数。Error对象的code属性有如下取值，说明错误的类型。

- **PERMISSION_DENIED**：用户拒绝提供信息。
- **NOT_SUPPORTED_ERROR**：浏览器不支持硬件设备。
- **MANDATORY_UNSATISFIED_ERROR**：无法发现指定的硬件设备。

##### 范例：获取摄像头

下面通过getUserMedia方法，将摄像头拍摄的图像展示在网页上。

首先，需要先在网页上放置一个video元素。图像就展示在这个元素中。

```
<video id="webcam"></video>
```

然后，用代码获取这个元素。

```
function onSuccess(stream) {
    var video = document.getElementById('webcam');
}
```

接着，将这个元素的src属性绑定数据流，摄影头拍摄的图像就可以显示了。

```
function onSuccess(stream) {
    var video = document.getElementById('webcam');
    if (window.URL) {
	    video.src = window.URL.createObjectURL(stream);
	} else {
		video.src = stream;
	}

	video.autoplay = true; 
	// 或者 video.play();
}

if (navigator.getUserMedia) {
	navigator.getUserMedia({video:true}, onSuccess);
} else {
	document.getElementById('webcam').src = 'somevideo.mp4';
}
```

在Chrome和Opera中，URL.createObjectURL方法将媒体数据流（MediaStream）转为一个二进制对象的URL（Blob URL），该URL可以作为video元素的src属性的值。 在Firefox中，媒体数据流可以直接作为src属性的值。Chrome和Opera还允许getUserMedia获取的音频数据，直接作为audio或者video元素的值，也就是说如果还获取了音频，上面代码播放出来的视频是有声音的。

获取摄像头的主要用途之一，是让用户使用摄影头为自己拍照。Canvas API有一个ctx.drawImage(video, 0, 0)方法，可以将视频的一个帧转为canvas元素。这使得截屏变得非常容易。

```
<video autoplay></video>
<img src="">
<canvas style="display:none;"></canvas>

<script>
  var video = document.querySelector('video');
  var canvas = document.querySelector('canvas');
  var ctx = canvas.getContext('2d');
  var localMediaStream = null;

  function snapshot() {
    if (localMediaStream) {
      ctx.drawImage(video, 0, 0);
      // “image/webp”对Chrome有效，
      // 其他浏览器自动降为image/png
      document.querySelector('img').src = canvas.toDataURL('image/webp');
    }
  }

  video.addEventListener('click', snapshot, false);

  navigator.getUserMedia({video: true}, function(stream) {
    video.src = window.URL.createObjectURL(stream);
    localMediaStream = stream;
  }, errorCallback);
</script>


```

##### 范例：捕获麦克风声音

通过浏览器捕获声音，需要借助Web Audio API。

```
window.AudioContext = window.AudioContext ||
                      window.webkitAudioContext;

var context = new AudioContext();

function onSuccess(stream) {
	var audioInput = context.createMediaStreamSource(stream);
	audioInput.connect(context.destination);
}

navigator.getUserMedia({audio:true}, onSuccess);


```

##### 捕获的限定条件

getUserMedia方法的第一个参数，除了指定捕获对象之外，还可以指定一些限制条件，比如限定只能录制高清（或者VGA标准）的视频。

```
var hdConstraints = {
  video: {
    mandatory: {
      minWidth: 1280,
      minHeight: 720
    }
  }
};

navigator.getUserMedia(hdConstraints, onSuccess, onError);

var vgaConstraints = {
  video: {
    mandatory: {
      maxWidth: 640,
      maxHeight: 360
    }
  }
};

navigator.getUserMedia(vgaConstraints, onSuccess, onError);


```

##### MediaStreamTrack.getSources()

如果本机有多个摄像头/麦克风，这时就需要使用MediaStreamTrack.getSources方法指定，到底使用哪一个摄像头/麦克风。

```
MediaStreamTrack.getSources(function(sourceInfos) {
  var audioSource = null;
  var videoSource = null;

  for (var i = 0; i != sourceInfos.length; ++i) {
    var sourceInfo = sourceInfos[i];
    if (sourceInfo.kind === 'audio') {
      console.log(sourceInfo.id, sourceInfo.label || 'microphone');

      audioSource = sourceInfo.id;
    } else if (sourceInfo.kind === 'video') {
      console.log(sourceInfo.id, sourceInfo.label || 'camera');

      videoSource = sourceInfo.id;
    } else {
      console.log('Some other kind of source: ', sourceInfo);
    }
  }

  sourceSelected(audioSource, videoSource);
});

function sourceSelected(audioSource, videoSource) {
  var constraints = {
    audio: {
      optional: [{sourceId: audioSource}]
    },
    video: {
      optional: [{sourceId: videoSource}]
    }
  };

  navigator.getUserMedia(constraints, onSuccess, onError);
}


```

上面代码表示，MediaStreamTrack.getSources方法的回调函数，可以得到一个本机的摄像头和麦克风的列表，然后指定使用最后一个摄像头和麦克风。

#### RTCPeerConnectionl，RTCDataChannel

##### RTCPeerConnectionl

RTCPeerConnection的作用是在浏览器之间建立数据的“点对点”（peer to peer）通信，也就是将浏览器获取的麦克风或摄像头数据，传播给另一个浏览器。这里面包含了很多复杂的工作，比如信号处理、多媒体编码/解码、点对点通信、数据安全、带宽管理等等。

不同客户端之间的音频/视频传递，是不用通过服务器的。但是，两个客户端之间建立联系，需要通过服务器。服务器主要转递两种数据。

- 通信内容的元数据：打开/关闭对话（session）的命令、媒体文件的元数据（编码格式、媒体类型和带宽）等。
- 网络通信的元数据：IP地址、NAT网络地址翻译和防火墙等。

WebRTC协议没有规定与服务器的通信方式，因此可以采用各种方式，比如WebSocket。通过服务器，两个客户端按照Session Description Protocol（SDP协议）交换双方的元数据。

下面是一个示例。

```
var signalingChannel = createSignalingChannel();
var pc;
var configuration = ...;

// run start(true) to initiate a call
function start(isCaller) {
    pc = new RTCPeerConnection(configuration);

    // send any ice candidates to the other peer
    pc.onicecandidate = function (evt) {
        signalingChannel.send(JSON.stringify({ "candidate": evt.candidate }));
    };

    // once remote stream arrives, show it in the remote video element
    pc.onaddstream = function (evt) {
        remoteView.src = URL.createObjectURL(evt.stream);
    };

    // get the local stream, show it in the local video element and send it
    navigator.getUserMedia({ "audio": true, "video": true }, function (stream) {
        selfView.src = URL.createObjectURL(stream);
        pc.addStream(stream);

        if (isCaller)
            pc.createOffer(gotDescription);
        else
            pc.createAnswer(pc.remoteDescription, gotDescription);

        function gotDescription(desc) {
            pc.setLocalDescription(desc);
            signalingChannel.send(JSON.stringify({ "sdp": desc }));
        }
    });
}

signalingChannel.onmessage = function (evt) {
    if (!pc)
        start(false);

    var signal = JSON.parse(evt.data);
    if (signal.sdp)
        pc.setRemoteDescription(new RTCSessionDescription(signal.sdp));
    else
        pc.addIceCandidate(new RTCIceCandidate(signal.candidate));
};


```

RTCPeerConnection带有浏览器前缀，Chrome浏览器中为webkitRTCPeerConnection，Firefox浏览器中为mozRTCPeerConnection。Google维护一个函数库[adapter.js](https://apprtc.appspot.com/js/adapter.js)，用来抽象掉浏览器之间的差异。

##### RTCDataChannel

RTCDataChannel的作用是在点对点之间，传播任意数据。它的API与WebSockets的API相同。

下面是一个示例。

```
var pc = new webkitRTCPeerConnection(servers,
  {optional: [{RtpDataChannels: true}]});

pc.ondatachannel = function(event) {
  receiveChannel = event.channel;
  receiveChannel.onmessage = function(event){
    document.querySelector("div#receive").innerHTML = event.data;
  };
};

sendChannel = pc.createDataChannel("sendDataChannel", {reliable: false});

document.querySelector("button#send").onclick = function (){
  var data = document.querySelector("textarea#send").value;
  sendChannel.send(data);
};


```

Chrome 25、Opera 18和Firefox 22支持RTCDataChannel。

##### 外部函数库

由于这两个API比较复杂，一般采用外部函数库进行操作。目前，视频聊天的函数库有[SimpleWebRTC](https://github.com/henrikjoreteg/SimpleWebRTC)、[easyRTC](https://github.com/priologic/easyrtc)、[webRTC.io](https://github.com/webRTC/webRTC.io)，点对点通信的函数库有[PeerJS](http://peerjs.com/)、[Sharefest](https://github.com/peer5/sharefest)。

下面是SimpleWebRTC的示例。

```
var webrtc = new WebRTC({
  localVideoEl: 'localVideo',
  remoteVideosEl: 'remoteVideos',
  autoRequestMedia: true
});

webrtc.on('readyToCall', function () {
    webrtc.joinRoom('My room name');
});


```

下面是PeerJS的示例。

```
var peer = new Peer('someid', {key: 'apikey'});
peer.on('connection', function(conn) {
  conn.on('data', function(data){
    // Will print 'hi!'
    console.log(data);
  });
});

// Connecting peer
var peer = new Peer('anotherid', {key: 'apikey'});
var conn = peer.connect('someid');
conn.on('open', function(){
  conn.send('hi!');
});
```

### Web Components

#### 概述

各种网站往往需要一些相同的模块，比如日历、调色板等等，这种模块就被称为“组件”（component）。Web Component就是网页组件式开发的技术规范。

采用组件进行网站开发，有很多优点。

（1）管理和使用非常容易。加载或卸载组件，只要添加或删除一行代码就可以了。

```
<link rel="import" href="my-dialog.htm">
<my-dialog heading="A Dialog">Lorem ipsum</my-dialog>

```

上面代码加载了一个对话框组件。

（2）定制非常容易。组件往往留出接口，供使用者设置常见属性，比如上面代码的heading属性，就是用来设置对话框的标题。

（3）组件是模块化编程思想的体现，非常有利于代码的重用。标准格式的模块，可以跨平台、跨框架使用，构建、部署和与其他UI元素互动都有统一做法。

（4）组件提供了HTML、CSS、JavaScript封装的方法，实现了与同一页面上其他代码的隔离。

未来的网站开发，可以像搭积木一样，把组件合在一起，就组成了一个网站。这是非常诱人的。

Web Components不是单一的规范，而是一系列的技术组成，包括Template、Custom Element、Shadow DOM、HTML Import四种技术规范。使用时，并不一定这四者都要用到。其中，Custom Element和Shadow DOM最重要，Template和HTML Import只起到辅助作用。

#### template标签

##### 基本用法

template标签表示网页中某些重复出现的部分的代码模板。它存在于DOM之中，但是在页面中不可见。

下面的代码用来检查，浏览器是否支持template标签。

```
function supportsTemplate() {
  return 'content' in document.createElement('template');
}

if (supportsTemplate()) {
  // 支持
} else {
  // 不支持
}

```

下面是一个模板的例子。

```
<template id="profileTemplate">
  <div class="profile">
    <img src="" class="profile__img">
    <div class="profile__name"></div>
    <div class="profile__social"></div>
  </div>
</template>

```

使用的时候，需要用JavaScript在模板中插入内容，然后将其插入DOM。

```
var template = document.querySelector('#profileTemplate');
template.content.querySelector('.profile__img').src = 'profile.jpg';
template.content.querySelector('.profile__name').textContent = 'Barack Obama';
template.content.querySelector('.profile__social').textContent = 'Follow me on Twitter';
document.body.appendChild(template.content);

```

上面的代码是将模板直接插入DOM，更好的做法是克隆template节点，然后将克隆的节点插入DOM。这样做可以多次使用模板。

```
var clone = document.importNode(template.content, true);
document.body.appendChild(clone);

```

接受template插入的元素，叫做宿主元素（host）。在template之中，可以对宿主元素设置样式。

```
<template>
<style>
  :host {
    background: #f8f8f8;
  }
  :host(:hover) {
    background: #ccc;
  }
</style>
</template>

```

##### document.importNode()

document.importNode方法用于克隆外部文档的DOM节点。

```
var iframe = document.getElementsByTagName("iframe")[0];
var oldNode = iframe.contentWindow.document.getElementById("myNode");
var newNode = document.importNode(oldNode, true);
document.getElementById("container").appendChild(newNode);

```

上面例子是将iframe窗口之中的节点oldNode，克隆进入当前文档。

注意，克隆节点之后，还必须用appendChild方法将其加入当前文档，否则不会显示。换个角度说，这意味着插入外部文档节点之前，必须用document.importNode方法先将这个节点准备好。

document.importNode方法接受两个参数，第一个参数是外部文档的DOM节点，第二个参数是一个布尔值，表示是否连同子节点一起克隆，默认为false。大多数情况下，必须显式地将第二个参数设为true。

#### Custom Element

HTML预定义的网页元素，有时并不符合我们的需要，这时可以自定义网页元素，这就叫做Custom Element。它是Web component技术的核心。举例来说，你可以自定义一个叫做super-button的网页元素。

```
<super-button></super-button>

```

注意，自定义网页元素的标签名必须含有连字符（-），一个或多个都可。这是因为浏览器内置的的HTML元素标签名，都不含有连字符，这样可以做到有效区分。

下面的代码用于测试浏览器是否支持自定义元素。

```
if ('registerElement' in document) {
  // 支持
} else {
  // 不支持
}

```

##### document.registerElement()

使用自定义元素前，必须用document对象的registerElement方法登记该元素。该方法返回一个自定义元素的构造函数。

```
var SuperButton = document.registerElement('super-button');
document.body.appendChild(new SuperButton());

```

上面代码生成自定义网页元素的构造函数，然后通过构造函数生成一个实例，将其插入网页。

可以看到，document.registerElement方法的第一个参数是一个字符串，表示自定义的网页元素标签名。该方法还可以接受第二个参数，表示自定义网页元素的原型对象。

```
var MyElement = document.registerElement('user-profile', {
  prototype: Object.create(HTMLElement.prototype)
});


```

上面代码注册了自定义元素user-profile。第二个参数指定该元素的原型为HTMLElement.prototype（浏览器内部所有Element节点的原型）。

但是，如果写成上面这样，自定义网页元素就跟普通元素没有太大区别。自定义元素的真正优势在于，可以自定义它的API。

```
var buttonProto = Object.create(HTMLElement.prototype);

buttonProto.print = function() {
  console.log('Super Button!');
}

var SuperButton = document.registerElement('super-button', {
  prototype: buttonProto
});

var supperButton = document.querySelector('super-button');

supperButton.print();

```

上面代码在原型对象上定义了一个print方法，然后将其指定为super-button元素的原型。因此，所有supper-button实例都可以调用print这个方法。

如果想让自定义元素继承某种特定的网页元素，就要指定extends属性。比如，想让自定义元素继承h1元素，需要写成下面这样。

```
var MyElement = document.registerElement('another-heading', {
  prototype: Object.create(HTMLElement.prototype),
  extends: 'h1'
});

```

另一个是自定义按钮（button）元素的例子。

```
var MyButton = document.registerElement('super-button', {
  prototype: Object.create(HTMLButtonElement.prototype),
  extends: 'button'
});

```

如果要继承一个自定义元素（比如`x-foo-extended`继承`x-foo`），也是采用extends属性。

```
var XFooExtended = document.registerElement('x-foo-extended', {
  prototype: Object.create(HTMLElement.prototype),
  extends: 'x-foo'
});

```

定义了自定义元素以后，使用的时候，有两种方法。一种是直接使用，另一种是间接使用，指定为某个现有元素是自定义元素的实例。

```
<!-- 直接使用 -->
<supper-button></supper-button>

<!-- 间接使用 -->
<button is="supper-button"></button>

```

总之，如果A元素继承了B元素。那么，B元素的is属性，可以指定B元素是A元素的一个实例。

##### 添加属性和方法

自定义元素的强大之处，就是可以在它上面定义新的属性和方法。

```
var XFooProto = Object.create(HTMLElement.prototype);
var XFoo = document.registerElement('x-foo', {prototype: XFooProto});

```

上面代码注册了一个x-foo标签，并且指明原型继承HTMLElement.prototype。现在，我们就可以在原型上面，添加新的属性和方法。

```
// 添加属性
Object.defineProperty(XFooProto, "bar", {value: 5});

// 添加方法
XFooProto.foo = function() {
  console.log('foo() called');
};

// 另一种写法
var XFoo = document.registerElement('x-foo', {
  prototype: Object.create(HTMLElement.prototype, {
    bar: {
      get: function() { return 5; }
    },
    foo: {
      value: function() {
        console.log('foo() called');
      }
    }
  })
});


```

##### 回调函数

自定义元素的原型有一些属性，用来指定回调函数，在特定事件发生时触发。

- **createdCallback**：实例生成时触发
- **attachedCallback**：实例插入HTML文档时触发
- **detachedCallback**：实例从HTML文档移除时触发
- **attributeChangedCallback(attrName, oldVal, newVal)**：实例的属性发生改变时（添加、移除、更新）触发

下面是一个例子。

```
var proto = Object.create(HTMLElement.prototype);

proto.createdCallback = function() {
  console.log('created');
  this.innerHTML = 'This is a my-demo element!';
};

proto.attachedCallback = function() {
  console.log('attached');
};

var XFoo = document.registerElement('x-foo', {prototype: proto});

```

利用回调函数，可以方便地在自定义元素中插入HTML语句。

```
var XFooProto = Object.create(HTMLElement.prototype);

XFooProto.createdCallback = function() {
  this.innerHTML = "<b>I'm an x-foo-with-markup!</b>";
};

var XFoo = document.registerElement('x-foo-with-markup',
  {prototype: XFooProto});


```

上面代码定义了createdCallback回调函数，生成实例时，该函数运行，插入如下的HTML语句。

```
<x-foo-with-markup>
   <b>I'm an x-foo-with-markup!</b>
</x-foo-with-markup>


```

#### Shadow DOM

所谓Shadow DOM指的是，浏览器将模板、样式表、属性、JavaScript代码等，封装成一个独立的DOM元素。外部的设置无法影响到其内部，而内部的设置也不会影响到外部，与浏览器处理原生网页元素（比如`<video>`元素）的方式很像。Shadow DOM最大的好处有两个，一是可以向用户隐藏细节，直接提供组件，二是可以封装内部样式表，不会影响到外部。Chrome 35+支持Shadow DOM。

Shadow DOM元素必须依存在一个现有的DOM元素之下，通过`createShadowRoot`方法创造，然后将其插入该元素。

```
var shadowRoot = element.createShadowRoot();
document.body.appendChild(shadowRoot);

```

上面代码创造了一个`shadowRoot`元素，然后将其插入HTML文档。

下面的例子是指定网页中某个现存的元素，作为Shadow DOM的根元素。

```
<button>Hello, world!</button>
<script>
  var host = document.querySelector('button');
  var root = host.createShadowRoot();
  root.textContent = '你好';
</script>

```

上面代码指定现存的`button`元素，为Shadow DOM的根元素，并将`button`的文字从英文改为中文。

通过innerHTML属性，可以为Shadow DOM指定内容。

```
var shadow = document.querySelector('#hostElement').createShadowRoot();
shadow.innerHTML = '<p>Here is some new text</p>';
shadow.innerHTML += '<style>p { color: red };</style>';

```

下面的例子是为Shadow DOM加上独立的模板。

```
<div id="nameTag">张三</div>

<template id="nameTagTemplate">
  <style>
    .outer {
      border: 2px solid brown;
    }
  </style>

  <div class="outer">
    <div class="boilerplate">
      Hi! My name is
    </div>
    <div class="name">
      Bob
    </div>
  </div>
</template>

```

上面代码是一个`div`元素和模板。接下来，就是要把模板应用到`div`元素上。

```
var shadow = document.querySelector('#nameTag').createShadowRoot();
var template = document.querySelector('#nameTagTemplate');
shadow.appendChild(template.content.cloneNode(true));

```

上面代码先用`createShadowRoot`方法，对`div`创造一个根元素，用来指定Shadow DOM，然后把模板元素添加为`Shadow`的子元素。

#### HTML Import

##### 基本操作

长久以来，网页可以加载外部的样式表、脚本、图片、多媒体，却无法方便地加载其他网页，iframe和ajax都只能提供部分的解决方案，且有很大的局限。HTML Import就是为了解决加载外部网页这个问题，而提出来的。

下面代码用于测试当前浏览器是否支持HTML Import。

```
function supportsImports() {
  return 'import' in document.createElement('link');
}

if (supportsImports()) {
  // 支持
} else {
  // 不支持
}


```

HTML Import用于将外部的HTML文档加载进当前文档。我们可以将组件的HTML、CSS、JavaScript封装在一个文件里，然后使用下面的代码插入需要使用该组件的网页。

```
<link rel="import" href="dialog.html">


```

上面代码在网页中插入一个对话框组件，该组建封装在`dialog.html`文件。注意，dialog.html文件中的样式和JavaScript脚本，都对所插入的整个网页有效。

假定A网页通过HTML Import加载了B网页，即B是一个组件，那么B网页的样式表和脚本，对A网页也有效（准确得说，只有style标签中的样式对A网页有效，link标签加载的样式表对A网页无效）。所以可以把多个样式表和脚本，都放在B网页中，都从那里加载。这对大型的框架，是很方便的加载方法。

如果B与A不在同一个域，那么A所在的域必须打开CORS。

```
<!-- example.com必须打开CORS -->
<link rel="import" href="http://example.com/elements.html">


```

除了用link标签，也可以用JavaScript调用link元素，完成HTML Import。

```
var link = document.createElement('link');
link.rel = 'import';
link.href = 'file.html'
link.onload = function(e) {...};
link.onerror = function(e) {...};
document.head.appendChild(link);


```

HTML Import加载成功时，会在link元素上触发load事件，加载失败时（比如404错误）会触发error事件，可以对这两个事件指定回调函数。

```
<script async>
  function handleLoad(e) {
    console.log('Loaded import: ' + e.target.href);
  }
  function handleError(e) {
    console.log('Error loading import: ' + e.target.href);
  }
</script>

<link rel="import" href="file.html"
      onload="handleLoad(event)" onerror="handleError(event)">


```

上面代码中，handleLoad和handleError函数的定义，必须在link元素的前面。因为浏览器元素遇到link元素时，立刻解析并加载外部网页（同步操作），如果这时没有对这两个函数定义，就会报错。

HTML Import是同步加载，会阻塞当前网页的渲染，这主要是为了样式表的考虑，因为外部网页的样式表对当前网页也有效。如果想避免这一点，可以为link元素加上async属性。当然，这也意味着，如果外部网页定义了组件，就不能立即使用了，必须等HTML Import完成，才能使用。

```
<link rel="import" href="/path/to/import_that_takes_5secs.html" async>


```

但是，HTML Import不会阻塞当前网页的解析和脚本执行（即阻塞渲染）。这意味着在加载的同时，主页面的脚本会继续执行。

最后，HTML Import支持多重加载，即被加载的网页同时又加载其他网页。如果这些网页都重复加载同一个外部脚本，浏览器只会抓取并执行一次该脚本。比如，A网页加载了B网页，它们各自都需要加载jQuery，浏览器只会加载一次jQuery。

##### 脚本的执行

外部网页的内容，并不会自动显示在当前网页中，它只是储存在浏览器中，等到被调用的时候才加载进入当前网页。为了加载网页网页，必须用DOM操作获取加载的内容。具体来说，就是使用link元素的import属性，来获取加载的内容。这一点与iframe完全不同。

```
var content = document.querySelector('link[rel="import"]').import;


```

发生以下情况时，link.import属性为null。

- 浏览器不支持HTML Import
- link元素没有声明`rel="import"`
- link元素没有被加入DOM
- link元素已经从DOM中移除
- 对方域名没有打开CORS

下面代码用于从加载的外部网页选取id为template的元素，然后将其克隆后加入当前网页的DOM。

```
var el = linkElement.import.querySelector('#template');

document.body.appendChild(el.cloneNode(true));


```

当前网页可以获取外部网页，反过来也一样，外部网页中的脚本，不仅可以获取本身的DOM，还可以获取link元素所在的当前网页的DOM。

```
// 以下代码位于被加载（import）的外部网页

// importDoc指向被加载的DOM
var importDoc = document.currentScript.ownerDocument;

// mainDoc指向主文档的DOM
var mainDoc = document;

// 将子页面的样式表添加主文档
var styles = importDoc.querySelector('link[rel="stylesheet"]');
mainDoc.head.appendChild(styles.cloneNode(true));


```

上面代码将所加载的外部网页的样式表，添加进当前网页。

被加载的外部网页的脚本是直接在当前网页的上下文执行，因为它的`window.document`指的是当前网页的document，而且它定义的函数可以被当前网页的脚本直接引用。

##### Web Component的封装

对于Web Component来说，HTML Import的一个重要应用是在所加载的网页中，自动登记Custom Element。

```
<script>
  // 定义并登记<say-hi>
  var proto = Object.create(HTMLElement.prototype);

  proto.createdCallback = function() {
    this.innerHTML = 'Hello, <b>' +
                     (this.getAttribute('name') || '?') + '</b>';
  };

  document.registerElement('say-hi', {prototype: proto});
</script>

<template id="t">
  <style>
    ::content > * {
      color: red;
    }
  </style>
  <span>I'm a shadow-element using Shadow DOM!</span>
  <content></content>
</template>

<script>
  (function() {
    var importDoc = document.currentScript.ownerDocument; //指向被加载的网页

    // 定义并登记<shadow-element>
    var proto2 = Object.create(HTMLElement.prototype);

    proto2.createdCallback = function() {
      var template = importDoc.querySelector('#t');
      var clone = document.importNode(template.content, true);
      var root = this.createShadowRoot();
      root.appendChild(clone);
    };

    document.registerElement('shadow-element', {prototype: proto2});
  })();
</script>


```

上面代码定义并登记了两个元素：<say-hi>和<shadow-element>。在主页面使用这两个元素，非常简单。

```
<head>
  <link rel="import" href="elements.html">
</head>
<body>
  <say-hi name="Eric"></say-hi>
  <shadow-element>
    <div>( I'm in the light dom )</div>
  </shadow-element>
</body>


```

不难想到，这意味着HTML Import使得Web Component变得可分享了，其他人只要拷贝`elements.html`，就可以在自己的页面中使用了。

#### Polymer.js

Web Components是非常新的技术，为了让老式浏览器也能使用，Google推出了一个函数库[Polymer.js](http://www.polymer-project.org/)。这个库不仅可以帮助开发者，定义自己的网页元素，还提供许多预先制作好的组件，可以直接使用。

##### 直接使用的组件

Polymer.js提供的组件，可以直接插入网页，比如下面的google-map。。

```
<script src="components/platform/platform.js"></script>
<link rel="import" href="google-map.html">
<google-map lat="37.790" long="-122.390"></google-map>


```

再比如，在网页中插入一个时钟，可以直接使用下面的标签。

```
<polymer-ui-clock></polymer-ui-clock>


```

自定义标签与其他标签的用法完全相同，也可以使用CSS指定它的样式。

```
polymer-ui-clock {
  width: 320px;
  height: 320px;
  display: inline-block;
  background: url("../assets/glass.png") no-repeat;
  background-size: cover;
  border: 4px solid rgba(32, 32, 32, 0.3);
}


```

##### 安装

如果使用bower安装，至少需要安装platform和core components这两个核心部分。

```
bower install --save Polymer/platform
bower install --save Polymer/polymer


```

你还可以安装所有预先定义的界面组件。

```
bower install Polymer/core-elements
bower install Polymer/polymer-ui-elements


```

还可以只安装单个组件。

```
bower install Polymer/polymer-ui-accordion


```

这时，组件根目录下的bower.json，会指明该组件的依赖的模块，这些模块会被自动安装。

```
{
  "name": "polymer-ui-accordion",
  "private": true,
  "dependencies": {
    "polymer": "Polymer/polymer#0.2.0",
    "polymer-selector": "Polymer/polymer-selector#0.2.0",
    "polymer-ui-collapsible": "Polymer/polymer-ui-collapsible#0.2.0"
  },
  "version": "0.2.0"
}


```

##### 自定义组件

下面是一个最简单的自定义组件的例子。

```
<link rel="import" href="../bower_components/polymer/polymer.html">
 
<polymer-element name="lorem-element">
  <template>
    <p>Lorem ipsum</p>
  </template>
</polymer-element>


```

上面代码定义了lorem-element组件。它分成三个部分。

**（1）import命令**

import命令表示载入核心模块

**（2）polymer-element标签**

polymer-element标签定义了组件的名称（注意，组件名称中必须包含连字符）。它还可以使用extends属性，表示组件基于某种网页元素。

```
<polymer-element name="w3c-disclosure" extends="button">


```

**（3）template标签**

template标签定义了网页元素的模板。

##### 组件的使用方法

在调用组件的网页中，首先加载polymer.js库和组件文件。

```
<script src="components/platform/platform.js"></script>
<link rel="import" href="w3c-disclosure.html">


```

然后，分成两种情况。如果组件不基于任何现有的HTML网页元素（即定义的时候没有使用extends属性），则可以直接使用组件。

```
<lorem-element></lorem-element>


```

这时网页上就会显示一行字“Lorem ipsum”。

如果组件是基于（extends）现有的网页元素，则必须在该种元素上使用is属性指定组件。

```
<button is="w3c-disclosure">Expand section 1</button>
```

































