# 一、指令

## v-text

```html
<span v-text="msg"></span>
<!-- 和下面的一样 -->
<span>{{msg}}</span>
```



## v-html

```html
<div v-html="html"></div>
```

* 尽量不要使用，易导致XSS攻击

## v-show ++

```html
<div v-show="true"></div>
```

* 利用CSS来控制元素显示和隐藏
* 当条件变化是指令触发过渡效果

## v-if ++

## v-else

## v-else-if

## v-for ++

```html
<div v-for="item in items">
  {{ item.text }}
</div>
<div v-for="(item, index) in items"></div>
<div v-for="(val, key) in object"></div>
<div v-for="(val, name, index) in object"></div>

<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>
```

* `v-for` 的默认行为会尝试原地修改元素而不是移动它们。要强制其重新排序元素，你需要用特殊 attribute `key` 来提供一个排序提示：
* 当和v-if同时作用于一个元素时候，`v-for`的优先级比`v-if`的优先级更高

## v-on ++

重要说明：

* 用在普通元素上时，只能监听[**原生 DOM 事件**](https://developer.mozilla.org/zh-CN/docs/Web/Events)

* 如果要使用事件对象，有两种方式

  ```jsx
  // 第一种 不带括号 事件对象自动传入函数的第一个参数
  <button v-on:click="greet">Greet</button>
  method:{
    greet:function(e){
      console.log(e)
    }
  }
  //第二种 带了括号 必须使用$event参变量
  <button v-on:click="greet($event)">Greet</button>
  method:{
    greet:function(e){
      console.log(e)
    }
  }
  ```

  

* 所有原生事件参考：https://developer.mozilla.org/zh-CN/docs/Web/Events

* 用在自定义元素组件上时，也可以监听子组件触发的**自定义事件**，自定义事件没有event对象

  ```jsx
  //不带括号 子组件传递的值默认传入事件函数的第一个参数
  <my-component @my-event="handleThis"></my-component>
  
  //带括号 子组件传递值给$event
  <!-- 内联语句 -->
  <my-component @my-event="handleThis(123, $event)"></my-component>
  
  <!-- 组件中的原生事件 -->
  <my-component @click.native="onClick"></my-component>
  ```

  

修饰符：

* `.stop` - 调用 `event.stopPropagation()`。
* `.prevent` - 调用 `event.preventDefault()`。
* `.capture` - 添加事件侦听器时使用 capture 模式。
* `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
* `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
* `.native` - 监听组件根元素的原生事件。
* `.once` - 只触发一次回调。
* `.left` - (2.2.0) 只当点击鼠标左键时触发。
* `.right` - (2.2.0) 只当点击鼠标右键时触发。
* `.middle` - (2.2.0) 只当点击鼠标中键时触发。
* `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

```html
<!-- 方法处理器 -->
<button v-on:click="doThis"></button>

<!-- 动态事件 (2.6.0+) -->
<button v-on:[event]="doThis"></button>

<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 缩写 -->
<button @click="doThis"></button>

<!-- 动态事件缩写 (2.6.0+) -->
<button @[event]="doThis"></button>

<!-- 停止冒泡 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认行为 -->
<button @click.prevent="doThis"></button>

<!-- 阻止默认行为，没有表达式 -->
<form @submit.prevent></form>

<!--  串联修饰符 -->
<button @click.stop.prevent="doThis"></button>

<!-- 键修饰符，键别名 -->
<input @keyup.enter="onEnter">

<!-- 键修饰符，键代码 -->
<input @keyup.13="onEnter">

<!-- 点击回调只会触发一次 -->
<button v-on:click.once="doThis"></button>

<!-- 对象语法 (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

## v-bind ++(class的绑定)

```html
<!-- 绑定一个 attribute -->
<img v-bind:src="imageSrc">

<!-- 动态 attribute 名 (2.6.0+) -->
<button v-bind:[key]="value"></button>

<!-- 缩写 -->
<img :src="imageSrc">

<!-- 动态 attribute 名缩写 (2.6.0+) -->
<button :[key]="value"></button>

<!-- 内联字符串拼接 -->
<img :src="'/path/to/images/' + fileName">

<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">

<!-- style 绑定 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>

<!-- 绑定一个全是 attribute 的对象 -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- 通过 prop 修饰符绑定 DOM attribute -->
<div v-bind:text-content.prop="text"></div>

<!-- prop 绑定。“prop”必须在 my-component 中声明。-->
<my-component :prop="someThing"></my-component>

<!-- 通过 $props 将父组件的 props 一起传给子组件 -->
<child-component v-bind="$props"></child-component>

<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```

## v-model ++

默认表单限制：

* `input`
* `select`
* `textarea`

修饰符：

* `.lazy` - 取代 `input` 监听 `change` 事件
* `.number` - 输入字符串转为有效的数字
* `.trim` - 输入首尾空格过滤

### 绑定文本框

```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

### 绑定多行文本

```html
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>

//在文本区域插值 (<textarea>{{text}}</textarea>) 并不会生效，应用 v-model 来代替。
```

### 绑定复选框

* 这样可以不使用input的name属性进行分组

```jsx
//单个复选框，绑定一个布尔值
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
  
//多个复选框，绑定到同一个数组
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
    
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```

### 绑定单选

* 这样可以不使用input的name属性进行分组

```jsx
//
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
```

### 绑定下拉选择框

```jsx
// 单选
<div id="example-5">
  <select v-model="selected">
    //当绑定的值为空的时候，使用一个占位符进行显示
    <option disabled value="">请选择</option> 
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})

//多选 绑定到一个数组
<div id="example-6">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
```



## v-slot ++

限用于：

* `<template>`
* 内部定义了插槽的组件

## v-pre

* 跳过这个元素和其子元素的编译

```html
<span v-pre>{{ this will not be compiled }}</span>
```

## v-cloak

## v-once

* 只渲染这个组件一次，随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。

# 二、特殊的标签和组件属性

## key

* 添加了key的组件，一旦key发生变化，会强制替换元素/组件而不是重复使用它，触发生命周期和过渡

  ```html
  <transition>
    <span :key="text">{{ text }}</span>
  </transition>
  当 text 发生改变时，<span> 总是会被替换而不是被修改，因此会触发过渡。
  ```

  

## ref

* 给组件或者元素添加一个引用信息，可以在其父组件的`$refs`对象上进行操作

## is

* 用于动态组件

  ```html
  <!-- 当 `currentView` 改变时，组件也跟着改变 -->
  <component v-bind:is="currentView"></component>
  ```

  

## slot [废弃] v-slot指令代替

## slot-scope [废弃] v-slot指令代替

## scope [废弃] v-slot指令代替

# 三、内置组件

## component

* 在动态组件中，依靠`is`属性来决定渲染哪一个组件

  ```html
  <component v-bind:is="currentTabComponent"></component>
  ```

## keep-alive

* 搭配`component`组件一起使用，用来保存切换过程中的状态。

* 也可以直接搭配自定义的组件

* 当组件在 `keep-alive` 内被切换，它的 `activated` 和 `deactivated` 这两个生命周期钩子函数将会被对应执行。而不会触发组件的创建和销毁生命周期钩子

  ```html
  <!-- 基本 -->
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>
  
  <!-- 多个条件判断的子组件 -->
  <keep-alive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </keep-alive>
  ```

## slot

### 基本使用

* 在组件中添加了`<slot>`组件，那么这个组件就可以使用`v-slot`指令或者组件包裹的内部可以写东西

* slot本身不会被渲染成任何标签

  ```jsx
  //目前有一个组件
  <navigation-link>
    <a v-bind:href="url" class="nav-link">
    	<slot></slot> //内部使用了slot组件，如果里面有内容将会被默认渲染
  	</a>
  </navigation-link>
  
  //使用这个组件
  //如果没有slot组件，那么该组件起始标签和结束标签之间的任何内容都会被抛弃。
  <navigation-link url="/profile">
    <span class="fa fa-user"></span>
    Your Profile
  </navigation-link>
  ```

### 具名插槽

```jsx
//目前有一个组件
<base-layout>
  <div class="container">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</base-layout>

//使用，利用v-slot指令搭配template标签
<div>
  <base-layout>
    <template v-slot:header>
      <h1>Here might be a page title</h1>
    </template>

    //这部分会放到没有名字的slot中，等价于
    /*<template v-slot:default>
      <p>A paragraph for the main content.</p>
      <p>And another one.</p>
    </template>*/
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>

    <template v-slot:footer>
      <p>Here's some contact info</p>
    </template>
  </base-layout>
</div>
```

### 作用域插槽

* 如果slot中使用了插值表达式，那么该插槽就变成了作用域插槽

* 绑定在 `slot` 元素上的 attribute 被称为**插槽 prop**

  ```jsx
  //目前有一个组件，其插槽接受一个来自父组件的值
  <current-user>
    <span>
      <slot v-bind:user = 'user'> //这里的user属性就相当于slot组件的prop
        {{ user.lastName }}
      </slot>
    </span>
  </current-user>
  
  //使用这个组件,父组件把这个参数通过v-slot传递进去,父组件上存在一个名为user的数据
  <div>
    <current-user>
      <template v-slot:default="slotProps">
        {{ slotProps.user.firstName }}
      </template>
       //可以这样写 使用解构
      <template v-slot:default="{ user }">
        {{ user.firstName }}
      </template>
    </current-user>
  </div>
  
  data:function(){
    return {
      user:{
        firstName:'x',
        lastName:'w'
      }
    }
  }
  ```

  

## transition

* 过渡建议自己用CSS写，不用这个

## transition-group

* 同上