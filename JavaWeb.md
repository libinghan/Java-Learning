# JavaWeb

## 数据库

SQL: Structured Query Language 结构化查询语言

### SQL

#### DDL

Data Definition Language 数据定义语言，定义数据库对象：数据库，表，列等

```sql
SHOW DATABASES;
CREATE DATABASE DB1;
CREATE DATABASE IF NOT EXISTS DB1;
DROP DATABASE DB1;
DROP DATABASE IF EXISTS DB1;
SELECT DATABASE(); # 查看当前数据库
USE DB1;

SHOW TABLES;
DESC TB1; # 查询表结构
CREATE TABLE TB1 (
	id int,
    username varchar(20),
    password varchar(32)
);

DROP TABLE TB1;
DROP TABLE IF EXISTS TB1;
ALTER TABLE TB1 RENAME TO TB2;
ALTER TABLE TB2 ADD sex char(1);
ALTER TABLE TB2 MODIFY id tinyint;
ALTER TABLE TB2 CHANGE id new_id int;
ALTER TABLE TB2 DROP sex;
```

#### DML

Data Manipulation Language 数据操作语言，增删改

```sql
INSERT INTO TB1(id,username,password) VALUES(1,"libinghan","123456");
INSERT INTO TB1 VALUES(1,"libinghan","123456");
INSERT INTO TB1(id,username,password) VALUES(1,"libinghan","123456"),(2,"huiyongjing","654321");
INSERT INTO TB1 VALUES(1,"libinghan","123456"),(2,"huiyongjing","654321");

UPDATE TB1 SET name="li" WHERE id=1;
DELETE FROM TB1 WHERE id=2;
```

#### DQL

Data Query Language 数据查询语言，查询数据库中表的记录

```sql
SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ... LIMIT ...;
SELECT * FROM TB1 LIMIT 1,10 # 从索引1开始查10条
```

null不参与所有聚合函数计算

查询条件

| 符号               | 功能                   |
| ------------------ | ---------------------- |
| >                  | 大于                   |
| <                  | 小于                   |
| >=                 | 大于等于               |
| <=                 | 小于等于               |
| =                  | 等于                   |
| <>或!=             | 不等于                 |
| BETWEEN... AND ... | 在某个范围内           |
| IN(...)            | 多选一                 |
| LIKE ...           | 模糊查询，%多个，_单个 |
| IS NULL            | 是NULL                 |
| IS NOT NULL        | 不是NULL               |
| AND或&&            | 并且                   |
| OR或\|\|           | 或者                   |
| NOT或!             | 非                     |

#### 多表查询

连接查询

- 内连接

  ```sql
  SELECT ... FROM TB1,TB2 WHERE ...; # 隐式内连接
  SELECT ... FROM TB1 JOIN TB2 ON ...; # 显示内连接
  ```

- 外连接

  左外连接，查询A表所有数据和交集部分数据

  右外连接，查询B表所有数据和交集部分数据

  ```sql
  SELECT ... FROM TB1 LEFT JOIN TB2 ON ...;
  SELECT ... FROM TB1 RIGHT JOIN TB2 ON ...;
  ```

子查询

- 单行单列

  ```sql
  SELECT ... FROM TB1 WHERE salary=(子查询);
  ```

- 多行单列

  ```sql
  SELECT ... FROM TB1 WHERE salary IN (子查询);
  ```

- 多行多列

  ```sql
  SELECT ... FROM (子查询) WHERE ...;
  ```

#### DCL

Data Control Language 数据控制语言，定义数据库的访问权限和安全级别，创建用户

### 约束

| 约束名称 | 描述                                               | 关键字      |
| -------- | -------------------------------------------------- | ----------- |
| 非空约束 | 列中数据不能为null                                 | NOT NULL    |
| 唯一约束 | 列中数据各不相同                                   | UNIQUE      |
| 主键约束 | 主键非空且唯一                                     | PRIMARY KEY |
| 检查约束 | 列中的值满足某一要求（MySQL不支持）                | CHECK       |
| 默认约束 | 未指定则采用默认值                                 | DEFAULT     |
| 外键约束 | 让两个表的数据之间简历连接，保证数据一致性和完整性 | FOREIGN KEY |

```sql
CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id)
ALTER TABLE emp DROP FOREIGN KEY fk_emp_dept;
ALTER TABLE emp ADD CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id);
```

### 数据库设计

#### 软件研发步骤

需求分析（产品经理，产品原型）=>设计（架构师，开发工程师，软件结构设计，数据库设计，接口设计，过程设计）=>编码（开发工程师）=>测试（测试工程师）=>安装部署（运维工程师）

#### 数据库设计步骤

需求分析（数据，数据属性，数据与属性的特点）=>逻辑分析（ER图逻辑建模）=>物理设计（根据数据库自身特点把逻辑设计转换为物理设计）=>维护设计（新的需求建表，表的优化）

一对一，用于表拆分，经常使用的放一张表，提升查询性能，在任意一方加入外键，并设置成唯一

### 事务

把一组操作命令作为一个整体，要么同时成功，要么同时失败，不可分割的工作逻辑单元

```sql
START TRANSACTION; # 或者BEGIN
COMMIT;
ROLLBACK;
```

#### 四大特征

- 原子性Atomicity，事务是不可分割的最小操作单位

- 一致性Consistency，事务完成时，所有数据都保持一致状态
- 隔离性Isolation，多个事务之间，操作的可见性
- 持久性Durability，事务一旦提交或回滚，改变是永久的

## JDBC

Java DataBase Connectivity，一套标准接口

### 步骤

```java
Class.forName("com.mysql.jdbc.Driver"); // 注册驱动，可不写
Connection conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/db1", "root", "1234"); // url, user, password 获取连接，本机3306url可以简写成jdbc:mysql:///db1
String sql = "update account set money = 2000 where id = 1"; // 定义sql
Statement stmt = conn.createStatement(); // 获取sql对象Statement
int count = stmt.executeUpdate(sql); // 执行sql，返回受影响的行数
// 释放资源
stmt.close();
conn.close();
```

### API

#### DriverManager

驱动管理类，注册驱动，获取数据库连接

#### Connection

获取执行SQL的对象

管理事务

```java
conn.setAutoCommit(false); // 开启事务
conn.commit();
conn.rollback();
```

#### Statement

执行SQL

executeUpdate(sql)，执行DML，DDL语句，返回受影响的行数

exequteQuery(sql)，执行DQL语句，返回ResultSet集合对象

```java
String sql = "select * from employee";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);
while (rs.next()) {
    System.out.println(rs.getString(1) + "===>" + rs.getString("empname"));
}
System.out.println(rs);
stmt.close();
conn.close();
```

#### PreparedStatement

预编译SQL语句并执行，预防SQL注入问题（通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法）

`' or '1' = '1`

```java
Class.forName("com.mysql.jdbc.Driver");
Connection conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/db1", "root", "1234");
String name = "libinghan";
String pwd = "1234";
String sql = "select * from tb_user where username = ? and password = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(name);
pstmt.setString(pwd);
ResultSet = pstmt.executeQuery();
pstmt.close();
conn.close();
```

预编译SQL性能更高，预编译功能开启：`useServerPrepStmts=true`加在url后，防止SQL注入（转义敏感字符）

## 数据库连接池

容器，负责分配管理数据库连接，允许程序重复使用一个现有的数据库连接，而不是再重新建立一个，释放空闲时间超过最大空闲时间的连接来避免数据库连接遗漏

资源重用，提升系统响应速度，避免数据库连接遗漏

需要实现DataSource接口，常见数据库连接池：DBCP，D3P0，Druid

```java
Properties prop = new Properties();
prop.load(new FileInputStream("JDBC1/src/druid.properties"));
DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
Connection connection = dataSource.getConnection();
System.out.println(connection);
```

## Maven

专门用于管理和构建Java项目的工具，提供了标准化的项目结构，构建流程和依赖管理（第三方资源）机制

使用标准坐标配置管理各种依赖

```xml
<dependencies>
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.32</version>
	</dependency>
</dependencies>
```

### Maven模型

- 项目对象模型（Project Object Model）
- 依赖管理模型（Dependency）
- 插件（Plugin）
