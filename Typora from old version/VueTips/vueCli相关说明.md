# vueCli 相关说明

## 1. 脚手架结构

### 1.1. 最外层文件

#### .gitignore

​	git忽略文件

#### bable.config.js

​	bable的配置文件，bable官网可以参考配置详细写法

#### package.json

​	所安装包的说明书，node配置文件

#### package.lock.json

​	包版本控制文件

#### README.md

​	工程说明

### 1.2. src/assets文件夹

assets文件夹中，存放静态资源。

静态资源：我的理解是前端的固定页面，这里面包含HTML、CSS、JS、图片等等，不需要查数据库也不需要程序处理，直接就能够显示的页面。
具体形式为：客户端发送请求到web服务器，web服务器拿到对应的文件，返回给客户端，客户端解析并渲染出来。

动态资源：需要程序处理或者从数据库中读数据，能根据不同的条件在页面显示不同的数据，优点是内容更新不需要修改页面，缺点是访问速度不及静态页面。
具体形式为：客户端请求的动态资源，先把请求交给web的一个存储点，web存储点连接数据库，数据库处理数据之后，将数据交给web服务器，web服务器返回给客户端解析渲染处理。

区别：
1、静态资源一般都是设计好的html页面，而动态资源依靠设计好的程序来实现按照需求的动态响应或者从数据库中读数据；
2、静态资源的交互性差，不好更改，而动态资源可以根据需求获取内容；
3、在服务器的运行状态不同，静态资源不需要与数据库参于程序处理，动态资源需要一个或多个数据库的参与运算。

#### 1.3. public/index.html

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>
        We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without 					               JavaScript enabled. Please enable it to continue.
      </strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

这个文件就是vue程序的html页面，因为vue是单页面程序，所以只有一个index.html。它将id=“app”的组件渲染到此页面，是渲染组件的容器。

#### 1.4. src/main.js

我们来了解一下main.js中的`render`函数

```js
// 原始写法
render: function(createElement) {// 参数为组件名，返回VNode（即：虚拟节点）
  return createElement(App)
}
// 简写
render(createElement) {
  return createElement(App)
}
// 写成箭头函数
render: h => h(App)
```

## 2. 一些主要的默认配置即修改

Vue脚手架隐藏了所有webpack配置，如果想查阅具体的webpack配置，请执行命令: `vue inspect > output.js`

此命令只是输出配置属性给人查阅，如果要修改配置，需要手动配置`vue.config.js`文件

**vue.config.js**

`vue.config.js` 是一个可选的配置文件，如果项目的 (和 `package.json` 同级的) 根目录中存在这个文件，那么它会被 `@vue/cli-service` 自动加载。这个文件应该导出一个包含了选项的对象：

```js
module.exports = {
  // options...
}
```

**还有一个注意点：每次修改vue.config.js，都需要重新启动npm run serve，才能加载配置**

#### 1. publicPath

默认情况下，Vue CLI 会假设你的应用是被部署在一个域名的根路径上，例如 `https://www.my-app.com/`。如果应用被部署在一个子路径上，你就需要用这个选项指定这个子路径。例如，如果你的应用被部署在 `https://www.my-app.com/my-app/`，则设置 `publicPath` 为 `/my-app/`。

#### 2. pages

在 multi-page 模式下构建应用。每个“page”应该有一个对应的 JavaScript 入口文件。其值应该是一个对象，对象的 key 是入口的名字，value 是：

- 一个指定了 `entry`, `template`, `filename`, `title` 和 `chunks` 的对象 (除了 `entry` 之外都是可选的)；
- 或一个指定其 `entry` 的字符串。

```js
module.exports = {
  pages: {
    index: {
      // page 的入口
      entry: 'src/index/main.js',
      // 模板来源
      template: 'public/index.html',
      // 在 dist/index.html 的输出
      filename: 'index.html',
      // 当使用 title 选项时，
      // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
      title: 'Index Page',
      // 在这个页面中包含的块，默认情况下会包含
      // 提取出来的通用 chunk 和 vendor chunk。
      chunks: ['chunk-vendors', 'chunk-common', 'index']
    },
    // 当使用只有入口的字符串格式时，
    // 模板会被推导为 `public/subpage.html`
    // 并且如果找不到的话，就回退到 `public/index.html`。
    // 输出文件名会被推导为 `subpage.html`。
    subpage: 'src/subpage/main.js'
  }
}
```

#### 3. lintOnSave

是否在开发环境下通过 [eslint-loader](https://github.com/webpack-contrib/eslint-loader) 在每次保存时 lint 代码。这个值会在 [`@vue/cli-plugin-eslint`](https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint) 被安装之后生效。

设置为 `true` 或 `'warning'` 时，`eslint-loader` 会将 lint 错误输出为编译警告。默认情况下，警告仅仅会被输出到命令行，且不会使得编译失败。

如果你希望让 lint 错误在开发时直接显示在浏览器中，你可以使用 `lintOnSave: 'default'`。这会强制 `eslint-loader` 将 lint 错误输出为编译错误，同时也意味着 lint 错误将会导致编译失败。

设置为 `error` 将会使得 `eslint-loader` 把 lint 警告也输出为编译错误，这意味着 lint 警告将会导致编译失败。