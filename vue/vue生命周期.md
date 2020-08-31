# 一、说明(11个)

* 无论是通过`new`方式还是构造函数的方式产生一个组件实例，都需要经过生命周期
* beforeCreated() 初始化组件的一些属性(带$符号的)
* created() 初始化状态(props,methods,data,computed,watch)
* beforeMount() 在挂载开始之前被调用，创建vm.$el属性
* mounted()  实例已经被挂载到视图上去了，这里可以操作DOM
* beforeUpdate() 该组件的数据更新之间调用
* updated() 该组件的数据更新之后调用
* beforeDestroy() 实例销毁之前调用。在这一步，实例仍然完全可用
* destroyed() 所有的实例和其子实例都被销毁，所有的东西会被接触绑定
* activated() 被keep-alive激活的时候
* deactivated() keep-alive没有被激活的时候
* errorCaptured()

# 二、生命周期

## 2.1 初始化阶段(还没有生成虚拟DOM)

`beforeCreated`之前：

* 初始化组件的属性（给实例上哪些带有$的属性进行初始化）

  ```js
  //比如,给这些属性赋予一个初始值，等到后续才对它们进行处理
  vm.$refs = {}
  vm.$children = []
  vm.$parent = parent
  ...
  ```

* 初始化事件

  ```js
  //将父组件在模板中使用v-on注册的事件添加到子组件的事件系统中
  ```

`beforeCreated`到`created`之间：

* 初始化inject

* 初始化状态

  ```js
  //比如：props,methods,data,computed,watch
  ```

* 初始化provide

## 2.2 模板编译阶段（生成虚拟DOM）

该阶段的主要目的进行模板编译生成虚拟DOM

模板编译：

* 先将Vue代理的模板解析成AST（抽象语法树），然后再使用AST生成渲染函数

* 解析器将模板解析成AST

* 优化器是遍历AST，检测出所有的静态子树，并打上标记，输出还是AST

* 代码生成器将优化后的AST转换成渲染函数中的内容

  ```jsx
  //比如一个模板
  <div>
  	<p>{{ name }}</p>
  </div>
  
  //经过解析器后
  {
      tag:'div',
      type:1,
      ....
      children:[
          {...},
          {...}
      ]
  }
  
  //经过优化器后
  //内容差不多
  
  //经过代码生产器后（类似下面这种字符串）
  'with(this){
      return _c(
          "div",
          {
              attrs:{"id":"el"}
          },
          [
              _v("Hello"+_s(name))
          ]
      
      )
  }'
  
  //渲染函数使用上述字符串就可以生成一个虚拟DOM，使用这个虚拟DOM就可以生成页面了
  ```

## 2.3 挂载阶段(将虚拟DOM挂载到页面中)

主要目的是将虚拟DOM挂载到页面中指定的位置

* 构造组件时候有`el`选项，调用函数vm.$mount(vm.$options.el)，将虚拟DOM挂载到页面中的el指定节点上

### 2.3.1 beforeUpdate  使用虚拟DOM重新渲染之前

### 2.3.2 updated 使用虚拟DOM重新渲染之后

## 2.4 卸载阶段（`beforeDestory`到`destoryed`之间的阶段）

* 调用`vm.$destory()`后组件销毁，将组件从父组件中删除，取消实例上的所有依赖的追踪
* v-if（该方法会在HTML中产生注释节点）
* v-for key

# 三、父子组件的生命周期

* 父组件先于子组件created，而子组件先于父组件mounted  
* 父组件的beforeCreate、created、beforeMount  ----> 所有子组件的beforeCreate、created、beforeMount、mounted --> 父组件的mounted
* 父组件到生成虚拟DOM就停止挂载，然后要等到子组件挂载完成后，父组件才能进行挂载

# 四、生命周期的源码

```js
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}

//这是_init()方法 添加到Vue的原型上
export function initMixin (Vue) {
  Vue.prototype._init = function (options) {
    const vm = this
    // 把用户传递的options选项与当前构造函数的options属性及其父级构造函数的options属性进行合并（关于属性如何合并的问题下面会介绍），得到一个新的options选项赋值给$options属性，并将$options属性挂载到Vue实例上，
    vm.$options = mergeOptions(
        resolveConstructorOptions(vm.constructor), //注入一些全局的选项，例如把全局过滤器添加到组件自己的filter，一些内置的组件加到组件的components上，这是为什么内置组件不用注册的原因,把内置的一些指令也加到组件上
        options || {},
        vm
    )
      
    vm._self = vm
    initLifecycle(vm) // 初始化生命周期
    initEvents(vm) // 初始化事件
    initRender(vm) // 初始化渲染
    callHook(vm, 'beforeCreate') // 调用生命周期钩子函数
    initInjections(vm) //初始化injections
    initState(vm) // 初始化props,methods,data,computed,watch
    initProvide(vm) // 初始化 provide
    callHook(vm, 'created') // 调用生命周期钩子函数

     //如果选项中存在el属性，则把虚拟dom挂载到指定的位置
     //如果没有传入el选项，则不进入下一个生命周期阶段，需要用户手动执行vm.$mount方法才进入下一个生命周期阶段。
    if (vm.$options.el) { 
      vm.$mount(vm.$options.el)
    }
  }
}
```

## 4.1 初始化阶段（created）

### initLifecycle()

```js
export function initLifecycle (vm: Component) {
  const options = vm.$options

  // locate first non-abstract parent
  let parent = options.parent
  if (parent && !options.abstract) {
    while (parent.$options.abstract && parent.$parent) {
      parent = parent.$parent
    }
    parent.$children.push(vm)
  }

  //初始化一些属性值
  vm.$parent = parent
  vm.$root = parent ? parent.$root : vm

  vm.$children = []
  vm.$refs = {}

  vm._watcher = null
  vm._inactive = null
  vm._directInactive = false
  vm._isMounted = false
  vm._isDestroyed = false
  vm._isBeingDestroyed = false
}
```

### initEvents()

```js
 // 初始化事件函数initEvents实际上初始化的是父组件在模板中使用v-on或@注册的监听子组件内触发的事件。
export function initEvents (vm: Component) {
  vm._events = Object.create(null)
 // 注册父组件在子组件上监听的事件，浏览器原生事件不是在这处理的
  const listeners = vm.$options._parentListeners
  if (listeners) {
    updateComponentListeners(vm, listeners)
  }
}
```

### initInjections()

### initState()

```js
export function initState (vm: Component) {
  vm._watchers = []
  const opts = vm.$options
  if (opts.props) initProps(vm, opts.props) //如果组件存在props，则初始化
  if (opts.methods) initMethods(vm, opts.methods)//如果组件存在methods，则初始化
  if (opts.data) { 
    initData(vm) //如果组件存在data，则初始化
  } else {
    observe(vm._data = {}, true /* asRootData */) //如果组件没有data，则初始化为一个空对象
  }
  if (opts.computed) initComputed(vm, opts.computed) //如果组件存在computed，则初始化
  if (opts.watch && opts.watch !== nativeWatch) { //如果组件存在watch并且不是浏览器原生watch，则初始化
    initWatch(vm, opts.watch)
  }
}
```

## 4.2 模板解析阶段（beforeMount）

## 4.3 模板挂载阶段（mounted）