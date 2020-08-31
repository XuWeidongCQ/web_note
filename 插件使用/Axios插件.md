# 一、简单说明

## 1.1 ajax

* ajax是一个实现网页的局部数据刷新的技术，可以通过XHR、Fetch、WebSocket等原生API实现
* fetch是下一代ajax技术的实现
* 传统 Ajax 指的是 XMLHttpRequest（XHR）， 最早出现的发送后端请求技术，隶属于原生js中，核心使用XMLHttpRequest对象，多个请求之间如果有先后关系的话，就会出现**回调地狱**。

## 1.2 ajax技术的封装

* jQuery通过对xhr原生接口进行包装，实现ajax技术
* axios也是对xhr原生接口进行了包装，并在其中加入了promise技术

## 1.3 axios的优势

* 从浏览器中创建 XMLHttpRequest
* 支持 Promise API
* 客户端支持防止CSRF
* 提供了一些并发请求的接口（重要，方便了很多的操作）
* 可以在 node.js环境中创建 http 请求
* 拦截请求和响应
* 转换请求和响应数据
* 取消请求
* 自动转换JSON数据（换言之，默认的HTTP传输数据都是JSON格式的）

# 二、安装

```js
//引入
npm install axios --save
```

* axios本身是函数，但不是作为构造函数，也就是说不能使用new语法
* 可以向该函数中传入config配置对象，返回一个promise对象

```js
//在浏览器中打印
console.dir(axios)
//出现的结果
ƒ anonymous() //说明axios是一个函数，以下这些都是函数的属性
  Axios: ƒ r(e)
  Cancel: ƒ n(e)
  CancelToken: ƒ r(e)
  all: ƒ (e)
  create: ƒ (e)
  default: ƒ ()
  defaults: {transformRequest: Array(1), transformResponse: Array(1), timeout: 0, xsrfCookieName: "XSRF-TOKEN", adapter: ƒ, …}
  delete: ƒ ()
  get: ƒ ()
  getUri: ƒ ()
  head: ƒ ()
  interceptors: {request: r, response: r}
  isCancel: ƒ (e)
  options: ƒ ()
  patch: ƒ ()
  post: ƒ ()
  put: ƒ ()
  request: ƒ ()
  spread: ƒ (e)
  arguments: (...)
  caller: (...)
  length: 0
  name: ""//函数名字为空，显示匿名函数
  prototype:
      constructor: ƒ ()
      __proto__: Object
  __proto__: ƒ ()
  [[FunctionLocation]]: spread.js:25
  [[Scopes]]: Scopes[3]
```

# 三、axios的属性

#### 3.1 axios.Cancel()

#### 3.2 axios.CancelToken()

* 用于取消请求

#### 3.3 axios.all()

* 并发请求

#### 3.4 axios.create()**

* 创建axios实例

#### 3.5 axios.default()

#### 3.6 axios.defaults** --{}

* 配置实例的部分config默认设置

#### 3.7 axios.delete()

* 别名

#### 3.8 axios.get()

* 别名

#### 3.9 axios.getUrl()

#### 3.10 axios.head()

#### 3.11 axios.interceptors** --{}

* 设置请求和响应拦截器

#### 3.12 axios.isCancel()

* 判断请求是否已经取消

#### 3.13 axios.options()

* 别名

#### 3.14 axios.patch()

* 别名

#### 3.15 axios.post()

* 别名

#### 3.16 axios.put()

* 别名

#### 3.17 axios.request()

* 别名

#### 3.18 axios.spread()

* 并发请求



# 四、axios({...config}) --config对象

#### 4.1 url**

* 用于请求的服务器URL

#### 4.2 method**

* 使用的HTTP方法，默认为get

#### 4.3 baseUrl**

* 将自动添加到url值的前面，除非url是一个绝对地址

#### 4.4 transformRequest

#### 4.5 transformResponse

#### 4.6 headers**

* 为将要发送的HTTP请求自定义请求头

#### 4.7 params**

* 跟随URL一起发送的参数

#### 4.8 paramsSerializer

#### 4.9 data**

* 作为请求体被发送的数据
* 只适用于PUT、POST、PATCH方式

#### 4.10 timeout

* 请求超过设定的时间，会被中断

#### 4.11 withCredentials

#### 4.12 adapter

#### 4.13 auth

#### 4.14  responseType**

* 表示接收到服务器响应数据的格式，默认为json

#### 4.15  responseEncoding

#### 4.16  xsrfCookieName

#### 4.17 xsrfHeaderName

#### 4.19 maxContentLength

#### 3.19 maxContentLength

#### 4.20 validateStatus**

* 设置除非promise中reject的状态码，保持默认就好

#### 4.21 maxRedirects

#### 4.22 socketPath

#### 4.23 httpAgent

#### 4.24 httpsAgent

#### 4.25 proxy

#### 4.26 cancelToken

```js
{
   // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // default

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data, headers) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

   // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

   // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // default

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

 // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

   // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // default

  // `responseEncoding` indicates encoding to use for decoding responses
  // Note: Ignored for `responseType` of 'stream' or client-side requests
  responseEncoding: 'utf8', // default

   // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

   // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

   // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // default

  // `socketPath` defines a UNIX Socket to be used in node.js.
  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  // Only either `socketPath` or `proxy` can be specified.
  // If both are specified, `socketPath` is used.
  socketPath: null, // default

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

# 五、请求的别名

* 该部分与直接使用`axios(config)`发起请求没有本质区别，只不过更具有语义化
* 在使用别名方法时， `url`、`method`、`data` 这些属性都不必在原始的`config`对象中配置。

#### 5.1 axios.request(config)

#### 5.2 axios.get(url,config) **

#### 5.3 axios.delete(url,config]) **

#### 5.4 axios.head(url[, config])

#### 5.5 axios.options(url[, config])

#### 5.6 axios.post(url[, data[, config]]) **

#### 5.7 axios.put(url[, data[, config]]) **

#### 5.8 axios.patch(url[, data[, config]])

# 六、创建实例

* 使用axios的create()方法可以创建实例，实例与默认的没有本质区别

```js
const http = axios.create([config])
```

# 七、设置请求默认值

## 7.1 全局设置

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

## 7.2 自定义实例的时候设置

```js
// 一种写法
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// 另一种写法
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;

```

## 7.3 优先级

请求自己的config > 实例的默认设置 > 全局的默认设置

# 八、拦截器

* 在请求或响应被 `then` 或 `catch` 处理前拦截它们

## 8.1 创建拦截器

* 必须有返回值，否则请求被中断

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });
//config对象
{
    adapter: ƒ xhrAdapter(config)
    baseURL: "https://www.zhxf.yuhualab.com:8080"
    data: undefined
    headers:
    	{
            Accept: "application/json, text/plain, */*"
        }
    maxContentLength: -1
    method: "get"
    timeout: 0
    transformRequest: [ƒ]
    transformResponse: [ƒ]
    url: "/caracara/info/project"
    validateStatus: ƒ validateStatus(status)
    xsrfCookieName: "XSRF-TOKEN"
    xsrfHeaderName: "X-XSRF-TOKEN"
}


// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
//response
{
    config: {…} //就是请求的config
    data: {code: 200, msg: Array(0)}
    headers: {content-type: "application/json"}
    request: { //这里面就是xhr对象上的属性，原封不动的(可以看到axios对齐进行了封装)
        onabort: ƒ handleAbort()
        onerror: ƒ handleError()
        onload: null
        onloadend: null
        onloadstart: null
        onprogress: null
        onreadystatechange: ƒ handleLoad()
        ontimeout: ƒ handleTimeout()
        readyState: 4
        response: "{"code":200,"msg":[]}"
        responseText: "{"code":200,"msg":[]}"
        responseType: ""
        responseURL: "https://www.zhxf.yuhualab.com:8080/caracara/info/project"
        responseXML: null
        status: 200
        statusText: ""
        timeout: 0
        upload: XMLHttpRequestUpload {onloadstart: null, onprogress: null, onabort: null, onerror: null, onload: null, …}
        withCredentials: false
    }
    status: 200
    statusText: ""
}
```

## 8.2 移除拦截器

```js
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

# 九、取消请求

* 取消请求多用在通过用户点击从后台获取数据的情景，不然重复点击会不断触发请求(也可以使用"节流"技术)
* 使用axios的CancelToken工厂函数
* 只有配置了cancelToken的请求才能被中断

```js
const CancelToken = axios.CancelToken;//得到产生中断令牌的工厂函数
const source = CancelToken.source();//使用该工厂函数产生一个可以被取消的资源

axios.get('/user/12345', {
  cancelToken: source.token //将这个资源的令牌给某一个请求，标记标记这个请求可以被取消
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
     // 处理错误
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');
//调用资源的cancel函数来取消请求，传入的产生会被请求的catch捕获
```



```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Axios!',
    source: null
  },
  methods: {
    sendRequest() {
      this.source = this.axios.CancelToken.source(); // 这里初始化source对象
      this.axios.get(url, {
        cancelToken: this.source.token // 这里声明的cancelToken其实相当于是一个标记，
                                       // 当我们要取消请求的时候，可以通过这个找到该请求
      })
      .then(res => {
        // 你的逻辑
      })
      .catch(res => {
        // 如果调用了cancel方法，那么这里的res就是cancel传入的信息
        // 你的逻辑
      })
    },
    cancel() {
      this.source.cancel('这里你可以输出一些信息，可以在catch中拿到')
    }
  }
})
```



# #、响应格式 --传入then函数中的参数

#### 4.1 data**

* 服务器响应的数据

#### 4.2 status**

* 响应的HTTP状态码

#### 4.3 statusText

#### 4.4 headers**

* 响应头部信息

#### 4.5 config

* 请求的配置信息（等价于config对象）

#### 4.6 request

```js
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 服务器响应的头
  headers: {},

   // `config` 是为请求提供的配置信息
  config: {},
 // 'request'
  // `request` is the request that generated this response
  // It is the last ClientRequest instance in node.js (in redirects)
  // and an XMLHttpRequest instance the browser
  request: {}
}
```

# #、错误格式 --传入catch函数中的参数

```js
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // The request was made and the server responded with a status code
      // that falls out of the range of 2xx
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // The request was made but no response was received
      // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
      // http.ClientRequest in node.js
      console.log(error.request);
    } else {
      // Something happened in setting up the request that triggered an Error
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

