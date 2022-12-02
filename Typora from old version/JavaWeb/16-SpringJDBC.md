### Spring JDBC

- Spring对JDBC进行了简单封装，提供了一个JDBCTemplate对象简化JDBC
- 步骤
  1. 导入jar包
  2. 创建JdbcTemplate对象，依赖于数据源DataSource
     - JdbcTemplate template = new JdbcTemplate(ds);
  3. 调用JdbcTemplate的方法来完成CRUD的操作
     - update()：执行DML语句
     - queryForMay()：查询结果封装为map集合
     - queryForList()：查询结果封装为list集合
     - query()：查询结果集封装为JavaBean对象
     - queryForObject()：查询结果封装为对象