# vue进阶学习

>Create Date ：2020
>
>Author ： Hongyu Chen
>
>Last Change ：2022/11/14
>
>Mail ：2314727037@qq.com



## 一、 vue-router

```js
vue add router
```

这里的路由并不是指我们平时所说的硬件路由器，这里的路由就是SPA（single page application单页应用）的路径管理器。再通俗的说，vue-router就是WebApp的链接路径管理系统。

用 Vue.js + Vue Router 创建单页应用，感觉很自然：使用 Vue.js ，我们已经可以通过组合组件来组成应用程序，当你要把 Vue Router 添加进来，我们需要做的是，将组件 (components) 映射到路由 (routes)，然后告诉 Vue Router 在哪里渲染它们。

> 单页面应用：单页Web应用，就是只有一张Web页面的应用。浏览器一开始会加载必需的HTML、CSS和JavaScript，之后所有的操作都在这张页面完成，这一切都由JavaScript来控制。因此，单页Web应用会包含大量的JS代码，模块化开发和架构设计的重要性不言而喻。
> 链接：https://www.jianshu.com/p/bae4f6604549
> 来源：简书

###  URL->hash  & HTML5->history

我们可以直接修改URL的哈希值，可以通过location.hash()修改。哈希值修改时，不会刷新页面，而是在前端匹配路由，更改页面。我们也可以通过HTML5的history来修改

### 1. 对router代码的解释

#### 1.1. 源码解释

首先，可以在项目目录下找到router文件夹，其中的index.js就是用来配置所有路由信息的接下来用代码和注释的方法来解释这个文件：

```js
import Vue from 'vue'                           // 导入vue
import VueRouter from 'vue-router'              // 导入router
import Home from '../views/Home.vue'            // 导入组件   about不需要导入是因为使用了懒加载

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    redirect: '/home', //重定向，将path值重新定向到/home
    name: 'Home',
    component: Home
  },
  {
    path: '/home',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = new VueRouter({
  mode: 'history',  // 也可以设置为hash模式，但是hash模式的url比较丑
  base: process.env.BASE_URL,       //baseURL
  routes            // 上面的路由配置    
})

export default router    //导出router给main.js

```

#### 1.2. router-link

`<router-link>` 组件支持用户在具有路由功能的应用中 (点击) 导航。 通过 `to` 属性指定目标地址，默认渲染成带有正确链接的 `<a>` 标签，可以通过配置 `tag` 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名。

`<router-link>` 比起写死的 `<a href="...">` 会好一些，理由如下：

- 无论是 HTML5 history 模式还是 hash 模式，它的表现行为一致，所以，当你要切换路由模式，或者在 IE9 降级使用 hash 模式，无须作任何变动。
- 在 HTML5 history 模式下，`router-link` 会守卫点击事件，让浏览器不再重新加载页面。
- 当你在 HTML5 history 模式下使用 `base` 选项之后，所有的 `to` 属性都不需要写 (基路径) 了。

router-link支持v-bind语法，它还有很多别的属性，可以查阅文档：[API 参考 | Vue Router (vuejs.org)](https://router.vuejs.org/zh/api/#exact)

其中有一个可用于动态设置css属性的：

#### 1.3. active-class

- 类型: `string`

- 默认值: `"router-link-active"`

  设置链接激活时使用的 CSS 类名。默认值可以通过路由的构造选项 `linkActiveClass` 来全局配置。

#### 1.4. 不使用router-link跳转路由

我们可以在页面上渲染按钮，给按钮绑定方法：

```js
methods: {
    homeClick() {
        this.$router.push('/home')
        //这里通过router属性下的api操作url
        //千万不要调用js原生的api来操作，这样就会让router这个插件失去意义，调试的时候可能监听不到
    }
}
```

### 2. 动态路由

有些时候，我们需要根据当前信息来匹配路径，比如进入用户界面时，URL为： /user/'userInformation'，此时，userInformation就需要动态决定。此时，我们可以使用v-bind。

|             模式              |      匹配路径       |             $route.params              |
| :---------------------------: | :-----------------: | :------------------------------------: |
|        /user/:username        |     /user/evan      |         `{ username: 'evan' }`         |
| /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: '123' }` |

$route是焦点路由的属性对象，当前选中什么路由，$route就是谁，并且对象中有很多属性，这里打印出来：

```js
Object
fullPath: "/user/Chy/goods/12345"
hash: ""
matched: [{…}]
meta: {}
name: "User"
params: {userID: "Chy", goodsList: "12345"}
path: "/user/Chy/goods/12345"
query: {}
[[Prototype]]: Object
```

### 3.  路由懒加载

懒加载：用到时才加载。

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

结合 Vue 的[异步组件 (opens new window)](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)和 Webpack 的[代码分割功能 (opens new window)](https://doc.webpack-china.org/guides/code-splitting-async/#require-ensure-/)，轻松实现路由组件的懒加载。

```js
 {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  },
      
  /**
   *或者把箭头函数抽取出来：   
   *const About = () => import('...')
   *然后component还写About就行了
   */
```

### 4. 嵌套路由

嵌套路由在开发中经常使用，比如在home页面中，我们希望通过/home/news和/home/message访问不同的内容。news和message分别对应两个组件。

实现嵌套路由有两个步骤：

- 创建对应的子组件，并且在路由映射中配置对应的子路由
- 在组件内部使用`<router-view>`

> 子路由的意思，就是不改变上一级路由的路径，在后面拼接设定的路径

### 5. vue-router参数传递

单数传递主要有两种类型： params和query

- params：

  配置路由格式： /router/:info

  传递的方式: 在path后面跟上对应的数值

  传递后形成的路径：/router/123, /router/abc

- query：

  配置路由格式：/router

  传递方式：对象中使用query的key作为传递方式

  传递后形成的路径：/router?info=123,/router?info=abc

虽然都可以用来传参，但是params和query是完全不同的两个东西。params是路由的路径，属于path；而query是跟随路径传递的信息，不参与路径匹配。

### 6. 全局导航守卫

导航首位其实有点类似于生命周期函数，导航首位就是监听路由的变化，并触发回调函数。

这里举例讲解一下全局前置守卫，其余还有一些别的：

##### 6.1. 全局前置守卫

你可以使用 `router.beforeEach` 注册一个全局前置守卫：

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**。

每个守卫方法接收三个参数：

- `to: Route`: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
- `from: Route`: 当前导航正要离开的路由
- `next: Function`**: 一定要调用该方法来** **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
  - `next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed (确认的)。
  - `next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - `next('/')` 或者 `next({ path: '/' })`: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。
  - `next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

**确保 `next` 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错**。这里有一个在用户未能验证身份时重定向到 `/login` 的示例：

```js
// BAD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  // 如果用户未能验证身份，则 `next` 会被调用两次
  next()
})
// GOOD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

### 7. 元信息

定义路由的时候可以配置 `meta` 字段：

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

那么如何访问这个 `meta` 字段呢？

首先，我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录

例如，根据上面的路由配置，`/foo/bar` 这个 URL 将会匹配父路由记录以及子路由记录。

一个路由匹配到的所有路由记录会暴露为 `$route` 对象 (还有在导航守卫中的路由对象) 的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 `meta` 字段。

下面例子展示在全局导航守卫中检查元字段：

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

### 8. keep-alive&transition

因为<router-view>也是个组件，所以可以配合 `<transition>` 和 `<keep-alive>` 使用。如果两个结合一起用，要确保在内层使用 `<keep-alive>`

##### 8.1. [keep-alive](https://cn.vuejs.org/v2/api/#keep-alive)

- **Props**：

  - `include` - 字符串或正则表达式。只有名称匹配的组件会被缓存。
  - `exclude` - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
  - `max` - 数字。最多可以缓存多少组件实例。

- **用法**：

  `<keep-alive>` 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 `<transition>` 相似，`<keep-alive>` 是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在组件的父组件链中。

  当组件在 `<keep-alive>` 内被切换，它的 `activated` 和 `deactivated` 这两个生命周期钩子函数将会被对应执行。

  > 在 2.2.0 及其更高版本中，`activated` 和 `deactivated` 将会在 `<keep-alive>` 树内的所有嵌套组件中触发。

  主要用于保留组件状态或避免重新渲染。

  ```
  <!-- 基本 -->
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>
  
  <!-- 多个条件判断的子组件 -->
  <keep-alive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </keep-alive>
  
  <!-- 和 `<transition>` 一起使用 -->
  <transition>
    <keep-alive>
      <component :is="view"></component>
    </keep-alive>
  </transition>
  ```

  注意，`<keep-alive>` 是用在其一个直属的子组件被开关的情形。如果你在其中有 `v-for` 则不会工作。如果有上述的多个条件性的子元素，`<keep-alive>` 要求同时只有一个子元素被渲染。

##### 8.2. [transition](https://cn.vuejs.org/v2/api/#transition)

- **Props**：

  - `name` - string，用于自动生成 CSS 过渡类名。例如：`name: 'fade'` 将自动拓展为 `.fade-enter`，`.fade-enter-active` 等。默认类名为 `"v"`
  - `appear` - boolean，是否在初始渲染时使用过渡。默认为 `false`。
  - `css` - boolean，是否使用 CSS 过渡类。默认为 `true`。如果设置为 `false`，将只通过组件事件触发注册的 JavaScript 钩子。
  - `type` - string，指定过渡事件类型，侦听过渡何时结束。有效值为 `"transition"` 和 `"animation"`。默认 Vue.js 将自动检测出持续时间长的为过渡事件类型。
  - `mode` - string，控制离开/进入过渡的时间序列。有效的模式有 `"out-in"` 和 `"in-out"`；默认同时进行。
  - `duration` - number | { `enter`: number, `leave`: number } 指定过渡的持续时间。默认情况下，Vue 会等待过渡所在根元素的第一个 `transitionend` 或 `animationend` 事件。
  - `enter-class` - string
  - `leave-class` - string
  - `appear-class` - string
  - `enter-to-class` - string
  - `leave-to-class` - string
  - `appear-to-class` - string
  - `enter-active-class` - string
  - `leave-active-class` - string
  - `appear-active-class` - string

- **事件**：

  - `before-enter`
  - `before-leave`
  - `before-appear`
  - `enter`
  - `leave`
  - `appear`
  - `after-enter`
  - `after-leave`
  - `after-appear`
  - `enter-cancelled`
  - `leave-cancelled` (`v-show` only)
  - `appear-cancelled`

- **用法**：

  `<transition>` 元素作为**单个**元素/组件的过渡效果。`<transition>` 只会把过渡效果应用到其包裹的内容上，而不会额外渲染 DOM 元素，也不会出现在可被检查的组件层级中。

  ```
  <!-- 简单元素 -->
  <transition>
    <div v-if="ok">toggled content</div>
  </transition>
  
  <!-- 动态组件 -->
  <transition name="fade" mode="out-in" appear>
    <component :is="view"></component>
  </transition>
  
  <!-- 事件钩子 -->
  <div id="transition-demo">
    <transition @after-enter="transitionComplete">
      <div v-show="ok">toggled content</div>
    </transition>
  </div>
  ```

  ```
  new Vue({
    ...
    methods: {
      transitionComplete: function (el) {
        // 传入 'el' 这个 DOM 元素作为参数。
      }
    }
    ...
  }).$mount('#transition-demo')
  ```

- **参考**：[过渡：进入，离开和列表](https://cn.vuejs.org/v2/guide/transitions.html)

## 二、 Promise

What is Promise？Promise是异步编程的解决方案。JavaScript是单线程的，所以它无法同时做两件事。当用户在页面上向后台发送请求时，如果请求较慢，就会影响用户体验，这是就需要通过异步编程解决问题，也就是说，用户发送请求时，我们继续运行程序，等后台返回请求时，再执行相应的操作。

```javascript
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

### 1. Promise的三种状态

Promise的异步操作有三种状态：

- pending：等待状态，比如网络请求的过程
- fulfill：满足状态，当调用了resolve函数时，就处于该状态，并且会触发回调.then()
- reject：拒绝状态，当我们调用了reject函数时，就处于该状态，并且出发回调.catch()

### 2. Promise的链式调用

之前所写的是一种最基本的链式调用，接下来我们带上参数传递：

```js
    new Promise((resolve,reject) => {
      setTimeout(() => {
        console.log('aaa');
        resolve('aaa')
      }, 1000);
    }).then(data => {
      return new Promise((resolve,reject) => {
        resolve(data + '111')
      })
    }).then(data => {
      return new Promise(resolve=> {
        resolve(data + '222')
      }) 
    }).then(data => {
      console.log(data);
    })
```

再提供一种简洁写法：

```js
 new Promise((res,rej) => {
      setTimeout(() => {
        console.log('aaa');
        res('aaa')
      }, 1000);
    }).then(data => {
      return Promise.resolve(data + '111')
    }).then(data => {
      console.log(data);
    })
```

**这里会产生一个疑问**：链式调用 Promise 时，为什么需要用 `return new Promise` ，而不是直接 `new Promise`，这和 JS 的变量声明顺序有关。如果直接 `new` 的话，就不会等待上一个 Promise 运行完毕再进行这一次的 Promise，因为 JS 会直接将声明的变量初始化。

[从 promise 到底要不要加 return 开始 - 掘金 (juejin.cn)](https://juejin.cn/post/6879692911680684040)

### 3. Promise.all

Promise.all() 方法接收一个promise的iterable类型（注：Array，Map，Set都属于ES6的iterable类型）的输入，并且只返回一个Promise实例， 那个输入的所有promise的resolve回调的结果是一个数组。这个Promise的resolve回调执行是在所有输入的promise的resolve回调都结束，或者输入的iterable里没有promise了的时候。它的reject回调执行是，只要任何一个输入的promise的reject回调执行或者输入不合法的promise就会立即抛出错误，并且reject的是第一个抛出的错误信息。

```js
const promise1 = Promise.resolve(3);
const promise2 = 42;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
});

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
// expected output: Array [3, 42, "foo"]

```



## 三、 axios

### 1. axios基本使用

```js
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
axios({
  method: 'get',
  url: 'http://bit.ly/2mTM3nY',
  responseType: 'stream'
})
  .then(function (response) {
    response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
  });
```

### 2. axios并发请求

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```

### 3. axios全局配置

#### 全局的 axios 默认值

```
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

#### 自定义实例默认值

```
// Set config defaults when creating the instance
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// Alter defaults after instance has been created
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```

 

### 4. axios实例

#### 创建实例

可以使用自定义配置新建一个 axios 实例

##### axios.create([config])

```
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});

instance({
  url:'',
  params:{}
}).then(data => {
  console.dir(data)
})
```

### 5. axios拦截器

#### 拦截器

在请求或响应被 `then` 或 `catch` 处理前拦截它们。

```
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

如果你想在稍后移除拦截器，可以这样：

```
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

可以为自定义 axios 实例添加拦截器

```
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

## 四、 vuex

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。状态可以理解为数据，也就是说，当一些数据需要所有组件都共享时，就通过Vuex管理。比如用户的名称、头像、位置，或者商品的收藏、购物车等等，这些状态信息，我们都可以通过Vuex进行保存和管理，并且是响应式的。

Vuex是Vue适配的一个插件，使用插件以后，可以vuex添加到我们的vue-root的原型里，这样就可以全局调用它。

```js
// index.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

```js
// other Vue components
store.commit('increment')

console.log(store.state.count) // -> 1
```

### 1. State

Vuex只包含一个状态树，简言之，它只有一个state对象，不支持更多的state。Vuex内的所有数据都存放在state中。

### 2. Mutations

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 `increment` 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 **store.commit** 方法：

```js
store.commit('increment')
```

#### 提交载荷（Payload）

你可以向 `store.commit` 传入额外的参数，即 mutation 的 **载荷（payload）**：

```js
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}
store.commit('increment', 10)
```

在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
store.commit('increment', {
  amount: 10
})
```

#### 对象风格的提交方式

提交 mutation 的另一种方式是直接使用包含 `type` 属性的对象：

```js
store.commit({
  type: 'increment',
  amount: 10
})
```

当使用对象风格的提交方式，整个对象都作为载荷传给 mutation 函数，因此 handler 保持不变：

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

### 3. Getter

如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

Getter 接受 state 作为其第一个参数：

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

#### 通过属性访问

Getter 会暴露为 `store.getters` 对象，你可以以属性的形式访问这些值：

```js
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```

Getter 也可以接受 getter 作为第二个参数：

```js
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
store.getters.doneTodosCount // -> 1
```

我们可以很容易地在任何组件中使用它：

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

注意，getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的。

#### 通过方法访问

你也可以通过让 getter 返回一个函数，来实现给 getter 传参。在你对 store 里的数组进行查询时非常有用。

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

注意，getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。

### 4. Action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作，Mutation不能包含异步函数

让我们来注册一个简单的 action：

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
    increment (context) {
      context.commit('increment')
    }
  }
})
```

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。当我们在之后介绍到 [Modules](https://vuex.vuejs.org/zh/guide/modules.html) 时，你就知道 context 对象为什么不是 store 实例本身了。

实践中，我们会经常用到 ES2015 的 [参数解构 (opens new window)](https://github.com/lukehoban/es6features#destructuring)来简化代码（特别是我们需要调用 `commit` 很多次的时候）：

```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

#### 分发 Action

Action 通过 `store.dispatch` 方法触发：

```js
store.dispatch('increment')
```

乍一眼看上去感觉多此一举，我们直接分发 mutation 岂不更方便？实际上并非如此，还记得 **mutation 必须同步执行**这个限制么？Action 就不受约束！我们可以在 action 内部执行**异步**操作：

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

Actions 支持同样的载荷方式和对象方式进行分发：

```js
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

来看一个更加实际的购物车示例，涉及到**调用异步 API** 和**分发多重 mutation**：

```js
actions: {
  checkout ({ commit, state }, products) {
    // 把当前购物车的物品备份起来
    const savedCartItems = [...state.cart.added]
    // 发出结账请求，然后乐观地清空购物车
    commit(types.CHECKOUT_REQUEST)
    // 购物 API 接受一个成功回调和一个失败回调
    shop.buyProducts(
      products,
      // 成功操作
      () => commit(types.CHECKOUT_SUCCESS),
      // 失败操作
      () => commit(types.CHECKOUT_FAILURE, savedCartItems)
    )
  }
}
```

注意我们正在进行一系列的异步操作，并且通过提交 mutation 来记录 action 产生的副作用（即状态变更）。

#### 在组件中分发 Action

你在组件中使用 `this.$store.dispatch('xxx')` 分发 action，或者使用 `mapActions` 辅助函数将组件的 methods 映射为 `store.dispatch` 调用（需要先在根节点注入 `store`）：

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

### 5. Module

## 五、 vue全局api

### 1. Vue.set()

这里我定义了一个列表数据，我将通过三个不同的按钮来控制列表数据。
 调用方法：Vue.set( target, key, value )
 target：要更改的数据源(可以是对象或者数组)
 key：要更改的具体数据
 value ：重新赋的值

```html
<body>
<div id="app2">
    <p v-for="item in items" :key="item.id">
        {{item.message}}
    </p>
    <button class="btn" @click="btn2Click()">动态赋值</button><br/>    
    <button class="btn" @click="btn3Click()">为data新增属性</button>
</div>
<script src="../../dist/vue.min.js"></script>
<script>
var vm2=new Vue({
    el:"#app2",
    data:{
        items:[
            {message:"Test one",id:"1"},
            {message:"Test two",id:"2"},
            {message:"Test three",id:"3"}
        ]
    },
    methods:{
        btn2Click:function(){
            Vue.set(this.items,0,{message:"Change Test",id:'10'})
        },
        btn3Click:function(){
            var itemLen=this.items.length;
            Vue.set(this.items,itemLen,{message:"Test add attr",id:itemLen});
        }
    }
});
</script>
</body>
```

## 六、 实例api

### 1. $on

$on可以注册在任意vue实例上，可以是根元素，也可以是别的子组件。

$on监听某个函数，函数被调用后会触发回调，可以结合$emit使用。

**vm.$on( event, callback )**

- **参数**：

  - `{string | Array<string>} event` (数组只在 2.2.0+ 中支持)
  - `{Function} callback`

- **用法**：

  监听当前实例上的自定义事件。事件可以由 `vm.$emit` 触发。回调函数会接收所有传入事件触发函数的额外参数。

- **示例**：

  ```js
  vm.$on('test', function (msg) {
    console.log(msg)
  })
  vm.$emit('test', 'hi')
  // => "hi"
  ```

### 2. $once

**vm.$once( event, callback )**

- **参数**：

  - `{string} event`
  - `{Function} callback`

- **用法**：

  监听一个自定义事件，但是只触发一次。一旦触发之后，监听器就会被移除。

### 3. $off

vm.$off( [event, callback\] )

- **参数**：

  - `{string | Array<string>} event` (只在 2.2.2+ 支持数组)
  - `{Function} [callback]`

- **用法**：

  移除自定义事件监听器。

  - 如果没有提供参数，则移除所有的事件监听器；
  - 如果只提供了事件，则移除该事件所有的监听器；
  - 如果同时提供了事件与回调，则只移除这个回调的监听器。

### 4. $nextTick

**vm.$nextTick( [callback] )**

- **参数**：

  - `{Function} [callback]`

- **用法**：

  将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 `Vue.nextTick` 一样，不同的是回调的 `this` 自动绑定到调用它的实例上。nextTick所绑定的回调函数，会在Dom更新后执行。

