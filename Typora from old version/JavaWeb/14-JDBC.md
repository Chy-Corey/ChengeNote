### JDBC

概念：Java Database Connectivity，java数据库连接，用java来操作数据库。

本质：由Sun公司定义的一套操作所有关系型数据库的规则，即接口。再由各个数据库厂商去实现这套接口，提供数据库驱动jar包。我们可以使用这套接口编程，真正执行的代码是驱动jar包中的实现类。

#### 1. JDBC快速入门

##### 1.1. 步骤

1. 导入驱动jar包
   1. 复制jar包到项目的libs目录下
   2. 右键 --> Add As Library
2. 注册驱动
3. 获取数据库连接对象 Connection
4. 定义sql
5. 获取执行sql语句的对象 Statement
6. 执行sql，接受返回结果
7. 处理结果
8. 释放资源

##### 1.2. 代码实现

```mysql
package com.chy.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

public class jdbcDemo1 {
    public static void main(String[] args) throws Exception {
        // 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
        // 获取数据库连接对象
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:921/test", "root", "Ysy.2001.09.21");
        // 定义sql语句
        String sql = "update stutent set age = 60 where id = 3";
        //获取执行sql的对象
        Statement stmt = conn.createStatement();
        //执行sql
        int count = stmt.executeUpdate(sql);
        // 处理结果
        System.out.println(count);
        // 释放资源
        stmt.close();
        conn.close();
    }
}
```

#### 2. 详解各个对象

##### 2.1. DriverManager：驱动管理对象

功能：

1. 注册驱动：告诉程序该使用哪一个数据库驱动jar

   使用calss.for加载类，这个类中存在静态代码块，被加载后就会执行，这个静态代码块中就存在注册驱动函数

2. 获取数据库连接

   方法：`static Connection getConnection(String url, String user, String password)`

   参数：

   - url：指定连接的路径
     - 语法：`jdbc:mysql://ip地址:端口号/数据库名称`
     - 例子：`jdbc:mysql://localhost:921/test`
     - 细节：如果连接的是本机mysql服务器，并且mysql服务默认端口为3306，则url可以简写为：`jdbc:mysql:///数据库名称`

   - user：用户名
   - password：密码

##### 2.2. Connection：数据库连接对象

功能：

1. 获取执行sql的对象
   - Statement createStatement()
   - PreparedStatement prepareStatement(String sql)
2. 管理事务
   - 开启事务：setAutoCommit(boolean autoCommit)
   - 提交事务：commit()
   - 回滚事务：callback() 

##### 2.3. Statement：执行sql的对象

1. 作用：执行sql

   - boolean execute(String sql)：可以执行任意的sql

   - int executeUpdate(Sting sql)：执行DML(insert  update  delete) 语句、DDL(create  alter  drop) 语句
     - 返回值：影响的行数，可以通过这个判断执行是否成功，返回值>0则成功。

   - ResultSet executeQuery(String sql)：执行DQL(select) 语句

2. 练习

   - student表	添加一条记录
   - student表    修改记录
   - student表     删除一条记录

```mysql
package com.chy.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

/**
 * student表	添加一条记录
 */
public class jdbcDemo2 {
    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;
        try {
            // 1. 注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            // 2. 定义sql
            String sql = "insert into student (id, name, age, score, birthday) values(null,'盖里老哥',25,60,'1999-09-10')";
            // 3. 获取Connection对象
            conn = DriverManager.getConnection("jdbc:mysql://localhost:921/test", "root", "Ysy.2001.09.21");
            // 4. 获取执行sql的对象Statement
            stmt = conn.createStatement();
            // 5. 执行sql
            int count = stmt.executeUpdate(sql);
            // 6. 处理结果
            System.out.println(count);
            if(count > 0 ) {
                System.out.println("添加成功");
            } else {
                System.out.println("添加失败");
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
    }
}
```

##### 2.4. ResultSet：结果集对象，封装查询结果

- next()：游标向下一行移动
- getXxx(参数)：获取数据
  - Xxx：代表数据类型	如：getInt()，getString()
  - 参数：
    - int：代表列的编号，从1开始
    - String：代表列的名称

##### 2.5. PreparedStatement：执行sql的对象

1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题。

   - 用户名随便输入，输入密码：`'a' or 'a' = 'a'`
   - sql：`select * from user where username = 'asdfad' and password = 'a' or 'a' = 'a'`

2. 解决sql注入问题：使用PreparedStatement对象来解决

3. 预编译的sql：参数使用?作为占位符

4. 步骤

   1. 导入驱动jar包
   2. 注册驱动
   3. 获取数据库连接对象Connection
   4. 定义sql
      - 注意：sql的参数使用?作为占位符。如 `select * from user where username = ? and password = ?;`
   5. 获取执行sql语句的对象 PreparedStatement    Connection.preparedStatement(String sql)
   6. 给?赋值：
      - 方法：setXxx(param1, param2)
        1. 参数1：？的位置编号，从1开始
        2. 参数2：？的值
   7. 执行sql，接受返回结果，不需要传递sql语句
   8. 处理结果
   9. 释放资源

   **注意**：以后全都使用PreparedStatement来完成增删改查所有操作

   		1. 可以防止sql注入
     		2. 效率更高

#### 3. 抽取工具类

- 目的：简化书写

- 分析：
  1. 抽取注册驱动
  2. 抽取一个方法获取连接对象
     - 需求：不传递参数，但保证函数的通用性
     - 方法：通过配置文件
  3. 抽取一个方法释放资源

#### 4. JDBC事务管理

1. 事务：一个包含多个步骤的业务操作，如果这个业务操作被事务管理，则多个步骤要么同时成功，要么同时失败
2. 操作
   1. 开启事务
   2. 提交事务
   3. 回滚事务
3. 用Connection来管理事务
   - 开启事务：`setAutocommit(boolean autoCommit)`：调用该方法设置参数为false，即开启事务
   - 提交事务：commit()
   - 回滚事务：rollback()
