# Vue基础学习

​	Vue 是最方便的前端框架，用就完事儿了！并且其生态也在逐步拓宽，有非常好的前景，而且是中国人开发的，对国猿比较友好，是中国的骄傲！

---

## 一、初步认识vue

### 1.  vue.js 安装

下载和引入：

1. 去官网下载vue.js的js框架，并由<script>标签引入：[安装 — Vue.js (vuejs.org)](https://cn.vuejs.org/v2/guide/installation.html)
2. 直接用npm引入：**npm install vue**（这个需配合脚手架使用）

刚开始学习时，我们使用第一种方法，下载js文件并引入到html文档中即可。

### 2.  helloworld

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>helloworld-vue</title>
</head>
<body>
  <script src="../vueFrame/vue.js"></script>
  <div>
    <p id="app">{{message}}</p> //将message渲染
  </div>

<script>
  const helloworld = new Vue({
    el: '#app', // 用于挂在要管理的元素
    data: { //元素的数据
      message: "helloworld"
    }
  })
</script>
</body>
```

我要好好解释这块代码：

​	首先是用script标签引入vue.js文件，这里直接用路径引入，这其实就是把vue.js里的代码全部 ctrl C，再 ctrl V 到这个html文件的script代码区里。

​	那么，我们就可以理解下面的“const app = new Vue()”了。这其实是调用了vue.js源码里的构造函数，来构造一个新的Vue对象，这个对象里面要有el(Element 元素)，这相当于Vue对象的钥匙。我们在html标签里挂上id="app"，那么这个Vue对象里的元素，就会挂载到这个标签上，这个标签就会到app所在的对象寻找数据。所以，上述代码中的message就会对应到对象中的message。

​	vue的编程范式是声明式编程，我们只要写出想要的结果就可以达到目标，不需要写清一步步过程。

### 3. helloworld进阶

​	来做一个列表和计数器。这里会用到v-for这个attribute（属性），也就是vue框架提供的功能。

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    <ul>
      <li v-for="item in animes">{{item}}</li>
      <li>{{ counter }}</li>
    </ul>
    <button @click="count(1)">+</button>
    <button @click="count(-1)">-</button>
  </div>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        counter: 0,
        animes: ['Your Lie in April','Fate stay night','K-ON']
      },
      methods: {
        count(ADDorSUB) {
          if(ADDorSUB==1) {
            this.counter++
          } else if(ADDorSUB==-1) {
            this.counter--
          }
        }
      }
    })
  </script>
</body>
```

## 二、vue运行模式

### 1. vue的MVVM

​	MVVM是Model-View-Viewmodel的缩写，它是一种软件架构模式。

- **View层**：视图展示。包含UIView以及UIViewController，View层是可以持有ViewModel的。相当于浏览器的dom。
- **ViewModel层**：视图适配器。暴露属性与View元素显示内容或者元素状态一一对应。一般情况下ViewModel暴露的属性建议是readOnly的，至于为什么，我们在实战中会去解释。还有一点，ViewModel层是可以持有Model的。是视图模型(View)和数据(Model)沟通的桥梁。
- **Model层**：数据模型与持久化抽象模型。数据模型很好理解，就是从服务器拉回来的JSON数据。而持久化抽象模型暂时放在Model层，是因为MVVM诞生之初就没有对这块进行很细致的描述。按照经验，我们通常把数据库、文件操作封装成Model，并对外提供操作接口。（有些公司把数据存取操作单拎出来一层，称之为**DataAdapter层**，所以在业内会有很多MVVM的变种，但其本质上都是MVVM）。
- **Binder**：MVVM的灵魂。可惜在MVVM这几个英文单词中并没有它的一席之地，它的最主要作用是在View和ViewModel之间做了双向数据绑定。如果MVVM没有Binder，那么它与MVC的差异不是很大。

我们发现，正是因为View、ViewModel以及Model间的清晰的持有关系，所以在三个模块间的数据流转有了很好的控制。

### 2. vue的options(选项)

​	当我们使用构造函数建立vue对象时，作为参数的对象里有很多属性。我们可以到vue官网的api里寻找。

​	[API | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/api/)

### 3. vue的生命周期

​	所谓的生命周期，其实就是一个vue实例从被构造到被释放内存的过程。Vue框架帮我们搭建了一个生命周期模型，并对应生命周期中的时间节点创建了对应的回调函数（就是vue实例达到这些时间节点时，系统自动调用这些函数）。

<img src="E:\Storage\博客\Typora\image\vue基础学习\vue生命周期\lifecycle.png" alt="生命周期及回调函数" style="zoom: 33%;" />

​	

比如我们想在vue实例建立好之后，提示“用户已登陆”：

``` html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">你好</div>
  <script>
    const app = new Vue({
      el: "#app",
      created() {
        console.log("User logged in！");
        alert("User logged in!")
      }
    })
  </script>
</body>
```

## 三、 vue的基本语法

### 1. mustache语法

​	为了将vue实例中date内的数据传递到html中，vue封装了插值操作，这个操作的语法叫mustache语法，因为两个大括号( {{}} )像胡须一样。使用时，我们只要用双大括号把变量括在里面就行了。

```html
{{ message from data}}
```

### 2. 插值操作的修饰符

- v-once

  使用此命令后，渲染dom元素时，只会绑定一次数据，之后数据改变时，显示不会改变。

  ```html
  <body>
      <script src="../vueFrame/vue.js"></script>
      <div id="app">
        <p v-once>{{ name }}</p>
        <p>{{ name }}</p>
        <button @click="flip()">return</button>
      </div>
      <script>
        const app = new Vue({
          el: "#app",
          data() {
            return {
              name: "chy",
              age: 18
            }
          },
          methods: {
            flip() {
              //this.name.split("").reverse().join("")
              const trans = this.name
              this.name = trans.split("").reverse().join("")
            }
          }
        })
      </script>
    </body>
  ```

- v-html

  如果有时从服务器请求到的数据就是一串HTML代码，可以使用v-html来解析代码，显示出正常的文本。
  
  ```html
  <body>
    <script src="../vueFrame/vue.js"></script>
    <div id="app">
      <p v-html="link"></p>
      <p v-html="baidu"></p>
    </div>
    <script>
      const app = new Vue({
        el: "#app",
        data() {
          return {
            link: '<a href="http://101.34.51.13/">煜宝的博客</a>',
            baidu:'<a href="http://baidu.com">百度</a>'
          }
        },
      })
    </script>
  </body>
  ```
  
  
  
- v-pre

  v-pre用于跳过这个元素的编译过程，直接显示代码的纯文本。

  ```html
  <body>
    <script src="../vueFrame/vue.js"></script>
    <div id="app">
      <p v-pre>{{name}}</p>
      <p>{{name}}</p>
    </div>
    <script>
      const app = new Vue({
        el: "#app",
        data() {
            return {
              name: "chy",
            }
          },
      })
    </script>
  </body>
  ```

- v-cloak

  cloak: 斗篷

  在某些情况下，我们浏览器可能会直接显示出为编译的mustache标签。举个例子，当用户打开网页时，网页突然卡了，那么可能没能解析mustache语法就直接展示出了原始代码。这时，就可以使用 v-cloak，当代码还未被解析时，先隐藏起来，等解析后再展示。

  ```html
  <p v-cloak>{{name}}</p>
  ```

### 3. v-bind

​	mustache语法主要的功能是绑定文本内容，但是我们也会需要动态绑定一些属性，比如a的href或者img的src。此时，我们可以使用 v-bind(语法糖为：)

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    <img :src="imgURL" alt="sweety"/>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data() {
        return {
          imgURL: "http://101.34.51.13/usr/uploads/2021/07/3853554592.jpg#vwid=1280&vhei=720"
        }
      }
    })
  </script>
</body>
```

#### 3.1. v-bind动态绑定class

​	v-bind动态绑定class时，有可能需要动态绑定好几个不同的class，vue支持对象语法：把所有要绑定的class放到对象里，类名作为键，键值为布尔值，如果布尔值为true，则将此类绑定到元素；如果为flase，则不绑定。

```html
<body>
  <style>
    .active {
      font-size: 100px;
    }
    .pink {
      color: pink;
    }
  </style>
  <script src="../vueFrame/vue.js"></script>
  <div id="app" :class=>
    <h2 :class="{active: isActive, pink: isPink}">{{message}}</h2>
    <h2 :class="getclasses()">{{message}}</h2>
    <button @click="activeClick()">big</button>
    <button @click="pinkClick()">pink</button>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data() {
        return {
          message: "Chy❤Ysy",
          isActive: false,
          isPink: false
        }
      },
      methods: {
        activeClick() {
          this.isActive = !this.isActive
        },
        pinkClick() {
          this.isPink = !this.isPink
        },
        getclasses() {
          return  {active: isActive, pink: isPink}   
        }
      }
    })
  </script>
</body>
```

#### 3.2. v-bind 和 v-for 结合

#### 3.3. v-bind动态绑定style

​	通过对象的方式动态绑定style，写css属性名时，记得使用驼峰法。但是如果使用短横线分隔，需要加单引号。

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    <ul>
      <li v-for="(item,index) in animes" :style="getStyles()">{{item}}</li>
    </ul>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data() {
        return {
          animes: ['Your Lie in April','Fate stay night','K-ON'],
          finalColor: '#666'
        }
      },
      methods: {
        getStyles() {
          return {fontSize: '18px', color: this.finalColor}  // 值不是变量就要加引号
          
        }
      }
    })
  </script>
</body>
```

### 4. computed计算属性

计算属性：可以理解为能够在里面写一些计算逻辑的属性。
作用：1）减少模板中的计算逻辑
			2）数据缓存。当我们的数据没有变化时，不在执行计算的过程
			3）依赖固定的数据类型（响应式数据），不能是普通的传入的一个全局数据

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    <p>{{fullName}}</p>
    <p>{{totalWages}}</p>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data() {
        return {
          husband: "Chy",
          wife: "Ysy",
          wages: [100000,120000]
        }
      },
      computed: {
        fullName() {
          return this.husband + '❤' + this.wife
        },
        totalWages() {
          let result = 0
          for(let i=0;i<this.wages.length;i++) {
            result += this.wages[i]
          }
          return result
        }
      }
    })
  </script>
</body>
```

#### 4.1. 计算属性的setter和getter

​	上述的computed其实是简写，只写了get方法(获取数据并保存为计算属性)，其实computed还有set方法，就是当computed的值被修改时，系统会回调set方法。我们要注意，修改计算属性并不会改变渲染出的值，因为只修改计算属性时，data里的数据没有修改，那么get方法得到的值也就不会被修改，我们要通过回调的set方法获取到修改的值，然后对应地修改data，这样计算属性会监听到data值修改，从而调用get方法，改变渲染。

**如果计算属性会被修改，那么必须要有setter，如果没有setter则不允许修改**

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    <p>{{fullName}}</p>
    <button @click="transValue()">change</button>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data() {
        return {
          husband: "Chy",
          wife: "Ysy",
          wages: [100000,120000]
        }
      },
      computed: {
        fullName: {
          get() {
            return this.husband + '❤' + this.wife
          },
          set(newValue) {
            console.log('-----',newValue,'-----');
            const names = newValue.split('❤')
            this.husband = names[0]
            this.wife = names[1]
          }
        },
      },
      methods: {
        transValue() {
          this.fullName = "CHY❤YSY"
        }
      }
    })
  </script>
</body>
```

#### 4.2. computed和methods的对比

- 用 computed 属性方法编写的逻辑运算，在调用时直接将返回值视为一个属性，并缓存到系统内。之后再次调用计算属性时，就不会进行运算，而是直接从缓存中抽取。直到计算结果发生改变时，再次调用get方法。

- 用 methods 方法编写的逻辑运算，每次调用都会进行一次函数运算，并没有缓存。

  所以，当数值经常改变时，可以使用methods，而不常改变，只是做一些复杂逻辑运算时，可以使用computed。

---

### 5. v-on事件监听

​	前端需要和用户的操作交互，比如点击、拖曳、键盘事件等，vue提供了v-on(缩写：@)这个aattribute，用于绑定事件监听器。

#### 5.1. v-on的参数问题

​	在事件定义时，如果方法本身需要传入参数，但是调用时连括号都没有写，这时vue会将浏览器产生的event对象传到方法里。

​	在方法定义时，如果既需要event对象，又需要别的参数。此时只需要写成$event，vue就会帮我们解析这个变量，并把监听的事件赋值给它。

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    <button @click="btnClick">111</button>
    <button @click="btnClick('chy')">222</button>
    <button @click="btnClick('chy',)">333</button>
    <button @click="btnClick('chy',$event)">444</button>
  </div>
  
  <script>
    const app = new Vue({
      el: "#app",
      methods: {
        btnClick(name, event) {
          console.log(name,event);
        }
      }
    })Redis
  </script>
</body>
```

#### 5.2. v-on的修饰符

​	在某些情况况下，我们拿到$event的目的可能是进行一些事件处理。Vue提供了修饰符来帮助我们方便的处理一些事件：

- .stop - 调用event.stopPropagation()	阻止事件冒泡

  某些时刻，当我们点击button时，button所在的div也会监听到点击，这是事件冒泡造成的。如果要阻止这个行为，就要使用.stop

- .prevent - 调用event.preventDefault()    阻止默认事件 

  某些时刻，html标签会有一些默认行为，比如点击了表单中的submit按钮，就会自动提交，如果要阻止自动的默认行为，就要使用.prevent

- .{keyCode | keyAlias} - 只当事件是从特定键触发时才触发回调

  某些时刻，我们需要监听特定键盘上按键的行为，比如监听回车键的按下或上弹事件，这时可以使用此修饰符。

- .native - 监听组件根元素的原生事件

  组件使用比较多，学到组件再讲

- .once - 只触发一次回调，即只起一次作用

- .capture - 事件捕获模式

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
  <div @click="divClick">
    <!-- .stop -->
    <p>K-ON</p>
    <button @click="btnClick">Best</button>
    <button @click.stop="btnClick">Stop</button>
    <HR width=100% color=#987cb9 SIZE=1>  <!-- 分割线 -->
  </div>
  <div>
    <!-- .prevent -->
    <form action="../vueTest/test.txt" method="POST">
      <input type="text" name="userName">
      <input type = "submit" value="submit" @click.prevent=btnClick>
    </form>
    <HR width=100% color=#987cb9 SIZE=1>  <!-- 分割线 -->
    <!-- .{keyCode | keyAlias} -->
    <input type="text" @keyup.enter.65="keyUp">  <!-- 65是A的键值，enter是回车键的别名 -->
    <HR width=100% color=#987cb9 SIZE=1>  <!-- 分割线 -->
    <!-- .once -->
    <button @click.once="btnClick">只能点一次有作用</button>  
  </div>   
  </div> 
  <script>
    const app = new Vue({
      el: "#app",
      methods: {
        btnClick() {
          console.log('btnClick');
        },
        divClick() {
          console.log('divClick');
        },
        keyUp() {
          console.log('keyUp');
        }
      }
    })
  </script>
</body>
```

### 6. v-fi & v-else & v-else-if

​	当我们需要根据条件来决定展示不同的元素时(比如根据分数所在的分数段来显示优秀、良好、及格)

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    score:<p v-if="score>=90">优秀</p>
    <p v-else-if="score>=80">良好</p>
    <p v-else-if="score>=60">及格</p>
    <p v-else>不及格</p>
  </div>

<script>
  const helloworld = new Vue({
    el: '#app', 
    data: {
      score: 90
    }
  })
</script>
</body>
```

其实我们一般不使用v-if，这种逻辑判断，可以交给computed来完成，这样更简洁，更模块化。

#### 登陆切换小案例

​	切换登录方式时，如果我们已经输入了一些数字，那么vue会帮我们复用这个输入框以提高效率。如果要让vue不复用，可以给两个输入框添加不同的key，vue在判断是否服用时，就是根据它们的key是否相同从而决定是否复用。

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    <span v-if="isUser">
      <label for="userName">用户账号</label>
      <input type="text" id="userName" placeholder="账号" key="userName">
    </span>
    <span v-else>
      <label for="userEmail">用户邮箱</label>
      <input type="text" id="userEmail" placeholder="邮箱" key="userEmail">
    </span>
    <button @click="isUser = !isUser">切换登陆方式</button>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data() {
        return {
          isUser: true,
        }
      }
    })
  </script>
</body>
```

### 7. v-show

v-show的用法和v-if的用法差不多，并且也用于决定元素是否渲染。

- v-if当条件为false时，会直接删除对应元素，使其从dom移除。
- v-show当条件为false时，会将元素的display设置为none，但dom中依然有此元素。

开发中的选择：

- 当需要不停地切换显示和隐藏时，使用v-show
- 当只有一次切换时，使用v-if

```html
<body>
  <script src="../vueFrame/vue.js"></script>
  <div id="app">
    <button @click="isShow=!isShow">切换</button>
    <h2 v-if="isShow" id="if">{{message}}</h2>
    <h2 v-show="isShow" id="show">{{message}}</h2>
  </div>
  <script>
    const app = new Vue({
      el: "#app",
      data() {
        return {
          message: "NIHAO",
          isShow: true
        }
      }
    })
  </script>
</body>
```

### 8. v-for

当存在一组数据(对象或数组)需要渲染时，可以使用v-for来循环遍历。v-for可以获取元素的值，也可以获取元素的索引(index)。

```html
<body>
  <div id="app">
    <ul>
      <li v-for="(item,index) in animes">{{index+1}} {{item}}</li>
    </ul>
    <ul>
      <!-- 对象也可以拥有索引，不过不怎么使用 -->
      <li v-for="(value,key,index) in info">{{value}} -- {{key}}</li>   
    </ul>
  </div>

  <script>
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          animes: ['K-ON','Lie in April','Fate','Death-Note'],
          info: {
            name: "Chy",
            wife: "Ysy",
            age: 18
          }
        }
      },
      methods: {}
    });
  </script>
</body>
```

#### v-for绑定key

​	vue官方推荐我们在使用v-for时，给对应的元素添加key属性。这里举个例子说明为什么需要：

​	当不绑定key时，dom的一个层级有很多不同的元素，这些元素都对应了系统默认的节点。当我们向中间插入一个元素时，此时新元素就会占据原来的元素的位置，接着后面的元素依次向后迭代移动。这样效率非常低。所以，我们可以给每个元素节点绑定一个不同的key，这样插入新元素时，新元素直接插入，且对应的节点不会迭代挪动，效率大大提高。

```html
<body>
  <div id="app">
    <ul>
      <!-- 这里key绑定index没用，因为插入元素时，index也会随之迭代改变 -->
      <li v-for="item in letters" :key="item">{{item}}</li>
    </ul>
  </div>

  <script>
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          letters: ['A','B','C','D','E']
        }
      },
      methods: {}
    });
  </script>
</body>
```

#### 列表过滤

使用filter方法

### 9. v-model双向绑定

表单控件在实际开发中非常常见，比如用户信息提交，登陆注册等等。

Vue使用v-model实现数据双向绑定。

`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。

#### 9.1. v-model基本使用

```html
<body>
  <div id="app">
    <input type="text" v-model="message">
    {{message}}
  </div>

  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        message: "Chy"
      },
      methods: {}
    });
  </script>
</body>
```

#### 9.2. v-model的本质

其实是把两个绑定封装成了一个v-model。先用v-bind把输入框的value绑定到data的数据中，再监听input事件，把data中的数据绑定到输入框的value。

```html
<body>
  <div id="app">
    <input type="text" :value="message" @input="valueChange($event)">
    <h2>{{message}}</h2>
  </div>

  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        message: "Hello!"
      },
      methods: {
        valueChange(event) {
          this.message = event.target.value
          /console.log(event);
        }
      }
    });
  </script>
</body>
```

#### 9.3. v-model结合radio

v-model绑定的是data值和radio中的value

```html
<body>
  <div id="app">
    <label for="male">
      <input type="radio" id="male" name="sex" value="男" v-model="sex">男</input>
    </label>
    <label for="female">
      <input type="radio" id="female" name="sex" value="女" v-model="sex">女</input>
    </label>
    {{sex}}
  </div>

  <script>
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          sex: "女"
        }
      },
      methods: {}
    });
  </script>
</body>
```

#### 9.4. v-model结合checkbox

当只有一个checkbox时，用于判断是否选中，绑定布尔值。有多个checkbox时，判断选了哪些，绑定一个数组。

```html
<body>
  <div id="app">
    <label for="license">
      <input type="checkbox" id="license" v-model="license" value="license">同意协议
    </label>
    {{license}}
    <button :disabled="!license">下一步</button>
    <br>
    <label for="hobbies">
      <input type="checkbox" id="hobbies" value="篮球" v-model="hobbies">篮球
      <input type="checkbox" id="hobbies" value="足球" v-model="hobbies">足球
      <input type="checkbox" id="hobbies" value="羽毛球" v-model="hobbies">羽毛球
      <input type="checkbox" id="hobbies" value="乒乓球" v-model="hobbies">乒乓球
      <input type="checkbox" id="hobbies" value="网球" v-model="hobbies">网球
    </label>
    {{hobbies}}
  </div>

  <script>
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          license: false,
          hobbies:[]
        }
      },
      methods: {}
    });
  </script>
</body>
```

#### 9.5. v-model结合select

```html
<body>
  <div id="app">
    <!-- 单选 -->
    <select name="fruits" v-model="fruit">
      <option value="西瓜">西瓜</option>
      <option value="柠檬">柠檬</option>
      <option value="草莓">草莓</option>
      <option value="橙子">橙子</option>
    </select>
    <p>What you choose is {{fruit}}</p> 
    <!-- 多选 注意多选时要按住ctrl -->
    <select name="balls" v-model="balls" multiple>
      <option value="篮球">篮球</option>
      <option value="足球">足球</option>
      <option value="乒乓球">乒乓球</option>
      <option value="羽毛球">羽毛球</option>
    </select>
    <p>What you choose are {{balls}}</p>
  </div>

  <script>
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          fruit: '西瓜',
          balls: []
        }
      },
      methods: {}
    });
  </script>
</body>
```

#### 9.6. v-model的修饰符

- .lazy

  取代 `input` 监听 `change` 事件

- .number

   输入字符串转为有效的数字

- .trim

  输入首尾空格过滤

```html
<body>
  <div id="app">
    <input type="text" v-model.lazy="message">
    <h3>{{message}}</h3> 
    <HR width=100% color=#987cb9 SIZE=1>  <!-- 分割线 -->
    <input type="number" v-model.number="age">
    <h3>{{age}}--{{typeof age}}</h3>
    <HR width=100% color=#987cb9 SIZE=1>  <!-- 分割线 -->
    <input type="text" v-model.trim="name">
    <h3>{{name}}</h3>  
  </div>

  <script>
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          message: '',
          age: undefined,
          name: ''
        }
      },
      methods: {}
    });
  </script>
</body>
```

#### 9.7. v-model收集表单数据

```html
<body>
  <script src="../../vueFrame/vue.js"></script>
  <div id="app">
    <form>
      账号：<input type="text" v-model="account"><br/>
      密码：<input type="password" v-model="password"><br/><br/>
      性别：男<input type="radio" name="sex" value="male" v-model="sex"> 女<input type="radio" name="sex" value="female" v-model="sex"><br/><br/>
      爱好：
      学习<input type="checkbox" value="study" v-model="hobbies">
      吃饭<input type="checkbox" value="eat" v-model="hobbies">
      打游戏<input type="checkbox" value="game" v-model="hobbies"><br/><br/>
      所属校区
      <select v-model="city">
        <option value="">请选择校区</option>
        <option value="tongji">同济医学院</option>
        <option value="yunyuan">韵苑</option>
        <option value="zisong">紫菘</option>
      </select><br/><br/>
      其他信息：
      <textarea v-model="other"></textarea><br/>
      <input type="checkbox" v-model="agree">阅读并接受<a href="#">《用户协议》</a>
      <button @click.prevent="submit">提交</button>
    </form>
  </div>

<script>
  const vm = new Vue({
    el: '#app', // 用于挂在要管理的元素
    data: { //元素的数据
      account: '',
      password: '',
      sex: '',
      hobbies:[],
      city: '',
      other: '',


    },
    methods: {
      submit() {
        
      }
    },
    props: {},
    
    
  })
  console.log(vm);
</script>
</body>
```



### 10. watch

#### 10.1. 定义

watch 是vue实例的监视属性，用于监视vue实例下数据的变化。watch是一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 `$watch()`，遍历 watch 对象的每一个 property。

#### 10.2. 用法

其实，$watch监视的是vue实例下的属性，所以不仅仅data内的属性可以监视，别的也可以监视到，比如计算属性。

```js
const vm = new Vue({
    el: '#root',
    data: {
        isHot: true
    },
    watch: {
        immediate: true,  // 当初始化成功时，调用handler
        isHot:{  // 当data.isHot的值变化时，调用handler函数，且传入新值和旧值
            handler(newValue, oldValue) {
              //some operations  
            }
        }
    }
})
```

深度监视：

$watch默认不检测对象内部的改变（多级数据改变），配置deep属性可以监视多层改变。

```js
const vm = new Vue({
    el: '#root',
    data: {
        numbers: {
            first: 100,
            second: 99,
            third: 88
        }
    },
    watch: {
        deep: true,  // 实行深度监视，numbers内任意属性改变触发回调函数
        numbers:{ 
            handler(newValue, oldValue) {
              //some operations  
            }
        }
    }
})
```

#### 10.3. 简写

只有不需要deep属性和immediate属性时，才能简写

```js
watch: {
    numbers(newValue, oldValue) {
        // some operations
    }
}
```

#### 10.4. 另一种注册方式

vue还提供了另一种注册watch的方法：直接在实例上注册。 

```js
vm.$watch('isHot', {
    immediate: true,
    deep: true,
    handler(new,old) {
        // operations
    }
})
```

#### 10.5. watch和computed的区别

computed：计算属性

计算属性是由data中的已知值，得到的一个新值。
这个新值只会根据已知值的变化而变化，其他不相关的数据的变化不会影响该新值。
计算属性不在data中，计算属性新值的相关已知值在data中。
别人变化影响我自己。
watch：监听数据的变化

监听data中数据的变化
监听的数据就是data中的已知值
我的变化影响别人

1.watch擅长处理的场景：一个数据影响多个数据

2.computed擅长处理的场景：一个数据受多个数据影响

### 11. 过度和动画



## 四、组件化

​	什么是组件化呢？在前端开发中，会有很多个页面，很多个逻辑，如果全都把它们揉在一起，那么无论是编写，还是维护，都会非常复杂，不利于后续的管理。但是我们可以将每个页面拆分成一个个功能块，每个功能块完成自己的逻辑任务，那么就会非常模块化且逻辑清晰。

​	组件化是vue的重要思想。我们可以开发一个个可以服用的小组件，来构成我们的应用，所有应用都应该被抽象成一棵组件树，开发一个个组件，再组成一个完整的应用。

组件使用分成三个步骤：

1. 创建组件构造器
2. 注册组件
3. 使用组件

### 1. 组件的基本使用过程  

#### 1.1. 基本写法

```html
<body>
  <div id="app">
    <my-cpn></my-cpn>
  </div>
  
  <script>
    // 注意！！！ 组件的声明要在Vue对象构造之前
    // 创建组件构造器
    const cpnC = Vue.extend({
      template: `
      <div>
        <h2>我是cpn组件</h2>
        <h3>嘻嘻</h3>
      </div>`
    })
    // 注册组件
    Vue.component('my-cpn',cpnC)
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {}
    });
  </script>
</body>
```

#### 1.2. 语法糖写法

舍弃了构造器的创建，直接在注册组件时创建构造器。

```html
<body>
  <div id="app">
    <button-counter></button-counter>
  </div>

  <script>
    Vue.component('button-counter', {
      data: function () {
        return {
          count: 0
        }
      },
      template: `<button v-on:click="count++">You clicked me {{ count }} times.</button>`
    })
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {}
    });
  </script>
</body>
```

#### 1.3. 将template抽离

可以将template抽离到注册组件外，这样看起来更简洁一些。

```html
<body>
  <div id="app">
    <button-counter></button-counter>
  </div>
  <template id="cpn">
    <button v-on:click="count++">You clicked me {{ count }} times.</button>
  </template>
  <script>
    Vue.component('button-counter', {
      data() {
        return {
          count: 0
        }
      }, 
      template: '#cpn'
    }
       
    )
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {}
    });
  </script>
</body>
```

### 2. 全局组件和局部组件

在Vue实例外注册的组件是全局组件，即使用Vue.component构造的组件。

在Vue实例内注册的组件是局部组件。局部组件只有对应的Vue实例可以使用。

```html
<body>
  <div id="app">
    <cpn></cpn>
  </div>
  <template id="cpn">
    <button v-on:click="count++">You clicked me {{ count }} times.</button>
  </template>
  <script>
    const cpnC = Vue.extend({
      data() {
        return {
          count: 0
        }
      }, 
      template: "#cpn"
    })
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      components: {
        cpn: cpnC
      }
    });
  </script>
</body>
```

### 3. 父子组件

父子组件是包含关系，子组件在父组件内注册。父组件可以调用子组件

```html
<body>
  <div id="app">
    <cpnf></cpnf>
  </div>
  <script>
    // 创建子组件
    const cpnS = Vue.extend({
      template: `
        <div>
          <p>我是儿子</p>
        </div>`
    })
    // 创建父组件
    const cpnF = Vue.extend({
      template: `
        <div>
          <p>我是你爹</p>
          <cpns></cpns>
        </div> `,
      components: {
        cpns: cpnS
      }  
    })
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      components: {
        cpnf: cpnF
      }
    });
  </script>
</body>
```

#### 3.1. 父子组件数据互通吗？

​	父子组件是无法直接进行数据通信的，需要根据相应的语法实现数据传递。并且所有组件的data属性，必须全都是函数，因为组件会被多处调用，并且这些被调用的组件互相独立，如果data直接用键值对保存，那么这些被调用的组件就会共用一组数据，所以要写成函数形式，这样就实现了数据独立。

#### 3.2. 父传子props

可以通过props将父组件内的数据传递到子组件。

props有对象形式和数组形式，对象形式可以设置默认值和数据类型，所以开发时大多使用对象语法。

由于HTML标签不区分大小写，所以我们写props时，不要用驼峰标识，可以用 -标识法。

```html
<body>
  <div id="app">
    <cpn  :canimes="animes"></cpn>
  </div>
  <template id="cpn">
    <p>{{canimes}}</p>
  </template>
  <script>
    const cpn = {
      template: '#cpn',
      props: {
        canimes: {
          type: Array,
          default: 'undefined',
          required: true
        }
      }
    }
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          animes: ['K-ON','fate']
        }
      },
      components: {
        cpn
      }
    });
  </script>
</body>
```

#### 3.3. 子传父自定义事件$emit

```html
<body>
  <div id="app">
    <!-- 这里$event就是自定义事件携带的参数，把这些参数放到$event这个对象里传递给父组件 -->
    <cpn @btnclick="cpnclick($event)"></cpn>  
  </div>
  <template id="cpn">
    <div>
      <button v-for="item in categories" @click="itemclick(item)">{{item.name}}</p>
    </div>
  </template>
  <script>
    const cpn = {
      template: "#cpn",
      data() {
        return {
          categories: [
            {id: 1, name: 'Chy'},
            {id: 2, name: 'Yxy'},
            {id: 3, name: 'Xhy'},
          ]
        }
      },
      methods: {
        itemclick(item) {
          // console.log(item);
          this.$emit('btnclick', item) // 前面是自定义事件的名字，后面是传递的参数
        }
      }
    }
    var vm = new Vue({
      el: '#app',
      methods: {
        cpnclick(item) {
          console.log(item);
          console.log(item.id);
        }
      },
      components: {
        cpn
      }
    });
  </script>
</body>
```

#### 3.4. 组件结合v-model

​	当我们使用props时，是通过父组件来修改子组件的props，所以我们不希望有别的途径来修改自组建的值，我们要让所有修改子组件值的方式都来源于父组件，所以，不要在子组件内直接改变props的值。应该建立data或computed，先修改它们的值，再传递到父组件，这样就可以修改props的值了。简单来说，父子组件的v-model需要我们自己来写。

```html
<body>
  <div id="app">
    <cpn :number1="num1" @cpnchange="num1Change"></cpn>
    <p>{{num1}}</p>
  </div>
  <template id="cpn">
    <div>
      <p>{{number1}}</p>
      <input type="text" :value="dnumber1" @input="number1Input($event)">
    </div>
  </template>
  <script>
    const cpn = {
      template: "#cpn",
      data() {
        return {
          dnumber1: this.number1
        }
      },
      methods: {
        number1Input(event) {
          //console.log('number1Input');
          this.dnumber1 = event.target.value
          this.$emit('cpnchange',event)
        }
      },
      props: {
        number1: Number
      }
    }
    var vm = new Vue({
      el: '#app',
      data: {
        num1: 1
      },
      methods: {
        num1Change(value) {
          console.log(value);
          this.num1 = parseFloat(value.target.value)
        }
      },
      components: {
        cpn
      }
    });
  </script>
</body>
```

不过，还有一种写法，就是使用vm.$watch实现，非常简单，直接看文档就行。

#### 3.4. 父子组件访问方式

##### 3.4.1. 父访问子 $children

​	this.$children是一个数组类型，它包含所有的子组件对象。

```html
<body>
  <div id="app">
    <cpn></cpn>
    <button @click="btnClick">访问children</button>
  </div>
  <template id="cpn">
    <p>{{animes}}</p>
  </template>
  <script>
    const cpn = {
      template: "#cpn",
      data() {
        return {
          animes: ['K-ON', 'Violet Evergarden', 'Fate']
        }
      },
      methods: {
        sayHello() {
          alert('Hello!')
        }
      }
    }
    var vm = new Vue({
      el: '#app',
      methods: {
        btnClick() {
          console.dir(this.$children)
          this.$children[0].sayHello()
        }
      },
      components: {
        cpn
      }
    });
  </script>
</body>
```

##### 3.4.2 父访问子 $refs

根据vue文档:  refs 是一个对象，持有注册过 [ref attribute](https://cn.vuejs.org/v2/api/#ref) 的所有 DOM 元素和组件实例。只有注册了ref的组件会被查询到。所以 refs更加灵活，查找更加精确，开发过程中一般使用的都是ref，只有我们需要遍历所有子组件时，才使用children。

```html
<body>
  <div id="app">
    <cpn ref="firstchild"></cpn>
    <cpn ref="srcondchild"></cpn>
    <button @click="btnClick">访问refs</button>
  </div>
  <template id="cpn">
    <p>{{animes}}</p>
  </template>
  <script>
    const cpn = {
      template: "#cpn",
      data() {
        return {
          animes: ['K-ON', 'Violet Evergarden', 'Fate']
        }
      },
      methods: {
        sayHello() {
          alert('Hello!')
        }
      }
    }
    var vm = new Vue({
      el: '#app',
      methods: {
        btnClick() {
          console.dir(this.$refs)
        }
      },
      components: {
        cpn
      }
    });
  </script>
</body>
```

#### 3.5. 子访问父

##### 3.5.1 $parent

​	$parent提供了子组件直接访问父组件的功能，但是我们最好不要使用，因为这样虽然增加了开发的便捷性，但是增加了各个组件之间的耦合度，如果我们只提供父组件修改子组件，而子组件不能修改父组件，这样的话代码弹性会非常强。如果要使用，最好只是访问属性，而不修改。

```html
<body>
  <div id="app">
    <cpn></cpn>
  </div>
  <template id="cpn">
    <div>
      <p>我是子组件</p>
      <button @click="btnClick">子组件按钮</button>
    </div>
  </template>
  <script>
    const cpn = {
      template: "#cpn",
      methods: {
        btnClick() {
          console.dir(this.$parent)
          //console.dir(this.$root)
        }
      }
    }
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          animes: ['K-ON','Violent Evergarden','Fate']
        }
      },
      components: {
        cpn
      }
    });
  </script>
</body>
```

##### 3.5.2 $root

​	$root可以直接访问到最底层的根元素，也就是Vue实例，用法和$parent一样.

### 4. slot 插槽

​	组件中，可以添加插槽，这样我们就不会把组件写死，我们可以根据需求，向组件中添加不同的slot，这样组件的复用性就更加强大，组件更加具有扩展性。

#### 4.1. 插槽基本使用

- 插槽定义后，可以在slot标签内设置默认值
- 在使用组件时，直接在组件内定义标签可替换插槽内容，并且可以定义多个标签

```html
<body>
  <div id="app">
    <cpn></cpn>
    <cpn><button>替换默认值</button></cpn>
    <cpn>
      <button>替换1</button>
      <button>替换2</button>
      <button>替换3</button>
    </cpn>
  </div>
  <template id="cpn">
    <div>
      <p>我是组件</p>
      <slot><p>默认值</p></slot>
    </div>
  </template>
  <script>
    const cpn = {
      template: "#cpn",
    }
    var vm = new Vue({
      el: '#app',
      components: {
        cpn
      }
    });
  </script>
```

#### 4.2. 具名插槽

​	当子组件的功能复杂时，子组件的插槽可能并非是一个。比如我们封装的一个NavBar，可能需要三个插槽，分别代表左边、中间、右边。那么，外面在给插槽插入内容时，如何区分插入的是哪一个呢？这时，我们就需要给插槽其一个名字。具名插槽的使用十分频繁，非常重要！

​	在使用标签替换具名插槽时，只要标签能找到对应名字的插槽，那么就一定可以替换，并且可以多标签替换一个插槽。

```html
<body>
  <div id="app">
    <cpn></cpn>
    <HR width=100% color=#987cb9 SIZE=1>  <!-- 分割线 -->
    <cpn>
      <span slot="left">左边</span>
    </cpn>
    <HR width=100% color=#987cb9 SIZE=1>  <!-- 分割线 -->
    <cpn>
      <span slot="left">左边</span>
      <span slot="middle">中间</span>
      <span slot="right">右边</span>
    </cpn>
    <HR width=100% color=#987cb9 SIZE=1>  <!-- 分割线 -->
    <cpn>
      <span slot="left">左边</span>
      <span slot="middle">中间</span>
      <span slot="right">右边</span>
      <span slot="right">右边边</span>
      <span slot="left">左边便</span>
    </cpn>
    
  </div>
  <template id="cpn">
    <div>
      <p>我是组件</p>
      <slot name="left"><p>slot-one</p></slot>
      <slot name="middle"><p>slot-two</p></slot>
      <slot name="right"><p>slot-three</p></slot>
    </div>
  </template>
  <script>
    const cpn = {
      template: "#cpn",
    }
    var vm = new Vue({
      el: '#app',
      components: {
        cpn
      }
    });
  </script>
</body>
```

#### 4.3. 作用域插槽

插槽内存在自己的数据，可能有时希望把插槽的数据传递给父组件，这时需要使用作用域插槽。

```html
<body>
  <div id="app">
    <cpn>
      <template slot-scope="slot">
        <span v-for="item in slot.data">{{item}} - </span>
      </template>
    </cpn>
  </div>
  <template id="cpn">
    <div>
      <slot :data="pLanguages"></slot>
    </div>
  </template>
  <script>
    const cpn = {
      template: "#cpn",
      data() {
        return {
          pLanguages: ['JS','Java','C#','Python']
        }
      }
    }
    var vm = new Vue({
      el: '#app',
      components: {
        cpn
      }
    });
  </script>
</body>
```

## 五、 自定义指令

除了核心功能默认内置的指令 (`v-model` 和 `v-show`)，Vue 也允许注册自定义指令。注意，在 Vue2.0 中，代码复用和抽象的主要形式是组件。然而，有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令。自定义指令其实就是对操作dom进行的封装。

### 1.  基本使用

自定义指令调用时间：1. 元素和指令刚绑定时    2. 所在模板更新时

```html
<body>
  <script src="../../vueFrame/vue.js"></script>
  <div id="app">
    <h2>{{name}}</h2>
    <h2>当前n值为：<span v-text="n"></span></h2>
    <h2>放大10倍后n值为：<span v-big="n"></span></h2>
    <button @click="n++">n+1</button>
  </div>

<script>
  const vm = new Vue({
    el: '#app',
    data: {
      name: 'chy',
      n: 1
    },
    directives: { //自定义指令，定义名称时不要加v-，使用时要加上v-
      big(element,binding) { element为指令所在标签的dom，binding为指令绑定的元素
        console.log('v-bing')
        element.innerText = binding.value * 10
       }
    }
  })
</script>
</body>
```

以上写法并不注重自定义指令执行的时间，下面的写法可以精确控制执行时间。

### 2. 精确写法

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- `update`：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

```js
<body>
  <script src="../../vueFrame/vue.js"></script>
  <div id="app">
    <h2>{{name}}</h2>
    <h2>当前n值为：<span v-text="n"></span></h2>
    <h2>放大10倍后n值为：<span v-big="n"></span></h2>
    <button @click="n++">n+1</button><br/>
    <input v-fbind="n">
  </div>

<script>
  const vm = new Vue({
    el: '#app', // 用于挂在要管理的元素
    data: { //元素的数据
      name: 'chy',
      n: 1
    },
    directives: {
      big(element,binding) {
        //console.log(binding)
        element.innerText = binding.value * 10
       },
      fbind: {
        bind(element,binding) {
          console.log('bind');
          element.value = binding.value
        },
        inserted(element,binding) {
          console.log('inserted');
          element.focus()
        },
        update(element,binding) {
          console.log('update');
          element.value = binding.value
        }
      } 
    }
  })
</script>
</body>
```



