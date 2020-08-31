## 1.将当前日期转换为大写

```js
function transformDate(){
  let date = new Date()
  let dayCode = ['星期日','星期一','星期二','星期三','星期四','星期五','星期六']
  let ymd = `${date.getFullYear()}年${date.getMonth()+1}月${date.getDate()}日`
  let hms = `${date.getHours()}时${date.getMinutes()}分${date.getSeconds()}秒`
  let day = `${dayCode[date.getDay()]}`
  let ret = `${ymd}:${hms} ${day}`
  console.log(ret)
}
transformDate()
//结果 2020年7月16日:10时48分6秒 星期四
```

## 2.计算当前日期往前 往后多少天的日期

```js
function transformDate(offsetDay){
  let date = new Date()
  date.setDate(date.getDate() + offsetDay)
  let dayCode = ['星期日','星期一','星期二','星期三','星期四','星期五','星期六']
  let ymd = `${date.getFullYear()}年${date.getMonth()+1}月${date.getDate()}日`
  let hms = `${date.getHours()}时${date.getMinutes()}分${date.getSeconds()}秒`
  let day = `${dayCode[date.getDay()]}`
  let ret = `${ymd}:${hms} ${day}`
  console.log(ret)
}
transformDate(-20) //2020年6月26日:10时52分45秒 星期五
transformDate(10)  //2020年7月26日:10时54分4秒 星期日
```

## 3.手写一个淡入淡出的banner

* 利用CSS3动画

```jsx
  <style>
    @keyframes fadeIn {
      from {
        opacity: 0;
      }
      to {
        opacity: 1;
      }
    }
    @keyframes fadeOut {
      from {
        opacity: 1;
      }
      to {
        opacity: 0;
      }
    }
    #box{
      height: 100px;
      width: 100px;
      background-color: pink;
      opacity: 0;
    }
    .fadeIn {
      animation: fadeIn 1s linear forwards;
    }
    .fadeOut {
      animation: fadeOut 1s linear forwards;
    }
  </style>

<button id="fadeIn">淡入</button>
<button id="fadeOut">淡出</button>
<div id='box'></div>
<script>
  let fadeInBtn = document.getElementById('fadeIn')
  let fadeOutBtn = document.getElementById('fadeOut')
  let box = document.getElementById('box')
  fadeInBtn.addEventListener('click',function(){
    box.className = 'fadeIn'
  })
  fadeOutBtn.addEventListener('click',function(){
    box.className = 'fadeOut'
  })
</script>
```

## 4.手写一个轮播图[见vscode]

## 5.鼠标放上去显示二维码

```jsx
<style>
    #qr-wrapper{
        position: fixed; //设置了定位会脱离文档流，但是同时没有设置top right等偏移，就会保持在原来的位置
        width: 100px;
        height: 100px;
        background-color: pink;
        display: none;
    }
</style>

<body>
  <button id="btn">显示二维码</button>
  <div id="qr-wrapper">
    这是二维码
  </div>
  <div>这是其他的内容</div>
</body>

<script>
  let btn = document.getElementById('btn')
  let qr = document.getElementById('qr-wrapper')
  btn.addEventListener('mouseover',function(){
    qr.style.display = 'block'
  })
  btn.addEventListener('mouseout',function(){
    qr.style.display = 'none'
  })
</script>
```

## 6.上下拉刷新

* 使用鼠标的两个事件 mousedown mouseup

```html
<body>
  <div id='wrapper'>页面</div>
</body>
<script>
  let wrapper = document.getElementById('wrapper')
  let startPointY;
  let endPointY;
  wrapper.addEventListener('mousedown',function(e){
    startPointY = e.clientY
  })
  wrapper.addEventListener('mouseup',function(e){
    endPointY = e.clientY
    if(endPointY - startPointY > 100){
      console.log('下拉刷新')
    }
    if(endPointY - startPointY < -100){
      console.log('上拉刷新')
    }
  })
</script>
```

## 7.瀑布流

```html
<style>
    .container {
        display: flex;
        border:  1px solid red;
    }
    .col {
        flex: 1;
        padding: 0 5px;

    }
    .item {
        border:  1px solid #cccccc;
        background-color: pink;
        margin-bottom: 5px;
    }
</style>
<div class="container">
    <div class="col">
        <div class="item" style="height: 100px;"></div>
        <div class="item" style="height: 600px;"></div>
        <div class="item" style="height: 400px;"></div>
        <div class="item" style="height: 400px;"></div>
    </div>
    <div class="col">
        <div class="item" style="height: 400px;"></div>
        <div class="item" style="height: 300px;"></div>
        <div class="item" style="height: 400px;"></div>
    </div>
    <div class="col">
        <div class="item" style="height: 300px;"></div>
        <div class="item" style="height: 300px;"></div>
        <div class="item" style="height: 300px;"></div>
    </div>
</div>

```

## 8.CSS实现半圆，扇形

```html
<style>
    .sector-90 {
        height: 0;
        width: 0;
        border-width: 50px;
        border-style: solid;
        border-color: red transparent transparent transparent;
        border-radius: 50%;
    }
    .sector-180 {
        height: 0;
        width: 0;
        border-width: 50px;
        border-style: solid;
        border-color: red red transparent transparent;
        border-radius: 50%;
    }
</style>
<div class="sector-90"></div>
<div class="sector-180"></div>
```



## 9.实现图片的缩放

```html
<div class="img-wrapper">
  <img src="https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2534506313,1688529724&fm=26&gp=0.jpg" alt="">
</div>
<script>
  let img = document.images[0]
  img.addEventListener('mouseenter',function(){
    img.style.transform = 'scale(1.5,1.5)'
  })
  img.addEventListener('mouseleave',function(){
    img.style.transform = 'scale(1,1)'
  })
</script>
```

## 10.实现一个元素的任意拖动

```html
<style>
    .wrapper {
        position: fixed;
        width: 300px;
        height: 300px;
        top: 50%;
        left: 50%;
        background-color: pink;
        transform: translate(-50%,-50%) //注意这里不能使用变形函数 否则回导致出错
    }
</style>
<body>
  <div class="wrapper"></div>
</body>
<script>
  let elm = document.getElementsByClassName('wrapper')[0]
  let initLeft = 0
  let initTop = 0
  let initPointX = 0
  let initPointY = 0
  let drag = false //标志是否在拖动过程中
  elm.addEventListener('mousedown',function(e){
    initLeft = elm.getBoundingClientRect()['left'];
    initTop = elm.getBoundingClientRect()['top'];
    initPointX = e.clientX
    initPointY = e.clientY
    drag = true
  })
  elm.addEventListener('mousemove',function(e){
    if(drag){
      elm.style.top = initTop + e.clientY - initPointY + 'px'
      elm.style.left = initLeft + e.clientX - initPointX + 'px'
    }
  })
  elm.addEventListener('mouseup',function(e){
    drag = false //鼠标抬起拖动结束
  })
  elm.addEventListener('mouseleave',function(e){
    drag = false //鼠标移出元素 拖动结束
  })
</script>
```

