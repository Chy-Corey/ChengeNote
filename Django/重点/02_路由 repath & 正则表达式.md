## 路由 repath & 正则表达式



### re_path

- 在 url 的匹配过程中可以使用正则表达式进行精确匹配

- 语法

  - `re_path(reg, view, name=xxx)`
  - 正则表达式为命名分组模式(?P\<name>pattern); 匹配提取参数后用关键字传参方式传递给视图函数

- 样例

  \# 可匹配 http://127.0.0.1:8000/20/mul/40

  \# 不可匹配 http://127.0.0.1:8000/20/mul/400（不能出现两位数以上的数字）

  ```python
  urlpatterns = [
      path('admin/', admin.site.urls),
      re_path(r'^(?P<x>\d{1,2})/(?P<OP>\w+)/(?P<y>\d{1,2})$', views.cal_view),
  ]
  ```

这里使用了正则表达式，但是我不会，所以我学了一下。



### 正则表达式

正则表达式(Regular Expression)是一种文本匹配方式，包括普通字符（例如，a 到 z 之间的字母）和特殊字符（称为"元字符"）。

可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等。

由于正则表达式语法太多，直接上链接算了。

[正则表达式 – 语法 | 菜鸟教程 (runoob.com)](https://www.runoob.com/regexp/regexp-syntax.html)