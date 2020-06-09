# 一、简介

* WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行**全双工**通讯的协议。

* 浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

* 为了建立一个 WebSocket 连接，客户端浏览器首先要向服务器发起一个 HTTP 请求，这个请求和通常的 HTTP 请求不同，包含了一些附加头信息，其中附加头信息"Upgrade: WebSocket"表明这是一个申请协议升级的 HTTP 请求，服务器端解析这些附加的头信息然后产生应答信息返回给客户端，客户端和服务器端的 WebSocket 连接就建立起来了，双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续存在直到客户端或者服务器端的某一方主动的关闭连接。

  ```js
  //创建方式
  let socket = new WebSocket(url,[protocol])
  ```

# 二、属性

## readyState

```js
socket.readyState 
// 0 - 表示连接尚未建立
// 1 - 表示连接已建立，可以进行通信
// 2 - 表示连接正在进行关闭
// 3 - 表示连接已经关闭或者连接不能打开
```

# 三、事件

## socket.onopen

* 连接建立成功后触发

## socket.onmessage ++

* 客户端收到服务器数据时候触发

  ```js
  socket.onmessage = function(event) {
    var data = event.data;
    // 处理数据
  };
  
  //这种方式可以指定多个回调函数
  socket.addEventListener("message", function(event) {
    var data = event.data;
    // 处理数据
  });
  ```

  

## socket.onerror

* 通信发生错误时候触发

## socket.onclose

* 连接关闭时候触发

# 四、方法

## socket.send()

* 向服务器发送数据

## socket.close()

* 关闭连接