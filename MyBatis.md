# MyBatis

## 背景

实现了数据持久化的开源框架，对 JDBC 的封装

ORM：Object Relationship Mapping 对象关系映射

Java 到 MySQL 的映射，开发者可以以面向对象的思想来管理数据库

### 优点

- 减少代码量
- 最简单的持久化框架
- 相当灵活，不会对应用程序或者数据库的现有设计强加任何影响，SQL 写在 XML 里，从程序代码彻底分离，降低耦合度，便于统一管理和优化，并可重用
- 提供 XML 标签，支持编写动态 SQL 语句
- 提供映射标签，支持对象与数据库的 ORM 字段关系映射

### 缺点

- SQL 工作量大
- SQL 语句依赖数据库，数据库移植性差，不能随意换数据库

核心接口和类

SqlSessionFactoryBuilder--build-->SqlSessionFactory-->openSession-->SqlSession

## 使用

- pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.5</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.30</version>
    </dependency>
</dependencies>
```

- 新建数据库表

```sql
use mybatis;
create table t_account(
    id int primary key auto_increment,
    username varchar(11),
    password varchar(11),
    age int
)
```

- 新建实体类 Account

```java
package com.alipay.entity;

import lombok.Data;

@Data
public class Account {
    private int id;
    private String username;
    private String password;
    private int age;
}
```

- 创建 MyBatis 配置文件 config.xml，文件名可自定义

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--    配置MyBatis运行环境-->
    <environments default="development">
        <environment id="development">
<!--            配置JDBC事务管理-->
            <transactionManager type="JDBC"></transactionManager>
<!--            POOLED配置JDBC数据源连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="szgwhwjsls"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

### 使用原生接口

1. MyBatis 框架需要开发者自定义 SQL 语句，写在 Mapper.xml 中，实际开发中，会为每个实体类创建对应的 Mapper.xml，定义管理该对象数据的 SQL

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.alipay.mapper.AccountMapper">
    <insert id="save" parameterType="com.alipay.entity.Account">
        insert into t_account(username,password,age) values(#{username},#{password},#{age})
    </insert>
</mapper>
```

- namespace 通常设置为文件所在包+文件名的形式

- insert，select，update，delete 标签表示执行添加，查询，更新，删除操作
- id 是实际调用 MyBatis 方法时需要用到的参数
- parameterType 是调用对应方法时参数的数据类型

2. 在全局配置文件 config.xml 中注册 AccountMapper.xml

```xml
<!--    注册AccountMapper.xml-->
<mappers>
    <mapper resource="com/alipay/mapper/AccountMapper.xml"></mapper>
</mappers>
```

3. 调用 MyBatis 原生方法添加数据

```java
package com.alipay.test;

import com.alipay.entity.Account;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

public class Test {
    public static void main(String[] args) {
        // 加载MyBatis配置文件
        InputStream inputStream = Test.class.getClassLoader().getResourceAsStream("config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        String statement = "com.alipay.mapper.AccountMapper.save";
        Account account = new Account(1, "zhangsan", "1234", 25);
        sqlSession.insert(statement, account);
        sqlSession.commit();
    }
}
```

4. 识别 java 包中的 xml 文件，pom.xml

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```

