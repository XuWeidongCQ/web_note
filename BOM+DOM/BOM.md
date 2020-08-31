# 一、 Window对象

全部内容查看https://www.runoob.com/jsref/obj-window.html

两个名词：

* 浏览器窗口：包含浏览器内容区和四周
* 浏览器内容区：显示东西的界面

## 1.1 属性

### history

* 对 History 对象的只读引用

### innerHeight ++

* 返回浏览器内容区的高度（不包含工具条和滚动条）

### innerWidth ++

* 返回浏览器内容区的宽度（不包含工具条和滚动条）

### localStorage 

### outerHeight

* 返回浏览器窗口的高度(包含工具条，F12窗口，滚动条)

### outerWidth

* 返回浏览器窗口的宽度(包含工具条，F12窗口，滚动条)

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
* 调用这些方法可以改变浏览器地址栏的值，但是页面不会刷新(注意，如果这个时候收到点击了刷新是会按照这个新地址发送请求的)

| 属性或者方法                  | 说明                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| length                        | 当前窗口中的历史url的数目                                    |
| state                         | 当前页面url中所附带的信息，当进入该页面触发popState事件的时候，可以通过e.state拿到 |
| back()                        | 加载当前url的前一个 URL(相当于浏览器的后退按钮) 相当于go(-1) |
| forward()                     | 加载当前url的后一个 URL(相当于浏览器的前进按钮) 相当于go(1)  |
| go()                          | 加载history列表中的某个具体页面                              |
| pushState(state,title,url)    | 在列表的末尾添加一个url记录 （state会作为popstate事件e中state，title所有的浏览器默认会忽略，urlt填入相对路径，必须是同一个域下的） |
| replaceState(state,title,url) | 修改浏览器地址栏当前显示的值                                 |

## 4.1 popState事件

* 当浏览器的地址栏发生变化的时候，会触发popState事件

# 五、Location对象（重点3个）

* 包含当前URL的信息
* 只要这里面有一个值被改变 页面就会重新加载 

| 属性或者方法 | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| hash++       | 设置或返回从井号 (#) 开始的 URL(结果包含#) 例如: #test1      |
| host         | 设置或返回主机名和当前 URL 的端口号 例如: space.bilibili.com:443 |
| hostname     | 设置或返回当前 URL 的主机名 例如: space.bilibili.com         |
| href++       | 设置或返回完整的 URL(包含#号之后的) 例如:https://space.bilibili.com/43536/#/ |
| port         | 设置或返回当前 URL 的端口号 例如：443                        |
| protocol     | 设置或返回当前 URL 的协议 例如：https                        |
| search++     | 设置或返回从问号 (?) 开始的 URL（包含?）例如：?source_id=profile_create&channel=1013 |
| assign(url)  | 加载一个新的文档(当前页面被替换，可以使用浏览器后退回到原来的页面) |
| reload()     | 重新加载当前文档(刷新)                                       |
| replace(url) | 用新的文档替换当前文档(当前页面被替换，不可以使用浏览器后退回到原来的页面) |



