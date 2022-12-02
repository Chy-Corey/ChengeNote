### 媒体查询

​	Media Query 是css3新语法，使用@media查询，可以针对不同媒体类型定义不同的样式，也就是根据不同的屏幕尺寸设置不同的样式。比如针对phone、pad、pc适配不同的样式。

#### 语法规范

```css
@media mediatype and|not|only (media feature) {
    css-code
}
```

1. mediatype 查询类型

   将不同的终端设备划分成不同的类型，称作媒体类型

2. 关键字

   and: 将多个媒体特性连接到一起

   not: 排除某个媒体特性

   only: 指定特定媒体

3. mediafeature媒体特性

   每种媒体类型都具有各自不同的特性，根据不同媒体设置不同的展示风格。

### 媒体查询引入资源

​	当样式比较繁多的时候，我们可以针对不同的媒体使用不同的css文件。原理就是直接在link中判断设备，然后引入不同的css。

#### 语法规范

```html
<link rel="stylesheet" media="mediatype and|not|only (mediafeature)" href="xxx.css">
```



