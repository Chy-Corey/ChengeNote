### DCL

#### 1. 基本操作

- 添加用户

  ```mysql
  create user '用户名' @ '主机名' identified by '密码';
  ```

- 删除用户

  ```mysql
  drop user '用户名' @ '主机名'
  ```

- 修改用户密码

  ```mysql
  update user set password = password('新密码') where user = 'y'
  ```

- 查询用户

  ```mysql
  -- 1. 切换到mysql数据库
  use mysql;
  -- 2. 查询user表
  select * from user;
  ```

  通配符： %（表示可以在任意主机使用用户登录）

#### 2. 权限管理

- 查询所具有的权限

  ```mysql
  show grants for '用户名' @ '主机名';
  ```

- 授予权限

  ```mysql
  grant 权限列表 on 数据库名.表名 to '用户名' @ '主机名';
  ```

  权限列表：select   delete   update

  ```mysql
  -- 授予所有权限
  grant all on *.* to '用户名' @ '主机名';
  ```

- 撤销权限

  ```mysql
  revoke 权限列表 on 数据库名.表名 from '' @ 'z';
  ```

  

