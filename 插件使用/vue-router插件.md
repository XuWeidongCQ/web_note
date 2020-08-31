# 一、起步

## 1.1 使用方式

* 路由在web项目中通过一个独立的js文件来进行管理，同时导出一个路由实例

  ```javascript
  import Vue from 'vue'
  import Router from 'vue-router'
  import Login from "@/views/Login";
  import Home from "@/views/Home";
  
  Vue.use(Router);
  
  export default new Router(options)
  ```

* 同时在main文件中，使用这个路由实例

  ```javascript
  import Vue from 'vue'
  import App from './App.vue'
  import router from './router'
  
  new Vue({
    router,
    render: h => h(App)
  }).$mount('#app');
  ```

* 使用了路由的web应用，会在所有的组件实例中注入一个`$router`属性(本质是加到vue.prototype上的)

* 使用了路由的web应用，所有的组件中都可以使用<router-link>组件和<router-view>组件

# 二、API

## 2.1 router-link组件的props

* <router-link>组件支持用户在具有路由功能的应用中 (点击) 导航。 
* 通过 `to` 属性指定目标地址，默认渲染成带有正确链接的<a>标签，可以通过配置 `tag` 属性生成别的标签.。
* 另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名。

###### to

* 表示目标路由的链接。当被点击后，内部会立刻把 `to` 的值传到 `router.push()`，所以这个值可以是一个字符串或者是描述目标位置的对象。

  ```html
  <!-- 字符串 -->
  <router-link to="home">Home</router-link>
  <!-- 渲染结果 -->
  <a href="home">Home</a>
  
  <!-- 使用 v-bind 的 JS 表达式 -->
  <router-link v-bind:to="'home'">Home</router-link>
  
  <!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
  <router-link :to="'home'">Home</router-link>
  
  <!-- 同上 -->
  <router-link :to="{ path: 'home' }">Home</router-link>
  
  <!-- 命名的路由 -->
  <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
  
  <!-- 带查询参数，下面的结果为 /register?plan=private -->
  <router-link :to="{ path: 'register', query: { plan: 'private' }}"
    >Register</router-link
  >
  ```

  

###### replace

* 布尔值，默认为false
* 设置 `replace` 属性的话，当点击时，会调用 `router.replace()` 而不是 `router.push()`，于是导航后不会留下 history 记录。

###### append

* 布尔值，默认为false

* 设置 `append` 属性后，则在当前 (相对) 路径前添加基路径。例如，我们从 `/a` 导航到一个相对路径 `b`，如果没有配置 `append`，则路径为 `/b`，如果配了，则为 `/a/b`

  ```html
  <router-link :to="{ path: 'relative/path'}" append></router-link>
  ```

  

###### tag

* 有时候想要 渲染成某种标签，例如 <li>。 于是我们使用 `tag` prop 类指定何种标签，同样它还是会监听点击，触发导航。

###### active-class

* 默认激活class类名为"router-link-active"，可以改成自己的
* 设置链接激活时使用的 CSS 类名。默认值可以通过路由的构造选项 `linkActiveClass` 来全局配置

###### exact-active-class

* 默认激活class类名为"router-link-exact-active"，可以改成自己的
* 配置当链接被精确匹配的时候应该激活的 class。注意默认值也是可以通过路由构造函数选项 `linkExactActiveClass` 进行全局配置的

###### exact

* 布尔值，默认为false
* 是否精确匹配路由，以激活css类

###### event

* 声明可以用来触发导航的事件。可以是一个字符串或是一个包含字符串的数组。

## 2.2 router-view组件的props

* <router-view>组件是一个 functional 组件，渲染路径匹配到的视图组件
* <router-view>组件可以进行相互嵌套，根据嵌套路径，渲染嵌套组件。

* 因为它也是个组件，所以可以配合transition和keep-alive使用。如果两个结合一起用，要确保在内层使用 

  ```html
  <transition>
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </transition>
  ```

###### name

* 命名视图
* 如果<router-view>设置了名称，则会渲染对应的路由配置中 `components` 下的相应组件。

## 2.3 router实例对象的API

#### 2.3.1 属性

###### router.app

* 返回配置了 `router` 的 Vue 根实例(一般为应用的根组件)

###### router.mode

* 路由使用的导航模式

###### router.currentRoute

* 当前路由对应的路由信息对象

#### 2.3.2 全局导航守卫方法

###### router.beforeEach()

###### router.beforeResolve()

###### router.afterEach()

```js
router.beforeEach((to, from, next) => {
  /* 必须调用 `next` */
})

router.beforeResolve((to, from, next) => {
  /* 必须调用 `next` */
})

router.afterEach((to, from) => {})
```

#### 2.3.3 编程式导航的方法

###### router.push()

###### router.replace()

###### router.go()

###### router.back()

###### router.forward()

```js
router.push(location, onComplete?, onAbort?)
router.push(location).then(onComplete).catch(onAbort)
router.replace(location, onComplete?, onAbort?)
router.replace(location).then(onComplete).catch(onAbort)
router.go(n)
router.back()
router.forward()

// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})

//如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

## 2.4 route对象的API

* 一个**路由对象 (route object)** 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的**路由记录 (route records)**
* 路由对象是不可变 (immutable) 的，每次成功的导航后都会产生一个新的对象
* 在组件内部通过`this.$route`访问

#### 2.4.1 属性

###### $route.path

- 类型: `string`

  字符串，对应当前路由的路径，总是解析为绝对路径，如 `"/foo/bar"`。

###### $route.params

- 类型: `Object`

  一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。

###### $route.query

- 类型: `Object`

  一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/foo?user=1`，则有 `$route.query.user == 1`，如果没有查询参数，则是个空对象。

###### $route.hash

- 类型: `string`

  当前路由的 hash 值 (带 `#`) ，如果没有 hash 值，则为空字符串。

###### $route.fullPath

- 类型: `string`

  完成解析后的 URL，包含查询参数和 hash 的完整路径。

###### $route.matched

- 类型: `Array`
- 得到包含这个路由的所有父路由和子路由的路由对象

* 一个数组，包含当前路由的所有嵌套路径片段的**路由记录** 。路由记录就是 `routes` 配置数组中的对象副本 (还有在 `children` 数组)。

###### $route.name

* 当前路由的名称，如果有的话

###### $route.redirectedFrom

* 如果存在重定向，即为重定向来源的路由的名字

# 三、options构建选项

* 使用路由构造函数生成一个路由实例的语法

  ```js
  const router = new Router({
    routes:[...],
    mode:'xxx',
    base:'/',
    ....   
  })
  ```

###### routes

* 一个数组,每一项可可包含

  ```js
  interface RouteConfig = {
    path: string,
    component?: Component,
    name?: string, // 命名路由
    components?: { [name: string]: Component }, // 命名视图组件
    redirect?: string | Location | Function,
    props?: boolean | Object | Function,
    alias?: string | Array<string>,
    children?: Array<RouteConfig>, // 嵌套路由
    beforeEnter?: (to: Route, from: Route, next: Function) => void,
    meta?: any,
  
    // 2.6.0+
    caseSensitive?: boolean, // 匹配规则是否大小写敏感？(默认值：false)
    pathToRegexpOptions?: Object // 编译正则的选项
  }
  ```

* routes数组的每一项都会把相应的组件绑定一个路由地址，同时在匹配到相应的路由地址的时候，把对应的组件替换到<router-view>组件中
* <router-link>组件和<router-link>组件只能同一个组件中使用，<router-link>组件可以没有，在使用编程式导航的时候

```js
export default new Router({
  routes: [
    {
      path: '/', //<router-link>组件默认显示的组件
      component: Login
    },
    {
      path: '/home',
      component: Home,
      children:[
        {
          path:'',//嵌套<router-link>组件默认显示的组件
          component:() => import('./components/HomeContent.vue'),
        },
        {
          path: '/instrument-info-anesthesia-depth',
          component: () => import('./components/InstrumentInfoAnesthesiaDepth.vue'),
        }
      ]
    }
  ]
})
```

* 举例说明，如上
  * 首先，最外层被绑定路由的组件为`Login`和`Home`，那么这两个组件可以被第一个<router-view>(根组件中的<router-view>组件)替换
  * `Home`组件具有一个嵌套路由，那么这个嵌套路由对应的组件只能被`Home`组件中的<router-view>组件替换

###### mode

* 类型: `string`
* 默认值: `"hash" (浏览器环境,导航带#) | "abstract" (Node.js 环境)`
* 可选值: `"hash" | "history"(导航不带#) | "abstract"`

###### base

* 应用的基路径
* 默认为`"/"`

###### linkActiveClass

* 设置导航被激活的CSS类名
* 默认为"router-link-active"

###### linkExactActiveClass

* 设置导航被精确激活的CSS类名
* 默认为"router-link-exact-active"

###### scrollBehavior

* 切换路由时候，是否回到浏览器顶部，滚动行为相关

  ```js
  const router = new VueRouter({
    routes: [...],
    scrollBehavior (to, from, savedPosition) {
      // return 期望滚动到哪个的位置
    }
  })
  
  scrollBehavior (to, from, savedPosition) {
    return { x: 0, y: 0 }
  }
  ```

  

###### parseQuery/stringQuery

* 提供自定义查询字符串的解析/反解析函数。覆盖默认行为。

###### fallback



# 四、导航守卫

### 4.1 全局前置守卫beforeEach()

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

* 在所有的导航进入之前执行
* `to` 将要去的route对象 
* `from` 即将离开的route对象 
* `next()`
  * **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。
  * **`next(false)`**: 中断当前的导航。
  * **`next()`**: 进行管道中的下一个钩子。

* **确保要调用 `next` 方法，否则钩子就不会被 resolved**

### 4.2 全局解析守卫

### 4.3 全局后置钩子afterEach()

```js
router.afterEach((to, from) => {
  // ...
})
```

* 在每个导航完成之后进行
* 没有next函数调用

### 4.4 路由独享的守卫

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

* 使用方法和全局是一样的

### 4.5 组件内的守卫[重要]

* 可以直接在产生组件实例的options中添加相关的导航守卫

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

* `beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用,因此即将登场的新组件还没被创建。

* `beforeRouteLeave`守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 `next(false)` 来取消。

```js
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

# 五、路由元信息

* 可以搭配导航守卫做登录验证

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

* 下面对元信息进行检查

```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```

# 六、数据获取

* 有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。

### 6.1 导航完成后获取

* 先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
* 当使用这种方式的时候，会立马把组件渲染出来，然后可以在created()中去获取数据，可以在获取数据期间展示一个loading状态

```html
<template>
  <div class="post">
    <div v-if="loading" class="loading">
      Loading...
    </div>

    <div v-if="error" class="error">
      {{ error }}
    </div>

    <div v-if="post" class="content">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>
<script>
	export default {
  data () {
    return {
      loading: false,
      post: null,
      error: null
    }
  },
  created () {
    // 组件创建完后获取数据，
    // 此时 data 已经被 observed 了
    this.fetchData()
  },
  watch: {
    // 如果路由有变化，会再次执行该方法
    '$route': 'fetchData'
  },
  methods: {
    fetchData () {
      this.error = this.post = null
      this.loading = true
      // replace getPost with your data fetching util / API wrapper
      getPost(this.$route.params.id, (err, post) => {
        this.loading = false
        if (err) {
          this.error = err.toString()
        } else {
          this.post = post
        }
      })
    }
  }
}
</script>
```



### 6.2 导航完成前获取

* 通过这种方式，可以在导航转入新的路由前获取数据。
* 但是在获取数据期间，会停留在当前界面，最好加上进度条。如果数据获取失败，同样有必要展示一些全局的错误提醒。

```js
export default {
  data () {
    return {
      post: null,
      error: null
    }
  },
  beforeRouteEnter (to, from, next) {
    getPost(to.params.id, (err, post) => {
      next(vm => vm.setData(err, post))
    })
  },
  // 路由改变前，组件就已经渲染完了
  // 逻辑稍稍不同
  beforeRouteUpdate (to, from, next) {
    this.post = null
    getPost(to.params.id, (err, post) => {
      this.setData(err, post)
      next()
    })
  },
  methods: {
    setData (err, post) {
      if (err) {
        this.error = err.toString()
      } else {
        this.post = post
      }
    }
  }
}
```

