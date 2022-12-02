# Vue 监测数据的原理

### 1. 监测对象

首先，我们来理解一下vue响应式的原理： 当我们在Vue实例下挂载data属性后，vue在渲染数据时会先对data内的数据进行一次处理，给他们添加上getter和setter，所以，每当data内的数据被修改时，都会回调setter，再触发vue的模板渲染，从而做到响应式。当然，vue会把对象的每一层级都检测到。

```js
const vm = new Vue({
  el: '#root',
  data: {
  	name: 'Chy'，
    friends: [{name: 'Siyu', age: 18}]
  }
})
```

### 2. Vue.set()

vue的响应式算法非常健壮，但也有没有覆盖之处，这里我写一个非常离谱的更改数据的方式：

```js
const vm = new Vue({
  el: '#root',
  data: {
    name: 'Chy'
  }
})
vm.data.sex = 'male' //不会触发响应式，不会添加setter和getter
vm.sex = 'male'  //不会触发响应式，不会添加setter和getter
```

在上面的两个写法里，不管是在data中添加属性，还是在vue实例下直接添加属性，都不会触发vue的数据监测。

当然，vue也想到了这种离谱的写法，为我们提供了一个api：`Vue.set(target, propertyName/index, value)`

**参数**：

- `{Object | Array} target` 要修改的对象或数组
- `{string | number} propertyName/index`对象的key或者数组的index
- `{any} value`值

以上是Vue的全局方法，也可以使用实例方法：`vm.$set(target, propertyName/index, value)`

### 3. 监测数组

Vue会给数组匹配setter和getter，但是不会为数组内的元素匹配，所以当我们通过索引值修改数组内的元素时，vue监测不到，不会触发响应式。Vue仅对js数组修改的api提供了响应式数据监测：`push pop shift unshift splice sort reverse`。

其实，vue对这几个方法进行了包装，每次调用它们时，会触发模板渲染。