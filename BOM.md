# 一、 Window对象

全部内容查看https://www.runoob.com/jsref/obj-window.html

两个名词：

* 浏览器窗口：包含浏览器内容区和四周
* 浏览器内容区：显示东西的界面

## 1.1 属性

### history

* 对 History 对象的只读引用

### innerHeight

* 返回浏览器内容区的高度（不包含工具条和滚动条）

### innerWideh

* 返回浏览器内容区的宽度（不包含工具条和滚动条）

### localStorage ++

### outerHeight ++

* 返回浏览器窗口的高度

### outerWidth ++

* 返回浏览器窗口的宽度

### pageXOffset ++ 等价于 scrollX

* **设置**或返回浏览器内容区水平滚动条的位置

### pageYOffset ++ 等价于 scrollY

* **设置**或返回浏览器内容区垂直滚动条的位置

### screenLeft ++ screenX(火狐)

* 返回整个浏览器窗口相对于电脑屏幕左边缘的距离（CSS像素）

### screenTop ++ screenY(火狐)

* 返回整个浏览器窗口相对于电脑屏幕顶部边缘的距离（CSS像素）

### sessionStorage

* 当前窗口被关闭就会清空

## 1.2 方法

### alert()

### clearInterval()

### clearTimeout()

### close()

* 关闭浏览器窗口

### open()

* 打开一个新的浏览器窗口

### resizeBy(with,height)

### resizeTo(with,height)

### scrollBy(x,y)

* 相对于上一次水平和垂直滚动条的位置，这次变化多少

### scrollTo(x,y) ++

* 用来控制水平和垂直滚动条的位置

### setInterval()

### setTimeout()

# 二、Navigator对象

* 该对象上的信息可以被篡改
* 包含浏览器本身的一些基本信息

## 2.1 属性

### appName

* 返回浏览器的名称（如Netscape）

### cookieEnabled

* 浏览器是否启用cookie

### platform

* 返回浏览器的操作平台

### userAgent

*  返回由客户机发送服务器的user-agent 头部的值 

# 三、screen对象

## 3.1 属性

### availHeight

*  返回屏幕的高度（不包括Windows任务栏） 

### availWidth

### height

*  返回屏幕的总高度

### width

# 四、History

* 包含浏览器URL地址的变化信息，以栈的格式
* 如果不点击后退和前进按钮，当前URL地址始终为栈顶，且当前URL标记为0

## 4.1 属性

### length

* 返回栈中的网址数目

## 4.2 方法

### back()

* 加载history列表中的前一个URL

  ```js
  history.back(-1) //url变为当前页面的上一个
  ```

  

### forward()

* 加载history列表中的下一个URL

### go(3)

* 加载history列表中的某个具体的界面

### pushState() --h5新增

* 在history列表末尾追加一个URL
* 浏览器的地址栏会发生改变，但是页面不会更新

```js

```



### replaceState() --h5新增

* 替换当前的URL
* 浏览器的地址栏会发生改变，但是页面不会更新

# 五、Location对象

* 包含当前URL的信息

## 5.1 属性

### hash ++

* 返回URL的锚部分

### host

* 返回主机名和端口

### hostname

### href

* 返回完整的URL（不包括hash #后面的部分）

### pathname

* 返回路径名，去除主机名

### port

### protocol

### search

* 返回URL的查询部分

## 5.2 方法

### assign()

### reload()

* 重新载入当前文档

### replace()

* 可以输入新的地址



