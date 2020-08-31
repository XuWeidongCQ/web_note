 

# 参考:《HTML5权威指南》

# 一、H5全局属性

* 全局属性可以用到任何一个HTML标签上，如下：

| 属性           | 作用                        | 例子                                                         |
| -------------- | --------------------------- | ------------------------------------------------------------ |
| accesskey      | 设置访问元素的键盘快捷键    | \<a href="//www.runoob.com/css/css-tutorial.html" accesskey="c">CSS 教程\</a> |
| class          | 规定元素的类名              |                                                              |
| contentedtable | 规定是否可编辑元素的内容    | \<p contenteditable="true">这是一个段落。是可编辑的。尝试修改文本。\</p> |
| data-*         | 用于存储页面的自定义数据    | \<li onclick="showDetails(this)" id="owl" data-animal-type="bird">Owl\</li> |
| dragable       | 指定某个元素是否可以拖动    | \<p draggable="true">这是一段可移动的段落。请把该段落拖入上面的矩形。\</p> |
| dropzone       | 与dragable搭配,无浏览器支持 | \<div dropzone="copy">\</div>                                |
| hidden         | 规定对元素进行隐藏          | \<p hidden>这是一段隐藏的段落。\</p>                         |
| id             | 规定元素的唯一 id           |                                                              |
| lang           | 设置元素中内容的语言代码    | \<p lang="fr">这是一个段落。\</p>                            |
| spellcheck     | 检测元素是否拼写错误        | \<input type="text" name="fname" spellcheck="true">          |
| style          | 规定元素的行内样式          |                                                              |
| tabindex       | 设置元素的 Tab 键控制次序   | \<a href="//www.google.com/" tabindex="1">Google\</a>        |
| title          | 鼠标放上去会显示额外信息    | \<p title="教程">教程\</p>                                   |

# 二、标签概览

## 2.1 文档和元数据元素

| 元素     | 说明                                           | 局部属性                           |
| -------- | ---------------------------------------------- | ---------------------------------- |
| base     | 设置相对URL的基础                              | href target                        |
| body     | 表示HTML文档的内容                             |                                    |
| DOCTYPE  | 表示HTML文档的开始                             |                                    |
| head     | 包含文档的元数据                               |                                    |
| html     | 表示文档中HTML部分的开始                       |                                    |
| link     | 定义与外部资源的关系                           | href rel hreflang media type sizes |
| meta     | 提供关于文档的信息                             | name content chartset http-equiv   |
| noscript | 当浏览器禁用脚本或者不支持脚本的时候显示的内容 |                                    |
| script   | 定义脚本程序                                   | type src defer async charset       |
| style    | 定义CSS样式                                    | type media scoped                  |
| title    | 设置文档的标题                                 |                                    |

### 2.1.1 base元素  href target

```html
<head>
    <base href='http://titain/listing/'target='_top'/>
</head>
```

* 设置一个基准URL，让整个HTML文档的相对链接在此基础上进行解析
* 设定链接在用户点击的时候的打开方式
* 提交表单时候浏览器如何反应

### 2.1.2 meta元素 name content http-equiv charset

* 用来定义文档的各种元数据，一个HTML文档可以包含多个meta元素
* 每一个meta元素只能用于一种用途

```html
用法一:name和content搭配 内置的name如下:
1.application name 当前页所属web应用系统的名称
2.author 当前页的作者名
3.descript 当前页的说明
4.generator 用来生成HTML的软件名称（如ASP.NET）
5.keywords 一批以逗号分隔的字符串，用来描述当前文档 (可以让搜索引擎发现，但是现在其重要程度在下降)
<head>
    <meta name='author' content='xwd'/>
</head>

用法二:声明字符编码格式
告诉浏览器这个页面采用了utf-8的编码格式
<head>
    <meta charset='utf-8'/>
</head>

用法三:改写HTTP头部字段的值，取值如下:
1.refresh 以秒为单位，让浏览器重新载入页面 搭配content属性
2.default-style 指定页面优先使用的样式表
3.content-type 另外一种声明文档编码的方式 <meta http-equiv='content-type' content='text/html charset=UTF-8'/>

<head>
    表示每隔5s让浏览器再次载入页面
    <meta http-equiv='refresh' content='5'/>
</head>
```

### 2.1.3 style元素   type media scoped

```html
定义的样式类型
<style tyep='text/css'></style>

指定作用范围
<style scoped></style>

指定样式适用的媒体,可取如下:
1.all 所有设备
2.aural
3.braille
4.handled 手持设备
5.projection 投影机
6.print 打印预览
7.screen 计算机显示屏幕
8.tty
9.tv
<style media='screen'></style>
<style media='screen AND (max-width:500px)'></style>
```

### 2.1.4 link元素 href rel hreflang media type sizes

* 可用来在当前文档和外部资源间建立联系

```html
href属性 指向资源的url
<link href='http://xxx.xxx.xxx/style.css'/>

hreflang属性 指定关联资源所使用的语言
media属性 资源用于哪种设备

rel属性 说明当前文档与外部资源的关系，可取:
1.alternate 
2.author
3.help
4.icon 指定网站的图标资源
5.license 链接到当前文档的相关许可证
6.pingback
7.prefetch 预先获取一个资源
8.stylesheet 载入外部样式表
<link href='http://xxx.xxx.xxx/style.css' rel='stylesheet' type='text/css'/>
<link href='http://xxx.xxx.xxx/favico.ico' rel='icon' type='image/x-icon'/>

sizes属性 指定图标大小
type属性 指定外部资源的MIME类型
```

### 2.1.5 script元素 type src defer async charset

```html
type属性 表示脚本的类型 如果是javascript可以忽略

src属性 外部脚本的url  设置了该属性的script元素内部不能有任何内容

defer属性 定义脚本的执行方式 异步加载js脚本 等待dom解析完毕才能执行脚本

async属性 定义脚本的执行方式 异步加载js脚本 加载完就执行

charset属性 外部脚本的字符编码
```



## 2.2 文本元素

| 元素   | 说明                         | 局部属性                            |
| ------ | ---------------------------- | ----------------------------------- |
| a      | 生成超链接                   | href hreflang media rel target type |
| abbr   | 缩略语                       |                                     |
| b      | 不带强调的一段文字           |                                     |
| br     | 换行                         |                                     |
| cite   | 表示其他作品的标题           |                                     |
| code   | 表示计算机代码片段           |                                     |
| del    | 表示从文档中删除的文字       | cite datetime                       |
| dfn    | 表示术语定义                 |                                     |
| em     | 表示着重强调的一段文字       |                                     |
| i      | 与周边文字秉性不同的一段文字 |                                     |
| ins    | 表示加入文档的文字           |                                     |
| kbd    | 表示用户输入的内容           |                                     |
| mark   | 表示突出显示的内容           |                                     |
| small  | 表示小号字体                 |                                     |
| span   | 一个没有自己语义的通用元素   |                                     |
| strong | 表示重要内容                 |                                     |
| u      | 添加下划线                   |                                     |
| sub    | 表示下标文字                 |                                     |

### 2.2.1 a元素  href hreflang media rel target type

```html
href
指定资源的url
<a href='http://www.baidu.com'></a>
<a href='#fruit'></a> 定位到当前页面id为fruit的元素


hreflang
链接资源所使用的语言

media 同link元素
rel 同link元素
target 打开资源的浏览方式
<a href='http://www.baidu.com' target='_blank'></a>  在新窗口打开
<a href='http://www.baidu.com' target='_parent'></a>  在父框组(frameset)中打开
<a href='http://www.baidu.com' target='_self'></a>  在当前窗口中打开(默认)
<a href='http://www.baidu.com' target='_top'></a>  在顶层窗口打开中打开
<a href='http://www.baidu.com' target='<frame>'></a> 在指定的frame元素中打开

type MIME类型
```



## 2.3 用于分组的元素

| 元素       | 说明                              | 局部属性                |
| ---------- | --------------------------------- | ----------------------- |
| dl         | 表示一段说明列表                  |                         |
| dd         | 用于dl中，表示定义                |                         |
| dt         | 用于dl中，表示术语                |                         |
| div        | 一个没有自己语义的通用元素        |                         |
| figure     | 表示图片                          |                         |
| figcaption | 表示图片的标题                    |                         |
| hr         | 表示段落级别的换行 br只是文本换行 |                         |
| ul         | 无序列表                          |                         |
| ol         | 有序列表                          | start reversed type     |
| li         | 列表项                            | value(仅用于父元素为ol) |
| p          | 段落                              |                         |
| pre        | 表示其格式被保留的内容            |                         |

### 2.3.1 ol元素   start reversed type

```html
start属性
指定标号从几开始，默认为1

type属性
指定标号的类型,可取值:
1  1,2,3,4,...
a  a,b,c,d,...
A  A,B,C,D,...
i  i,ii,iii,iv,...
I  I,II,III,IV,...

reversed属性
表示是否倒序
```

## 2.4 用于划分内容的元素

| 元素    | 说明                         |
| ------- | ---------------------------- |
| address | 表示文档的联系信息           |
| article | 表示一段独立的内容           |
| aside   | 表示与周边内容稍有牵涉的内容 |
| details | 生产一个区域                 |
| footer  | 表示尾部                     |
| h1~h6   | 表示标题                     |
| header  | 表示首部                     |
| nav     | 表示导航                     |
| section | 表示一个重要的概念或主题     |

## 2.5 制表

| 元素     | 说明             | 局部属性                      |
| -------- | ---------------- | ----------------------------- |
| caption  | 表示表格标题     |                               |
| col      | 表示一列         |                               |
| colgroup | 表示一列组       |                               |
| table    | 表示表格         |                               |
| tbody    | 表示表格主体     | 可包含tr                      |
| td       | 表示单元格       | colspan rowspan headers       |
| tfoot    | 表示脚注         |                               |
| th       | 表示标题行单元格 | colspan rowspan headers scope |
| thead    | 表示标题行       | 可包含tr                      |
| tr       | 表示一行单元格   | 可包含 td th                  |

### 2.5.1 table元素

```html
这些属性在H5中都不推荐使用，用css设置,它们是:
summary
align 对齐方式
width 表格宽度
bgcolor 背景颜色
cellpadding 单元格内边距
cellspacing
frame
rules
```



## 2.6 表单

| 元素     | 说明                         | 局部属性                                                     |
| -------- | ---------------------------- | ------------------------------------------------------------ |
| button   | 提交按钮 重置按钮 一般按钮   | type                                                         |
| datalist | 定义一组提供给用户的建议值   |                                                              |
| fieldset | 表示一组表单元素             | name form disabled                                           |
| form     | 表示html元素                 | action method enctype name accept-charset target autocomplete novalidate |
| input    | 输入控件                     | 多达29个                                                     |
| keygen   | 生成一对公钥和私钥           |                                                              |
| label    | 表单的说明标签               | for form                                                     |
| legend   | 表示fieldset元素的说明性标签 |                                                              |
| optgroup | 表示一组相关的option元素     |                                                              |
| option   | 表示供用户选择的一个选项     |                                                              |
| output   | 表示计算结果                 |                                                              |
| select   | 给用户提供一组固定的选项     |                                                              |
| textarea | 多行文本输入框               |                                                              |

### 2.6.1 form元素

```html
1.action属性 说明用户在提交表单的时候把数据发送到说明地方
method属性 发送使用的HTTP方式 可选get和post
<form action='http://xxxx' method='post'>
    
</form>

2.enctype属性 指定浏览器发送给服务器的数据采用的编码方式,可取值:
application/x-www-form-urlencoded 默认，不能用来上传文件
multipart/form-data 该编码方式用于上传文件
text/plain 这种编码方式因浏览器而异
<form action='http://xxxx' method='post'>
    <input name='fave'/>
    <input name='name'/>
    <button>提交</button>
</form>
application/x-www-form-urlencoded
fave=Apples&name=xwd

multipart/form-data
--WebkitFormBoundary2hsdsjdksk
Content-Disposition:form-data;name='fave'
Apples
--WebkitFormBoundary2hsdsjdksk
Content-Disposition:form-data;name='fave'
xwd

text/plain(以chorme为例)
fave=Apples
name=xwd

3.autocomplete属性
<form autocomplete='on'>   </form> 允许自动填充表单(默认)
<form autocomplete='off'>   </form> 不允许

4.target属性 点击表单提交按钮后，浏览器的反馈信息在什么位置显示 同a标签

5.name属性 为表单设置一个独一无二的标识符，以便在DOM中区分各个表单
```

### 2.6.2 fieldset元素

```html
<fieldset>
    <lenged>一组输入表单</lenged>
    <input type='text' name='username'>
    <input type='text' name='password'>
</fieldset>
```

### 2.6.3 button元素 submit reset button

```html
1.submit属性 让按钮的作用为提交表单
2.reset属性 重置表单
3.button属性 没有语义
```

### 2.6.4 input [type=text password number range email url tel]

| type取值                       | 可取的属性                                       |
| ------------------------------ | ------------------------------------------------ |
| text 普通单行文本框            | dirname 指定内容的文字方向 [只能用于text类型的]  |
| password 密码框                | maxlength 最大字符数                             |
| number 只能输入数字            | pattern 用于验证的正则表达式                     |
| range 一个可以拉动的数字条     | placeholder                                      |
| email 符合邮箱地址的文本输入框 | readonly 只读                                    |
| url 符合url的文本输入框        | required 必须                                    |
| tel 符合电话号码的文本输入框   | value 初始值                                     |
|                                | list 指定文本框的datalist值 [只能用于text类型的] |
|                                | name 用来标识这个input元素                       |
|                                | size 一个数字 让浏览器确保达到该size能够被显示   |
|                                | min 最小值 [只能用于number类型]                  |
|                                | max 最大值 [只能用于number类型]                  |
|                                | step 数值的步长 [只能用于number类型]             |

```html
<input list='fruitlist' id='mylovefruit' name='mylovefruit'/>
<datalist>
	<option value='Apple'>xxxx</option> value值为真正的值 标签之间的为显示的值
    <option value='Oranges'></option>
    <option value='Cherries'></option>
</datalist>
```

### 2.6.5 input [type=reset submit button]

* 同button元素

### 2.6.6 input [type=checkbox] 布尔型

* 该元素只会被渲染为一个小方块

| type属性            | 可取的属性                           |
| ------------------- | ------------------------------------ |
| checkbox 布尔选择框 | checked 是否勾选                     |
|                     | required 必须勾选，否则无法通过验证  |
|                     | value 选中的时候，提交给服务器的数据 |
|                     | name 用来标识这个input元素           |

```html
记住密码:<input type='checkbox' name='rememberPassword'/>
```

### 2.6.7 input [type=radio] 单选按钮

* 只有name一样才会被关联到一起
* value设置的值会被渲染出来

| type属性   | 可取的属性                           |
| ---------- | ------------------------------------ |
| radio 单选 | checked 是否勾选                     |
|            | required 必须勾选，否则无法通过验证  |
|            | value 选中的时候，提交给服务器的数据 |
|            | name 用来标识这个input元素           |

```html

<input type='radio' name='fruit' value='apple'/>
<input type='radio' name='fruit' value='orange'/>
<input type='radio' name='fruit' value='banana'/>
```

### 2.6.8 input [type=datetime datetime-local date month time week] 时间与日期

| type取值                                   | 可取的属性                                       |
| ------------------------------------------ | ------------------------------------------------ |
| datetime 如:2011-07-19T16:49:39.491Z       | readonly 只读                                    |
| datetime-local 如:2011-07-19T16:49:39.491Z | required 必须                                    |
| date 如:2011-07-19                         | value 初始值                                     |
| month 如:2011-07                           | list 指定文本框的datalist值 [只能用于text类型的] |
| time 如:16:49:39.491                       | name 用来标识这个input元素                       |
| week 如:2011-W30                           | size 一个数字 让浏览器确保达到该size能够被显示   |
|                                            | min 最小值                                       |
|                                            | max 最大值                                       |
|                                            | step 数值的步长                                  |

### 2.6.9 input [type=color]

* 选择颜色

### 2.6.10 input [type=hidden]

* 不会被显示的数据项

### 2.6.11 input [type=file]

* 选择文件，点击会弹出系统文件选择框

| type | 属性                                   |
| ---- | -------------------------------------- |
| file | accept 指定可以接受的MEME类型          |
|      | multiple 设置这个input可以上传多个文件 |
|      | required 必须                          |

### 2.6.12 select元素 下拉列表选择框

| 属性     | 作用                   |
| -------- | ---------------------- |
| name     | 唯一标识符             |
| disabled |                        |
| multiple | 可以让用户选择多个选项 |
| size     | 可以显示的option数目   |

* 其内部可以包含optgroup > option

```html

<select name='lovefruit'>
    <optgroup label='fruit'>
        <option value='Apple' selected>xxxx</option> value值为真正的值 标签之间的为显示的值 selected表示被选中
    	<option value='Oranges'></option>
    	<option value='Cherries'></option>
    </optgroup>
    <optgroup label='other'>
        <option></option>
    </optgroup>
</select>
```

### 2.6.13 textarea元素

| 属性     | 作用                    |
| -------- | ----------------------- |
| name     | 唯一标识符              |
| disabled |                         |
| rows     | 行数                    |
| cols     | 列数                    |
| wrap     | 换行方式 可取hard和soft |

```html
<textarea cols='20' rows='5' name='story'></textarea> 
```



## 2.7 嵌入内容

| 元素     | 说明                           | 局部属性                                      |
| -------- | ------------------------------ | --------------------------------------------- |
| area     | 用于客户端分区响应图的区域     |                                               |
| audio    | 音频资源                       |                                               |
| canvas   | 生成一个动态的图形画布         |                                               |
| embed    | 用插件在HTML文档中嵌入内容     | src type height width allowfullscreen         |
| iframe   | 在一个文档中嵌入另一个文档     | src srcdoc name width height sandbox seamless |
| img      | 嵌入图像                       | src alt height width usemap ismap             |
| map      | 定义客户端分区的响应图         |                                               |
| meter    | 范围选择                       | value min max low high optimum form           |
| object   |                                |                                               |
| param    |                                |                                               |
| progress | 进度条                         | value max form                                |
| source   | 表示媒体资源                   |                                               |
| svg      | 矢量图                         |                                               |
| track    | 表示媒体的附加轨迹（例如字幕） |                                               |
| video    | 视频资源                       |                                               |

### 2.7.1 img元素

```
1.src属性 指定图像的url
2.width height属性 让图像还没有加载的时候，预留空间
```

### 2.7.2 iframe元素 在当前文档嵌入另一个HTML文档

```html
<iframe name='myFrame' width='300' height='300' sandox src='xxxx'>
    
</iframe>

1.src属性 iframe一开始载入要显示的url
2.srcdoc属性 定义一张内嵌的HTML文档
3.sandbox属性 对iframe进行限制，可取如下值：
  无 脚本，表单，插件，指向其他浏览上下文的链接会被禁用
  allow-forms 启用表单
  allow-scripts 启用脚本
  allow-top-navigation 允许链接指向顶层的浏览器上下文，这样就能用另一个文档替换当前文档
  allow-same-origin 允许iframe里的内容被视为与文档其余部分拥有同一个来源位置
```

# 三、拖(drag)放(drop)++

## 3.1 属性

| 属性      | 取值                                                         | 作用                     |
| --------- | ------------------------------------------------------------ | ------------------------ |
| draggable | true false auto(默认,让浏览器决定元素是否可以拖动,图片超链接默认可以拖) | 一个HTML元素是否可以拖动 |

## 3.2 事件

| 事件名          | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| dragstart       | 元素刚开始被拖动时触发(只触发一次)                           |
| drag            | 元素在拖动过程中一直触发(触发多次)                           |
| dragend         | 结束拖动时候触发                                             |
|                 |                                                              |
| dropenter       | 拖动元素进入(鼠标进入)某一个元素内部触发(该元素上必须监听dragenter事件，使其成为释放区) |
| dragover        | 拖动元素只要在释放区中移动(鼠标在释放区中移动)就一直触发     |
| dragleave(必需) | 拖动失败触发，或者拖动元素离开释放区触发                     |
| drop(必需)      | 拖动成功触发(拖动成功后，拖动元素在原来位置被删除)           |

```html
<style>
    .wrapper1 {
        height: 300px;
        width: 300px;
        border: 1px solid black;
    }
    .wrapper2 {
        height: 300px;
        width: 300px;
        border: 1px solid red;
    }
    p {
        margin: 5px 0;
        background-color: pink;
    }
</style>
<body>
  <div class="wrapper1" id="drop2">
    <p>林俊杰</p>
    <p>周杰伦</p>
    <p draggable="true" id="drag">许嵩</p> //只有这个元素可以被拖动
  </div>
  <div class="wrapper2" id='drop1'>
    <p>《江南》</p>
    <p>《七里香》</p>
    <p>《大千世界》</p>
  </div>
</body>
<script>
  let dragElm = document.getElementById('drag')
  let dropContainer1 = document.getElementById('drop1')
  let dropContainer2 = document.getElementById('drop2')
  
  //让drop1可以接受拖放的元素
  dropContainer1.addEventListener('dragover',function(e){//这里的e代表drop1的事件对象
    e.preventDefault()//必须阻止默认事件，否则drop1不能正常接收拖动的元素
  })
  dropContainer1.addEventListener('drop',function(e){
    dropContainer1.appendChild(dragElm)
    console.log('drop')
  })
    
   //让drop2可以接受拖放的元素  
  dropContainer2.addEventListener('dragover',function(e){
    e.preventDefault()//必须阻止默认事件，否则drop1不能正常接收拖动的元素
  })
  dropContainer2.addEventListener('drop',function(e){
    dropContainer2.appendChild(dragElm)
    console.log('drop')
  })
</script>
```

### 3.3 拖动文件

```html
<style>
  .file-wrapper {
    width: 200px;
    height: 120px;
    border: 1px dashed black;
    border-radius: 5px;
    position: relative;
  }
  .text {
    position: absolute;
    top: 50%;
    left: 50%;
    font-size: 24px;
    color: #cccccc;
    transform: translate(-50%,-50%);
  }
  input {
    width: 100%;
    height: 100%;
    opacity: 0;
  }
  input:focus{
    outline: 0;
  }
</style>
<body>
  <div class="file-wrapper">
    <span class="text">拖动文件</span>
    <input type="file" name="" id="fileUpZone">//建立一个文本域，用来接收文件
  </div>
</body>
<script>
  let fileZone = document.getElementById('fileUpZone')
  fileZone.addEventListener('click',function(e){//阻止文件域默认的点击事件(打开系统文件选择框)
    e.preventDefault()
  })
  fileZone.addEventListener('change',function(e){
    console.log(e.target.files[0])//获取拖动过来的文件
  })
   //将文本域变为可以接受拖动元素的区域
  fileZone.addEventListener('dragover',function(e){
    e.preventDefault()
  })
  fileZone.addEventListener('drop',function(e){
    console.log('drop')
  })
</script>
```

