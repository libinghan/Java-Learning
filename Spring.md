# Spring

两大核心机制

- IoC（控制反转，Inversion of Control）/DI（依赖注入，Dependency Injection）
- AOP（面向切面编程，Aspect Oriented Programming）

软件设计层面的框架，可以将应用程序进行分层，开发者可以自主选择组件

优点

- 低侵入式设计
- 独立于各种应用服务器
- 依赖注入特性将组件关系透明化，降低了耦合度
- 面向切面编程特性允许将通用任务进行集中式处理
- 与第三方框架的良好整合

## IoC

在Spring框架中创建对象的工作不再由调用者完成，而是交给IoC容器来创建，再推送给调用者，整个流程完成反转

### 使用IoC

- 创建Maven工程，pom.xml添加依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>IoCDemo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.11.RELEASE</version>
        </dependency>
        <!-- 简化实体类的开发 -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.6</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

</project>
```

- 创建实体类

```java
package com.alipay.entity;

import lombok.Data;

@Data
public class Student {
    private long id;
    private String name;
    private int age;
}
```

- 通过IoC创建对象，在配置文件中添加，配置文件为xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="student" class="com.alipay.entity.Student">
        <property name="id" value="1"></property>
        <property name="age" value="22"></property>
        <property name="name" value="zhangsan"></property>
    </bean>
</beans>
```

- 从IoC中获取对象，通过id

```java
public class StudentTest {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Student student = (Student) applicationContext.getBean("student");
        System.out.println(student);
    }
}
```

### 配置文件

- 通过配置bean标签完成对象的管理
- id：对象名，class：对象的模板类（必须有无参构造器，因为Spring底层通过反射机制来创建对象）
- 对象的成员变量通过property标签完成复制，name：成员变量名，value：成员变量值（基本数据类型，String可以直接赋值，其他引用类型不能通过value赋值，通过ref赋值（DI））

```xml
<bean id="student" class="com.alipay.entity.Student">
    <property name="id" value="1"></property>
    <property name="age" value="22"></property>
    <property name="name" value="zhangsan"></property>
    <property name="address" ref="address"></property>
</bean>
<!-- 有参构造器，name可省略 -->
<bean id="student2" class="com.alipay.entity.Student">
    <constructor-arg name="id" value="2"></constructor-arg>
    <constructor-arg name="age" value="23"></constructor-arg>
    <constructor-arg name="name" value="lisi"></constructor-arg>
    <constructor-arg name="address" ref="address"></constructor-arg>
</bean>
<bean id="address" class="com.alipay.entity.Address">
    <property name="id" value="1"></property>
    <property name="name" value="yajule"></property>
</bean>
```

### 底层原理

- 读取配置文件，解析XML
- 通过反射机制实例化配置文件中所配置所有的bean
- 通过运行时类来获取bean，配置文件中一个数据类型的对象只能有一个实例，否则会抛出异常
- 通过有参构造创建bean，name可省略，或者用index

### IoC特性

#### 给bean注入集合

```xml
<bean id="student" class="com.alipay.entity.Student">
    <property name="id" value="1"></property>
    <property name="age" value="22"></property>
    <property name="name" value="zhangsan"></property>
    <property name="addresses">
        <list>
            <ref bean="address"></ref>
            <ref bean="address2"></ref>
        </list>
    </property>
</bean>
```

#### Scope作用域

Spring管理的bean是根据scope来生成的，标识bean的作用域，共4种

- singleton（默认）：单例，表示通过IoC容器获取的bean是唯一的
- prototype：原型，表示通过IoC容器获取的bean是不同的
- request：请求，表示在一次HTTP请求内有效
- session：会话，表示在一次用户会话内有效

request和session只适用于Web项目，大多数情况下使用单例和原型较多

#### Spring的继承

Spring是对象层面的继承，子对象可以继承父对象的属性值

```xml
<bean id="student2" class="com.alipay.entity.Student" parent="student">
    <property name="name" value="lisi"></property>
</bean>
```

Spring的继承关注点在于具体的对象，而不在于类，即不同的两个类的实例化对象可以完成继承，前提是子对象必须包含父对象的所有属性，同时可以在此基础上添加其他的属性

#### Spring的依赖

依赖也是描述bean和bean之间的一种关系，配置依赖后，先创建被依赖的bean，再创建依赖的bean

```xml
<bean id="user" class="com.alipay.entity.User" depends-on="student"></bean>
```

#### Spring的p命名空间

p命名空间是对IoC/DI的简化操作，可以更加方便地完成bean的配置以及bean之间的依赖注入

```xml
<bean id="student" class="com.alipay.entity.Student" p:id="1" p:age="22" p:name="zhangsan">
    <property name="addresses">
        <list>
            <ref bean="address"></ref>
            <ref bean="address2"></ref>
        </list>
    </property>
</bean>
```

### Spring工厂方法

IoC通过工厂模式创建bean的方式

- 静态工厂

```java
public class StaticStudentFactory {
    private static Map<Long, Student> studentMap;
    static {
        studentMap = new HashMap<Long, Student>();
        studentMap.put(1L, new Student(1L, "zhangsan"));
        studentMap.put(2L, new Student(2L, "lisi"));
    }
    public static Student getStudent(long id){
        return studentMap.get(id);
    }
}
```

```xml
<bean id="student" class="com.alipay.factory.StaticStudentFactory" factory-method="getStudent">
    <constructor-arg value="2"></constructor-arg>
</bean>
```

- 实例工厂

```xml
<bean id="studentFactory" class="com.alipay.factory.InstanceStudentFactory"></bean>
<bean id="student2" factory-bean="studentFactory" factory-method="getStudent">
    <constructor-arg value="1"></constructor-arg>
</bean>
```

```java
public class InstanceStudentFactory {
    private Map<Long, Student> studentMap;
    public InstanceStudentFactory() {
        studentMap = new HashMap<Long, Student>();
        studentMap.put(1L, new Student(1L, "zhangsan"));
        studentMap.put(2L, new Student(2L, "lisi"));
    }
    public Student getStudent(long id) {
        return studentMap.get(id);
    }
}
```

### IoC自动装载（Autowire）

IoC负责创建对象，DI完成对象的依赖注入，通过配置property标签的ref属性完成，Spring提供了自动装载方式，不需要手动配置property，容器会自动选择bean完成注入

- byName：通过属性名自动装载

```xml
<bean id="student" class="com.alipay.entity.Student" p:id="1" p:age="22" p:name="zhangsan" autowire="byName"></bean>
```

- byType：通过属性类型自动装载，如果同时存在两个及以上的符合条件的bean，自动装载会抛出异常

```xml
<bean id="student" class="com.alipay.entity.Student" p:id="1" p:age="22" p:name="zhangsan" autowire="byType"></bean>
```

## AOP

Aspect Oriented Programming：面向切面编程

