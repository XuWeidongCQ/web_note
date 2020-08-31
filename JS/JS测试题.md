# 一、输出是什么

## 1 ++

```js
const one = (false || {} || null)  // ||运算符会一直找到不为false的值给变量
const two = (null || false || "")
const three = ([] || 0 || true)

console.log(one, two, three) // {} "" []
```

## 2

```js
[..."Lydia"]
//得到
["L", "y", "d", "i", "a"]
```

## 3

```js
[[0, 1], [2, 3]].reduce(
 (acc, cur) => {
  return acc.concat(cur);
 },
 [1, 2]
);

//得到
[1,2,0,1,2,3]
```

## 4

```js
(() => {
 let x, y;
 try {
  throw new Error();
 } catch (x) {
  (x = 1), (y = 2);//这里x为catch块中的，而y直接对全局的进行的了赋值
  console.log(x); //1
 }
 console.log(x); //undefined
 console.log(y);// 2
})();

//得到
1 undefined 2
```

## 5

```js
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
//得到
[1, 2, 3, 7 x empty, 11]
```

## 6

```js
function sayHi() {
 return (() => 0)(); //立即执行 相当于 return 0
}

typeof sayHi(); //得到'number sayHi函数返回立即调用的函数（IIFE）的返回值。 该函数返回0，类型为数字。
```

## 7

```js
const person = { name: "Lydia" };

function sayHi(age) {
 console.log(`${this.name} is ${age}`);
}

sayHi.call(person, 21); //Lydia is 21
sayHi.bind(person, 21);//bind()会返回一个函数 得到function
```

## 8

```jsx
<div onclick="console.log('div')">
    <p onclick="console.log('p')">Click here!</p>
</div>

//得到 p div
```

## 9

```js
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"));
const baz = () => console.log("Third");

bar();
foo();
baz();
//得到 First Third Second
```

## 10

```js
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]); //456
```

## 11

```js
function Person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}

const lydia = new Person('Lydia', 'Hallie')
const sarah = Person('Sarah', 'Smith')

console.log(lydia) //得到Person {firstName: "Lydia", lastName: "Hallie"}
console.log(sarah) //得到undefined
```

## 12

```js
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
}

console.log(member.getFullName()); //TypeError
```

## 13

```js
function sum(a, b) {
  return a + b
}

sum(1, '2') //12
```

## 14

```js
let number = 0
console.log(number++) //0 先执行这个代码 再进行++
console.log(++number) //2 先执行++ 再执行打印
console.log(number) 2
```

## 15 

```js
let a = 3
let b = new Number(3) //包含的内容比a和c多
let c = 3

console.log(a == b) // true
console.log(a === b)// false
console.log(b === c)// false
```

## 16

```js
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log('You are an adult!')
  } else if (data == { age: 18 }) {
    console.log('You are still an adult.')
  } else {
    console.log(`Hmm.. You don't have an age I guess`)
  }
}

checkAge({ age: 18 }) //Hmm.. You don't have an age I guess
```

## 17

```js
function getAge(...args) {
  console.log(typeof args) //object
}

getAge(21)
```

## 18

```js
function getAge() {
  'use strict'
  age = 21
  console.log(age) //ReferenceError
}

getAge()
```

## 19

```js
const getList = ([x, ...y]) => [x, y]
const getUser = user => { name: user.name, age: user.age } //箭头函数这样写不会返回任何值

const list = [1, 2, 3, 4]
const user = { name: "Lydia", age: 21 }

console.log(getList(list)) //[1, [2, 3, 4]]
console.log(getUser(user)) //undefined
```

## 20

```js
const info = {
  [Symbol('a')]: 'b'
}

console.log(info) //{Symbol('a'): 'b'}
console.log(Object.keys(info)) // [] Object.keys()只返回info上可以枚举的属性，Symbol是不可见的
```

## 21

```js
class Person {
  constructor() {
    this.name = "Lydia"
  }
}

Person = class AnotherPerson {
  constructor() {
    this.name = "Sarah"
  }
}

const member = new Person()
console.log(member.name) //Sarah
```

## 22

```js
function nums(a, b) {
  if
  (a > b)
  console.log('a is bigger')
  else 
  console.log('b is bigger')
  return 
  a + b
}

console.log(nums(4, 2)) //a is bigger, undefined
console.log(nums(1, 2)) //b is bigger, undefined
```

## 23

```js
function getItems(fruitList, ...args, favoriteFruit) {
  return [...fruitList, ...args, favoriteFruit]
}

getItems(["banana", "apple"], "pear", "orange") //SyntaxError 剩余参数只能放在最后
```

## 24

```js
const person = {
  name: "Lydia",
  age: 21
}
//Object.entries(person)得到[[name,'Lydia'],[age,21]]

for (const [x, y] of Object.entries(person)) {
  console.log(x, y) // 第一次循环name,'Lydia'    第二次循环age,21
}
```

## 25 ++

```js
function giveLydiaPizza() {
  return "Here is pizza!"
}

const giveLydiaChocolate = () => "Here's chocolate... now go hit the gym already."

console.log(giveLydiaPizza.prototype) // {constructor: ...}
console.log(giveLydiaChocolate.prototype)// undefined 箭头函数没有原型属性
```

## 26

```js
let newList = [1, 2, 3].push(4) //返回长度4

console.log(newList.push(5)) //Error
```

## 27

```js
console.log("I want pizza"[0]) // I
```

## 28 ++

```js
function sum(num1, num2 = num1) {
  console.log(num1 + num2)
}

sum(10) //20
//可以将默认参数的值设置为函数的另一个参数，只要另一个参数定义在其之前即可
```

## 29 ++

```js
// module.js 
export default () => "Hello world"
export const name = "Lydia"

// index.js 
import * as data from "./module"

console.log(data) //{ default: function default(), name: "Lydia" }
```

## 30

```js
class Person {
  constructor(name) {
    this.name = name
  }
}

const member = new Person("John")
console.log(typeof member) // object
```

## 31

```js
console.log(typeof typeof 1) //string
```

# 二、输出

## 1-3

```js
//#1
!!null
!!''
!!1
//false false true



//#2
setInterval(() => console.log('Hi'), 1000) //setInterval()函数会返回一个唯一的ID




//#3
function* generator(i) {
  yield i;
  yield i * 2;
}
const gen = generator(10);
console.log(gen.next().value); //10
console.log(gen.next().value); //20

```

## 4++

```js
//setTimeout和setInterval函数的第三个参数是作为第一个回调函数的参数传递进去的，经常被忽略了
const firstPromise = new Promise((res, rej) => {
  setTimeout(res, 500, "one");
});

const secondPromise = new Promise((res, rej) => {
  setTimeout(res, 100, "two");
});

Promise.race([firstPromise, secondPromise]).then(res => console.log(res)); //two
```

## 5-11

```js
//#5
let person = { name: "Lydia" };
const members = [person];
person = null;

console.log(members); //[{ name: "Lydia" }]



//#6
const person = {
  name: "Lydia",
  age: 21
};

for (const item in person) {
  console.log(item); //"name", "age"
}



//#7
//parseInt设定了进制后 (也就是第二个参数，指定需要解析的数字是什么进制: 十进制、十六机制、八进制、二进制等等……),parseInt 检查字符串中的字符是否合法. 一旦遇到一个在指定进制中不合法的字符后，立即停止解析并且忽略后面所有的字符。
const num = parseInt("7*6", 10); //7 


//#8
[1, 2, 3].map(num => {
  if (typeof num === "number") return;
  return num * 2;
}); //得到[undefined, undefined, undefined]



//#9
function getInfo(member, year) {
  member.name = "Lydia";
  year = "1998";
}
const person = { name: "Sarah" };
const birthYear = "1997";
getInfo(person, birthYear);
console.log(person, birthYear); // { name: "Lydia" }, "1997"



//#10
function greeting() {
  throw "Hello world!";
}

function sayHi() {
  try {
    const data = greeting();
    console.log("It worked!", data);//这句话被跳过
  } catch (e) {
    console.log("Oh no an error!", e);
  }
}
sayHi();//"Oh no an error: Hello world!




//#11
function Car() {
  this.make = "Lamborghini";
  return { make: "Maserati" };//返回一个复合数据，原来的new产生的数据被忽略
}
const myCar = new Car();
console.log(myCar.make);//Maserati
```

## 12++

```js
//let x = y = 10; 是下面这个表达式的缩写:
y = 10;//增加一个全局变量
let x = y;


(() => {
  let x = (y = 10);
})();

console.log(typeof x);//undefined
console.log(typeof y);//number
```

## 13++

```js
//引入的模块是 只读 的: 你不能修改引入的模块。只有导出他们的模块才能修改其值

// counter.js
let counter = 10;
export default counter;

// index.js
import myCounter from "./counter";

myCounter += 1;
console.log(myCounter); //Error
```

## 14

```js
const name = "Lydia";
age = 21;

console.log(delete name);//false
console.log(delete age);//true
//name变量由const关键字声明，所以删除不成功:返回 false. 而我们设定age等于21时,我们实际上添加了一个名为age的属性给全局对象。对象中的属性是可以删除的，全局对象也是如此，所以delete age返回true.
```

