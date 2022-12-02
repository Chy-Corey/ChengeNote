## XML

- Conception:Extensible Markup Language 可扩展标记语言
  - 可扩展：标签都是自己定义的。
- 功能：存储数据
  - 作为配置文件
  - 在网络中传输
- 与HTML的区别
  - xml标签都是自定义的，html都是预定义
  - xml语法严格，html语法松散
  - xml用来存数据，html展示数据

```xml
<? xml version='1.0' encoding='utf-8' ?>
<note>
<to>George</to>
<from>John</from>
<heading>Reminder</heading>
<body>Don't forget the meeting!</body>
</note>
```



### 1. 语法

- 基本语法

  1. xml第一行必须是文档声明
  2. xml文档中有且仅有一个跟标签
  3. 属性值必须用引号括起来
  4. 标签必须正确关闭

- 组成部分

  1. 文档声明
     - 格式：`<? xml 属性列表 ?>`
     - 属性列表：
       - version：版本号，必须有的属性
       - encoding：编码方式
       - standalone：是否独立
         - yes：不依赖其他文件
         - no：依赖其他文件

  2. 指令：结合css
  3. 标签：名称自定义
  4. 属性：要用引号括起来
  5. 文本：
     - CDATE区：在该区域中的数据会被原样展示
       - 格式： `<![CDATA[数据]]>`

### 2. 约束

约束：规定xml文档的书写规则

- 作为框架的使用者：
  1. 能够在xml中引入约束文档
  2. 能够读懂约束文档

- 分类：
  1. DTD：一种简单的约束技术
  2. Schema：一种复杂的约束技术

#### 2.1. DTD

- 引入DTD文档到XML中
  - 内部dtd：将约束规则定义在xml文档中
  - 外部dtd：将约束的规则定义在外部的dtd文件中
    - 本地：`<!DOCTYPE 跟标签名 SYSTEM "dtd文件位置">`
    - 网络：`<!DOCTYPE 跟标签名 PUBLIC "dtd文件名字" dtd文件位置>`

#### 2.2. Schema

- 引入：
  1. 填写xml文档根元素
  2. 引入xsi前缀
  3. 引入xsd文件ing民工见
  4. 为每个xsd约束声明一个前缀作为标识

### 3. 解析

xml解析的思想：

1. DOM思想：将标记语言文档一次性加载进内存，形成一棵DOM树
   - 优点：操作方便，可以对文档进行CRUD所有操作
   - 缺点：太占内存

2. SAX思想：逐行读取，基于文件驱动
   - 优点：不占内存
   - 缺点：不能增删改查

### 4. Jsoup

步骤：

1. 导入jar包
2. 获取Document对象
3. 获取对应标签

具体内容百度即可