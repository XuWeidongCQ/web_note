# $API

## 一、说明

* vue中有两个构造函数来产生组件

  ```javascript
  //第一种：一般用来产生根组件
  //通过el属性或者render属性来指定HTML模板
  new Vue(options)
  
  Vue.extend()
  
  //第二种:用来产生自定义组件，这种方式产生的组件默认都为根组件的子组件
  //通过template属性指定HTML模板
  Vue.component() //是Vue.extend()的简单写法
  ```

  

* 只有被Vue代理的HTML代码才能使用vue的特性

* 参考`https://cn.vuejs.org/`

### 1.1 vue工作流程

* step1  创建一个根组件（一般一个web应用只要一个）
* step2 将根组件显示在全局HTML文档中
  * 可以通过在实例化根组件的时候的el属性来指定将根组件显示在何处
  * 若不用el属性，可以使用根组件的$mount()方法来指定将组件显示在何处
  * 通过使用组件标签名的方式，前提是该组件被注册
* step3  在根组件中可以使用多个子组件

## 二、全局配置（部分）

* `Vue.config`是一个对象，包含`Vue`的全局配置

###### keyCodes

```javascript
Vue.config.keyCodes = {
  v: 86,
  f1: 112,
  // camelCase 不可用
  mediaPlayPause: 179,
  // 取而代之的是 kebab-case 且用双引号括起来
  "media-play-pause": 179,
  up: [38, 87]
}

//使用
<input type="text" @keyup.media-play-pause="method">
```

给`v-on`自定义键位别名

###### productionTip

设置为 `false` 以阻止 vue 在启动时生成生产提示。

## 三、全局API

###### **Vue.extend(options)

* 返回一个组件构造器，后续需要实例化这个构造器才能进行使用

```javascript
<div id="mount-point"></div>

// 创建一个组件构造器
var Profile = Vue.extend({
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {//data必须是函数，因为这个构造器可能会实例化多个组件
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})

//使用组件，将这个这个组件挂载在id为mount-point的元素中
new Profile().$mount('#mount-point')
```

###### **Vue.component()

* 可以直接产生一个组件实例（对Vue.extend进行了进一步的封装），有三种使用方式
* 可以省略掉使用组件构造函数产生组件的麻烦

```javascript
// 注册组件，传入一个扩展过的构造器
// 直接在全局产生一个名叫my-component
Vue.component('my-component', Vue.extend({ /* ... */ }))

// 注册组件，传入一个选项对象 (自动调用 Vue.extend)
// 直接在全局产生一个名叫my-component
Vue.component('my-component', { /* ... */ })

// 返回组件my-component的构造函数
var MyComponent = Vue.component('my-component')
```



###### Vue.nextTick()

###### Vue.set()

* 向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。

###### Vue.delete()

* 删除对象的属性。如果对象是响应式的，确保删除能触发更新视图。

###### Vue.directive(id,definition)

* 注册或获取全局指令

###### Vue.filter(id.function)

* 注册或获取全局过滤器

###### Vue.use()

* 使用插件

###### Vue.compile()

* 将一个模板字符串编译成 render 函数。**只在完整版时可用**。

## 四、options(构造组件的选项) ++

### 4.1 数据相关的选项

### data

* 在组件构造函数中，该选项必须是一个函数

### props

* props 可以是数组或对象，用于接收来自父组件的数据。props 可以是简单的数组，或者使用对象作为替代，对象允许配置高级选项，如类型检测、自定义验证和设置默认值。

```javascript
// 数组语法
Vue.component('props-demo-simple', {
  props: ['size', 'myMessage']
})

// 对象语法，提供验证
Vue.component('props-demo-advanced', {
  props: {
    // 检测类型
    height: Number,
    // 检测类型 + 其他验证
    age: {
      type: Number,
      default: 0,
      required: true,
      validator: function (value) {
        return value >= 0
      }
    }
  }
})
```

### propsData

* 只用于 `new` 创建的实例中

### computed

* 计算属性
* 所有计算属性的 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例

### methods ++

* methods 将被混入到 Vue 实例中
* 可以直接通过 VM 实例访问这些方法
* 在指令表达式中使用
* 方法中的 `this` 自动绑定为 Vue 实例

### watch ++

* 一个对象，键是需要观察的表达式，值是对应回调函数。
* 值也可以是方法名，或者包含选项的对象。
* Vue 实例将会在实例化时调用 `$watch()`，遍历 watch 对象的每一个属性。

```javascript
var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: {
      f: {
        g: 5
      }
    }
  },
  watch: {
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // 方法名
    b: 'someMethod',
    // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    },
    // 该回调将会在侦听开始之后被立即调用
    d: {
      handler: 'someMethod',
      immediate: true
    },
    // 你可以传入回调数组，它们会被逐一调用
    e: [
      'handle1',
      function handle2 (val, oldVal) { /* ... */ },
      {
        handler: function handle3 (val, oldVal) { /* ... */ },
        /* ... */
      }
    ],
    // watch vm.e.f's value: {g: 5}
    'e.f': function (val, oldVal) { /* ... */ }
  }
})
vm.a = 2 // => new: 2, old: 1
```

### 4.2 DOM相关的选项

### el

* 只在用 `new` 创建实例时生效
* 声明Vue组件实例要代理在全局html文档的哪个位置

### template ++

* vue组件被代理的html模板（不从全局的HTML文档中选择）
*  模板将会**替换**挂载的元素 
* 如果同时存在el和template，则el指向的HTML模板将会被**替换**（不是在该元素中插入一个子元素），除非该模板中有slot插槽

### render ++

* 在不使用template的是时候，利用javascript产生
* 构造函数选项中的 `render` 函数若存在，则构造函数不会从 `template` 选项或通过 `el` 选项指定的挂载元素中提取出的 HTML 模板编译渲染函数。

### renderError

* 当 `render` 函数遭遇错误时，提供另外一种渲染输出。其错误将会作为第二个参数传递到 `renderError`。这个功能配合 hot-reload 非常实用。

### 4.4 资源

### directives

* 包含 Vue 实例可用指令的哈希表

### filters ++

* 包含 Vue 实例可用过滤器的哈希表
* 在过滤器中不能使用`this`

### components ++

* 包含 Vue 实例可用组件的哈希表

### 4.5 其他

### name

* 只有作为组件选项时起作用，组件的名字

### functional

* 创建函数式组件使用

### model

* 允许一个自定义组件在使用 `v-model` 时定制 prop 和 event。
* 默认情况下，一个组件上的 `v-model` 会把 `value` 用作 prop 且把 `input` 用作 event，但是一些输入类型比如单选框和复选框按钮可能想使用 `value` prop 来达到不同的目的。使用 `model` 选项可以回避这些情况产生的冲突。

```javascript
//产生一个my-checkbox组件，使其可以使用v-model指令
Vue.component('my-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    // this allows using the `value` prop for a different purpose
    value: String,
    // use `checked` as the prop which take the place of `value`
    checked: {
      type: Number,
      default: 0
    }
  },
  // ...
})
<my-checkbox v-model="foo" value="some value"></my-checkbox>
//相当于
<my-checkbox
  :checked="foo"
  @change="val => { foo = val }"
  value="some value">
</my-checkbox>
```

## 五、实例化的组件的方法

### vm.$on(event,callback)

* 监听当前实例上的自定义事件。
* 事件可以由 `vm.$emit` 触发。
* 回调函数会接收所有传入事件触发函数的额外参数。

```javascript
vm.$on('test', function (msg) {
  console.log(msg)
})
vm.$emit('test', 'hi')
// => "hi"
```



### vm.$emit(eventName,[...args])

* 触发当前实例上的事件。附加参数都会传给监听器回调。

### vm.$once

* 监听一个自定义事件，但是只触发一次。一旦触发之后，监听器就会被移除。

### vm.$off

* 移除自定义事件监听器。



### vm.$mount() ++ 

* 如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。可以使用 `vm.$mount()` 手动地挂载一个未挂载的实例。

```javascript
// 产生一个组件构造函数
var MyComponent = Vue.extend({
  template: '<div>Hello!</div>'
})

// 创建一个组件并挂载到 #app (会替换 #app元素里面的内容)
new MyComponent().$mount('#app')

// 同上 这里同时存在el和template，那么el元素中的内容会被template替换
new MyComponent({ el: '#app' })

// 或者，在文档之外渲染并且随后挂载
var component = new MyComponent().$mount()
document.getElementById('app').appendChild(component.$el)
```



### vm.$destroy()

* 完全销毁一个实例。清理它与其它实例的连接，解绑它的全部指令及事件监听器。
* 触发 `beforeDestroy` 和 `destroyed` 的钩子。

## 六、实例化组件的属性

### vm.$data

* 得到vm的数据选项

### vm.$props

### vm.$el ++

* 只读，该实例使用的根DOM元素

### vm.$options

### vm.$parent

### vm.$root

### vm.$children

### vm.$refs

### vm.attrs

### vm.$listener

# $组件

## 全局组件

* 通过`Vue.component()`产生的组件实例为全局组件

## 局部组件

* 通过在实例化组件的`components`属性中传入的组件构造对象可以产生这个组件的专有的子组件
* 注意：这里传入`components`属性的只是一个用于产生组件实例的`options`,而不是一个实例化过后的组件

```html
<script>
  			//这是实例化子组件的options
        var childcom ={
            data:function(){
                return{
                    num:0,
                }
            },
            template:`<button v-on:click='change'>点我{{num}}次</button>`,
            methods:{
                change:function(){
                    this.num += 1;
                }
            }
        }
        //这里产生一个组件
       var vm = new Vue({
           el:'#app',
           data:{
 
           },
           components:{
         // 将要产生子组件的options传入，则父组件实例化的时候，也会把这个子组件实例化
               'com-btn':childcom,
           }
           
       })
    </script>
```

# $VUE插件使用方式

在vue中添加插件的方式有以下几种：

* 添加到全局方法或者 property，比如插件[vue-custom-element](https://github.com/karol-f/vue-custom-element)

  ```jsx
  //在全局中插入组件
  <widget-vue prop1="1" prop2="string" prop3="true"></widget-vue>
  
  //使用插件的全局方法注册组件
  Vue.customElement('widget-vue', {
    props: [
      'prop1',
      'prop2',
      'prop3'
    ],
    data: {
      message: 'Hello Vue!'
    },
    template: '<p>{{ message }}, {{ prop1  }}, {{prop2}}, {{prop3}}</p>'
  });
  ```

  

* 添加一个全局资源：指令/过滤器/过渡等。如插件 [vue-touch](https://github.com/vuejs/vue-touch)

  ```jsx
  //通过在全局中插入指令
  <a v-touch:tap="onTap">Tap me!</a>
  <div v-touch:swipeleft="onSwipeLeft">Swipe me!</div>
  ```

  

* 通过全局混入来添加一些组件选项。如 [vue-router](https://github.com/vuejs/vue-router)

* 添加 Vue 实例方法，通过把它们添加到 `Vue.prototype` 上实现。