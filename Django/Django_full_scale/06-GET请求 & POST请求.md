### GET & POST



#### 1. 定义

- 无论是GET还是POST，都是由视图函数接受请求，通过判断request.method区分具体的请求动作

- 样例

  ```python
  if request.method == 'GET':
     #some opration#
  elif request.method == 'POST':
     #some opration#
  else:
     #some opration#
  ```

#### 2. GET处理

- GET请求动作一般用于向服务器获取数据
- 能够产生GET请求的场景：
  - 浏览器地址栏中输入URL然后回车
  - \<a href="地址?参数1=值&参数2=值">
  - form表单中的method为get

- GET请求方式中，如果有数据需要传递给服务器，通常会用查询字符串(Query String)传递    **【不要传递敏感数据！】**

  - URL格式：xxx?参数名1=值1&参数名2=值2
    - 如：http://<span>127.0.0.1:8000/page1?a=100&b=200</span>

  - 服务端接收参数，获取GET请求提交的数据

- 方法示例

  ```python
  request.GET['参数名']
  request.GET.get('参数名', '默认值')
  request.GET.getlist('参数名')
  # mypage?a=100&b=200&c=300&b=400
  # request.GET=QueryDict({
  #     'a': ['100'],
  #     'b': ['200', '400'],
  #     'c': ['300']
  # })
  # a = request.GET['a']
  # b = request.GET['b']  # b
  ```


#### 3. POST处理

- POST请求动作，一般用于向服务器提交大量数据或隐私数据

- 客户端通过表单等POST请求将数据传递给服务器端，如：

  ```html
  <form method='post' action='/login'>
  	姓名：<input type="text" name="username">
      <input type="submit" value="登录">
  </form>
  ```

- 使用post方式接收客户端数据

  ```python
  request.POST['参数名']    # request.POST 绑定 QueryDict
  request.POST.get('参数名', '默认值')
  request.POST.getlist('参数名')
  ```

  **注意：**CSRF验证需要关闭，否则服务器会拒绝POST请求

- 取消CSRF验证

  ```python
  # settings.py
  # 把c
  MIDDLEWARE = [
      'django.middleware.security.SecurityMiddleware',
      'django.contrib.sessions.middleware.SessionMiddleware',
      'django.middleware.common.CommonMiddleware',
      # 'django.middleware.csrf.CsrfViewMiddleware',
      'django.contrib.auth.middleware.AuthenticationMiddleware',
      'django.contrib.messages.middleware.MessageMiddleware',
      'django.middleware.clickjacking.XFrameOptionsMiddleware',
  ]
  ```

  

