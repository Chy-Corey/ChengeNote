## less引入

### css不足之处

CSS非程序语言，没有变量、函数、作用域。

CSS书写时逻辑不清晰，冗余度高，所以非常不方便维护和复用。

### less介绍

Less是一门CSS扩展语言，作为CSS的一种形式的扩展，它并没有减少CSS的功能，而是在现有的CSS语法上，为CSS加入程序语言的特性。Less引入了变量、Mixin、运算和函数等功能，增加了CSS代码的逻辑性、可读性、可维护性。

### less安装

使用node安装: `npm install less -g`

检查是否安装成功：`lessc -v`

## less使用

### 变量

 命名规范：`@variable-name: value;`

- @作为前缀
- 大小写敏感

### 编译

vscode可配置easyLess插件，每次写完less文件，保存后会自动编译为css。

### 嵌套

css中的嵌套：

```css
#header .logo {
    width: 300px;
}
a:hover {
    color: pink;
}
```

less写法：

```less
#header {
    .logo {
        width: 300px;
    }
}
a {
	&:hover: pink;
}
```

### 运算

```less
@border: 5px + 5;
@test: 200px - 5;
@height: 82 * 50rem;
@width: 82 / 50rem;
```

1. 运算符左右两侧必须有一个空格隔开
2. 两个数参与运算如果只有一个有单位，结果以此单位为准
3. 如果两个数都有单位，就以前面一个数(左边的)的单为准