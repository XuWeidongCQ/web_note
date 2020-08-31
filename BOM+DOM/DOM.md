# 一 DOM节点概览

## 1.1 常用节点类型

* 文档节点（Object>Node>Document>HTMLDocument）
  	* 整个HTML文档就是一个文档节点
  	* nodeType = 9
* ==元素节点（Object>Node>Element>HTMLElement）==
  * 一个HTML元素，如`<title>节点类型</title>`
  * nodeType = 1
* 属性节点（Object>Node>Attr）
  * HTML元素上的属性，如id='div'
  * nodeType = 2
* 文本节点（Object>Node>CharacterData）
  * html元素中包裹的文本信息
  * nodeType = 3
* 注释节点



## 1.2 继承自Node对象的属性和方法

### 1.2.1 属性

* childNodes--得到所有子节点
* firstChild--得到第一个子节点
* lastChild--得到最后一个子节点
* nextSibing--得到下一个兄弟节点
* nodeName--得到节点名称，如'text'
* nodeType--得到节点类型，以数字的形式
* nodeValue--得到节点中的文本字符串，绝大多数都是null，除了文本节点和注释节点
* parentNode--得到父节点
* previousSibing--得到前一个兄弟节点

### 1.2.2 元素节点上的属性

* innerHTML
* outerHTML
* textContent
* innerText
* outerText
* firstElementChild--得到第一个子元素节点
* lastElementChild--得到最后一个子元素节点
* nextElementChild--得到下一个兄弟元素节点
* previousElementChild--得到前一个兄弟元素节点
* children--得到所有子元素节点

### 1.2.3 节点方法

* appendChild()
* cloneNode()
* compareDocumentPosition()
* contains()
* hasChildNodes()
* insertBefore()
* isEqualNode()
* removeChild()
* replaceChild()

### 1.2.4 文档节点上的方法

* document.createElement()
* document.createTextNode()



# 二 文档节点HTMLDocument--document

## 2.1属性

* doctype
* activeElement
* body
* head
* title
* URL
* cookie

## 2.2 方法

* createElement()
* createTextNode()
* 所有创建元素的方法
* getElementsByClassName()
* getElementsByName()--用于input元素
* getElementsByTagName()
* getElementById()
* querySelector()--匹配css选择器的第一个元素
* querySelectorAll()

# 三 [重要]元素节点HTMLElement--如div

## 3.1 属性

* tagName -- 得到元素的标签名，如A（大写）
* children -- 得到元素所有的子元素
* attributes --得到元素所有属性的集合
* className -- 得到该元素的类
* classList -- 得到元素上所有的样式类名（返回一个 DOMTokenList 对象 ，其包含的方法如下：）
  * add(class1,class2,...)
  * remove(class1.class2,...)
  * toggle(class,true|false)

* dataset -- 得到元素上所有已data-*起始的属性

## 3.2 方法

* getAttribute('href')--获取属性的值
* setAttribute('class','yes')--给元素添加一个属性和值
* hasAttrbute('href')--判断元素是否有某一个属性
* removeAttrbute('href')--移除元素的属性
* 以下这四个在文档节点上也有定义
* getElementsByTagName()
* getElementsByClassName()
* querySelector()
* querySelectorAll()



# 四 元素节点几何量与滚动几何量++

## 4.1 相对于父元素

* offsetTop -- 元素==上边框外沿==到父元素==上边框内沿==的距离
* offsetLeft -- 元素左边框外沿到父元素左边框内沿的距离
* offsetParent -- 参考CSS定位==不为static==的祖先元素

## 4.2 相对于视区++

* 视区是浏览器的窗口

* 利用elm.getBoundingClientRect()函数，返回一个对象，包括（不完全）：
  * top 元素上边到视区顶部的距离，可以为负数
  * right 元素右边到视区左边的距离
  * bottom 元素下边到视区顶部的距离
  * left 元素左边到视区左边的距离
  * witdth 元素的宽度 (等于left - right)
  * x （等于left）
  * y (等于top）
  
  ![image-20200720175905583](../../../AppData/Roaming/Typora/typora-user-images/image-20200720175905583.png)

## 4.3 元素在视区中的尺寸（边框+填充+内容）

* offsetHeight
* offsetWidth

## 4.4 获取元素在视区中的尺寸（填充+内容，不含边框,滚动条）

* clientWidth
* clientHeight

## 4.5 滚动元素的尺寸

* 滚动条加在哪个元素上的，哪个元素就是滚动元素
* scrollHeight
* scrollWidth
* scrollTop
* scrollLeft



# 五 css行内样式操作

* div.style -- 获得div上所有的行内样式对象（以下标记该对象为divStyle）
* divStyle.removeProperty('border') -- 移除某一个样式
* divStyle.cssText --  获得div上所有的行内样式(以字符串的形式)
* ==window.getComputedStyle(div).height== -- 获取元素上非行内样式的样式



# 六 文本节点

# 七 DOM中的JavaScript

## 7.1 说明

```html
这是外部脚本，可以跨域
<script src='http://xxxxx'></script>如果同时在里面写内联的脚本，会导致该脚本被忽略
这是内联脚本
<script>
    console.log('hi');
</script>
```



## 7.2 js的解析

### 7.2.1 默认行为

​		在默认情况下，当DOM在解析且它遇到第一个<script>元素时，将停止解析文档，阻止任何进一步的渲染和下载，并执行该javascript。直到该<script>元素中的代码被执行完毕，DOM才能进行下一步操作。

### 7.2.2 defer属性

​		使用该属性后，可以使得脚本在DOM在解析<html>节点后才进行脚本的加载与执行。

```html
<script src='http://xxxxx' defer></script>
```

### 7.2.3 async属性

​		使用该属性后，可以不堵塞DOM解析，同时进行外部脚本的加载，加载完就执行。如果同时存在defer属性，async属性会覆盖defer属性。

```html
<script src='http://xxxxx' async></script>
```

### 7.2.4 更普遍的用法

​		把所有的<script>元素写在<body>元素的闭合标签之前。



# 八 DOM事件

## 8.1 事件分类

DOM事件==捕捉阶段==：从DOM树主干到分支（向下）

DOM事件==冒泡阶段==：从DOM树分支到主干（向上）

```html
1.内联事件处理程序
<div onclick=""></div>
2.属性事件处理程序--DOM 0级事件
document.querySelector('div').onclick = function(){ }
3.addEventListener()--DOM 2级事件
```

## 8.2 事件流程

​		完整的事件执行流程为先捕捉阶段，后冒泡阶段。比如有一个文档结构：

​		window > document > html > body > div(事件目标)

​		如果在每个元素上都绑定了一个点击事件，那么我们点击div的时候，首先按照window > document > html > body > div(事件目标）触发--捕捉阶段，然后按照div>body>html>document>window触发--冒泡阶段。

​		addEventListener()函数的第三个参数为（false|true），false表示可以在捕获阶段触发，true表示可以在冒泡阶段触发，默认为false。

## 8.3 撤销浏览器默认事件

使用event.preventDefault()可以阻止HTML元素节点上的默认事件，比如点击a标签跳转到URL。

## 8.4 终止捕捉和冒泡

使用event.stopPropagation()可以在该元素上阻止事件传播流程，但是该元素上的事件会被执行。

使用event.stopImmediatePropagation()不但可以阻止时间的传播流程，还将终止该元素之后添加的事件。