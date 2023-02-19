

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

# Spring MVC

目前主流的实现MVC设计模式的框架，是Spring框架的一个分支产品，以Spring IoC容器为基础，并利用容器的特性简化配置。相当于Spring的子模块，可以和Spring结合开发

Controller接收客户端请求，调用Model生成业务数据，传递给View，Spring MVC对这套流程进行了封装

## 核心组件

- DispatcherServlet：前置控制器，整个流程控制的核心，控制其他组件的执行，进行统一调度，降低组件之间的耦合性，相当于总指挥
- Handler：处理器，完成具体的业务逻辑，相当于Servlet或Action
- HandlerMapping：DispatcherServlet接收到请求之后，通过HandlerMapping将不同的请求映射到不同的Handler
- HandlerInterception：处理器拦截，是一个接口，如果需要完成一些拦截处理，可以实现该接口
- HandlerExecutionChain：处理器执行链，包括Handler和HandlerInterception（系统有一个默认的，若要额外设置拦截，可以添加拦截器）
- HandlerAdapter：处理器适配器，Handler执行业务方法之前，需要进行一系列的操作，包括表单数据的验证、数据类型的转换，将表单数据封装到JavaBean等，这些操作都由HandlerAdapter完成，DispatcherServlet通过HandlerAdapter执行不同的Handler
- ModelAndView：装载了模型数据和视图信息，作为Handler的处理结果返回给DispatcherServlet
- ViewResolver：视图解析器，DispatcherServlet通过它将逻辑视图解析为物理视图，最终将渲染结果响应给客户端

## Spring MVC的工作流程

- 客户端请求被DispatcherServlet接收
- 根据HandlerMapping映射到Handler
- 生成Handler和HandlerInterception
- Handler和HandlerInterception以HandlerExecutionChain的形式一并返回给DispatcherServlet
- DispatcherServlet通过HandlerAdapter调用Handler方法完成业务逻辑处理
- Handler返回一个ModelAndView给DispatcherServlet
- DispatcherServlet将获取的ModelAndView对象传给ViewResolver视图解析器，将逻辑视图解析为物理视图View
- ViewResolver返回一个View给DispatcherServlet
- DispatcherServlet根据View进行视图渲染，将模型数据Model填充到视图View中
- DispatcherServlet将渲染后的结果响应给客户端

![spring mvc流程图](https://img-blog.csdnimg.cn/img_convert/1af3f4cbeb45e022bbfd4bf4b1c7d7dd.png)

流程复杂但实际开发简单，大部分组件不需要创建管理，只需要通过配置文件的方式完成配置即可，真正需要开发者进行处理的只有Handler和View

## 使用方法

- 创建maven工程，pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.0.11.RELEASE</version>
    </dependency>
</dependencies>
```

- 在web.xml中配置DispatcherServlet

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

- springmvc.xml

```java
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
<!--        自动扫描-->
    <context:component-scan base-package="com.alipay"></context:component-scan>
<!--    配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
</beans>
```

- 创建Handler

```java
package com.alipay.handler;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping(value = "/hello", method = RequestMethod.GET, params = {"name", "id=10"})
public class HelloHandler {
    @RequestMapping("/index")
    public String index() {
        System.out.println("执行了index");
        return "index";
    }
}
```

## 注解

- @RequestMapping：通过该注解将URL请求与业务方法进行映射，在Handler的类定义处以及方法定义处都可以添加，在类定义处添加，相当于客户端多了一层访问路径
  - 相关参数：value（指定URL请求的实际地址，是默认值），method（RequestMethod.GET，RequestMethod.POST，RequestMethod.PUT，RequestMethod.DELETE），params（请求中必须包含的参数）

```java
@Controller
@RequestMapping(value = "/hello")
public class HelloHandler {
    @RequestMapping(value = "/index", method = RequestMethod.GET, params = {"name", "id"})
    public String index(String name, int id) {
        System.out.println(name + id + "号执行了index");
        return "index";
    }
}
```

上述代码将请求的参数name，id分别赋给了形参str，id，同时自动完成了数据类型转换，这些工作都是由HandlerAdapter完成的

- @Controller：在类定义中添加，将该类交给IoC容器来管理（结合springmvc.xml中的自动扫描配置使用），同时使其成为一个控制器，可以接收客户端请求

Spring MVC也支持RESTful风格的URL

传统类型：http://localhost:8080/hello/index?name=libinghan&id=10

RESTful类型：http://localhost:8080/hello/index/libinghan/10

```java
@RequestMapping("/rest/{name}/{id}")
public String rest(@PathVariable("name") String name, @PathVariable("id") int id){
    System.out.println(name + id + "号执行了index");
    return "index";
}
```

通过@PathVariable注解完成请求参数与形参的映射

- 映射Cookie

Spring MVC通过映射可以直接在业务方法中获取Cookie的值

```java
@RequestMapping("/cookie")
public String cookie(@CookieValue("JSESSIONID") String sessionId) {
    System.out.println(sessionId);
    return "index";
}
```

- 使用JavaBean绑定参数

Spring MVC会根据请求参数名和JavaBean属性名进行自动匹配，自动为对象填充属性值，同时支持级联属性

```java
package com.alipay.entity;

import lombok.Data;

@Data
public class Address {
    private String value;
}
```

```java
package com.alipay.entity;

import lombok.Data;

@Data
public class User {
    private long id;
    private  String name;
    private Address address;
}
```

```jsp
<%--
  Created by IntelliJ IDEA.
  User: lbh01
  Date: 2023/2/12
  Time: 15:17:59
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<form action="/hello/save" method="post">
    用户id：<input type="text" name="id"/><br>
    用户名：<input type="text" name="name"/><br>
    用户地址：<input type="text" name="address.value"/><br>
    <input type="submit" value="注册">
</form>
</body>
</html>
```

```java
@RequestMapping(value = "/save", method = RequestMethod.POST)
public String save(User user){
    System.out.println(user);
    return "index";
}
```

如果出现中文乱码问题，在web.xml中添加Spring MVC自带的过滤器即可

```xml
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/</url-pattern>
</filter-mapping>
```

- JSP页面的转发和重定向

Spring MVC默认以转发的形式响应JSP

```java
@RequestMapping("/forward")
public String forward() {
    return "forward:/index.jsp";
}

@RequestMapping("/redirect")
public String redirect() {
    return "redirect:/index.jsp";
}
```

## Spring MVC数据绑定

数据绑定：在后端的业务方法中直接获取客户端HTTP请求中的参数，将请求参数映射到业务方法的形参中，Spring MVC中数据绑定的工作由HandlerAdapter绑定

```java
@Controller
@RequestMapping("/data")
public class DataBindHandler {
    @RequestMapping("/baseType")
    // 直接将返回值响应给客户端，不经过视图解析器
    @ResponseBody
    public String baseType(int id) {
        return id + "";
    }
}
```

注：上述写法必须传id，否则会报错（id为null），可以使用包装类解决该问题（Integer id）

```java
@RequestMapping("/packageType")
@ResponseBody
public String packageType(@RequestParam(value = "num", required = false, defaultValue = "0") Integer id) {
    return id + "";
}
```

@RequestParam：value = "num"：将HTTP请求中名为num的参数赋给形参id，defaultValue：若没有num参数，默认值为0

### 数组

```java
@RestController
@RequestMapping("/data")
public class DataBindHandler {
    @RequestMapping("/array")
    public String array(String[] name) {
       String str = Arrays.toString(name);
       return str;
    }
}
```

@RestController表示该控制器会直接将业务方法的返回值返回给客户端，不进行解析

@Controller表示该控制的每一个业务方法的返回值都会交给视图解析器。如果只需要将数据响应给客户端，而不需要进行视图解析，则需要在对应的业务方法定义处添加@ResponseBody

### List

Spring MVC不支持List类型的直接转换，需要对List集合进行包装

集合封装类

```java
@Data
public class UserList {
    private List<User> users;
}
```

JSP

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/data/list" method="post">
      用户1编号：<input type="text" name="users[0].id"/><br>
      用户1名称：<input type="text" name="users[0].name"/><br>
      用户2编号：<input type="text" name="users[1].id"/><br>
      用户2名称：<input type="text" name="users[1].name"/><br>
      用户3编号：<input type="text" name="users[2].id"/><br>
      用户3名称：<input type="text" name="users[2].name"/><br>
      <input type="submit" value="提交">
    </form>
</body>
</html>
```

业务方法

```java
@RequestMapping("/list")
public String list(UserList userList) {
    StringBuffer str = new StringBuffer();
    for (User user : userList.getUsers()) {
        str.append(user);
    }
    return str.toString();
}
```

处理@ResponseBody中文乱码，在springmvc.xml中配置消息转换器

```xml
<!--    消息转换器-->
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

### Map

自定义封装类

```java
@Data
public class UserMap {
    private Map<String, User> users;
}
```

业务方法

```java
@RequestMapping("/map")
public String map(UserMap userMap) {
    StringBuffer str = new StringBuffer();
    for (String key : userMap.getUsers().keySet()) {
        User user = userMap.getUsers().get(key);
        str.append(user);
    }
    return str.toString();
}
```

JSP

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<body>
<form action="/data/map" method="post">
    用户1编号：<input type="text" name="users['a'].id"/><br>
    用户1名称：<input type="text" name="users['a'].name"/><br>
    用户2编号：<input type="text" name="users['b'].id"/><br>
    用户2名称：<input type="text" name="users['b'].name"/><br>
    用户3编号：<input type="text" name="users['c'].id"/><br>
    用户3名称：<input type="text" name="users['c'].name"/><br>
    <input type="submit" value="提交">
</form>
</body>
</body>
</html>

```

### JSON

客户端发送JSON格式的数据，直接通过Spring MVC绑定到业务方法的形参中

处理Spring MVC无法加载静态资源，在web.xml中添加配置即可

```xml
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.js</url-pattern>
</servlet-mapping>
```

```jsp
<%--
  Created by IntelliJ IDEA.
  User: lbh01
  Date: 2023/2/12
  Time: 23:47:29
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="js/jquery-3.6.3.min.js"></script>
    <script type="text/javascript">
        $(function (){
            var user = {
                "id": 1,
                "name": "zhangsan"
            };
            $.ajax({
                url: "data/json",
                data: JSON.stringify(user),
                type:"POST",
                contentType: "application/json;charset=UTF-8",
                dataType: "JSON",
                success: function (data) {
                    alert(data.id + "---" + data.name)
                }
            })
        });
    </script>
</head>
<body>

</body>
</html>
```

业务方法

```java
@RequestMapping("/json")
public User json(@RequestBody User user) {
    System.out.println(user);
    user.setName("zhangliu");
    return user;
}
```

Spring MVC中的JSON和JavaBean转换需要借助于fastjson，添加fastjson相关依赖

- pom.xml

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.32</version>
</dependency>
```

- springmvc.xml

```xml
<!--    消息转换器-->
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
        </bean>
        <!--            配置fastjson-->
        <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter4"></bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

## Spring MVC模型数据解析

JSP 四大作用域对应的内置对象：pageContext，request，session，application

模型数据的绑定由ViewResolver完成，实际开发中，要先添加模型数据，再交给ViewResolver绑定

Spring MVC提供了以下几种方式添加模型数据：

- Map
- Model
- ModelAndView
- @SessionAttribute
- @ModelAttribute

### Map

```java
@RequestMapping("/map")
public String map(Map<String, User> map) {
    User user = new User();
    user.setId(1L);
    user.setName("zhangsan");
    map.put("user", user);
    return "view";
}
```

JSP

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${requestScope.user}
</body>
</html>
```

### Model

```java
@RequestMapping("/model")
public String model(Model model) {
    User user = new User();
    user.setId(1L);
    user.setName("zhangsan");
    model.addAttribute("user", user);
    return "view";
}
```

### ModelAndView

```java
@RequestMapping("/modelAndView7")
public ModelAndView modelAndView7() {
    User user = new User();
    user.setId(1L);
    user.setName("zhangsan");
    ModelAndView modelAndView = new ModelAndView("view", "user", user);
    return modelAndView;
}
```

### HttpServletRequest

```java
@RequestMapping("/request")
public String request(HttpServletRequest request) {
    User user = new User();
    user.setId(1L);
    user.setName("zhangsan");
    request.setAttribute("user", user);
    return "view";
}
```

### @ModelAttribute

定义一个方法，专门用来返回要填充到模型数据中的对象（无论进哪个业务方法都会先走这个注解的方法）

```java
@ModelAttribute
public User getUser() {
    User user = new User();
    user.setId(1L);
    user.setName("zhangsan");
    return user;
}
```

```java
@ModelAttribute
// map，model都行
public void getUser(Map<String, User> map) {
    User user = new User();
    user.setId(1L);
    user.setName("zhangsan");
    map.put("user", user);
}
```

业务方法中无需再处理模型数据，只需返回视图即可

```java
@RequestMapping("/modelAttribute")
public String modelAttribute() {
    return "view";
}
```

### @SessionAttribute

```java
//@SessionAttributes(value = {"user", "address"})
@SessionAttributes(types = {User.class, Address.class})
public class ViewHandler {
}
```

对于ViewHandler中的所有业务方法，只要向request中添加了key="user"，key="address"的对象时，Spring MVC会自动将该数据添加到Session中，保持key不变

对于ViewHandler中的所有业务方法，只要向request中添加了数据类型是User，Address的对象时，Spring MVC会自动将该数据添加到session中，保持key不变

原生方法

```java
@RequestMapping("/session")
public String session(HttpSession session) {
    User user = new User();
    user.setId(1L);
    user.setName("zhangsan");
    session.setAttribute("user", user);
    return "view";
}
```

### 绑定到Application对象

```java
@RequestMapping("/application")
public String application(HttpServletRequest request){
    ServletContext application = request.getServletContext();
    User user = new User();
    user.setId(1L);
    user.setName("zhangsan");
    application.setAttribute("user", user);
    return "view";
}
```

## Spring MVC自定义数据转换器

数据转换器是指将客户端HTTP请求中的参数转换为业务方法中的形参，自定义表示开发者可以自主设计转换的方式，HandlerAdapter已经提供了通用的转换，String转int，String转double，表单数据的封装等，但是在特殊的业务场景下，HandlerAdapter无法进行转换，就需要开发者自定义转换器

客户端输入String类型2023-02-19，自定义转换器将该数据转换为Date类型对象

- 创建DateConverter转换器实现Converter接口

```java
package com.alipay.converter;

import org.springframework.core.convert.converter.Converter;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateConverter implements Converter<String, Date> {
    private String pattern;
    public DateConverter(String pattern){
        this.pattern = pattern;
    }
    @Override
    public Date convert(String s) {
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(this.pattern);
        Date date = null;
        try {
            date = simpleDateFormat.parse(s);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return date;
    }
}

```

- springmvc.xml中配置转换器

```xml
<!--配置自定义转换器-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <bean class="com.alipay.converter.DateConverter">
                <constructor-arg type="java.lang.String" value="yyyy-MM-dd"></constructor-arg>
            </bean>
        </list>
    </property>
</bean>
<!--    消息转换器-->
<mvc:annotation-driven conversion-service="conversionService">
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
        </bean>
        <!--            配置fastjson-->
        <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter4"></bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

JSP

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/converter/date" method="post">
        请输入日期：<input type="text" name="date">(yyyy-MM-dd)
        <input type="submit" value="提交">
    </form>
</body>
</html>		
```

Handler

```java
package com.alipay.handler;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;

@RestController
@RequestMapping("/converter")
public class ConverterHandler {
    @RequestMapping("/date")
    public String date(Date date) {
        return date.toString();
    }
}
```

### Spring MVC REST

REST：Representational State Transfer，资源表现层状态转换，是目前比较主流的一种互联网软件架构，结构清晰，标准规范，易于理解，便于扩展

- 资源Resource：网络上的一个实体，如一张图片，可以用一个URI（统一资源定位符）指向它，每个资源都有对应的一个特定的URI
- 
