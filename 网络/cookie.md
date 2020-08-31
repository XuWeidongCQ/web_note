## 1 cookie的作用

* 由于http的连接是无状态的，使用cookie可以进行状态管理

```
cookies的起源
       早期Web开发面临的最大问题之一是如何管理状态。简言之，服务器端没有办法知道两个请求是否来自于同一个浏览器。那时的办法是在请求的页面中插入一个token，并且在下一次请求中将这个token返回（至服务器）。这就需要在form中插入一个包含token的隐藏表单域，或着在URL的qurey字符串中传递该token。这两种办法都强调手工操作并且极易出错。


cookie是什么？
       坦白的说，一个cookie就是存储在用户主机浏览器中的一小段文本文件。Cookies是纯文本形式，它们不包含任何可执行代码。一个Web页面或服务器告之浏览器来将这些信息存储并且基于一系列规则在之后的每个请求中都将该信息返回至服务器。Web服务器之后可以利用这些信息来标识用户。多数需要登录的站点通常会在你的认证信息通过后来设置一个cookie，之后只要这个cookie存在并且合法，你就可以自由的浏览这个站点的所有部分。再次，cookie只是包含了数据，就其本身而言并不有害。
```



## 2 cookie的属性

![image-20200730150937771](../../../AppData/Roaming/Typora/typora-user-images/image-20200730150937771.png)

| 属性            | 作用                                                         | 值     | js权限   |
| --------------- | ------------------------------------------------------------ | ------ | -------- |
| name            | cookie的名称                                                 |        | 可以添加 |
| value           | cookie的值                                                   |        | 可以添加 |
| domain          | 可以访问此cookie的域，不能带端口号和协议                     |        | 可以添加 |
| path            | 可以访问此cookie的页面路径                                   |        | 可以添加 |
| expires/Max-Age | 为此cookie超时的时间(不设置默认是session,浏览器关闭后此cookie失效) |        | 可以添加 |
| size            | 此cookie的大小                                               | 数字   |          |
| HttpOnly++      | 若为true，则不能通过document.cookie来访问这条cookie          | 布尔值 |          |
| secure++        | 设置是否只能通过https才传递此条cookie(默认false)             | 布尔值 |          |
| priority        | 当cookie数量超出时，低优先级的cookie会被优先清除(Low/Medium/High) |        |          |
| SameSite        | strict()                                                     |        |          |

```
1.利用js添加cookie的时候，只要新添加的path domain字段和已存在的cookie重复，那么这条cookie会被覆盖
2.删除cookie只需要把要删除的cookie的expires设置为过去的时间即可(注意要找对path domain字段)



domain值
.abc.com(所有以abc.com结尾的域可以拿到这个cookie值)
a.abc.com(这个域是可以拿到abc.com域下面的cookie的,因为a.abc.com是abc.com的子域)


path值
表示哪些路径可以访问???(是否是对的)
abc.com/test(只有test路径下的页面才能访问,比如abc.com/test/a也能访问)
```





## 3 怎么才能写入cookie

* 通过设置HTTP的响应头Set-Cookie，一个Set-Cookie字段只能设置一条cookie，下图设置了5条cookie

![image-20200730160030025](../../../AppData/Roaming/Typora/typora-user-images/image-20200730160030025.png)

```
格式(不同的属性之间以分号隔开)
Set-Cookie:value [ ;expires=date][ ;domain=domain][ ;path=path][ ;secure]

比如
Set-Cooke:name=Greg; domain=nczonline.net; path=/blog

```

## 4 js读取cookie

* js读取cookie只能读取当前页面上所有cookie的name和value(以分号隔开),比如

![image-20200730161705594](../../../AppData/Roaming/Typora/typora-user-images/image-20200730161705594.png)

