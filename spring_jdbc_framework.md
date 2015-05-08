# Spring——JDBC 框架概述

在使用普通的 JDBC 数据库时，就会很麻烦的写不必要的代码来处理异常，打开和关闭数据库连接等。但 Spring JDBC 框架负责所有的低层细节，从开始打开连接，准备和执行 SQL 语句，处理异常，处理事务，到最后关闭连接。

所以当从数据库中获取数据时，你所做的是定义连接参数，指定要执行的 SQL 语句，每次迭代完成所需的工作。

Spring JDBC 提供几种方法和数据库中相应的不同的类与接口。我将给出使用 **JdbcTemplate** 类框架的经典和最受欢迎的方法。这是管理所有数据库通信和异常处理的中央框架类。

## JdbcTemplate 类

JdbcTemplate 类执行 SQL 查询、更新语句和存储过程调用，执行迭代结果集和提取返回参数值。它也捕获 JDBC 异常并转换它们到 org.springframework.dao 包中定义的通用类、更多的信息、异常层次结构。

*JdbcTemplate* 类的实例是*线程安全*配置的。所以你可以配置 JdbcTemplate 的单个实例，然后将这个共享的引用安全地注入到多个 DAOs 中。

使用 JdbcTemplate 类时常见的做法是在你的 Spring 配置文件中配置数据源，然后共享数据源 bean 依赖注入到 DAO 类中，并在数据源的设值函数中创建了 JdbcTemplate。

## 配置数据源

我们在数据库 **TEST** 中创建一个数据库表 **Student**。假设你正在使用 MySQL 数据库，如果你使用其他数据库，那么你可以改变你的 DDL 和相应的 SQL 查询。
 
``` 
CREATE TABLE Student(
   ID   INT NOT NULL AUTO_INCREMENT,
   NAME VARCHAR(20) NOT NULL,
   AGE  INT NOT NULL,
   PRIMARY KEY (ID)
);
```

现在，我们需要提供一个数据源到 JdbcTemplate 中，所以它可以配置本身来获得数据库访问。你可以在 XML 文件中配置数据源，其中一段代码如下所示：

<pre class="prettyprint notranslate">
&lt;bean id="dataSource"
class="org.springframework.jdbc.datasource.DriverManagerDataSource"&gt;
   &lt;property name="driverClassName" value="com.mysql.jdbc.Driver"/&gt;
   &lt;property name="url" value="jdbc:mysql://localhost:3306/TEST"/&gt;
   &lt;property name="username" value="root"/&gt;
   &lt;property name="password" value="password"/&gt;
&lt;/bean&gt;
</pre>


## 数据访问对象（DAO）

DAO 代表常用的数据库交互的数据访问对象。DAOs 提供一种方法来读取数据并将数据写入到数据库中，它们应该通过一个接口显示此功能，应用程序的其余部分将访问它们。

在 Spring 中，数据访问对象(DAO)支持很容易用统一的方法使用数据访问技术，如 JDBC、Hibernate、JPA 或者 JDO。

## 执行 SQL 语句

我们看看如何使用 SQL 和 jdbcTemplate 对象在数据库表中执行 CRUD(创建、读取、更新和删除)操作。

查询一个整数类型：

``` 
String SQL = "select count(*) from Student";
int rowCount = jdbcTemplateObject.queryForInt( SQL );
```

查询一个 long 类型：

``` 
String SQL = "select count(*) from Student";
long rowCount = jdbcTemplateObject.queryForLong( SQL );
```

一个使用绑定变量的简单查询：

``` 
String SQL = "select age from Student where id = ?";
int age = jdbcTemplateObject.queryForInt(SQL, new Object[]{10});
```

查询字符串：

``` 
String SQL = "select name from Student where id = ?";
String name = jdbcTemplateObject.queryForObject(SQL, new Object[]{10}, String.class);
```

查询并返回一个对象：

``` 
String SQL = "select * from Student where id = ?";
Student student = jdbcTemplateObject.queryForObject(SQL, 
                  new Object[]{10}, new StudentMapper());
public class StudentMapper implements RowMapper<Student> {
   public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
      Student student = new Student();
      student.setID(rs.getInt("id"));
      student.setName(rs.getString("name"));
      student.setAge(rs.getInt("age"));
      return student;
   }
}
```

查询并返回多个对象：

``` 
String SQL = "select * from Student";
List<Student> students = jdbcTemplateObject.query(SQL,
                         new StudentMapper());
public class StudentMapper implements RowMapper<Student> {
   public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
      Student student = new Student();
      student.setID(rs.getInt("id"));
      student.setName(rs.getString("name"));
      student.setAge(rs.getInt("age"));
      return student;
   }
}
```

在表中插入一行：

``` 
String SQL = "insert into Student (name, age) values (?, ?)";
jdbcTemplateObject.update( SQL, new Object[]{"Zara", 11} );
```

更新表中的一行：

``` 
String SQL = "update Student set name = ? where id = ?";
jdbcTemplateObject.update( SQL, new Object[]{"Zara", 10} );
```

从表中删除一行：

``` 
String SQL = "delete Student where id = ?";
jdbcTemplateObject.update( SQL, new Object[]{20} );
```

## 执行 DDL 语句

你可以使用 *jdbcTemplate* 中的 **execute(..)** 方法来执行任何 SQL 语句或 DDL 语句。下面是一个使用 CREATE 语句创建一个表的示例：

``` 
String SQL = "CREATE TABLE Student( " +
   "ID   INT NOT NULL AUTO_INCREMENT, " +
   "NAME VARCHAR(20) NOT NULL, " +
   "AGE  INT NOT NULL, " +
   "PRIMARY KEY (ID));"
jdbcTemplateObject.execute( SQL );
```

## Spring JDBC 框架例子：

基于上述概念，让我们看看一些重要的例子来帮助你理解在 Spring 中使用 JDBC 框架：

<table class="table table-bordered">
<tr><th style="width:5%">序号</th><th>例子 &amp; 描述</th></tr>
<tr><td>1</td><td><a href="/spring/spring_jdbc_example.htm">Spring JDBC Example</a>
<p>这个例子将解释如何编写一个简单的基于 Spring 应用程序的 JDBC。</p></td></tr>
<tr><td>2</td><td><a href="/spring/calling_stored_procedure.htm">SQL Stored Procedure in Spring</a>
<p>学习在使用 Spring 中的 JDBC 时如何调用 SQL 存储过程。</p></td></tr>
</table>




