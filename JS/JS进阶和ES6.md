# $$JS进阶

## 一、词法作用域

* js中函数的作用域由其声明的时候决定。而不是执行时候的位置

## 二、this

### 2.1 this的指向

* this的指向由函数的调用位置决定
* this是一个存在于函数作用域中的东西
* 箭头函数的this在书写的时候就确定了，不是在执行的时候确定的

### 2.2 绑定规则(手写call,apply,bind)

* 不带有任何修饰的函数调用，this在非严格模式下绑定window，严格模式绑定undefined

* 作为某对象的方法调用，this通常指向调用的对象

* 使用apply、call、bind 可以绑定this的指向

  ```js
  //手写实现call
  //call的使用方式为fn.call(obj,arg1,arg2)
  Function.prototype.myCall = function(context=window,...args){
    const innerFn = Symbol()//保证不会覆盖context中的innerFn属性
    context[innerFn] = this //这里的this指向需要执行的函数
    let result = context[innerFn](...args)
    delete context[innerFn]
    return result
  }
  ```

  ```js
  //手写实现apply
  //apply的使用方式fn.apply(obj,[arg1,arg2])
  Function.prototype.myApply = function(context=window,args=[]){
    if(context === null){
      context = window
    }
    const innerFn = Symbol()//保证不会覆盖context中的innerFn属性
    context[innerFn] = this //这里的this指向需要执行的函数
    let result = context[innerFn](...args)
    delete context[innerFn]
    return result
  }
  ```

  ```js
  //手写实现bind
  //bind的使用方式，返回一个指定this的函数
  Function.prototype.myBind = function(context=window,...args){
    let fn = this;
    return function(...newArgs){
      return fn.apply(context,[...args,...newArgs])
    }
  }
  let f = fn.bind(obj)
  f()
  ```

  ```js
  function myCall(ctx=window,...args){
    ctx['fn'] = this
    let res = ctx['fn'](...args)
    delete ctx['fn']
    return res
  }
  
  function myApply(ctx=window,args=[]){
    ctx['fn'] = this
    let res = ctx['fn'](...args)
    delete ctx['fn']
    return res
  }
  
  function myBind(ctx=window){
    let fn = this
    return function(...args){
      return fn.call(ctx,...args)
    }
  }
  ```

  

* 使用new操作符，绑定内部构造的新对象

* 箭头函数没有this，如果里面写了，会被绑定到申明这个箭头函数的外层函数的this

### 2.3 优先级

new > 显示绑定(使用apply等) > 作为对象的方法

箭头函数的this一旦被指定，无法修改

## 三、闭包

* 当一个函数能够可以记住并访问所在的词法作用域时候，就会形成闭包。常见的例子是一个函数内部嵌套一个函数，然后将内部的函数绑定到全局中，就会形成闭包。

* 作用

  ```
  1.形成模块，不污染全局变量，搭配立即执行函数对外进行暴露API
  ```

  

## 四、原型链与继承

* 继承的本质：将父类的属性复制到子类的实例中，将父类的原型对象上方法深复制到子类的原型对象中

### 4.1 js中创建对象的方式

* 通过对象字面量

  ```js
  let obj = {name:'xu'}
  //obj的原型指向Object.prototype
  ```

* 通过构造函数

  ```js
  let obj = new User('xu')
  //obj的原型指向user.prototype
  ```

* 通过Object.create()

  ```js
  let obj = Object.create(P)
  //obj的原型指向P,本质是将obj.__proto__指向P
  ```

### 4.2 instanceof原理

* 判断实例对象的\_\__proto\__属性与构造函数的prototype是不是用一个引用。如果不是，他会沿着对象的\__proto__向上查找的，直到顶端Object

### 4.3 构造函数继承

```js
function Parent(age){
  this.age = age
  this.score = [1,2,3]
}
Parent.prototype.showAge = function(){
  console.log(age)
}



function Child(age,sex){
  Parent.call(this,age) //调用父类的构造函数
  this.sex = sex
}

let child = new Child(24,1)
```

问题：

* Parent原型链上的属性和方法并不会被子类继承

### 4.4 原型链继承

```js
function Parent(age){
  this.age = age
  this.score = [1,2,3]
}
Parent.prototype.showAge = function(){
  console.log(age)
}

function Child(age,sex){
  this.sex = sex
  this.age = age
}

//关键
Child.prototype = new Parent()

Child.prototype.showAge = function(){}
Child.prototype.showSex = function(){}

let child = new Child()
```

问题：

* 如果父类中有些属性是引用类型的，那么子类修改这个属性会对所有的子类实例参数影响

### 4.5 组合继承(构造函数继承+原型链继承)

```js
function Parent(age){
  this.age = age
  this.score = [1,2,3]
}
Parent.prototype.showAge = function(){
  console.log(age)
}

function Child(age,sex){
  Parent.call(this,age)
  this.sex = sex
}

//关键 (这一步有问题)
Child.prototype = new Parent()

Child.prototype.showAge = function(){}
Child.prototype.showSex = function(){}

let child = new Child()
```

问题：

* 父类构造函数在子类构造函数中执行了一次，在子类绑定原型时又执行了一次

改进1:

```js
Child.prototype = Parent.prototype;
/*
问题1:constructor指向不对
因为原型上有一个属性为constructor，此时直接使用父类的prototype的话那么会导致 实例的constructor为Parent，即不能区分这个实例对象是Child的实例还是父类的实例对象。
*/
/*
问题2:在Child中添加原型方法会直接修改父类的原型方法
子类不可直接在prototype上添加属性和方法，因为会影响父类的原型
*/
```

改进2:

```js
Child.prototype = Object.create(Parent.prototype)
Child.prototype.constructor = Child; //修改指向
//上面这个方法的唯一缺点:会抛弃原来的Child.prototype(如果有的话)，将其指向一个新的

//ES6写法，这个不会出现问题
Object.setPrototypeOf(Child.prototype,Parent.protype)
```

注意:

* Object.create()是ES5中增加的，如果需要在之前版本中运行需要如下

  ```js
  //又叫圣杯模型
  const extend = function(parent,child){
    function F(){}
    F.prototype = parent.prototype
    child.prototype = new F()
    child.prototype.constructor = child
  }
  //使用
  extend(Parent,Child) //这条语句等价于Child.prototype = Object.create(Parent.prototype)
  ```

### 4.6 最终的继承写法

```js
//父类
function Parent(age){
  this.age = age
  this.score = [1,2,3]
}
Parent.prototype.showAge = function(){
  console.log(age)
}

//子类
function Child(age,sex){
  Parent.call(this,age) //调用父类构造函数
  this.sex = sex
}
//关键步骤
Child.prototype = Object.create(Parent.prototype)
//给子类添加原型方法
Child.prototype.showAge = function(){}
Child.prototype.showSex = function(){}

let child = new Child()
```

## 五、面向对象

### 5.1 prototype属性和\__proto__属性和constructor属性

* js中函数是对象实例（由构造函数Function()产生，只不过我们写的时候不用这种方式）
* ==每一个函数==都有一个prototype属性（它是一个对象）和\__proto__属性和constructor属性
* 在大多数浏览器中，==每一个对象都有\__proto__属性和constructor属性==，指向对应的构造函数的prototype属性

```js
let obj = {}
let func = function(){}

//针对对象{}的结论
obj.__proto__ === Object.prototype true
Object.prototype.__proto__ === null true
//Object.prototype是最终的继承点，包括一些公用的方法,如下:
hasOwnProperty: ƒ hasOwnProperty()
isPrototypeOf: ƒ isPrototypeOf()
propertyIsEnumerable: ƒ propertyIsEnumerable()
toLocaleString: ƒ toLocaleString()
toString: ƒ toString() //增强类型检查必须调用的方法
valueOf: ƒ valueOf()

//针对函数的结论
func.__proto__ === Function.prototype true
func.prototype.__proto === Object.prototype true //函数有prototype属性
Function.prototype.__proto__ === Object.prototype true
Function.prototype.__proto__ === Function.prototype false
//针对这一点，类型检查显示Function.prototype是一个函数,那么为什么其__proto__不指向Function.prototype???
//查阅资料这是一个特殊的函数,执行它总是返回undefined,是为了兼容ES5

//Function.prototype上有一些函数可以使用的方法或属性,如下:
apply: ƒ apply()
arguments: (...)//得到函数参数的类数组
bind: ƒ bind()
call: ƒ call()
caller: (...)
constructor: ƒ Function()
length: 0 //得到函数参数的个数
name: "" //得到函数名字，匿名函数无法获取名字
toString: ƒ toString()

//一些内置的构造函数都是由Function构造函数产生
Array.__proto__ === Function.prototype true
Object.__proto__ === Function.prototype true
Set.__proto__ === Function.prototype true
Function.__proto__ === Function.prototype true //甚至Function函数本身

//js中所有对象的根 Object.prototype
```



#### 5.1.1 \__proto__属性

* \__proto__属性是对象实例所独有的（函数也是对象，所有函数也有这个属性）
* \__proto__属性指向产生该对象实例的构造函数上的原型对象（即prototype）

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20191230173456484.png" alt="image-20191230173456484" style="zoom:80%;" />

#### 5.1.2 prototype属性

* prototype属性是函数独有的， 含义是函数的原型对象，里面包含所有实例共享的属性和方法
* ==所有函数都可以作为构造函数==
* 任何函数在创建的时候，其实会默认同时创建该函数的prototype对象

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20191230203148275.png" alt="image-20191230203148275" style="zoom:80%;" />

#### 5.1.3 constructor属性

*  constructor属性是对象才拥有的
*  指向该对象实例的构造函数
*  所有constructor属性的终点就是Function这个构造函数，而Function对象的constructor是其本身

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20191230205032180.png" alt="image-20191230205032180" style="zoom:80%;" />

### 5.2 new 发生了什么

```js
function Student(name,age){
    this.name = name;
    this.age = age;
}

//该函数的本质是 当new的时候
function Student(name,age){
    var this = {
        __proto__:Student.prototype,
    }
    this.name = name;
    this.age = age;
    return this;
}

//当使用new Student('xxx',23)时,构造函数中的this才会生效。

//添加方法
Student.prototype.方法1 = function(){}
Student.prototype.方法2 = function(){}
```

### 5.3 Object上的一些重要的静态方法

```js
assign: ƒ assign() //合并对象要使用的函数
create: ƒ create() //原型继承需要使用的函数
defineProperties: ƒ defineProperties()
defineProperty: ƒ defineProperty() //vue实现双向绑定的函数
entries: ƒ entries() //++
freeze: ƒ freeze()
fromEntries: ƒ fromEntries()
getOwnPropertyDescriptor: ƒ getOwnPropertyDescriptor()
getOwnPropertyDescriptors: ƒ getOwnPropertyDescriptors()
getOwnPropertyNames: ƒ getOwnPropertyNames()
getOwnPropertySymbols: ƒ getOwnPropertySymbols()
getPrototypeOf: ƒ getPrototypeOf()
is: ƒ is()
isExtensible: ƒ isExtensible()
isFrozen: ƒ isFrozen()
isSealed: ƒ isSealed()
keys: ƒ keys() //++
preventExtensions: ƒ preventExtensions()
seal: ƒ seal()
setPrototypeOf: ƒ setPrototypeOf()
values: ƒ values() //++
```

## 六、函数

### 6.1 给函数指定默认值

```js
function log(x,y='wd'){
    console.log(x,y)
}
//当某一个参数为undefined的时候，会使用默认值填充
```

### 6.2 剩余变量

* 剩余变量之后不能有其他的参数

```js
//会把传入到函数中的多余参数，形成一个数组[]，赋给args

function test(x,...args){//注意：这里的args是一个数组，
    
}
test(1,2,3,4)


```

## 七、JS中类型转换

### 7.1 相等条件

* 在使用过程中一定要使用===操作符，可以避免不必要的麻烦

![image-20200723152542507](../../../AppData/Roaming/Typora/typora-user-images/image-20200723152542507.png)

### 7.2 能够被转换为false的只有5个

```
undefined
null
0 +0 -0
''
NaN

!!undefind为false
!!null为false
!!0为false
!!''为false
!!NaN为false
```



# $$ES6



## 0、模块

### 0.1 使用

```js
//commonJS nodeJS
//导出
module.exports = {
    name:'calculator',
    add:function(a,b){
        return a + b
    }
}
//导入
const calculator = require('./calculator.js')
console.log(calculator.add(2,3))//执行

//es6
export default {
   name:'calculator',
    add:function(a,b){
        return a + b
    }
}
//对应导入写法
import '自定义变量名' from './calculator.js'


//导出写法
export const name = 'calculator';
export const add = function(a,b) { return a + b };
//等同于
const name = 'calculator';
const add = function(a,b) { return a + b };
export {name,add}

//对应导入写法
import {name,add} from './calculator.js'
```

### 0.2 区别

- es6模块中的顶层this指向undefined
- CommonJS模块输出是一个值的复制（就是复制出一个{}），es6模块输出值的引用(只读)
- CommonJS在导入模块的时候就会运行代码，es6只会在编译的时候提供一个接口

## 一、兼容性说明

将ES6转换为低版本的方式，以兼容更多版本的浏览器

高版本：IE10+

* 在线编译

* 提前编译（较好）--babel(browser.js)

  ```html
  <script src='browser.js'></script>
  <script type='text/babel'>
  let a = 9
  let b = 1
  </script>
  ```

#### 1.1 新的改变

* 变量
* 函数
* 数组
* 字符串
* 面向对象
* Promise
* yield/generator
* 模块化(import,export)

## 二、变量

### 2.1 var -- 缺陷

* 可以重复声明

  ```js
  var a = 1;
  var b = 2;
  ```

  

* 无法限制修改（没有常量）

* 没有块级作用域

### 2.2 let const

* 不能重复声明
* const限制对变量的修改
* 只在块级作用域中生效

## 三、函数

### 3.1 箭头函数

* 如果只有一个参数，()可以省略
* 如果只有一个return语句，{}可以省略
* 箭头函数中的this指向其声明时的词法作用域，而普通函数的this需要在其执行的时候才能确定其指定的对象

### 3.2 函数参数

* 剩余参数

  ```js
  //...args必须放在最后，将多余的参数放在args数组中，通过扩展运算符可以展开
  function show(a,b,...args){
    console.log(args) // [24 23 25]
  }
  show(12,13,24,23,25)
  ```

* 默认参数

  ```js
  //如果b和c不传，就使用默认值
  function show(a,b=5,c=12){
    
  }
  show(1)
  ```

## 四、解构赋值

* 左右两边结构必须一样

* 右边必须是一个东西

* 声明和赋值不能分开（必须在一句话中完成）

  ```js 
  //左边是数组右边也是数组，左边是对象字面量右边也要说对象字面量
  let [a,b,c] = [1,2,3]
  let {a,b} = {a:12,b:13}
  
  let [{a,b},[c,d,e],f,g] = [{a: 12,b: 13},[12,3,4],8,'xu']
  //控制拆分的粒度
  let [json,arr,num,str] = [{a: 12,b: 13},[12,3,4],8,'xu']
  ```

## 五、数组

* map --映射 --返回数组

  ```js
  array.map(function(currentValue,index,arr), thisValue)
  //thisValue --函数中的this指向对象
  //arr --当前使用filter的数组实例
  ```

  

* reduce --汇总 --返回计算结果

  ```js
  array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
  //initialValue --初始迭代值，没有会把数组的第一个元素作为初始迭代值
  ```

  

* filter --过滤 --返回数组

  ```js
  array.filter(function(currentValue,index,arr), thisValue)
  //thisValue --函数中的this指向对象
  //arr --当前使用filter的数组实例
  ```

  

* forEach --迭代 --无返回值

  ```js
  array.forEach(function(currentValue, index, arr), thisValue)
  //thisValue --函数中的this指向对象
  //arr --当前使用filter的数组实例
  ```

  

## 六、字符串

* 多了两个新方法

  ```js
  let a = 'http://wwww.xxxx.com'
  let b = 'xu.txt'
  //方法一
  if(a.startsWith('http://')){
    
  }else if(a.startsWith('https://')){
    
  }
  //方法二
  if(b.endsWith('.txt')){
    
  }else if(b.endsWith('.jpg')){
    
  }
  ```

* 字符串模板

## 七、面向对象

### 7.1 原来的缺陷

* 类的构造函数和普通函数一样

### 7.2 新的

* class关键字
* 专门的构造器

#### 7.2.1 继承

```js
//原来
function User(name,pass){
  this.name = name
  this.pass = pass
}
User.prototype.showName = function(){
  console.log(this.name)
}
User.prototype.showPass = function(){
  console.log(this.pass)
}
//继承
function VipUser = function(name,pass.level){
  User.call(this,name,pass) //调用父类的构造函数
  this.level = level
}
VipUser.prototype = new User()
VipUser.prototype.constructor = VipUser
VipUser.prototype.showLevel = function(){
  console.log(this.level)
}
```

```js
//现在
class User{
  constructor(name,pass){
    this.name = name
    this.pass = pass
  }
  showName(){
    console.log(this.name)
  }
  showPass(){
    console.log(this.pass)
  }
}

class VipUser extends User{
  constructor(name,pass,level){
    super(name,pass)
    this.level = level
  }
  showLevel(){
    console.log(this.level)
  }
}
```

#### 7.2.2 面向对象应用 --React

* 一个组件就是一个class（Vue中一个组件是一个.vue文件）
* JSX --就是babel，就是browser.js

```jsx
//利用react写一个组件
class Item extends React.Component{
  constructor(...args){
    super(...args)
  }
  
  //这个函数必须写
  render(){
    return <li>{{this.props.str}}</li>
  }
}

//渲染组件到页面
ReactDOM.render(
  <ul>
    <item str='abc'></item>
  </ul>
)
```

#### 7.2.3 静态方法

* 在方法名前面加上关键字static
* 静态方法不能在类的实例中使用，只能这样使用Student.classMethod()
* 父类的静态方法可以被子类继承

```js
class Student {
    static classMethod(){
        
    }
}
```

#### 7.2.4 super关键字

*  super关键字既可以当函数使用，也可以当对象使用
* 作为函数使用的时候super()，只能存在于子类的构造函数中，代表调用父类的构造函数
* 作为对象使用时，指向父类的原型对象（即父类的prototype对象）

## 八、对象字面量

#### 8.1 json对象

* 只能用双引号
* 所有的名字必须用双引号括起来

```js
JSON.stringify()
JSON.parse()
```

#### 8.2 对象字面量名字的简写

* key和value一样，可以只写一个

```js
let a = 8
let b = 4
let obj = {
  a,
  b
}
```

## 九、Promise

* 异步：操作之间没什么关系，可以同时进行多个操作
* 同步：同时只能做一件事
* 作用，可以使用写同步代码的方式写异步代码，从而避免回调地狱

### 9.1 写法 + 手写实现Promise

```js
let p1 = new Promise((resolve,reject) => {
  //写异步代码
})
let p2 = new Promise((resolve,reject) => {
  //写异步代码
})
```

```js
//手写
//这个不能进行链式调用且只能封装有异步操作的函数
class MyPromise {
    constructor(fn){
        this.status = 'pending'
        this.message = null
        this.resolveCallback = null
        this.rejectCallBack = null
        if(typeof fn !== 'function'){
            throw Error('argument must be a function')
        } else {
            fn(this.resolve.bind(this),this.reject.bind(this))
        }
    }
    then(resolveCallback,rejectCallBack = null){
        this.resolveCallback = resolveCallback
        this.rejectCallBack = rejectCallBack
    }
    resolve(message){
        if(this.status === 'pending'){
            this.status === 'resolved'
            this.resolveCallback(message)
        }
    }
    reject(message){
        if(this.status === 'pending'){
            this.status === 'reject'
            this.rejectCallBack(message)
        }
    }
    catch(rejectCallBack){
        this.then(null,rejectCallBack)
    }
}

let p = new MyPromise((resolve,reject) => {
    setTimeout(function(){
        reject('error')
    },500)
})
p.then(res => {
    console.log(res)
},err => {
    console.log(err)
})
```



### 9.2 静态方法

#### 9.2.1 Promise.all()

```js
//只有p1和p2都执行了resolve函数(全部成功了)，才会执行then中的函数，否则执行catch中的函数
//arr是一个数组，分别存放前面两个的结果
Promise.all([p1,p2]).then(function(arr){
  let [res1,res2] = arr
}).catch()

```

#### 9.2.2 Promise.race()

```js
//只有p1和p2有一个执行了resolve函数(有一个成功了)，才会执行then中的函数，否则执行catch中的函数
Promise.race([p1,p2]).then(function(res){
  console.log(res)
}).catch()
```

#### 9.2.3 Promise.resolve()

* 将普通对象变为promise对象，返回一个promise对象

  ```js
  let p = Promise.resolve('foo')
  // 等价于
  let p = new Promise(resolve => resolve('foo'))
  ```

* 如果传入的参数本身就是一个promise对象，那么直接返回这个promise对象

* 参数是一个`thenable`对象

  ```js
  let thenable = {
    then: function(resolve, reject) {
      resolve(42);
    }
  };
  
  let p1 = Promise.resolve(thenable);
  p1.then(function(value) {
    console.log(value);  // 42
  });
  ```

* 参数不是具有`then`方法的对象，或根本就不是对象 --直接返回一个状态为resolved的promise对象

  ```js
  
  const p = Promise.resolve('Hello');
  
  p.then(function (s){
    console.log(s)
  });
  // Hello
  ```

#### 9.2.4 Promise.reject()

* 返回一个状态为rejected的promise对象，reject()的参数就是传递进去的原来参数

```js
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```

### 9.3 原型方法(实例方法)

#### 9.3.1 p.then()--可以链式调用

```js
//getJSON函数会返回一个promise实例
//在then函数中，如果在return中也返回一个promise实例，则会覆盖原本then()函数返回的promise实例

getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);//这里返回了一个新的promise实例
}).then(function (comments) {
  console.log("resolved: ", comments);
}, function (err){
  console.log("rejected: ", err);
});
```

#### 9.3.2 p.catch()

* 等价于p.then(null,callback)

  ```js
  p.then((val) => console.log('fulfilled:', val))
    .catch((err) => console.log('rejected', err));
  // 等同于
  p.then((val) => console.log('fulfilled:', val))
    .then(null, (err) => console.log("rejected:", err));
  
  //promise抛出一个错误，被catch捕获
  const promise = new Promise(function(resolve, reject) {
    throw new Error('test');
  });
  promise.catch(function(error) {
    console.log(error);
  });
  ```

* 在promise中如果出错，不会终止程序的运行,如下：promise中出现的错误，但是后面的定时还是会执行，即使浏览器打印了错误

  ```js
  const someAsyncThing = function() {
    return new Promise(function(resolve, reject) {
      // 下面一行会报错，因为x没有声明
      resolve(x + 2);
    });
  };
  
  someAsyncThing().then(function() {
    console.log('everything is great');
  });
  
  setTimeout(() => { console.log(123) }, 2000);
  // Uncaught (in promise) ReferenceError: x is not defined
  // 123
  ```

## 十、生成器

* 生成器是特殊的函数，可以在执行中间暂停
* 普通函数不能在中间暂停
* 使用场景：请求数据

### 10.1 yield

* 理解为可以在这个位置暂时放弃执行

* 本质将一个大函数分割成多个小函数（JS引擎内部完成）

* yield可以接受外面的参数，也可以发射参数到外面

* n个yield可以对应n+1个函数

* 第一个next()不能传值

* 最后一个next()没有返回的value

  ```js
  //执行该函数可以产生一个生成器对象实例
  function *show(){
    alert('a')
    yield; //yield将这个大函数切割为两个小函数
    alert('b')
  }
  //生成一个生成器对象
  let gen = show()
  
  gen.next()//执行第一个小函数
  gen.next()/执行第二个小函数
  ```

  ```js
  //yield可以接受next给的参数
  function *show(){
    alert('a')
    let a = yield; //yield接受到外面的参数，同时把参数给a
    alert('b')
  }
  //生成一个生成器对象
  let gen = show()
  
  gen.next(5)//第一个next不能传参，会被忽略
  gen.next(8)//执行第二个小函数，同时将5传递给yield 
  ```

  ```js
  //yield可以发射参数出去
  function *show(){
    alert('a')
    yield 12; //yield把12传递到外面的next()的结果
    alert('b')
  }
  //生成一个生成器对象
  let gen = show()
  
  let res1 = gen.next()//res1为第一个yield发射出的值，为{value:12,done:false}
  let res2 = gen.next()//res2没有值，为{value:undefined,done:true}
  ```

  ```js
  //形象展示
  function *炒菜(菜市场买回来的原料){
    洗菜 -> 洗好的菜
    let 干净的菜 = yield 洗好的菜
    
    干净的菜 -> 切丝
    let 切好的菜 = yield 丝
    
    切好的菜 -> 熟的菜
    return 熟的菜
  }
  ```

### 10.2 请求数据相互依赖的写法

* 一个数据请求完成后，根据这个请求的结果去请求另一个请求

## 十一、async/await

* 这两个属于ES2017 同时注意到Promise Generator属于ES2016
* 使用babel进行转义 会将async函数转换为promise和generator的组合（所以可以说async是Generator函数的语法糖）
* 通俗来讲，async函数就是将Generator函数的*号变为async，将yield替换为await

```
特点
1.async表示函数中可能有异步操作
2.await表示紧跟在后面的表达式需要等待结果
3.await后面表达式的值可以是原始类型的值(数字，字符串等，这就相当于是同步操作了)，也可以是promise对象(但是要等待其状态固定   才能执行下面的)
4.async一定返回一个promise对象，如果返回一个普通值，会默认调用Promise.resolve()对其进行封装
```

```js
console.log('script start') 1
async function async1() {
    await async2()
    console.log('async1 end') 5
}

async function async2() {
    console.log('async2 end')  2 
}

async1()

setTimeout(function() {
    console.log('setTimeout') 8
}, 0)

new Promise(resolve => {
    console.log('Promise') 3
    resolve()
})
    .then(function() {
    console.log('promise1') 6
})
    .then(function() {
    console.log('promise2') 7
})

console.log('script end')  4
```

