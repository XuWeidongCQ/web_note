# 一、为什么要使用？

设想下面两种情况

* 多个视图依赖同一个状态
* 来自不同组件的行为需要变更同一个状态

原始的解决方案

* 针对问题一：利用多层嵌套的组件进行传参
* 针对为题二：多层嵌套组件通过事件来变更和同步状态的多份拷贝

高级解决方案

* 使用vuex

# 二、使用

```js
npm install vuex --save
```

新建一个文件夹store/store.js用来管理集中的数据

```js
import Vue from 'vue'
import Vuex from 'vuex'

//生成一个store对象
const store = new Vuex.Store(options)

//最后导出
export default store

//使用
import store from '@/store/store'
new Vue({
  el: '#app',
  store: store,
})
```

* 通过在根组件中的构造选项中注入store对象，可以让这个根组件的每一个子组件都可以使用vuex中的数据

# 三、option构建对象选项

## 3.1 state以及子组件怎么使用

* 在state中写数据就像在组件中的data选项中书写一样，只是不要用函数的形式

  ```js
  state:{
      homePage:{
        mapCenter:null,//地图中心
        infoWindowCenter:{},//信息窗口弹出位置
        infoWindowShown:false,//是否显示信息窗口
      }
        ....
        //其他的数据
    },
  ```

### 3.1.1 子组件使用原始方法

* 通过在根组件中注入了store对象，那么每一个组件实例都会添加一个$store属性指向vuex的option构造对象

* 故可以使用`this.$store.state`来直接访问state中的数据

* 可以把vuex中的数据映射到组件的计算属性上

  ```jsx
  const Counter = {
    template: `<div>{{ count }}</div>`,
    computed: {
      count () { //将vuex中的数据映射到组件的计算属性上
        return this.$store.state.count
      }
    }
  }
  ```

### 3.1.2 子组件使用mapState辅助函数

* 如果一个组件需要获取多个vuex中的数据，使用辅助函数可以减少代码量

* mapState()可以接收一个对象

* 将这个对象的键映射为组件计算属性的键，值映射为组件计算属性的值，如果值为函数，函数默认可以接收一个参数代表vuex中的state对象

  ```js
  import { mapState } from 'vuex'
  
  export default {
    // ...
    computed: mapState({
      // 箭头函数可使代码更简练
      count: state => state.count, //函数默认接收一个state参数代表vuex中的state对象
  
      // 传字符串参数 'count' 等同于 `state => state.count`
      countAlias: 'count',
  
      // 为了能够使用 `this` 获取局部状态，必须使用常规函数
      countPlusLocalState (state) {
        return state.count + this.localCount
      }
    })
  }
  ```

* mapState()可以接收一个数组

  ```js
  computed: mapState([
    // 映射 this.count 为 store.state.count
    'count'
  ])
  //等价于
  computed: mapState({
      count:state => state.count
  })
  ```

* 如果子组件本身就有计算属性，那么要使用扩展运算符

  ```js
  computed: {
      //这是子组件自己的计算属性
    localComputed () { /* ... */ },
        
    // 使用对象展开运算符将此对象混入到外部对象中
    ...mapState({
      // ...
    })
  }
  ```

  

## 3.2 getters以及子组件怎么使用

* vuex中的getters对于state就相当于组件的computed对于data属性

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: { 
    doneTodos: state => { //默认接收state作为函数的第一个参数
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

### 3.2.1 子组件使用原始方法

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

### 3.2.2 子组件通过mapGetters辅助函数

* 与mapState()所不同的是，该函数不支持在对象的值中传入函数

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([ //数组写法
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}

...mapGetters({//对象写法
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})

...mapGetters({//对象写法 这种写法是错误的
  doneCount: getters => getters.doneTodoCount
})
```

## 3.3 mutations以及子组件怎么使用

* 用于子组件操作vuex中的state中的数据
* mutations中的函数必须是同步函数

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
      //这个函数默认有两个参数
      //state: 代表vuex中的state对象
      //payload: 子组件提交上来的数据
    increment (state,payload) { 
      // 变更状态
      state.count++
    }
  }
})
```

### 3.3.1 在子组件中使用

* 一般写在子组件的methods属性中

```js
methods: {
    handle(payload){
        this.$store.commit('increment',payload)
    }
}
//也可以 此时整个对象都将传入到payload中
methods: {
    handle(){
        this.$store.commit({
            type:'increment',
            amount: 10
        })
    }
}
```

### 3.3.2 使用辅助函数mapMutations

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

## 3.4 actions

* actions是对mutations扩展，可以进行异步操作

  ```js
  const store = new Vuex.Store({
    state: {
      count: 0
    },
    mutations: {
      increment (state) {
        state.count++
      }
    },
    actions: {
        //该函数的第一个参数默认会传入一个context上下文对象 指向store实例
       //第二个参数接收组件传递过来的数据
      increment (context，payload) { 
        context.commit('increment') //
      }
        //可以搭配解构
      increment ({commit,state}，payload) { 
        commit('increment') //
      }
    }
  })
  ```

### 3.4.1 在子组件中实用

* 一般写在子组件的method中

  ```js
  methods: {
      handle(payload){
          this.$store.dispatch('increment',payload)
      }
  }
  //也可以 此时整个对象都将传入到payload中
  methods: {
      handle(){
          this.$store.commit({
              type:'increment',
              amount: 10
          })
      }
  }
  ```

### 3.4.2 使用辅助函数mapActions

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```

## 3.5 modules