# 一.CSS小知识

## 1.1 BFC和IFC

​		块级格式化上下文和内联格式化上下文，为CSS2.1规范中提出的一个概念，为页面中的一块独立的渲染区域，同一个格式化上下文内和不同格式化上下文之间的定位方式会发生变化。

### 1.1.1 BFC的形成

* 根元素形成一个BFC
* 脱离普通文档流的元素（浮动，固定定位，绝对定位）
* 非块级元素（display=inline-block,table,flex等）
* overflow不等于visible的块级元素

### 1.1.2 BFC的影响

* 同一个BFC内的"相邻"块级元素（相邻的兄弟，父子）的==垂直外边距==会发生折叠，两者的边距取决于双方边距的最大值而不是两者之和
* BFC是页面上隔离的容器，内部元素不会与外部元素相互影响
* 在计算BFC的高度时，内部的浮动元素也会被计算在内

### 1.1.3 IFC的形成

* 多个内联（块级）元素排列在一起的时候就会形成一个IFC，且它们之间不能穿插块级元素，否则会被切割多个IFC

### 1.1.4 IFC的影响

* 一个IFC中的元素都是水平排列的
* 横向的margin,border,padding对于这些元素是有效的
* 垂直方向可以调整对齐方式



## 1.2 绝对定位的问题

* 绝对定位的元素一般包裹在relative元素中
* 如果默认不指定top,bottom,left,right，那么元素就在其原本该在的位置（脱离文档流，但不影响其前面的元素，只会影响后面元素的排列）
* 在绝对定位元素上使用外边距会有作用



## 1.3 文本过长显示省略号

## 1.4 CSS继承

* css继承的属性没有特指度，连零都没有

* ==可以被继承的属性==

  * 字体相关的属性
    * font 、font-family 、 font-size 、 font-style 、 font-variant、font-weight

  * 文本相关的属性
    * text-indent：文本缩进
    * text-align：文本水平对齐
    * text-shadow：文本阴影
    * line-height：行高
    * word-spacing：单词间的空白
    * letter-spacing：字符间的空白
    * text-transform：控制文本大小写
    * direction：规定文本的书写方向
    * color：文本颜色

  * 列表属性
    *  list-style、list-style-type、list-style-position、list-style-image 

  * 表格布局属性
    * border-collapse、empty-cells

* ==不能被继承的属性==
  * display
  * 与盒子模型相关的属性：宽度、高度、内外边距、边框
  * 背景相关属性
  * 定位属性
  * 轮廓样式属性

# 二.JS小知识

## 1.1 blur事件和click事件

* 点击其他元素会使得input触发blur事件
* 但是在点击其他元素的时候，事件执行顺序为mousedown > blur > mouseup > click

## 1.2 js计算时间间隔

* 一天有86400,000ms
* 一小时有3600,000ms
* 一分钟有60,000ms

```javascript
serviceLifeFilter:function (value) {
        const hours = Math.floor(value / 3600000);
        const minutes = Math.floor(value % 3600000 / 60000);
        return `${hours}h${minutes}min`
      }
```

## 1.3 sessionStorage和localStorage

* ==localStorage和sessionStorage只能存储字符串类型==，对于复杂的对象可以使用ECMAScript提供的JSON对象的stringify和parse来处理 
* sessionStorage可以保存同一个窗口的数据
* 关闭窗口或者标签页后会删除这些数据
* sessionStorage是一个对象

```javascript
window.sessionStorage

//1 保存数据语法
sessionStorage.setItem("key", "value");
//2 读取数据语法
var lastname = sessionStorage.getItem("key");
//3 删除指定键的数据语法
sessionStorage.removeItem("key");
//4 删除所有数据
sessionStorage.clear();
```

* localStorage保存的数据没有过期时间，除非手动删除
*  localStorage 属性是只读的

```javascript
window.localStorage

//1 保存数据
localStorage.setItem("key", "value");

//2 读取数据
var lastname = localStorage.getItem("key");

//3 删除数据
localStorage.removeItem("key");
```

![image-20200107200738764](../../AppData/Roaming/Typora/typora-user-images/image-20200107200738764.png)

