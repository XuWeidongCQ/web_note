# 一、HTML5简介

* h5改变了用户与文档的交互方式，增加了多媒体（video,audio,canvas）
* 增加了一些新特性
  * 语义特性
  * 本地存储特性
  * 网页多媒体
  * 二维三维
  * 特效

* 相比于H4，抛弃了一些不合理不常用的标签和属性
* 增加了一些标签和属性——主要在表单

# 二、语义化标签

nav header footer main article aside ——全是块级元素

在IE9及以上是有选择的支持，IE8及以下完全不支持H5

若要支持，可以引入第三方插件：html5shiv.min.js进行转换



# 三、表单

## 3.1 input的属性

* type
  * 之前：button checkbox file hidden password radio reset image text...
  * 新增：color **date** datetime datetime-local month week time **email number** range search tel url

* placeholder
* autofocus 打开页面自动聚焦
* autocomplete
* required 
* pattern 可写入正则验证
* multiple 选择多个文件 或输入多个邮箱地址
* form 指定该元素所属的表单ID
* list 可与datalist标签进行关联

## 3.2 新增datalist标签

* 该标签很多浏览器不支持，很少使用

## 3.3 新增的表单事件

* oninput 只要元素内容发生变化就会触发这个事件
* onkeyup 键盘弹起的时候触发 每一个键都会触发
* oninvalid 当表单元素验证不通过时候触发

# 四、多媒体标签

在没有h5之前，需要使用插件才能使用多媒体，如：flash

## 4.1 audio音频

* 该元素是行内元素

```html
<audio src='../aa.mp3' controls></audio>
```

* controls 属性用于控制是否显示控制面板，如果没有就不能显示面板
* autoplay 自动播放属性
* loop 循环

## 4.2 video视频

* 不同的浏览器所支持的视频格式不同

```html
<video src='./mp4.mp4' poster='./1.jpg'></video>
解决浏览器不支持视频格式的问题
<video>
  <source src='./1.flv' type='video/flv'> 
  <source src='./2.mp4' type='video/mp4'> 
</video>
```

* 该元素是行内元素
* width 宽高只用设置一个，剩下的自动等比缩放
* height
* poster 默认显示视频画面的第一帧
* controls 属性用于控制是否显示控制面板，如果没有就不能显示面板
* autoplay 自动播放属性

## 4.3 自定义播放器控制面板

* 在不同的浏览器中，多媒体标签的样式显示不一样
* w3c中有详细的配置方法

4.3.1 

# 五、H5新增的获取元素的方法

```html
<ul>
  <li>123</li>
  <li>123</li>
  <li>123</li>
</ul>
```

* document.querySelector('') 里面放入css选择器
* document.querySelectorAll('')

# 六、自定义属性

* 以data-开头

```html
<p data-school-name='cast'></p>
<script>
  let p = document.querySelector('p')
  let value = p.dataset['schoolName']
  console.log(value)
</script>
```

# 七、新增的接口

大部分用于移动端

## 7.1 网络

* online事件 网络连通的时候触发这个事件
* offline事件 网络断开时候触发

## 7.2 全屏接口

不同浏览器需要添加不同的前缀

chrome:webkit    firefox:moz   ie:ms   opera:o

* requestFullScreen() 开启全屏显示
* cancelFullScreen() 关闭全屏显示
* fullScreenElement 判断元素是否全屏显示

```html
<div>
<input type='button' id='full' value='全屏'>
</div>
<scrpit>
  let div = document.getElementByTagName('div')
  document.querySelector('#full').onclick = function(){
  		div.webkitRequestFullScreen() //谷歌浏览器
  }
</scrpit>
```

## 7.3 FileReader

* 可以实现文件即时预览

```html
<form>
  文件：<input type='file' name='myFile' id='myFile' onchange='getFileContent()'><br>
  <img id='myImg' src=''>
  <input type='submit'>
</form>

<script>
	function getFileContent(){
    var reader = new FileReader()//创建文件读取对象
    var file = document.querySelector('#myFile').files[0]//拿到文件
    //readAsText() 读取为txt格式的文件，默认编码为UTF8
    //readAsBinaryString() 这个函数可以将文件读取为二进制格式，方便存储
    //readAsDataURL() 主要使用这个函数实现文件预览
    reader.readAsDataURL(file) //读取上传的文件，这个函数没有返回值，但会将结果存放在reader的result属性中
    /*onabort 读取文件中断时触发
    *onerror 读取文件出错时触发
    *onload 读取完成时触发
    *onloadend 读取完成触发，无论成功还是失败
    *onloadstart 开始读取时候触发
    *onprogress 读取文件过程中持续触发
    */
    reader.onload = function(){
      console.log(reader.result)
      //将文件放到img标签中实现实时预览
      document.querySelector('#myImg').src = reader.result
      
    }
  }
</script>
```

## 7.4 拖拽接口

```html
<div class='div1' id='div1'>
  在h5中,需要把要要拖拽的元素设置draggable属性，图片和超链接默认可以拖拽
  <p id='pe' draggable='true'>试着把我拖过去</p>
</div>
<div class='div1' id='div1'>
  
</div>
<script>
  //拖拽事件
  //应用于被拖拽的元素的事件
  //ondragstart 当拖拽开始时调用(点击鼠标的时候)
  //ondrag 整个拖拽过程都会调用
  //ondragleave 当鼠标离开拖拽元素时调用 
  //ondragend 当拖拽结束时调用(松开鼠标的时候)
  var p = document.querySelector('#pe')//这是要拖拽的元素
  p.ondragstart = function(){}
  //应用于目标元素的事件
  //ondragenter 当拖拽元素的鼠标进入目标元素时调用
  //ondragover 当停留在目标元素时调用
  //ondrop 当在目标元素上松开鼠标时调用 浏览器会阻止这个事件，需要在ondragover中消除这个
  //ondragleave 当鼠标离开目标元素时调用
  var div2 = docunment.querySelector('#div2')//这是目标元素
  div2.ondragstart = function(){}
  div2.ondragover = function(e){
    e.preventDefault()
  }
  div2.ondrop = function(){
    div2.appendChild(p)
  }
</script>
```

## 7.6 地理定位接口

### 7.6.1 获取位置的方式

* IP地址 不精确
* GPS 精确 需要额外设备支持
* Wi-Fi
* 手机信号
* 用户自定义

定位信息属于隐私信息，浏览器中可以设置是否开启定位（浏览器会自动选择最佳的定位方式），手机应该要打开GPS

```html
<div id='demo' class='de'></div>
<script>
  var x=document.getElementById('demo')
  function getLocation(){
    if(navigator.geolocation){
      //第三个参数是当前获取地理位置的方式
      navigator.geolocation.getCurrentPositon(showPostion,showError,{})
    }else{
      x.innerHtml = 'Geolocation is not supported by this browser'
    }
  }
  //成功获取定位的回调
  function showPostion(position){
    //position中具有如下的值
    //position.coords.latitude 纬度
    //position.coords.longitude 经度
    //position.coords.accuracy 精度
    //postion.coords.altitude
  }
  //失败定位的回调
  function showError(error){
    //error.code为
    //PERMISSION_DENIED
    //POSTIION_UNAVAILABLE
    //TIMEOUT
    //UNKNOWN_ERROR
  }
</script>
```

### 7.6.1 第三方（以百度地图为例）

## 7.7 web存储

* 之前使用cookie存储（但是只有4k），现在可用其他的(可存储5M)

### 7.7.1  sessionStorage(关闭当前页面清空)

* 可以存储数据到本地，大小为5MB左右，本质是存放在**当前页面**的内存中（如果当前页面中存在iframe，而且iframe和父页面属于同源的，那么iframe可以共享sessionStorage中的数据）
* setItem(key,value)
* getItem(key)
* removeItem(key)
* clear()

### 7.7.2 localStorage(永久生效，存在硬盘中)

* 可以存储数据到本地，大小为20MB左右，必须手动清除
* 不同浏览器（比如一个是谷歌，一个是火狐）不能共享数据，但是同一个浏览器的不同窗口（这些窗口必须属于是相同的域名和端口）可以共享数据
* setItem(key,value)
* getItem(key)
* removeItem(key)
* clear()

## 7.8 应用程序缓存

* 以前的缓存，要么不缓存，要么缓存整个资源
* h5做了改进，可以选择要缓存的资源

### 7.8.1 启用缓存

* 在文档中增加一个manifest属性，指定应用程序缓存清单文件的路径 建议文件扩展名为appcache，本质是一个文本文件

```html
<html lang='en' manifest="demo.appcache">
  
</html>
```

* demo.appcache文件内容如下

```txt
CACHE MANIFEST #第一句必须写这个
#后面写注释

CACHE:#需要缓存的文件清单列表
../images/l1.jpg
../images/l2.jpg
* #代表所有文件缓存

NETWORK:#下面的文件每次都要从服务器请求，不要缓存
../images/l3.jpg

FALLBACK: #如果文件找不到，用空格后面的文件代替
../images/l4.jpg ../images/banner.jpg 
```

# 八、CSS选择器

## 8.1 新增属性选择器 E[attr]

```css
li[style]{}
li[class=red]{}
.red[style]{}
.red[href='www.xxx']
.red[href*='http']
```

## 8.2 新增伪类选择器

* 之前有a:hover a:active a:link 
* 

### 8.2.1 兄弟伪类

```css
.first + li{} 挨着.first的li元素
.first ~ li{} 选择li元素是.first的兄弟元素
```

### 8.2.2 相对于父元素的伪类

```css
n的默认范围是从0到子元素的个数

li:first-child{} 选择的li元素是它父元素的第一个孩子
li:last-child{}
li:nth-child(3){} 索引从1开始
li:nth-child(2n){}

li:first-of-type{} 限制类型的选择
li:last-of-type{}
li:nth-of-type{}
```

### 8.2.3 target锚链接

* 可以为锚点添加样式

```css
h2:target { //当h2为锚链接的目标时候，会改变颜色
  color:red
}
```

### 8.2.4 伪元素

```css
E::before{}
E::after{}
```



# 九、CSS3

## 9.1 简介

* 移动端支持优于PC端
* 支持浏览器前缀

## 9.2 渐变

### 9.3.1 线性渐变

* 关键在于设置起点和终点（或者说添加不同的关键点）
* 可以用来设置同一个背景中不同的色块

```css
background-image:linear-gradient(方向，开始颜色 位置1，颜色2 位置2，...)
background-image:linear-gradient(to right,red 0%,blue 100%)
```

### 9.3.2 径向渐变

````css
语法：(circle|ellipse,at position,)
background-image:radial-gradient()
````

## 9.3 过渡 transition

* 过渡结束以后，默认会还原到原来的状态
* 可以这样来理解过渡，被设置了过渡的元素，它的指定的css属性会被transition监听，一旦其发生变化，就产生过渡行为

```css
div{
  width:200px;
  height:200px;
  background-color:red;
  position:absolute;
  left:100px;
  top:100px;
  transiton-property:left; //对div的left属性进行监听
  transition-duration:2s;
}
:active为鼠标点击到松开的一段时间
div:active{
  left:1000px //此时div的left属性和原来相比发生了变化
}
```

### 9.3.1 案例

* 轮播图

## 9.4 2D变形 transform

* 能够对元素进行移动、缩放、旋转、斜切
* 元素进行变形，其变形前的位置不会脱离文档流，除非这个过渡元素使用了定位
* 平移默认参考点为元素的左上角 旋转默认参考点为元素中心 缩放也是元素中心
* translate(x,y)
* rotate(deg)
* scale(x,y)
* 同时添加多个变形,如下(以空格隔开)

```css
transform:translateX(400px) rotate(90deg)
```

* * 但是书写的先后顺序有影响

## 9.5 3D变形

* 在z轴上要能看见效果，需要在其父元素上设置perspective属性才能看见
* Translate3d(x,y,z)
* rotate3d(x,y,z,angle) -- x,y,z为旋转轴的(方向)向量
* scale3d(x,y,z)

## 9.6 动画 animation

* 在动画的关键帧中 一般搭配“变形”相关的属性
* 动画完成后默认会还原到原来的状态（不是0%时候的帧,是元素本来的效果）

```css
animation-iteration-count:infinite 一直重复播放动画
animation-direction:alternate | normal指定一个动画完成后，怎么回到原来的状态，默认直接回到起始状态

用来设置动画完成后，要不要回到原来起始状态
animation-fill-mode:forwards | backwards | 
forwards -- 不回到原始状态，保持最终的状态（100%的时候的帧）
backwards -- 保持动画开始的状态（0%的时候的帧）

animation-play-state:paused | running 控制动画是否暂停
```

### 9.6.1 案例

* 无缝滚动

## 9.7 弹性布局

* 弹性伸缩项如果没有设置宽度，默认为内容的宽度
* 弹性伸缩项一般**设置flex值**，来控制占据的比例
* 当子元素的宽度和大于容纳块的宽度时候，子元素会进行收缩，子元素设置了宽度也会收缩
* 为了保持不变形，**弹性容器要设置一个最小宽度**，不然会一直压缩

```css
display:flex 将一个容纳块声明为弹性容器，里面的所有元素变成弹性伸缩项
flex-wrap:nowrap | wrap | wrap-reverse   如果容纳块宽度不够,是否换行还是收缩
flex-direction: 设置主轴的方向
flex-grow:integer 弹性容器有宽度剩余时候，子元素怎么进行扩展
flex-shrink:integer 弹性容器宽度不够时候，子元素怎么进行收缩
flex: flex-grow flex-shrink flex-basis 如果弹性容器有宽度剩余或者宽度不够的时候，弹性元素怎么进行放大还是怎么收缩


justify-content:设置主轴对齐方式
align-items:设置侧轴的对齐方式
align-self:在单独的弹性元素上设置，以覆盖align-items的值
align-content:在弹性容器有多行的时候可以设置这些行在侧轴上的对齐方式
```

