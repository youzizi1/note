### 服务器

服务器，又称伺服器，是提供计算服务的设备。服务器可以进行如下分类：

* 按照服务类型可分为：文件服务器、数据库服务器、邮件服务器、web服务器等。
* 按照操作系统可分为：linux服务器、windows服务器。
* 按照应用软件可分为apache服务器，nginx服务器、IIs服务器、Tomcat服务器、node服务器等

使计算机具备提供某种服务能力的应用软件，称为服务器软件。

- 文件服务器：Serve-U、FileZilla、VsFTP等
- 数据库服务器：Oracle、MySQL、PostgreSQL、MSSQL、DB2等
- 邮件服务器：Postfix、Sendmail等
- HTTP服务器：Apache、Nginx、IIS、Tomcat、NodeJS等

### HTTP协议

计算机与计算机之间通过协议进行通信，常见的协议有：http、ftp、smtp/pop等。协议的本质就是交互双方以约定的方式进行沟通。

其中HTTP协议即超文本传输协议(HyperText Transfer Protocol)，它是计算机通过网络通信的规则，规定Web浏览器是如何从Web服务器获取文档和向Web服务器提交表单内容，以及Web服务器是如何响应这些请求。HTTP是一种无状态的协议，无状态是指不建立持久的连接。简单来说就是客户端发出请求，服务器作出响应，接着连接就被关闭，所以服务器不保留连接的相关信息。

一次完整的HTTP通常会经历下面步骤：

1. 建立TCP连接
2. web浏览器向Web服务器发送请求
3. Web浏览器发送请求头信息
4. Web服务器作出响应
5. Web服务器发送响应头信息
6. Web服务器向浏览器发送响应主体
7. Web服务器关闭TCP连接

### 脚本化HTTP

在Web早期，HTTP请求只能通过用户单击链接，提交表单和输入URL产生。而现在，可以使用JavaScript代码发出一个HTTP请求。例如：使用脚本设置Window对象的location属性或者调用表单对象的submit()方法，都会初始化一个HTTP请求。但是浏览器通常会加载新页面处理这些HTTP请求。

所以接下来将要了解如何让Web浏览器不重新加载任何窗口或窗体的情况下，实现Web浏览器和Web服务器之间的通信。这项技术通常称为Ajax(Asynchronous JavaScript and XML)。

Ajax描述了一种主要使用脚本操纵HTTP的Web应用架构。这些应用的特点就是在不导致页面重载的情况下，使用脚本操纵Web浏览器和Web服务器进行数据交换，使得Web应用更像是传统的桌面应用。注意：Ajax不会把用户的交互数据记录到服务器中。

而Comet是另一种使用脚本操纵HTTP的Web应用架构。它和Ajax正好相反。在Comet中，Web服务器发起异步通信并异步发送消息到客户端。简单来说，使用Ajax让客户端从服务器端拉数据，使用Comet让服务器端向客户端推数据。

实现Ajax和Comet的方式有多种，而这些底层的实现有时称为传输协议。例如img元素的src属性设置为URL时，浏览器会发出一个HTTP GET请求从这个URL中下载图片，此时可以将有关信息作为URL的查询字符串部分，这样就能够实现和服务器端之间相互通信。

而img元素无法实现完整的Ajax传输协议，因为数据交换时单向的。因为客户端能发送数据到服务器，但是服务器却一直以图片作为响应结果，而无法提供有效数据信息。因此可以通过设置iframe元素的src属性为该URL，这样服务器就可以返回一个包含数据信息的HTML文档返回给Web浏览器。一般来讲，都会使用CSS让iframe元素隐藏，然后通过JavaScript提出返回的HTML文档中包含的数据信息。注意：这种访问受到同源策略限制。

实际上，script元素的src属性也能设置URL并发起HTTP GET请求，使用这种方式可以服务器跨域通信。通常，使用基于script的Ajax传输协议时，服务器的响应采用JSON编码的数据格式，因此这种Ajax传输协议也被称为JSONP。

虽然可以使用元素的src属性实现Ajax技术，但是通常还是通过所有浏览器都支持的XMLHttpRequest对象来实现。该对象上定义操纵HTTP的API。该API既可以实现GET请求，也可以实现POST请求。该对象还支持包括XML在内的任何基于文本的格式来返回服务器的响应，并且它还能用于HTTP和HTTPS请求。

无论是Comet还是Ajax，都需要客户端和服务器端之间建立连接，同时需要服务器保持连接处于打开状态，只有这样才能发送异步信息。但是实现可靠的跨平台Comet传输协议是非常困难的，大多是依赖工具库。此外：HTML5标准草案中定义了Server-Sent事件，它以EventSource对象的形式定义了简单的COmet API。

### XMLHttpRequest

浏览器在XMLHttpRequest对象上定义了HTTP API，这个对象的每个实例都表示一个独立的请求/响应对，并且这个对象的属性和方法允许指定请求细节和提取响应数据。目前多数浏览器支持支持XMLHttpRequest，并且W3C还在指定2级XMLHttpRequest标准草案，简称为XHR2。

要使用XMLHttpRequest对象上定义的HTTP API，必须先实例化一个XMLHttpRequest对象。当然还可以重用已存在的XMLHttpRequest实例对象。注意：IE5,6使用ActiveX对象代替XMLHttpRequest对象，IE7不支持非标准的XMLHttpRequest()构造函数。

```javascript
if(window.XMLHttpRequest === undefined){
    window.XMLHttpRequest = function(){
        try{
          	//如果支持最新的ActiveX对象就是用它
            return new ActiveXObject("Msxml2.XMLHTTP.6.0");
        }catch(e1){
            try{
              	//否则退回旧的版本
                return new ActiveXObject("Msxml2.XMLHTTP.3.0");
            }catch(e2){
                throw new Error("XMLHttpRequest is not support");
            }
        }
    }
}
var xhr = = new XMLHttpRequest();
```

一个HTTP请求由四部分组成：请求方法，请求地址，请求头(可能包含身份验证信息)和请求主体。而一个HTTP响应由三部分组成：响应头，响应主体以及一个由数字和文字组成的状态码，用来显示请求的成功和失败。

注意：XMLHttpRequest不是协议级的HTTP API而是浏览器级的API，所以浏览器会考虑cookie、重定向、缓存和代码问题，而脚本只需要考虑请求和响应。

此外，XMLHttpRequest同HTTP和HTTPS协议工作，因此使用XMLHttpRequest进行Ajax编程时，需要将文件上传到Web服务器或者本地服务器。

#### 发出请求

实例化XMLHttpRequest对象之后，发起HTTP请求的下一步是使用XMLHttpRequest对象的open()方法来指定请求方法和请求地址。

```javascript
xhr.open("GET","http://www.baidu.com");
```

open()方法的第一个参数表示HTTP请求方法或动作，该参数不区分大小写，但是通常使用大写来匹配HTTP协议。通常请求方法有GET和POST两种，它们之间的区别如下：

* 当请求对服务器没有任何副作用以及当服务器的响应是可缓存时，常用GET请求。也就是说GET请求用于常规请求。
* POST请求多用于HTML表单。它在请求主体中包含表单数据且这些数据常存储到服务器上的数据库中(副作用)。注意：相同的请求地址URL的重复POST请求从服务器得到的响应可能不同，并且还不应该缓存使用该方法的请求。

除了GET和POST之外，XMLHttpRequest规范还允许DELETE，HEAD，OPTIONS和PUT作为open()方法的第一个参数。但是目前仅有HEAD得到广泛支持。

open()方法的第二个参数表示请求地址URL。如果指定的是绝对URL，那么极有可能会发生跨域请求报错。当然可以使用XHR2指定服务器允许跨域请求。

一次完整的HTTP请求还需要设置请求头。可以使用Content-Type头指定请求主题的MIME类型：

```javascript
xhr.setRequestHeader("Content-Type","text/plain")
```

注意：如果对相同的头多次调用setRequestHeader()多次，新值不会取代之前指定的值。相反的是，HTTP请求将包含这个头的多个副本或这个头将指定多个值。

此外，不能手动指定Content-Length，Date，Refer或User-Agent头，XMLHttpRequest将自动添加这些头以防止伪造它们。类似地，XMLHttpRequest对象会自动处理cookie，连接时间，字符集和编码判断，所以还无法向setRequestHeader()设置这些头：

> Accept-Charset,Accept-Encoding,Connection,Content-Length,Cookie,Cookie2
>
> Content-Transfer-Encoding,Date,Expect,Host,Keep-Alive,Referer
>
> TE,Trailer,Transfer-Encoding,Upgrade,User-Agent,Via

只能手动为请求指定Authorization头，但是通常无需这么做。如果请求一个受密码保护的URL，需要把用户名和密码作为第四个和四五个参数传递给open()，此时XMLHttpRequest将设置合适的头。

使用XMLHttpRequest发起HTTP请求的最后一步是指定可选的请求主体并向服务器发送它。

```javascript
xhr.send(null);
```

注意：GET请求没有请求主体，所以需要传递null为参数或省略该参数。对于POST请求来说，通常拥有请求主体，并且请求主体应该匹配使用setRequestHeader()方法设置的Content-Type头。

此外，HTTP请求的各部分都有指定顺序：请求方法和URL先到达，然后是请求头，最后是请求体。XMLHttpRequest规范指定直到调用send()才开始启动网络，但是浏览器的实现却使每个方法都写入网络流，这样就意味着调用XMLHttpRequest方法的顺序必须匹配HTTP请求的架构。例如调用setRequestHeader()方法必须在open()方法之前，否则将会抛出异常。

#### 得到响应

一个完整的HTTP响应由状态码，响应头集合和响应主体组成，这些都可以通过XMLHttpRequest对象上定义的属性和方法来获取。

* status和status属性是以数字和文本的形式返回HTTP状态码，这些属性保存标准的HTTP值。常见的状态码有：
  * 1xx：信息类，表示收到Web浏览器请求，正在进一步的处理中
  * 2XX：成功，表示用户请求被正确接收，理解和处理，例如200
  * 3XX：重定向，表示请求没有成功，客户必须采取进一步的动作
  * 4XX：客户端错误，表示客户端提交的请求有错误，例如404
  * 5XX：服务器错误，表示服务器不能完成对请求的处理，例如500
* 使用getResponseHeader()和getAllResponseHeaders()方法来查询响应头。XMLHttpRequest会自动处理cookie，它会从getAllResponseHeaders()方法返回的响应头集合中过滤掉cookie头，但是如果给getResponseHeader()传递`Set-Cookie`和`Set-Cookie2`则返回null。
* 响应主体可以从responseText属性中得到其文本形式，从responseXML属性中得到Document形式。注意：该属性针对XHTML和XML文档有效，但是XHR2草案还指定对HTML文档有效。

上面列举的属性和方法只有响应返回时才有效，所以必须监听XMLHttpRequest对象上的readystatechange事件来为响应准备就绪时得到通知。在了解这个事件之前，先了解readyState属性。

readyState属性表示HTTP请求的状态，其属性值是一个整数。

| 常量               | 值    | 含义         |
| ---------------- | ---- | ---------- |
| UNSENT           | 0    | open()尚未调用 |
| OPENED           | 1    | open()已调用  |
| HEADERS_RECEIVED | 2    | 接收到头信息     |
| LOADING          | 3    | 接收到响应主体    |
| DONE             | 4    | 响应         |

上表中第一列表示XMLHttpRequest构造函数定义的常量，这些常量是XMLHttpRequest规范的一部分。注意：老旧浏览器并没有定义这些常量，通常使用数字值来代表前面的常量。

理论上，readyState属性每次改变都会触发readystatechange事件。但是实际上，当readyStae改变为0或1时可能没有触发该事件。一些浏览器在LOADING状态时会触发多次该事件来及时给出进度反馈，并且所有的浏览器都会在属性值为4时触发readystatechange事件。

可以使用addEventListener()方法为XMLHttpRequest的实例对象注册事件处理程序，但是通常每个请求只需要一个事件处理程序，通常通过设置元素对象属性来注册事件处理程序。

##### 同步响应

通常异步处理HTTP响应是最好的方式，但是XMLHttpRequest同样支持同步响应。如果open()的第三个参数传入false，那么send()方法将阻塞并直到请求完成，也就是说send()方法后面的代码不会执行直到请求成功。由于是同步响应，那么就不需要监听readystatechange事件了。

但是通常情况下很少使用同步响应，因为客户端JavaScript是单线程的。当send()方法阻塞时，通常会导致整个浏览器ui冻结，如果服务器响应很慢，那么用户需要等待很久。

##### 响应解码

如果服务器使用`text/plain`、`text/html`或`text/css`这样的MIME类型发送文本响应，就可以使用XMLHttpRequest对象的responseText属性获取响应主体。如果服务器发送XML或XHTML文档作为其响应，需要使用responseXML来获取一个解析形式的XML文档，该属性值是一个Document对象，使用标准的DOM操作来提取数据信息。当然：XHR2规定浏览器也应该自动解析`text/html`类型的响应，使它同样可以通过responseXML属性获取其Document对象。

如果服务器以对象或数组这样的结构化数据作为响应，那么应该传输JSON编码的字符串形式。然后使用responseText属性接收并传递给JSON.parse()方法将其序列化。

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET","json.php");
xhr.setRequestHeader("Content-Type","application/json");
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4 && xhr.status === 200){
      //readyState为4表示得到响应数据，但是未知是成功还是失败的数据，所以同时还要判断status是否为200
        if(xhr.getResponseHeader("Content-type") === "application/json" && xhr.responseText){
            var data = JSON.parse(xhr.responseText);
        }
    }
}
```

根据上面代码，此时可能会希望是否存在MIME类型为`application/javascript`或者`text/javascript`，这样就可以请求一个脚本，然后使用eval()函数执行它。但是此时，这种情况多数会使用script元素而不是XMLHttpRequest对象，因为script元素本身具有操纵HTTP脚本的能力并且可以实现加载并执行脚本。注意：script元素的可以发出跨域请求，但是XMLHttpRequest对象禁止发出跨域请求。

Web服务器端通常使用二进制数据(图片文件)响应HTTP请求。例如，responseText属性只能用于文本，而无法处理二进制响应，即使是使用字符串的charCodeAt()方法。还好，XHR2定义了处理二进制响应的方法，不过还未广泛实现。

此外，服务器响应的正常解码是假设服务器为这个响应发送了Content-Type头和正确的MIME类型。如果服务器发送XML文档，但是却没有在请求头上设置适当的MIME类型，那么XMLHttpRequest对象将不会解析它且设置responseText属性。或者服务器在`Content-Type`头中包含了错误的`charset`参数，那么XMLHttpRequest将使用错误的编码来解析响应，并且responseText中的字符可能是错的。因此，XHR2定义了overrideMimeType()方法来解决这个问题，并且大量的浏览器已经实现了它。如果相对于服务器你更了解资源的MIME类型，那么在调用send()之前把类型传递给overrideMimeType()，这将使XMLHttpRequest忽略"Content-Type"头而使用指定的类型。假设你将下载XML文件，而你计划把它当成纯文本对待。可以使用setOverrideMimeType()让XMLHttpRequest知道它不需要把文件解析成XML文档：

```javascript
xhr.overrideMimeType("text/plain;charset=utf-8");
```

#### 编码请求主体

当发出HTTP POST请求时需要设置一个请求主体，它包含客户端传递给服务器的数据。请求主体中可以是简单的文本字符串，也可以是更复杂的数据。

##### 表单编码的请求

当用户提交表单时，表单中的数据，级每个表单元素的名字和值被编码到一个字符串中并且随请求发送。默认情况下，HTML表单通过POST请求发送给服务器，而编码后的表单数据则被当做请求主体。对表单数据使用的编码方案相对简单：对每个表单元素的名字和值执行普通的URL编码，即用十六进制转义码替换特殊字符，然后使用等号把编码后的名字和值分开，最后用&符号分开名/值对。

```
find=pizza&zipcode=02134&radius=1km
```

并且表单数据编码格式有一个标准的MIME类型。并且当使用POST方法提交这种顺序的表单数据时，必须Content-Type请求头为该值。

```
application/x-www-form-urlencoded
```

实际上，这种类型的编码并不需要HTML表单。通过标准的DOM操作获取用户输入的数据，并且序列化为JSON格式。然后通过Ajax发送到服务器，从而不需要设置Content-Type请求头。

```javascript
{
    find:'pizza',
    zipcode:'02134',
    radius:'1km'
}
```

如果表单提交的目的是为了执行只读查询，那么GET请求比POST请求更加合适。GET请求没有主体，所以需要发送给服务器的表单编码数据负载要作为URL(问号后面)的查询部分。

##### JSON编码的请求

目前，JSON作为Web的主要数据交换格式。例如上面的JSON格式数据。

```javascript
var data =  {
    find:'pizza',
    zipcode:'02134',
    radius:'1km'
}
xhr.onreadystatechange = function(){}
xhr.setRequestHeader("Content-Type","application/json");
xhr.send(JSON.stringify(data));
```

##### XML编码的请求

当然除了使用表单编码和JSON编码作为请求主体的数据格式，XML文档也是一种方法。

```xml
<find>pizza</find>
```

注意：当使用XML文档作为数据格式时，不需要设置指定的Content-Type头，因为XMLHttpRequest对象会自动设置一个合适的头。类似地，如果给send()传入一个字符串但没有指定Content-Type头，那么XMLHttpRequest将会添加`text/plain;charset=UTF-8`头。

##### 上传文件

HTML表单的特性之一就是允许使用类型为file的表单元素选择文件，此时将会产生一个POST请求并且发送该文件内容。但是目前的XMLHttpRequest API还无法做到手动上传文件，必须借助于表单元素。注意：XHR2标准草案中允许通过向send()方法传入File对象来实现上传文件操作。

没有File()构造函数，脚本只能只能获得用户当前选择文件的File对象。在支持File对象的浏览器中，每个`<input type="file" />`元素都有一个files属性，它是File对象中的类数组对象。注意：拖放API允许通过拖放事件的dataTransfer.files属性访问用户拖放到元素上的文件。

文件类型是更通用的二进制大对象(Blob)类型中的一个子类型。XHR2允许向send()方法传入任何Blob()对象。如果没有显式设置Content-type头，那么这个Blob对象的type属性用于设置待上传的Content-Type头。如果需要上传已经产生的二进制数据，可以将数据转化为Blob并将其作为请求主体。

##### multipart/form-data请求

当HTML表单同时包含文件上传元素和其它元素时，浏览器不能使用普通的表单编码而需要使用`multipart/form-data`编码作为Content-Type头来用POST方法提交表单。这种编码包括使用长边界字符串把请求主体分离成多个部分。对于文本数据，手动创建"multipart/form-data"请求主体是可能的，但很复杂。

XHR2定义了新的FormData API，它容易实现多部分请求主体。首先，使用FormData()构造函数创建FormData对象，然后按需多次调用这个对象的append()方法把个体部分(可以是字符串、File或Blob对象)添加到请求中。最后，把FormData对象传递给send()方法。send()方法将对请求定义合适的边界字符串和设置Content-Type头。

#### HTTP进度事件

前面了解监听readystatechange事件可以嗅探HTTP请求的完成。但是在XHR2草案中定义了更多有用的事件集，这些事件会在请求的不同阶段触发，所以不再需要检测readyState属性。但是这些事件还未广泛支持。

在支持这些事件的浏览器中，当调用send()方法时，会触发loadStart事件。当正在加载服务器的响应时，会触发progress事件，通常每隔50毫秒就触发一次，所以这些事件反馈给用户请求的进度。如果请求快速完成还可能不触发progress事件。但请求完成，还会触发load事件。但是一次完成的请求不一定代表请求成功，所以应该手动检查status属性值是否是200。

HTTP请求无法完成还会触发三种事件。如果请求超时，会触发timeout事件。如果请求中止，会触发abort事件。如果太多重定向的网络错误而阻止请求完成，会触发error事件。当然浏览器只会触发load，abort，timeout和error事件中的一个。主意：XHR2规范草案规定这些事件中的一个发生后，浏览器应该触发loadend事件，但是该事件还没有浏览器实现。

这些新定义了事件通常和readystatchange事件一样使用元素对象属性注册事件处理程序。progress事件的事件对象定义除了常用的type和timestamp属性之外，还定义了三个有用的属性。其中loaded属性是目前传输的字节数值。total属性是自Content-Length头传输的数据的整体长度(单位是字节)，如果不知道内容长度则为0。最后一个属性是lengthComputable属性，如果知道内容长度为true，否则为false。果知道内容长度则lengthComputable属性为true，否则为false。

##### 上传进度事件

除了新增了监控HTTP响应的事件之外，XHR2还新增了监控HTTP请求上传的事件。在支持这些事件的浏览器中，XMLHttpRequest对象上定义了upload属性，该属性值是一个对象。并且该对象定义了addEventListener()方法和整个progress事件集合，例如onprogress和onload，但是upload对象没有定义onreadystatechange属性，upload技能触发新的事件类型。但是这些事件还未广泛实现。

此外，可以像使用progress事件处理程序一样使用upload事件处理程序。例如：xhr.onprogress来监控响应的下载Suunto，设置xhr.upload.onprogress来监控请求的上传速度。

#### 终止请求和超时

可以通过调用XMLHttpRequest对象上的abort()方法来取消正在进行的HTTP请求，并且调用该方法会触发该对象上的abort事件。注意：abort事件还未广泛实现，但是该方法在所有XMLHttpRequest版本和XHR2中都有定义。

XHR2还定义了timeout属性来指定请求自动终止后的毫秒数，以及请求超时时会触发timeout事件。

#### 跨域HTTP请求

由于同源策略的限制，XMLHttpRequest对象通常只能发出和文档具有相同服务器的HTTP请求，而form元素和iframe元素可以进行跨域请求。

幸运的是，XHR2标准中允许通过在HTTP响应中选择发送合适的CORS(Cross-Origin Resource Sharing跨域资源共享)来进行跨域访问。但是目前只有主流浏览器支持CORS，IE8并不支持。使用CORS的好处就是同源策略不会放宽，同时跨域请求也能正常工作。

注意：如果给open()方法传入用户名和密码，那么无论如何都不会通过跨域请求发送。除此之外，跨域请求通常也不会包含其它任何的用户证书。例如cookie和HTTP身份验证令牌通常不会作为请求的内容部分发送且任何作为跨域响应来接收的cookie都会丢弃。如果跨域请求需要这几种凭证才能成功，那么必须在用send()发送请求前设置XMLHttpRequest的withCredentials属性为true。

### JSONP

使用script元素进行Ajax传输的原因是它不受同源策略限制，并且包含JSON编码数据的响应体会自动解码执行。当使用script元素进行Ajax传输时，那就意味着Web页面可以执行远程服务器发送过来的任何JavaScript代码，所以对于不可信的服务器不应该采用这项技术。当然就算是自己的服务器也要小心攻击者进入并传入他的脚本代码。

这种使用script元素作为Ajax传输的技术称为JSONP。注意：当通过script元素调用数据时，响应内容必须用JavaScript函数名和圆括号包裹起来，而不是发送一段JSON数据。

#### 原理

利用script标签的src属性没有跨域限制。

```html
<script>
    function fn(a){
		console.log(a)
	}	
	document.body.appendChild(document.createElement('script').src = 'http://localhost:3000/jsonp?cb=fn')
</script>
```

JSONP的缺点就是只能发送get请求，并且fn函数必须是全局。

## 跨域

#### 后端允许跨域

后端可以通过设置来允许跨域。

你可以本地起一个服务跑html页面，这个页面请求3000端口的接口，此时，就会跨域。

这时你就可以通过设置头信息来允许跨域。

```js
const http = require('http')
const fs = require('fs')
const url = require('url')


http.createServer((req,res) => {  
  let { pathname,query } = url.parse(req.url,true)
  //允许所有源来请求，当然你也可以设置指定源
  res.setHeader('Access-Control-Allow-Origin', '*')
  //默认只支持GET，POST请求方法跨域，你可以扩充到下面五个请求
  res.setHeader('Access-Control-Allow-Methods','GET,POST,PUT,DELETE,OPTIONS')
  //默认不支持客户端写入请求头参数，可以设置后允许
  res.setHeader("Access-Control-Allow-Headers","content-type")
  
  if(pathname === '/clock'){
    if(req.methods === 'OPTIONS'){
      res.end()
    }
    res.end('hi,ugu')
  }

  

}).listen(3000,() => {
  console.log('服务开启')
})
```

```html
<script>
		const xhr = new XMLHttpRequest()
		xhr.open('PUT','http://localhost:3000/clock',true)
		xhr.onload = function(){
			console.log(xhr.response)
		}
		xhr.send()
	</script>
```
