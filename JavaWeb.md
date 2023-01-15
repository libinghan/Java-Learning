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

### 注解开发

用于完成简单功能

```java
@Select("select * from tb_user where id = #{id}")
public User selectById(int id);
```

## web核心

B/S架构，浏览器/服务器架构模式

### HTTP

超文本传输协议，浏览器和服务器之间数据传输的规则

基于TCP，安全，基于请求-响应模型，无状态

请求数据格式：请求行+请求头+请求体

GET请求请求参数在请求行中，没有请求体，POST请求参数在请求体中，GET请求参数大小有限制

响应数据格式：响应行+响应头+响应体

### Tomcat

web服务器，对HTTP协议的操作进行封装，不必直接对协议进行操作，提供网上信息浏览服务

部署项目：JavaWeb项目会被打成war包，将war包放到webapps目录下，会自动解压缩war文件

### Servlet

动态web资源开发技术

JavaEE规范之一，是一个接口，需要定义Servlet类实现，由web服务器运行

- 创建web项目，导入Servlet依赖坐标（scope为provided，运行环境下不引入）

  ```xml
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
  </dependency>
  ```

- 创建：定义类实现接口，重写接口的方法

- 配置：在类上用@WebServlet注解，配置该Servlet的访问路径 @WebServlet("/demo")

- 访问：启动Tomcat，浏览器输入URL

#### 生命周期

- 加载和实例化：第一次被访问时，容器创建，可用loadOnStartup改变
- 初始化：实例化后，容器调用Servlet的init方法初始化对象，完成初始化
- 请求处理：每次请求Servlet时，容器调用service方法
- 服务终止：需要释放内存或容器关闭时，调用destroy方法

自定义Servlet，继承HttpServlet，重写doGet，doPost方法

#### urlPattern

一个Servlet可以配置多个urlPattern

@WebServlet(urlPatterns = {"/demo1", "/demo2"})

配置规则

- 精确匹配 /user/select
- 目录匹配 /user/*
- 扩展名匹配 *.do
- 任意匹配 /或者/*

## Request&Response

### Request

#### 继承体系

ServletRequest（Java提供的请求对象根接口）->HttpServletRequest（Java提供的对Http协议封装的请求对象接口）->RequestFacade（Tomcat定义的实现类）

```java
@WebServlet("/demo2")
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String name = req.getParameter("name");
        resp.setHeader("content-type", "text/html;charset=utf-8");
        resp.getWriter().write("<h1>" + name + ", welcome to servlet!");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post");
    }
}
```

Tomcat解析请求数据，封装为request方法，并且创建request对象传递到service方法中

#### 获取请求数据

请求行：getMethod，getContextPath（虚拟目录，项目访问路径），getRequestURL（获取URL，统一资源定位符），getRequestURI（获取URI，统一资源标识符），getQueryString（获取请求参数）

请求头：String getHeader(String name)，根据请求头名称获取值

请求体：ServletInputStream getInputStream()：字节输入流，BufferedReader getReader()：字符输入流

```java
BufferedReader br = req.getReader();
String line = br.readLine();
System.out.println(line);
```

统一

#### 通用方式获取请求参数

Map<String, String[]> getParameterMap()：获取所有参数Map集合

String[] getParameterValues(String name)：根据名称获取参数值（数组）

String getParameter(String name)：根据名称获取单个值

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    Map<String, String[]> map = req.getParameterMap();
    for (String key : map.keySet()) {
        System.out.print(key + ":");
        for (String value : map.get(key)) {
            System.out.print(value);
        }
        System.out.println("");
    }
}
```

#### 中文乱码

```java
req.setCharacterEncoding("UTF-8"); // POST方式
/**
GET方式
URL编码：字符串按编码方式转为二进制，每个字节转为两个16进制数并在前面加上%
先转换为字节数据，编码再解码
**/
username = new String(username.getBytes(StandardCharsets.ISO_8859_1), StandardCharsets.UTF_8);
```

#### 请求转发

forward：一种在服务器内部的资源跳转方式

```java
req.getRequestDispatcher("资源B路径").forward(req, resp);
```

请求转发资源间共享数据，Request对象的方法

```java
void setAttribute(String name, Object o) // 存储数据到request域中
Object getAttribute(String name)
void removeAttribute(String name)
```

- 浏览器地址栏路径不发生变化
- 只能转发到服务器内部资源
- 一次请求，可以在转发的资源间使用request共享数据

### Response

#### 设置响应数据

- 响应行：void setStatus(int sc)：设置响应状态码
- 响应头：void setHeader(String name, String value)：设置响应头键值对
- 响应体：printWriter getWriter()：获取字符输出流，ServletOutputStream getOutputStream()：获取字节输出流

#### 重定向

Redirect，一种资源跳转方式，资源A处理不了，响应浏览器状态码302，响应头location:xxx，浏览器再请求资源B

```java
// resp.setStatus(302);
// resp.setHeader("location", "资源B的路径");
resp.sendRedirect("资源B的路径");
```

- 浏览器地址栏路径发生变化
- 可以重定向到任意位置的资源
- 两次请求，不能在多个资源使用request共享数据

#### 路径问题

浏览器使用：需要加虚拟目录（项目访问路径）

服务端使用：不需要加虚拟目录

动态获取虚拟目录：request.getContextPath()

#### 响应体

响应字符

```java
// 响应数据为html格式，获取字符输出流，流不需要手动关闭
// resp.setHeader("content-type", "text/html;charset=utf-8");
resp.setContentType("text/html;charset=utf-8");
resp.getWriter().write("<h1>" + username + ", welcome to servlet!</h1>");
```

响应字节

```java
FileInputStream fis = new FileInputStream("d://a.jpg");
ServletOutputStream os = resp.getOutputStream();
/*
byte[] buff = new byte[1024];
int len = 0;
while ((len = fis.read(buff)) != -1) {
    os.write(buff, 0, len);
}
*/
// 可引入commons-io依赖
IOUtils.copy(fis, os); // 输入流，输出流
fis.close();
```

### SqlSessionFactory工具类抽取

```java
public class SqlSessionFactoryUtils {
    private static SqlSessionFactory sqlSessionFactory;
    // 静态代码块随着类的加载自动执行，且只执行一次
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static SqlSessionFactory getSqlSessionFactory() {
        return SqlSessionFactory;
    }
}

SqlSessionFactory sqlSessionFactory = SqlSessionFactoryUtils.getSqlSessionFactory();
SqlSession sqlSession = sqlSessionFactory.openSession();
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
User u = userMapper.selectByUsername(username);
```

## JSP

Java Server Pages，Java服务端页面，动态的网页技术，既可以定义HTML，JS，CSS等静态内容，还可以定义Java代码的动态内容，本质上就是一个Servlet

导入JSP坐标（scope为provided）->创建JSP文件->编写HTML标签和Java代码

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2</version>
    <scope>provided</scope>
</dependency>
```

### JSP脚本

- <%...%>：内容会直接放到_jspService()方法中
- <%=...%>：内容会放到out.print()中，作为out.print()的参数
- <%!...%>：内容会放到_jspService()方法之外，被类直接包含

```jsp
<%
	for (int i = 0; i < brands.size(); i++) {
        Brand brand = brands.get(i);
%>
<tr align="center">
	<td><%=brand.getId()%></td>
    <td><%=brand.getBrandName()%></td>
    <td><%=brand.getOrdered()%></td>
</tr>
<%
	}
%>
```

### 缺点

书写麻烦，阅读麻烦，复杂度高，占内存和磁盘，调试困难，不利于团队合作

HTML+AJAX

### EL表达式

${expression}：获取域中存储的key为brands的数据

四大域对象：page当前页面有效，request当前请求有效，session当前会话有效，application当前应用有效，会依次从这四个域中寻找，直到找到为止

### JSTL标签

JSP标准标签库，使用标签取代JSP页面上的Java代码

```jsp
<c:if test="${flag == 1}">
    男
</c:if>
<c:if test="${flag == 2}">
	女
</c:if>
```

步骤

1. 导入坐标

   ```xml
   <dependency>
       <groupId>jstl</groupId>
       <artifactId>jstl</artifactId>
       <version>1.2</version>
   </dependency>
   <dependency>
       <groupId>taglibs</groupId>
       <artifactId>standard</artifactId>
       <version>1.1.2</version>
   </dependency>
   ```

2. 在JSP页面引入JSTL标签库

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   ```

3. 使用

   ```jsp
   <c:if test="${status == 1}">
   	启用
   </c:if>
   ```

   ```jsp
   <%--items：被遍历的容器，var：遍历产生的临时变量，varStatus：遍历状态--%>
   <c:forEach items="${brands}" var="brand" varStatus="status">
   	<tr align="center">
       	<td>${status.count}</td>
           <td>${brand.brandName}</td>
           <td>${brand.companyName}</td>
       </tr>
   </c:forEach>
   <c:forEach begin="0" end="10" step="1" var="i">
   	${i}
   </c:forEach>
   ```

## MVC模式

分成开发的模式

- M：Model，业务模型，处理业务（JavaBean）
- V：View，视图，界面展示（JSP）
- C：Controller，控制器，处理请求，调用模型和视图（Servlet）

职责单一，互不影响，有利于分工协作，有利于组件重用

## 三层架构

对MVC模式的实现

- 数据访问层：对数据库的CRUD基本操作（com.alipay.dao/mapper），MyBatis/Hibername
- 业务逻辑层：对业务逻辑进行封装，组合数据访问层层中基本功能，形成复杂的业务逻辑功能（com.alipay.service），Spring
- 表现层：接收请求，封装数据，调用业务逻辑层，响应数据（com.alipay.web/controller），SpringMVC/Struts2，View+Controller

## 会话跟踪技术

用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接，会话结束，一次会话中包含多次请求和响应

会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间共享数据

HTTP是无状态的，每次浏览器发送请求，服务器都视为新的请求，因此需要会话跟踪技术实现会话间数据共享

- 客户端会话跟踪技术：Cookie
- 服务端会话跟踪技术：Session

### Cookie

将数据保存在客户端，以后每次请求都携带Cookie数据进行访问

发送Cookie

```java
// 创建Cookie对象，设置数据
Cookie cookie = new Cookie("key", "value");
// 发送Cookie到客户端，使用response对象
reponse.addCookie(cookie);
```

获取Cookie

```java
// 获取客户端携带的所有Cookie，使用request对象
Cookie[] cookies = request.getCookies();
// 遍历数组，获取每一个Cookie对象：for
// 使用Cookie对象方法获取数据
cookie.getName();
cookie.getValue();
```

基于HTTP协议，响应头：set-cookie，请求头：cookie

#### 存活时间

默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁

cookie.setMaxAge(int seconds)：设置Cookie存活时间

- 正数：将Cookie写入浏览器所在电脑的硬盘，持久化存储，到时间自动删除
- 负数：默认值
- 零：删除对应Cookie

#### 存储中文

不能直接存储中文，需要存储，要转码（URL编码）

### Session

将数据保存在服务端，使用HttpSession接口

获取Session对象

```java
HttpSession session = request.getSession();
```

- void setAttribute(String name, Object o)：存储数据到Session中
- Object getAttribute(String name)
- void removeAttribute(String name)

Session是基于Cookie实现的

#### 钝化活化

- 钝化：在服务器正常关闭后，Tomcat会自动将Session数据写入硬盘的文件中

- 活化：再次启动后，会从文件中加载数据到Session中

#### 销毁

默认情况下，无操作，30min自动销毁

```xml
<session-config>
	<session-timeout>100</session-timeout>
</session-config>
```

调用Session对象的invalidate()方法

### 区别

- Cookie在客户端，Session在服务端
- Cookie不安全，Session安全
- Cookie最大3KB，Session无限制
- Cookie可以长期存储，Session默认30分钟
- Cookie不占用服务器资源，Session占用
