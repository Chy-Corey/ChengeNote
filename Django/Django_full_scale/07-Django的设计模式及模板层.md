### Django的设计模式及模板层



#### 1. MVC & MTV

##### 传统的MVC

MVC代表 Model - View - Controller (模型 - 视图 - 控制器) 模式。

- M：主要用于对数据库层的封装
- V：用于向用户展示结果结果
- C：用于处理请求、获取数据、返回结果

作用：降低模块间的耦合度(解耦)

##### MTY

MTV代表Model - Template - View（模型 - 模板- 视图）模式

- M：负责与数据库交互
- T：负责呈现内容到客户端
- V：负责接受请求、获取数据、返回结果



#### 2. 模板层

1. 模板是可以根据字典数据动态变化的html网页
2. 模板可以根据视图中传递的字典数据动态生成相应的HTML网页



#### 3. 模板配置

模板层配置在文件夹中，创建模板文件夹`templates`，且该文件夹在项目目录下，即 <项目名>/templates，注意这个templates文件夹和settings.py这些配置文件不在同一级，而是在上一级。

**在settings.py 中 TEMPLATES 配置项**

1. BACKEND: 指定模板的引擎
2. DIRS: 模板的搜索目录(可以是一个或多个)
3. APP_DIRS: 是否要在应用中的 templates 文件夹中搜索模板文件
4. OPTIONS: 有关模板的选项

**配置项中需要修改的部分**

- 设置DIRS - 'DIRS': [os.path.join(BASE_DIR, 'templates')]

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

#### 4. 模板加载

##### 方案1

通过 loader 获取模板，通过HttpResponse进行响应，这个操作可以在视图函数中完成

```python
# views.py
from django.template import loader
# 1. 通过loader加载模板
t = loader.get_template("model_name")
# 2. 将t转换成 HTML 字符串
html = t.render(dictionary_data)
# 3. 用响应对象将转换的字符串内容返回给浏览器
return HttpResponse(html)
```

##### 方案2（推荐）

使用render()直接加载并响应模板    (这里的render和方案1中的render不一样，不属于同一个包)

```python
# views.py
from django.shortcuts import render
return render(request, "model_name", dictionary_data)
```

##### 字典变量的应用

视图函数中可以将Python变量封装到字典中传递到模板

```python
# views.py
def xxx_view(request):
    dic = {
        key1: value1,
        key2: value2
    }
    return render(request, 'xxx.html', dic)
```

在模板中，可以用`{{ 变量名 }}`的语法调用视图传进来的变量



