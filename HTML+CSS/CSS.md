参考书:《CSS权威指南上、下》

# #、元素分类

```
按照布局分:
    块级元素
	div p nav table footer section article footer ul ol li
	
    行内元素
    span strong em a
    
    行内块元素
    input img button 
    
按照内容分:
   置换元素(表单元素和img)
   非置换元素
```



# 一、特指度和层叠

```
无论样式是内联式 嵌入式还是外部引入的，都应用这些规则

!important  无穷大
每个行类样式 1000
每个ID选择器   0100
每个类选择器 属性选择器 伪类选择器  0010
每个伪元素选择器 元素选择器   0001
通用选择符    0000
继承属性      无

相同的特指度，按照就近原则（哪一个样式离标签近就使用哪个,比如内联式比外部引入的样式距离近）
```

# 二、字体

```css
font-family:<family-name> <generic-family>

<family-name> 通用字体簇
serif 衬线字体
sans-serif 无衬线字体
monospace 等宽字体
cursive   草书字体
fantasy   奇幻字体

<generic-family>具体的字体
```

```
使用自定义的字体
@font-face {
	font-family:'xxx'
	src:url('xxx.otf'),
		url('xxx.woff')
}
这样就可以使用字体 'xxx'
```

## 2.1 字形font-style

```
normal(常规,默认)
italic(斜体)
oblique(倾斜体)
```

## 2.2 字体拉伸font-stretch

* 将文字上下拉伸，变高还是变矮

# 三、文本

```css
text-indent:3em 首行缩进
vertical-align   纵向文本对齐，只能用于行内元素和置换元素(图像和表单)
word-spacing 单词间距
letter-spacing 字符间距
text-decoration:none | underline | overline | line-through | blink 文本装饰
text-shadow: 文本阴影

white-space:normal | nowrap | pre | pre-wrap | pre-line 对文本中空格、换行符和制表符的处理方式
word-break:normal | break-all | keep-all 以何种方式换行
```

## 3.1 让一行文本多余显示...的方式

```css
span {
    white-space:nowrap;
    overflow:hidden;
    text-overflow: ellipsis   
}
```

# 四、常规流动下的布局方式++

* 常规流动：没有浮动 没有定位 没有弹性盒 没有栅格布局
* 以下的盒模型出现的width都是W3C标准盒模型，即content-box，width就是内容的宽

## 4.1 块级元素

### 4.1.1 横向格式化  

* 由七个属性决定

```
初始值为默认值
margin-left:auto
border-left:0
padding-left:0
width:auto
padding-right:0
border-right:0
margin-right:auto

约束:这七个属性的值的和要等于父元素的width
```

```
该初始值有三个auto值，会有以下几种情况:
1.三个都为auto  -->  左右外边距为0，width为父元素的宽度
2.只设置width的值 --> 左右外边距均分(居中)
3.只设置一个外边距的值 --> width占据剩下的宽度，另一个外边距为0
```

```
即使左右外边距设置为负数，这七个值的和加起来也要等于父元素的width(可能导致子元素的宽度比父元素还要宽)
```

```
百分数单位是相对父元素的宽度而言的
```

### 4.1.2 纵向格式化

* 由七个属性决定

```
初始值为默认值
margin-top:auto
border-top:0
padding-top:0
height:auto
padding-bottom:0
border-bottom:0
margin-bottom:auto

约束:这七个属性的值的和要等于父元素的height(只针对父元素没有指定高度的情况)
```

```
该初始值有三个auto值，会有以下几种情况:
1.三个都为auto  -->  上下外边距为0,高度等于内部元素的高度
2.只设置height的值 --> 上下外边距为0,高度等于设置的值
3.只设置一个外边距的值 --> 外边距为设置的值，另一个外边距为0,高度为内容的高度
```

```
高度height 百分数单位相对于父元素的高度来说
上下padding 上下margin百分数单位相对于父元素的宽度来说
```

```
相邻(兄弟之间，第一个孩子和父元素的顶部外边距，最后一个孩子和父元素的顶部外边距)的纵向外边距进行折叠，取较大的值
**但是如果父元素添加了边框(或者父元素形成BFC)，父子的外边距合并现象就会消失
```

### 4.1.3 画正方形

* 利用这些特点可以画出自适应的正方形

```
块级元素的百分比width    都是相对于父元素宽度
               padding
可以画出正方形(是随着父元素的宽度变化而变化)
```

```html
<div style="padding-top: 20%;width: 20%;background-color:pink;"></div>
```

## 4.2 行内元素

* 给行内元素设置width和height没有作用（它们由内部的元素或者文本决定）
* 给行内元素设置上下的padding生效(但是不会改变行内元素的高度，如果背景，会改变背景的大小)，设置左右的padding有用
* 给行内元素设置上下的margin不生效，设置设置左右的margin有用
* 给行内元素设置边框生效（但是不会挤开元素）

# 五、内边距 边框 轮廓和外边距

## 5.1 内边距

* 内边距不能取负数
* 如果是百分数，相对于父元素的宽度

## 5.2 边框（画三角形）

* 边框不能设置为负数，百分数
* 利用边框画三角形

```css
.triangle {
      width: 0;
      height: 0;
      border: 50px solid red;
      border-left-color: transparent;
      border-bottom-color: transparent;
      border-right-color: transparent;
}
<div class="triangle"></div>
```

* 使用图像作为边框

```
border-image-source: url('xxxx')  指定图像的地址 默认将图像显示在四个角上
border-image-slice:
border-image-width:
border-image-outset:
border-image-repeat:
```

## 5.3 轮廓

* 轮廓不占空间
* 无法为单独一边设置轮廓
* 表单通常在focus的时候显示轮廓

## 5.4 外边距

* 外边距的百分数值相对于父元素的宽度

# 六、颜色 背景和渐变

## 6.1 颜色

* 前景色指的是元素的文本和边框
* 边框如果不指定颜色默认使用内容的颜色
* color属性始终会被继承

## 6.2 背景颜色

* background-color不会被继承

```css
background-clip:
	border-box(背景颜色会包含边框,默认值)
	padding-box(背景颜色会包含内边距)
	content-box(背景颜色只包含内容部分)
	text(将背景裁剪到元素的文本线条)
```

##  6.3 背景图

* 背景图不会被继承

```css
background-image:
	url(xxx.png)
	linear-gradient
	...

background-position:给背景图片定位 (一个指向横向坐标 一个指向纵向坐标)
	left center 这个的意思是把背景的左边线中间位置与容器的左边线中间位置重合
	left top 这个的意思是把背景的左上顶点与容器的左上顶点重合
	...
			可以是使用关键字
			可以是使用长度值（相对于容器的左上角的偏移）
			可以使用百分数(相对于容器的宽高)
			可以是使用负数

background-origin:
	border-box(背景图包含到边框)
	padding-box(背景图包含到内边距)
	content-box(背景图包含到元素内容)

background-repeat:定义背景的重复方式(如果背景没有填满容器的话)
	repeat-x(横向重复)
	repeat-y
	repeat(横向和纵向重复)
	space(均匀平铺)

background-attachment:可以将背景图固定在视区中，不会随着浏览器滚动而消失
	scroll(默认，滚动会消失)
	fixed(背景图不随着文档滚动而滚动，浏览器窗口的变化背景图的位置会实时变化)
	local

background-size:设置背景图片的大小
	<length1> <length2> 设置具体的大小
	cover(按原比例缩放图片以适应整个容器，背景图会被裁剪)
	contain(按原比例缩放图片以适应整个容器，背景图不会被裁剪,但可能覆盖不完)
```

* 多个背景可以叠到一起(以逗号分隔)

```css
background-image:url(xxx1.gif),url(xxx2.gif)
background-position:top left,top center 分别设置图片的位置
```

### 6.3.1 背景图片自适应居中方式

```css
.box {
    background-image:url(xxx1.gif);
    backgroun-position:center center;
    background-size:cover
}
```

## 6.4 渐变

### 6.4.1 线性渐变(书上册p447)

* 关键在于设置起点和终点（或者说添加不同的关键点）
* 可以用来设置同一个背景中不同的色块

```css
background-image:linear-gradient(方向，颜色1 位置1，颜色2 位置2，...)
```

### 6.4.2 径向渐变(书上册p461)

```
background-image:radial-gradient()
```

## 6.5 盒子投影

```css
box-shadow:横向偏移(左右移动) 纵向偏移(上下移动) 模糊距离 展开距离(改变投影的尺寸) 颜色 内陷还是外凹
box-shadow:1em 1em 2px 2px rgba(0,0,0,0.5) outset
```

# 七、浮动 形状

## 7.1 浮动

* 任何元素都可以使用float属性
* 浮动元素的容纳块是最近的块级祖辈元素
* 浮动之后的元素直观上变成行内块元素盒模型
* 浮动元素设置负的外边距会让该元素从父元素中冒出来（如果父元素设置overflow:hidden,超出部分会被隐藏）
* 浮动元素不会盖住文字，会让文本对齐进行环绕(因为浮动的产生本身就是为了解决排版中文字环绕的问题)

### 7.1.1 规则（书下册p487）

1. 浮动元素左右外边界不能超过容纳块的内边距界
2. ...

## 7.2 清除浮动

```css
h3 {clear:left} 确保h3的左侧不会出现浮动元素(如果左侧有浮动元素,那么h3会排在这个浮动元素下面)
h3 {clear:right} 确保h3的右侧不会出现浮动元素(如果右侧有浮动元素,那么h3会排在这个浮动元素下面)
```

* 一般而言把所有的浮动元素放到同一个父盒子中，这样清除浮动可以使用伪元素

```css
.box::after {
    content:'';
    display:block;
    clear:both
}
```

* 如果浮动元素必须和其他元素放到同一个盒子中，清除浮动的方式为

```css
直接在不需要收到浮动元素影响的元素上添加clear:both
```

# 八、定位

## 8.1 定位类型

* static(默认)
* relative(元素相对于自己本来的位置偏移，元素的形状不会发生改变，本来位置所占据的空间不会消失)
* absolute(元素相对于第一个不为static的父元素偏移，脱离文档流)
* fixed(元素相对于视区偏移，脱离文档流)
* sticky

## 8.2 偏移属性（后面为默认值）

* top: auto
* right: auto
* bottom: auto
* left: auto

```
right left百分数单位是相对于参考元素的宽度
top bottom百分数单位是相对于参考元素的高度
偏移值可以取负数(如果容纳块设置了overflow:hidden,超出部分将会被隐藏)

min-width min-height
max-width max-height

overflow:visible(默认) | hidden | scroll(一直显示滚动条) | auto(在必要的时候出现滚动条)
```

## 8.3 绝对定位

* 绝对定位的参考元素是position值不为static的第一个元素

```
**在使用绝对定位的时候，如果不指定偏移，那么该元素将会在其原来的位置
**如果不设置绝对定位元素的宽高，那么其默认大小为包含内部内容的大小
```

## 8.4 固定定位

# 九、弹性盒布局++

* display: flex（块级弹性容器,，独占一行）
* display: inline-flex (行内块级弹性容器，不独占一行)

## 9.1 弹性容器

```css
flex-direction:声明弹性盒子的主轴
	row 主轴是从左往右(默认)
	column 主轴从上到下
	row-reverse
	column-reserve


**默认情况下弹性元素不会换行，也不会自己调整尺寸(除非设置了flex属性)
flex-wrap:控制弹性元素的换行行为
	nowrap 不换行(默认)
	wrap 换行
	wrap-reserve

flex-flow: flex-direction和flex-wrap的合成属性
	row nowrap
	...
```

## 9.2 布置弹性元素

* 注意：这部分的说明文字主轴和侧轴都是默认情况下的

```css
justify-content:指定弹性元素在主轴方向上怎么排列
	flex-start (左对齐,默认)
	flex-end (右对齐)
	center (左右居中对齐)
	space-between (两端对齐)
	space-around (保证每个弹性元素两边的留白一样,除了第一个和最后一个弹性元素)
	space-evenly (保证每个弹性元素两边的留白一样)

align-items: 指定弹性元素在侧轴上的对齐方式(对于每个弹性元素的高度不一样，效果很明显)
	flex-start (顶部对齐)
	flex-end (底部对齐)
	center (上下居中对齐)
	baseline (如果弹性元素中有文本,使弹性元素保持第一条基线对齐，书下册p604)
	stretch (将高度变成所有弹性元素中最高的那一个,默认)

align-self: 单独给弹性元素在【侧抽】上指定对齐方式
	auto(默认)
	flex-start
	flex-end
	center
	baseline
	stretch

align-content: 如果弹性元素【存在多行】，可以使用这个属性指定这些行在侧轴方向的对齐方式
	flex-start(默认)
	flex-end
	center
	space-between
	space-around
	space-evenly
	stretch
```

## 9.3 弹性元素

* 弹性容器内部的直接子元素(不管元素的类型是什么，但是孙子元素不行!!!)自动转为弹性元素
* 弹性元素设置float,clear无效
* 弹性元素设置绝对定位有效

## 9.4 适用于弹性元素的属性

* 如果不显式为弹性元素指定宽度，默认宽度为其内容高度宽度，默认高度为弹性容器的高度（因为默认align-items:stretch）

```css
flex-grow:如果弹性盒有多余空间,怎么增长
	<number> 没有单位(增长比例，负数无效)

flex-shrink:如果弹性盒没有有多余空间,怎么压缩
	<number> 没有单位(压缩比例，负数无效)

flex-basis:定义弹性元素初始宽度
	content 关键字,保证宽度为元素内容的宽度
	<length> 是百分数就相对于主轴计算(这里设置的优先级高于给弹性元素直接设置width)


flex: <flex-grow> <flex-shrink> <flex-basis> 以上三个的合成属性
	initial 只允许缩小  相当于设置0 1 auto
	auto    既可以缩小，也可以增大 相当于设置1 1 auto
	none    没有弹性 相当于设置0 0 auto
	<number> 自定义增长因子
	
```

# 十、表格

* 几个常用的属性

```css
border-collapse: 设置表格单元格之间的空隙
	collapse 合并
	sparate 分开(默认)
	inherit

border-spacing: 设置单元格之间分开的具体大小
	<length> <length> 左右间隔 上下间隔
```

# 十一、列表和生成的内容

* 去除列表前面的符号

```css
ul {
    list-style-type:none 
}
```

# 十二、变形 transform++

* 变形元素不会脱离文档流，除非使用了定位
* 可以同时添加多个变形，以空格隔开(书写的先后有影响)
* 谁要进行变形，就把变形操作设置在该元素上

## 12.1 2D变形

```css
transform: 让元素进行变形
	translateX(100px) 坐标原点为变形元素的左上顶点
	translateY(100px) 沿着Y轴正方向前进100px
	translateZ(100px) 
	translate(x,y)

	scaleX(1.5) 缩放，不变点为变形元素的中心点
	scaleY(1.5)
	scale(x,y)

	rotateX(45deg) 旋转，不变点为变形元素的中心点
	rotateY(45deg)
	rotateZ(45deg) 沿着Z轴旋转45度
	rotate3d(x,y,z)

	skewX(45deg) 倾斜，不变点为变形元素的中心点
	skewY(-20deg)

	perspective(200px) 视域函数 只有设置了这个函数的才能看见平移 旋转在Z轴方向上的变化

transform-orgin: 可以用来修改默认变形的原点
	left center
	......

backface-visibility:处理元素背面
	visible 可见,默认
	hidden 
```

# 十三、过渡transition++

## 13.1 CSS过渡

* 默认CSS的样式变化会默认完成，整个过程不超过16毫秒
* CSS过渡发生在元素的样式发生改变的时候(一般hover，添加class的时候)
* 只声明一个transition属性，应该放到起始状态中

## 13.2 定义过渡的属性

```css
transition-property: 指定要运用过渡的css属性名(只有那些可以连续变化的才行，比如颜色相关)
	none (默认)
	transform 
	color 只要文本颜色发生变化就会触发过渡效果
	...

transition-duration: 指定过渡的持续时间
	0s (默认)
	200ms

transition-timing-function: 指定过渡函数
	ease (默认)
	ease-in (先慢后快)
	ease-out (先快后慢)
	ease-in-out (中间快，两端慢)
	linear (匀速)
	cubic-bezier(x1,y1,x2,y2)

transition-delay:指定延迟多少才开始进行过渡
	50ms

transition: 上面属性的简写
	<transition-property> <transition-duration> <transition-timing-function> <transition-delay>
	color 200ms ease-in 0
```

* 元素过渡完成会触发"transitionend"事件，可以进行监听

```js
document.querySelector('div').addEventListener('transitionend',function(e){
    console.log(e.propertyName)
})
```

# 十四、动画animation

* 动画设置到需要使用的元素上
* 动画完成后默认会还原到原来的状态（不是0%时候的帧,是元素本来的效果）

## 14.1 设置一个动画

```css
@keyframes animationName {
    0% {
        opacity:0
    }
    25%,50% {
        opacity:0.5
    }
    100% {
        opacity:1
    }
}
```

## 14.2 把动画应用到元素上

```css
animation-name: 动画名称(多个以逗号隔开)

aniamtion-duration:0s动画持续时长

animation-iteration-count:指定动画的播放次数
	1 (默认)
	infinite

animation-direction: 定义动画按照什么方向播放动画的关键帧
	normal(动画每次迭代都是从0%帧到100%帧,默认)
	reverse(动画每次迭代都是从100%帧到0%帧)
	alternate(第一次迭代从0%帧到100%帧,第二次迭代从100%帧到0%帧,往复下去)
	alternate-reverse(第一次迭代从100%帧到0%帧,第二次迭代从0%帧到100%帧,往复下去)

animation-delay:0s 定义延迟多少时间后开始执行动画

animation-timing-function:动画时序(同transition-timing-function)

animation-play-state:控制动画暂停(可以通过鼠标hover修改这个值,暂停动画进行)
	running (默认)
	paused (暂停)

animation-fill-mode:定义动画结束后是否应用元素原来的样式
	none (动画不播放就没有效果,默认)
	backwards (动画开始立马运用0%帧的样式,即使有延迟)
	forwards (动画结束后,保持100%帧的样式,不回到原始的样式)
	both (backwards forwards都生效)

animation:写为一个属性
	<name> <duration> <timing-function> <delay> <iteration-count> <direction> <fill-mode> <play-state>
	moveIn 1s ease 0s 1 normal none running
animation: name duration timing-function delay iteration-count direction fill-mode;
```

## 14.3 利用动画实现无缝滚动

* 缺点:元素会重复两遍

```html
<style>
    ul,li {
        margin: 0;
        padding: 0;
    }
    li {
        list-style-type: none;
    }
    @keyframes move {
        from {
            transform: translateX(0px);
        }
        to {
            transform: translateX(-500px);
        }
    }
    .wrapper {
        width: 500px;
        height: 100px;
        overflow: hidden;
        margin: 0 auto;
    }
    .wrapper > ul {
        width: 200%;
        font-size: 0;
        animation: move 4s linear 0s infinite;
    }
    .wrapper > ul > li {
        height: 100px;
        width: 100px;
        display: inline-block;
        font-size: 16px;
    }
    .wrapper > ul:hover {
        animation-play-state: paused;
    }
</style>
<div class="wrapper">
    <ul>
      <li style="background-color: blue;">1</li>
      <li style="background-color: blueviolet;">2</li>
      <li style="background-color: brown;">3</li>
      <li style="background-color: burlywood;">4</li>
      <li style="background-color: chartreuse;">5</li>
      <li style="background-color: blue;">1</li>
      <li style="background-color: blueviolet;">2</li>
      <li style="background-color: brown;">3</li>
      <li style="background-color: burlywood;">4</li>
      <li style="background-color: chartreuse;">5</li>
    </ul>
  </div>
```

