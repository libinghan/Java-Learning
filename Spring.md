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

### 使用方式

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

优点

- 降低模块之间的耦合度
- 使系统更容易扩展
- 更好的代码复用
- 非业务代码更集中，便于统一管理
- 非业务代码更加简洁纯粹，不掺杂其他代码的影响

将复杂的需求分解出不同方面，将散布在系统中的公共功能集中解决，在运行时，动态地将代码切入到类的指定方法，指定位置（如把日志，安全，持久层提取出来）

AOP是对面向对象编程的一个补充，运行时，动态地将代码切入到类的指定方法、指定位置上。将不同方法的同一个位置抽象成一个切面对象，对该切面对象进行编程就是AOP。

### 使用方式

- 创建maven工程，创建依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.0.11.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.0.11.RELEASE</version>
</dependency>
```

#### 动态代理实现

- 使用动态代理的方式来实现AOP，业务代码只需要关注自身的业务

```java
package com.alipay.utils;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;

public class MyInvocationHandler implements InvocationHandler {
    // 接收委托对象
    private Object object = null;
    // 返回代理对象
    public Object bind(Object object) {
        this.object = object;
        return Proxy.newProxyInstance(object.getClass().getClassLoader(), object.getClass().getInterfaces(), this);
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println(method.getName() + "方法的参数是" + Arrays.toString(args));
        Object result = method.invoke(this.object, args);
        System.out.println(method.getName() + "方法的结果是" + result);
        return result;
    }
}
```

- 代理对象调用方法

```java
package com.alipay.test;

import com.alipay.utils.Cal;
import com.alipay.utils.MyInvocationHandler;
import com.alipay.utils.impl.CalImpl;

public class MyInvocationHandlerTest {
    public static void main(String[] args) {
        Cal cal = new CalImpl();
        MyInvocationHandler myInvocationHandler = new MyInvocationHandler();
        Cal proxy = (Cal) myInvocationHandler.bind(cal);
        proxy.add(1, 1);
    }
}
```

#### Spring实现

Spring框架对AOP进行了封装，使用Spring框架可以用面向对象的思想实现AOP，只需要创建一个切面对象，将所有的非业务代码在切面对象中完成即可，Spring框架底层会自动根据切面以及目标类生成一个代理对象

```java
package com.alipay.aop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Aspect
@Component
public class LoggerAspect {
    @Before("execution(public int com.alipay.utils.impl.CalImpl.*(..))")
    public void before(JoinPoint joinPoint) {
        // 获取方法名
        String name = joinPoint.getSignature().getName();
        // 获取参数
        String args = Arrays.toString(joinPoint.getArgs());
        System.out.println(name + "方法的参数是" + args);
    }
    @After("execution(public int com.alipay.utils.impl.CalImpl.*(..))")
    public void after(JoinPoint joinPoint) {
        String name = joinPoint.getSignature().getName();
        System.out.println(name + "方法执行完毕");
    }

    /**
     *
     * @param joinPoint
     * @param result
     *
     * returning的值和afterReturning方法中的形参名相同即可，无需和委托对象方法中的返回值名相同
     */
    @AfterReturning(value = "execution(public int com.alipay.utils.impl.CalImpl.*(..))", returning = "result")
    public void afterReturning(JoinPoint joinPoint, Object result) {
        String name = joinPoint.getSignature().getName();
        System.out.println(name + "方法的结果是" + result);
    }
    @AfterThrowing(value = "execution(public int com.alipay.utils.impl.CalImpl.*(..))", throwing = "exception")
    public void afterException(JoinPoint joinPoint, Exception exception) {
        String name = joinPoint.getSignature().getName();
        System.out.println(name + "方法抛出了异常：" + exception);
    }
}
```

LoggerAspect类定义处添加两个注解：

- @Aspect：表示该类是切面类
- @Component：表示将该类的对象注入到IoC容器

具体方法处添加的注解：

@Before：表示方法执行的具体位置和时机

委托类（CallImpl）也需要添加Component注解，交给IoC容器来管理

- spring.xml中配置AOP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
<!--    自动扫描-->
    <context:component-scan base-package="com.alipay"></context:component-scan>
<!--    使aspect注解生效，为目标类自动生成代理对象-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

`context:component-scan`是将com.alipay包中的所有类进行扫描，如果该类同时添加了@Component，则将该类扫描到IoC容器中，即IoC管理它的对象

`aop:aspectj-autoproxy`让Spring框架结合切面类和目标类自动生成动态代理对象

<font color="red">**注意：委托类前面也要加@Component**</font>，创建的bean默认名是类名首字母小写，@Component("test")则创建的bean名为test

```java
package com.alipay.test;

import com.alipay.utils.Cal;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class LoggerAspectTest {
    public static void main(String[] args) {
        // 加载配置文件
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-aop.xml");
        Cal proxy = (Cal) applicationContext.getBean("calImpl");
        proxy.add(1, 1);
    }
}
```

- 切面：横切关注点被模块化的抽象对象（LoggerAspect）
- 通知：切面对象完成的工作（非业务代码）
- 目标：被通知（即被横切的对象）（计算器）
- 代理：切面、通知、目标混合之后的对象（proxy）
- 连接点：通知要插入业务代码的具体位置（joinpoint）
- 切点：AOP通过切点定位到连接点