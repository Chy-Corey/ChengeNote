# Webpack学习

---

## 一、 Webpack简介

### 1. Webpack是什么

​	本质上，*webpack* 是一个现代 JavaScript 应用程序的*静态模块打包器(module bundler)*。当 webpack 处理应用程序时，它会递归地构建一个*依赖关系图(dependency graph)*，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 *bundle*。说白了就是打包前端JS代码的工具。

### 2. Webpack五个核心概念

1. Entry

   **入口起点(entry point)**指示 webpack 应该使用哪个模块，来作为构建其内部*依赖图*的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。每个依赖项随即被处理，最后输出到称之为 *bundles* 的文件中。

2. Output

   **output** 属性告诉 webpack 在哪里输出它所创建的 *bundles*，以及如何命名这些文件，默认值为 `./dist`。基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中。

3. Loader

   *loader* (加载器)让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效[模块](https://www.webpackjs.com/concepts/modules)，然后你就可以利用 webpack 的打包能力，对它们进行处理。

   本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。

4. Plugins

   loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。[插件接口](https://www.webpackjs.com/api/plugins)功能极其强大，可以用来处理各种各样的任务。

   想要使用一个插件，你只需要 `require()` 它，然后把它添加到 `plugins` 数组中。

5. Mode

   Mode(模式)指示Webpack使用相应模式的配置。有development模式和production模式。develop模式能让代码本地调试更加方便，production模式能让代码优化，用户使用更加友好。

## 二、Webpack初体验

### 1. 用`npm init`创建package.json

#### 1.1. 背景故事

> ```中文
> 在很久很久以前，dk 要开发一个前端项目，在计算机的某个旮沓地方建立了一个文件夹叫 dk_project，就称为这是一个“项目”了。
> 
> 又过了很久，dk 离开了公司，来了位新同事，在接手 dk 工作的时候发现计算机上面的 dk_project 文件夹，因为没有任何明显的
> 标识，就被当成普通文件夹给 DELETE 掉了。
> 
> 回到现代，随着 npm 的诞生，人们意识到建立一个项目目录不应该这么草率，于是乎规定，
> 如果某个文件夹被创建作为一个项目目录，那么它就应该包含一个 package.json 的文件。
> 
> package.json 文件里记录项目的描述信息：项目作者、项目描述、项目依赖哪些包、插件配置信息等等数不清的好处。
> ```

#### 1.2. 创建项目描述文件 package.json

如果是使用继承编辑器的话，我们直接在编辑器自带的终端里，cd到目标文件夹，然后`npm init`即可。

命令行里会以交互的形式让你填一些项目的介绍信息，依次介绍如下：（不知道怎么填的直接回车、回车...）

- name 项目名称
- version 项目的版本号
- description 项目的描述信息
- entry point 项目的入口文件
- test command 项目启动时脚本命令
- git repository 如果你有 Git 地址，可以将这个项目放到你的 Git 仓库里
- keywords 关键词
- author 作者叫啥
- license 项目要发行的时候需要的证书，平时玩玩忽略它

全部配置好以后，就会生成`package.json`文件，此时可以打开看看，里面都是项目描述。

### 2. 新建src文件夹和build文件夹

- src:  项目的源代码目录
- build: 打包输出的目录

### 3. 新建入口文件index.js

在src文件夹下新建`index.js`。我们可以在入口文件中书写js代码进行test。

### 4. 进行打包

运行webpack进行打包的指令：

 \*  开发环境：webpack '入口文件路径' -o '输出文件路径' --mode=development

 	这段代码的意思是： webpack会从入口文件开始打包，打包后输出到输出文件。并且整体打包环境是开发环境

 \*  生产环境：webpack '入口文件路径' -o '输出文件路径' --mode=production

 	这段代码打包环境是生产环境

生产环境和开发环境打包的区别就是生产环境打包后多一个压缩的js代码。

### 5. 测试打包后的项目

我们可以在build文件夹下新建index.html文件，然后引入打包后的built.js文件，打开到浏览器观察我们之前test的js代码是否生效。

## 三、 Webpack打包样式资源

​	打包js、json以外的代码时，我们需要调用loader，此时就需要配置`webpack.config.js`文件，作用为指示webpack做什么工作，当运行webpack指令时，会加载其中的配置。所有的前端构建工具都是基于node.js平台运行的，所以我们在写配置文件时，要使用commonJS语法。

```js
// webpack.config.js中的代码
const {resolve} = require('path') 
module.exports = {
  //webpack配置
  entry: './src/index.js',
  output: {
    filename: 'built.js',
    path: resolve(__dirname, 'build')
  },
  module: {
    rules: [
      //loader的配置
      {
        // 匹配哪些文件
        test: /\.css$/,
        // 使用哪些loader进行处理
        use: [
          // 创建style标签，将js中的样式资源插入进行，添加到head中生效
          'style-loader',
          // 将css文件变成commonjs模块加载到js中，里面的内容是样式字符串
          'css-loader'
        ]
      }
    ],
  },
  plugins: [

  ],
  mode: 'development'
}
```

写完配置后，下载所需要的loader ：npm i css-loader style-loader。

因为已经配置好了打包方式，所以打包时直接输入webpack即可。

打包完成后，可以新建index.html测试效果。

## 四、 打包其他资源

打包html、图片等资源，可以查阅webpack文档。



