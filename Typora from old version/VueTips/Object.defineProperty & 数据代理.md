# Object.defineProperty & 数据代理

---

### Object.defineProperty

此方法用于给对象定义属性

```js
let number = 18
let person = {
    name: 'Chy',
    sex: 'Male'
}
Object.defineProperty(person, 'age', {
    value: 18,
    enumerable: ture,  // 控制添加的属性是否可以枚举，默认为false
    writable: true,  // 控制添加的属性是否可写，默认为false
    configurable: true,  // 控制添加的属性是否可删除，默认为false
    // 以上为difineProperty的basic option
    get() {  // 当读取person的age属性时，getter被调用，且age的值是getter的返回值
        return number
    },
    set(value) {  // 当修改person的age属性时，setter被调用，且会收到修改的值作为参数
        number = value  // 实现number和age双向绑定
    }
})
```

### 数据代理

数据代理： 通过一个对象代理对另一个对象的操作（读/写）

```javascript
// 实现最简单的数据代理
let obj1 = {x: 100}
let obj2 = {y: 200}
Object.defineProperty(obj2, 'x', {
    get() {
        return obj1.x
    },
    set(value) {
        obj1.x = value
    }
})
```

### vue中的数据代理

Vue 实例的数据对象。Vue 会递归地把 data 的 property 转换为 getter/setter，从而让 data 的 property 能够响应数据变化。**对象必须是纯粹的对象 (含有零个或多个的 key/value 对)**：浏览器 API 创建的原生对象，原型上的 property 会被忽略。大概来说，data 应该只能是数据 - 不推荐观察拥有状态行为的对象。

一旦观察过，你就无法在根数据对象上添加响应式 property。因此推荐在创建实例之前，就声明所有的根级响应式 property。

实例创建之后，可以通过 `vm.$data` 访问原始数据对象。Vue 实例也代理了 data 对象上所有的 property，因此访问 `vm.a` 等价于访问 `vm.$data.a`。