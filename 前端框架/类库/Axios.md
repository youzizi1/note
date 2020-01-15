### 简介

axios是一个基于promise的http库，可以用于浏览器端和服务器端。

### 特点

* 支持Promise API
* 拦截请求和响应
* 转换请求数据和响应数据
* 取消请求
* 自动转换JSON数据
* 客户端支持防御XSRF

### axios实例

有的时候你可能需要创建不同的axios实例，例如每个接口的配置选项不同，此时你就需要生成有不同的配置选项的实例。

```js
const instance1 = axios.create({
  baseURL: 'httP://localhost:8080',
  timeout: 1000
})

const instance2 = axios.create({
  baseURL: 'httP://localhost:7001',
  timeout: 2000
})

instance1.get('/data.json')
instance2.get('/data.json')
```

### 配置

你可以在请求的时候配置axios。

```js
axios({
  baseURL:'http://localhost:3000', 	// 请求的域名
  timeout: 1000,        			// 超时时长
  url: '/data.json',   				// 请求路径
  method: 'get',      				// 请求方法
  headers: {
    token: ''
  },         						// 设置请求头
  params: {},   					// 请求参数拼接在url上
  data:{}     						// 请求参数放在请求体上
})
```

你还可以生成一个axios实例，然后对实例进行配置。这样通过该实例去发送请求都会携带这些参数。

```js
const instance = axios.create({
  baseURL: 'http://localhost:3000'
})
instance.defaults.timeout = 1000
```

当然，你也可以在全局进行配置，这样每次请求都会携带这些配置。

```js
axios.defaults.timeout = 1000
axios.defaults.baseURL = 'http://localhost:3000'
```

优先级依次是：请求配置>实例配置>全局配置，相同的配置会被优先级高的覆盖。

在实际开发中，全局配置很少使用，请求配置和实例配置用的比较多。

### 拦截器

```js
// 请求拦截器
axios.interceptors.request.use(
  config => {
    // 在发送请求前，做点什么
    $('#model').show()    	// 显示loading
    return config
  },err=>{
    // 统一错误处理，而不是每次都catch错误
    return Promise.reject(err)
  }
)

// 响应拦截器
axios.interceptors.response.use(res=>{
  // 在请求成功后，做点什么
  $('#model').hide()    	// 隐藏loading
  return res
},err=>{
  if (!window.navigator.onLine) {
      // 断网处理
      return 
  } 
  // 统一错误处理，而不是每次都catch错误
  return Promise.reject(err)
})
```

其实上面只是简单的封装，可以更进一步。

另外，你还可以取消拦截器。

```js
const interceptors = axios.interceptors.request.use(config=>{
    config.headers.token = ''
    return config
})
axios.interceptors.request.eject(interceptors)    // 取消拦截器
```

### 并发请求

如果你需要的数据在两个不同的接口里面，此时就需要使用axios的并发请求。

```js
axios.all(['/data1.json','/data2.json']).then(axios.spread(res1, res2)=>{
  console.log(res1, res2)
})
```

### 取消请求

在极端情况下切换Tab页面，很可能当前页面的请求还未发送完毕，你就切换了。此时你就需要取消那些未完成的请求，以提高性能。

```js
const source = axios.CancelToken.source()
axios.get('/data.json',{
  cancelToken: source.token
}).then(res=>{
    // 正常请求处理逻辑
}).catch(err=>{
  	// 取消请求会走到这里
  	console.log(err)    // 'cancel http'，是cancel的参数内容
})

// 取消请求
source.cancel('cancel http')    // message可选，可以为空
```

### 开发规范

在实际开发过程中，可以定义错误码`ERR_NO`来进行错误判断，具体值需要和后端商量。

