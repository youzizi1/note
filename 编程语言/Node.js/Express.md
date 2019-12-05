## 初体验

```js
const express = require('express')
const app = express()

app.get('/', (req, res) => {
    res.end('hello world')
})

app.listen(3000,()=>{
    console.log('server is running')
})
```

其中Express的`listen`方法就是对原生HTTP模块上的listen方法的一次封装。

```js
const express = require('express')

const app = new express()

app.myListen = function(...args) {
    require('http').createServer(app).listen(...args)
}

app.myListen(3000,()=>{
    console.log('server is running')
})
```

## 路由

### 路由匹配

路由是从上到下进行匹配的，一旦匹配成功，就不会向下执行。

```js
const express = require('express')
const app = express()

// app上扩展了许多请求方法，例如get，post，put等
app.get("/users", (req, res) => {
  const { name } = req.query;		// 访问http://localhost:3000/users?name=ugu
  res.end(`welcome ${name}`);
});

// 匹配不成功
app.all('*', (req, res) => {
    res.end('Not Found')
})

app.listen(3000);
```

从上面代码中可以看出，Express扩展了req和res的属性。例如：查询参数可以直接从`req.query`中获取，而原生操作需要借助url模块。

`req`上具体扩展属性如下：

* query：获取查询参数
* path：获取路径
* headers：获取请求头
* url：获取请求url
* method：获取请求方法

`res`上具体扩展属性如下：

* json：直接返回JSON

  ```js
  const express = require('express')
  const app = express()
  
  app.listen(3000)
  
  app.get('/json', (req,res) => {
    res.json({name: '李四',age: 21})		// 自动判断类型头信息以及解决中文乱码
  })
  ```

* sendFile：直接返回文件

  ```js
  const express = require('express')
  const app = express()
  
  app.listen(3000)
  
  app.get('/file', (req,res) => {
    res.sendFile('./index.html',{root: __dirname})	// 自动判断类型头信息以及解决中文乱码
  })
  ```

  你不再需要先通过fs模块去读取文件，然后再返回文件。

* send：发送响应体

  ```js
  const express = require('express')
  const app = express()
  
  app.listen(3000)
  
  app.get('/', (req,res) => {
    res.send({name: '李四'})			// 自动判断类型头信息以及解决中文乱码
  })
  
  app.get('/404', (req, res) => {
      res.send(404)					// 还可以直接发送状态码
  })
  ```

  你不再需要先通过`res.setHeader`设置`Content-Type`以及`res.end`去返回响应体。

* sendStatus：返回状态码

  ```js
  const express = require('express')
  const app = express()
  
  app.listen(3000)
  
  app.get('/', (req,res) => {
    res.sendStatus(404)			// 自动判断类型头信息以及解决中文乱码
  })
  ```

  你不再需要先通过`res.statusCode`设置状态码以及`res.end()`去返回响应体。

* redirect：重定向

### 路由参数

```js
const express = require("express");
const app = express();

app.get("/users/:id", (req, res) => {
  const { id } = req.params;		// 访问http://localhost:3000/users/ugu
  res.end(`welcome ${id}`);			// welcome ugu
});

app.all("*", (req, res) => {
  res.end("Not Found");
});

app.listen(3000);
```

具体原理就是：

```js
const urlParam = "/user/:id/:name";
const url = "/user/1/ugu";

const reg = /:[^\/]+/g;
const arr = [];

const newReg = urlParam.replace(reg, function() {
  arr.push(arguments[0].slice(1));
  return "([^/]+)";
});

const reg1 = new RegExp(newReg);
const newArr = reg1.exec(url);
const result = {};

arr.forEach((item, index) => {
  result[item] = newArr[index + 1];
});
console.log(result); 			// {id:1,name:'ugu'}
```

另外，Express提供了param方法来分别处理参数，从而避免了参数处理的大量逻辑在get中处理。

```js
const express = require("express");
const app = express();

app.param("id", (req, res, next) => {
  if (req.params.id === "ugu") {
    next();
  } else {
    res.setHeader("Content-Type", "text/plain; charset=utf-8");
    res.end("参数错误");
  }
});

app.get("/users/:id", (req, res) => {
  const { id } = req.params;
  res.end(`welcome ${id}`);
});

app.all("*", (req, res) => {
  res.end("Not Found");
});

app.listen(3000);
```

### 重定向

```js
const express = require('express')
const app = express()
app.listen(3000)

app.get('/', (req, res) => {
  res.redirect('http://www.baidu.com')
})
```

封装的redirect方法等价于：

```js
res.setHeader('Location','http://www.baidu.com')
res.statusCode = 302
res.end()
```

### 路由拆分

具体看官方文档。

## 中间件

* 权限验证
* req和res属性扩充

中间件默认是应用在所有路由上的。

```js
const express = require("express");
const app = express();

app.use((req, res, next) => {
  res.setHeader("Content-Type", "text/plain; charset=utf-8");
  next()
});

app.get("/users", (req, res) => {
  res.end(`hello world`);
});

app.listen(3000);
```

当然，你也可以指定中间件作用的路由。

```js
const express = require("express");
const app = express();

app.use('/users', (req, res, next) => {
  res.setHeader("Content-Type", "text/plain; charset=utf-8");
  next()
});

app.get("/users", (req, res) => {
  res.end(`hello world`);
});

app.listen(3000);
```

另外，`next`方法中还可以传递参数，这个参数一般会被err中间件捕获。

```js
const express = require("express");
const app = express();

app.param("id", (req, res, next) => {
  if (req.params.id === "ugu") {
    next();
  } else {
    next('参数错误')
  }
});

app.get("/users/:id", (req, res) => {
  const { id } = req.params;
  res.end(`welcome ${id}`);
});

app.use((err, req, res, next) => {
    res.setHeader("Content-Type", "text/plain; charset=utf-8");
    res.end(err)		// 参数错误
})

app.listen(3000);
```

## 模板引擎

* ejs
* pug（原Jade）
* handlebars
* swig
* nunjucks

## 静态服务

本质上是依赖`serve-static`模块，安装express的时候会自动安装。具体使用看官网。

## bodyParser

如果数据是通过Post提交，此时你就需要解析请求过来的数据。

```js
const express = require("express");
const app = express();

function bodyParse(){
  return function(req,res,next){
    let str = ''

    req.on('data',chunk=>{
      str += chunk
    })

    req.on('end',()=>{
        //同一个req，所以直接赋予在req.content上
      req.content = require('querystring').parse(str)
        
      next()
    })
  }
}

let user = require('./user')
app.use(bodyParse())
app.use('/user',user)
```

上面自定义的`bodyParse`中间件可以实现对表单数据的解析，如果是通过对象形式提交过来，你需要对数据类型再次进行判断，然后再进行一些细节处理。

当然，我们也可以直接使用`bodyParser`模块，它帮我们做了这一切。它会将获取的数据解析后并且挂载在`req.body`上。

## 原理

```js
const http = require("http");
const url = require("url");

const createApp = () => {
  const app = (req, res) => {
    const m = req.method.toLowerCase();
    const { pathname } = url.parse(req.url, true);

    let index = 0;
    const next = () => {
      if (index === app.routes.length) {
        res.end(`Cannot ${m} ${pathname}`);
      }

      const { method, path, cb } = app.routes[index++];
      // 当method为middle时，表示处理中间件；否则表示处理路由
      if (method === "middle") {
        // 中间件作用的路由
        if (
          path === "/" ||
          pathname === path ||
          pathname.startsWith(`${path}/`)
        ) {
          cb(req, res, next);
        }else {
          next()
        }
      } else {
        if (m === method && pathname === path) {
          cb(req, res);
        } else {
          next();
        }
      }
    };
  };

  next();

  app.routes = [];

  // 获取methods
  http.METHODS.forEach(method => {
    method = method.toLowerCase();
    app[method] = (path, cb) => {
      const layer = {
        method,
        path,
        cb
      };
      app.routes.push(layer);
    };
  });

  app.use = (path, cb) => {
    if (typeof cb !== "function") {
      cb = path;
      path = "/";
    }

    const layer = {
      method: "middle",
      path,
      cb
    };

    app.routes.push(layer);
  };

  app.listen = (...args) => {
    const server = http.createServer(app);
    server.listen(...args);
  };

  return app;
};

module.exports = createApp;
```

未实现的部分：

* app.all()方法
* 中间件的next参数
* 扩展res方法





