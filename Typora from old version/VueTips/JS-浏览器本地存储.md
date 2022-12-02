# 浏览器本地存储

浏览器的本地存储，其实就是网页存储到本地的一些数据，比如搜索记录、浏览记录等。本地存储大致可分为LocalStorage和SessionStorage。

在HTML5之前，开发人员一般是通过使用Cookie在客户端保存一些简单的信息的。在HTML5发布后，提供了一种新的客户端本地保存数据的方法，那就是Web Storage，它也被分为：LocalStorage和SessionStorage，它允许通过JavaScript在Web浏览器中以键值对的形式保存数据。而相比Cookie有如下优点：

1. 拥有更大的存储容量，Cookie是4k，Web Storage为5M。
2. 操作数据相比Cookie更简单。
3. 不会随着每次请求发送到服务端。

## 使用

SessionStorage & LocalStorage 的所有api都相同

可以使用浏览器window对象访问SessionStorage和LocalStorage。请看下面的示例：

```js
sessionStorage = window.sessionStorage
localStorage = window.localStorage
```

以下是这两种存储类型可用的功能：

```js
//存储一个item
 storage.setItem('name', 'Alice')
 storage.setItem(``'age'``, ``'5'``)
//读取一个item
 storage.getItem('name') // returns "Alice"
//get存储对象长度
 storage.length // returns 2
//通过索引get对应的key名
 storage.key(0) // returns "name"
//移除一个item
 storage.removeItem('name')
//清空存储对象
 storage.clear()
```

## 区别

LocalStorage和SessionStorage之间的主要区别在于浏览器窗口和选项卡之间的数据共享方式不同。

LocalStorage可跨浏览器窗口和选项卡间共享。就是说如果在多个选项卡和窗口中打开了一个应用程序，而一旦在其中一个选项卡或窗口中更新了LocalStorage，则在所有其他选项卡和窗口中都会看到更新后的LocalStorage数据。

但是，SessionStorage数据独立于其他选项卡和窗口。如果同时打开了两个选项卡，其中一个更新了SessionStorage，则在其他选项卡和窗口中不会反映出来。举个例子：假设用户想要通过两个浏览器选项卡预订两个酒店房间。由于这是单独的会话数据，因此使用SessionStorage是酒店预订应用程序的理想选择。

 LocalStorage存储的数据在关闭浏览器后会继续保存，而SessionStorage不会保存。