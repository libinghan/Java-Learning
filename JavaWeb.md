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

- 项目对象模型（Project Object Model），pom.xml
- 依赖管理模型（Dependency）
- 插件（Plugin）

仓库：本地仓库，中央仓库，远程仓库（私服）

### 常用命令

- compile 编译
- clean 清理
- test 测试
- package 打包
- install 安装

### 生命周期

同一生命周期，执行后面的语句会自动执行之前的

- clean：清理工作，pre-clean->clean->post->clean
- default：核心工作，compile->test->package->install
- site：产生报告，发布站点等，pre-site->site->post->site

### Maven坐标

资源的唯一标识，使用坐标来定义项目或引入项目中需要的依赖

- groupId：Maven项目隶属组织名称
- artifactId：Maven项目名称，通常是模块名
- version：版本号

### 依赖管理

```xml
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.30</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.15</version>
    </dependency>
</dependencies>
```

### 依赖范围

设置坐标的依赖范围scope，对应jar包的作用范围：编译环境，测试环境，运行环境，默认值为compile

## MyBatis

持久层（数据保存到数据库）框架（半成品软件，高效规范通用可扩展），简化JDBC开发

JDBC缺点：硬编码（字符串多，维护性差），操作繁琐（手动设置参数，封装结果集）

入门：https://mybatis.org/mybatis-3/zh/getting-started.html

### 步骤

1. 创建user表，添加数据
2. 创建模块，导入坐标
3. 编写MyBatis核心配置文件
4. 编写SQL映射文件
5. 编码
   1. 定义POJO类
   2. 加载核心配置文件，获取SqlSessionFactory对象
   3. 获取SqlSession对象，执行SQL语句
   4. 释放资源

```java
public class MyBatisDemo {
    public static void main(String[] args) throws IOException {
        // 加载mybatis的核心配置文件，获取SqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        // 获取SqlSession对象，用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 执行sql
        List<User> users = sqlSession.selectList("test.selectAll");
        System.out.println(users);
        // 释放资源
        sqlSession.close();
    }
}
```

### Mapper代理开发

1. 定义与SQL映射文件同名的Mapper接口，并将Mapper接口和SQL映射文件放置在同一目录下
2. 设置SQL映射文件的namespace属性为Mapper接口全限定名
3. 在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致
4. 编码
   1. 通过SqlSession的getMapper方法获取Mapper接口的代理对象
   2. 调用对应方法完成sql执行

```java
// 执行sql
/*
List<User> users = sqlSession.selectList("test.selectAll");
*/
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
List<User> users = userMapper.selectAll();
```

数据库表的字段名和实体类的属性名不一样：起别名（每次都要起）或用sql片段（不灵活）

```xml
<sql id="brand_column">
	id, brand_name as brandName
</sql>
<select id="selectAll" resultType="brand">
    select
    	<include refid="brand_column"/>
    from tb_brand;
</select>
```

```xml
<resultMap id="brandResultMap" type="brand">
	<result column="brand_name" property="brandName"/>
    <!--
		主键用id，不用result
	-->
</resultMap>

<select id="selectAll" resultMap="brandResultMap">
    select * from tb_brand;
</select>
```

```xml
<select id="selectById" resultMap="brandResultMap">
    select * from tb_user where id = #{id};
</select>

<!--
	Brand brand = brandMapper.selectById(id);
-->
```

参数占位符：

- #{}：会替换为?，防止sql注入
- ?{}：拼sql，会存在sql注入

特殊字符（如<）可以用转义字符或CDATA区

### 核心配置文件

- environments：配置数据库连接环境信息，可配置多个environment，通过default切换
- typeAliases：别名

### 条件查询

```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select * from tb_brand where status = #{status} and company_name like #{companyName} and brand_name like #{brandName};
</select>
```

```java
// 多条件查询
List<Brand> selectByCondition(@Param("status")int status, @Param("companyName")String companyName, @Param("brandName")String brandName);
List<Brand> selectByCondition(Brand brand); // 传对象，自动找get方法
List<Brand> selectByCondition(Map map);
```

#### 动态条件查询

1. if语句，多条件动态查询

```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select * from tb_brand
    <where> 
    	<if test="status != null">and status = #{status}</if>
    	<if test="companyName != null and companyName != ''">and company_name like #{companyName}</if>
    	<if test="brandName != null and brandName != ''">and brand_name like #{brandName}</if>
    </where>
</select>
```

2.  choose(when, otherwise)语句，单条件动态查询

```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select * from tb_brand
    <choose>
    	<when test="status != null">status = #{status}</if>
    	<when test="companyName != null and companyName != ''">company_name like #{companyName}</if>
    	<when test="brandName != null and brandName != ''">brand_name like #{brandName}</if>
        <otherwise>1 = 1</otherwise>
    </choose>
</select>
```

### 添加

```xml
<insert id="add">
	insert into tb_brand (brand_name, company_name, ordered, description, status)
    values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status});
</insert>
<!--
	需要提交事务
	sqlSession.commit();
	或者
	SqlSession sqlSession = sqlSessionFactory.openSession(true);
-->
```

主键返回（插入后需要得到主键）

```xml
<insert id="add" useGeneratedKeys="true" keyProperty="id">
	insert into tb_brand (brand_name, company_name, ordered, description, status)
    values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status});
</insert>
```

### 修改

```xml
<update id="update">
	update tb_brand set brand_name = #{brandName}, company_name = #{companyName}, ordered = #{ordered}, description = #{description}, status = #{status}
    where id = #{id};
</update>
```

修改动态字段

```xml
<update id="update">
	update tb_brand
    <set> 
        <if test="brandName != null and brandName != ''">
            brand_name = #{brandName}, 
        </if>
        <if test="companyName != null and companyName != ''">
            company_name = #{companyName}, 
        </if>
        <if test="ordered != null">
            ordered = #{ordered}, 
        </if>
        <if test="description != null and description != ''">
            description = #{description}, 
        </if>
        <if test="status != null">
            status = #{status} 
        </if>
    </set>
    where id = #{id};
</update>
```

### 删除

```xml
<delete id="deleteById">
	delete from tb_brand where id = #{id}
</delete>
```

批量删除

```xml
<!--
void deleteByIds(@Param("ids") int[] ids);
-->
<delete id="deleteByIds">
	delete from tb_brand
    where id in
    <foreach collection="ids" item="id" separator="," open="(" close=")">
    	#{id}
    </foreach>
</delete>
```

