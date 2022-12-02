# Plugins In Vue

首先我们来了解插件的本质：

在vue中，插件的本质其实就是一个对象。

插件通常用来为 Vue 添加全局功能。插件的功能范围没有严格的限制，一般有下面几种：

1. 添加全局方法或者 property。如：[vue-custom-element](https://github.com/karol-f/vue-custom-element)
2. 添加全局资源：指令/过滤器/过渡等。如 [vue-touch](https://github.com/vuejs/vue-touch)
3. 通过全局混入来添加一些组件选项。如 [vue-router](https://github.com/vuejs/vue-router)
4. 添加 Vue 实例方法，通过把它们添加到 `Vue.prototype` 上实现。
5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 [vue-router](https://github.com/vuejs/vue-router)

## 1. 插件使用

我们可以借鉴以下vue自带的插件vue-router：

通过全局方法 `Vue.use()` 使用插件。它需要在你调用 `new Vue()` 启动应用之前完成：

```js
// 调用 VueRouter.install(Vue)
import VueRouter from 'vue-router'
Vue.use(VueRouter)

new Vue({
  // ...组件选项
})
```

接下来，我们就可以写一个自己的插件。在vue中，任何插件都要有一个install函数，函数的第一个参数是Vue构造函数，第二个以后的参数是插件使用者传递的数据。

```js
// 调用插件并传参
Vue.use(myPlugin,x,y,z)

// 创建插件
export let myPlugin = {
  install(Vue,x,y,z) {
    console.log(Vue);
    console.log(x,y,z);
  }
}
```

**插件既然传入了Vue构造函数，那么所有的全局api都可以被插件使用**：

- [Vue.extend](https://cn.vuejs.org/v2/api/#Vue-extend)
- [Vue.nextTick](https://cn.vuejs.org/v2/api/#Vue-nextTick)
- [Vue.set](https://cn.vuejs.org/v2/api/#Vue-set)
- [Vue.delete](https://cn.vuejs.org/v2/api/#Vue-delete)
- [Vue.directive](https://cn.vuejs.org/v2/api/#Vue-directive)
- [Vue.filter](https://cn.vuejs.org/v2/api/#Vue-filter)
- [Vue.component](https://cn.vuejs.org/v2/api/#Vue-component)
- [Vue.use](https://cn.vuejs.org/v2/api/#Vue-use)
- [Vue.mixin](https://cn.vuejs.org/v2/api/#Vue-mixin)
- [Vue.compile](https://cn.vuejs.org/v2/api/#Vue-compile)
- [Vue.observable](https://cn.vuejs.org/v2/api/#Vue-observable)
- [Vue.version](https://cn.vuejs.org/v2/api/#Vue-version)

这些api我们可以任意定义在插件中，当我们调用插件后，就可以使用这些功能。

```js
// d
export let Plugin1 = {
  install(Vue) {
    console.log(Vue,'你使用了Plugin1');
    Vue.filter('my-filter',function(value) {
      return value * 10;
    })
  }
}
```

