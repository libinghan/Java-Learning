# Java

# 背景知识

sun公司，95年

早期成为oak（橡树）

Java之父，詹姆斯·高斯林

09年后，oracle（甲骨文）

## 特点

- 最流行
- 可移植性、安全可靠、性能较好
- 开发社区最完善，功能最丰富

# 安装及常用命令

javac 编译

java 执行

# Java入门

## Hello World

编写代码=（javac）=》编译代码=（java）=》运行代码

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

javac HelloWord.java==>java HelloWorld

## JDK组成、跨平台原理

### JDK组成

- JVM(Java Virtual Machine)：Java虚拟机，运行Java的地方
- 核心类库：Java写好的程序
- JRE(Java Runtime Environment)：Java运行环境（JVM+核心类库）
- JDK(Java Development Kit)：Java开发工具包（JVM+核心类库+开发工具（java，javac））

### 跨平台工作原理

一次编译，处处可用，各平台有对应的JVM

### PATH环境变量

Path %JAVA_HOME%\bin

## IDE

Integreted Development Environment 集成开发环境

破解版：https://www.exception.site/essay/idea-reset-eval

### IDEA项目结构

project（项目、工程）=>module（模块）=>package（包）=>class（类）

### IDEA快捷键

- main
- sout
- ctrl+D 复制一行
- ctrl+Y/X 删除一行
- ctrl+alt+l 格式化
- alt+shift+up/down
- ctrl+/ / ctrl+shift+/

## Java语法

### 注释

// 单行

/* */ 多行

/** */ 文档注释

### 字面量

各种变量的书写格式

### 变量

变量是用来存储一个数据的内存区域，里面存储的数据可以变化

格式：数据类型 变量名称 = 初始值;

要先声明再使用；变量声明后不能存其他类型；变量声明从定义到对应右大括号结束；同一范围内不能定义同名变量；定义可以没有初值，但使用时要

### 变量在计算机中的底层原理

8个二进制位（字节）为一组，计算机最小的组成单元

字符：ASCII码

图片：每个像素点的RGB

## 进制

0B/0b 二进制

0 八进制

0X/0x 十六进制

## 数据类型

### 基本数据类型

| 数据类型 | 关键字         | 取值范围         | 内存占用（字节） |
| -------- | -------------- | ---------------- | ---------------- |
| 整数     | byte           | -128~127         | 1                |
|          | short          | -32768~32767     | 2                |
|          | int（默认）    | 10位数           | 4                |
|          | long           | 19位数           | 8                |
| 浮点数   | float          | 1.4e-45~3.4e+38  | 4                |
|          | double（默认） | 4.9e-324~1.8e308 | 8                |
| 字符     | char           | 0-65535          | 2                |
| 布尔     | boolean        | true, false      | 1                |

随便写一个整数默认是int类型的，若希望是long类型，末尾写个L/l

随便写一个小数默认是double类型的，若希望是float类型，末尾写个F/f

### 引用数据类型

String

## 关键字和标识符

不能做变量

### 标识符的要求：

由数字、字母、下划线和美元符组成

不能以数字开头，不能是关键字，区分大小写

#### 变量名称

建议全英文、有意义、首字母小写，满足驼峰模式

#### 类名称

建议全英文、有意义、首字母大写，满足驼峰模式

## Java语言

### 基础知识

#### 自动类型转换

类型范围小的变量，可以直接赋值给类型范围大的变量（byte赋值给int）

byte、short、char=>int=>long=>float=>double

表达式中，小范围类型变量会自动转成较大的再运算，最终结果由最高类型决定，byte、short、char直接转成int

#### 强制类型转换

byte b = (byte)a;

#### 运算符

+号能算则算，不能算就放一起

##### 自增自减

```java
int a = 10;
int rs = ++a; //11
int rs = a++; //10
```

```java
int k = 3;
int p = 5;
int rs = k++ + ++k - --p + p-- - k-- + ++p + 2;
//  k    4       5               4
//  p                  4   3             4
//  rs   3   +   5 -   4 + 4   - 5   +   4 + 2 = 9
```

##### 赋值

a += b （会强制转换为a的类型）

```java
int j = 5;
int i;
System.out.println(i = j); // 5
```

##### 逻辑运算符

&，|，!，^

与或非异或

&&，||

短路与，短路或，性能好一些，左边符合则右边不执行

实际开发中，&&、||和!比较常用

##### 三元运算符

条件表达式?表达式1:表达式2

##### 运算符优先级

先与后或，赋值最低

#### 键盘录入

API 应用程序编程接口，java写好的程序

1. 导包`import java.util.Scanner;`

2. 写一行代码代表得到键盘扫描器对象`Scanner sc = new Scanner(System.in);`
3. 等待接收用户输入数据`int age = sc.nextInt();`

### 程序流程控制

- 顺序结构
- 分支结构：if、switch
- 循环结构： for、while、do...while

#### if语句

适合区间匹配

```java
if (condition) {
    语句;
} else if (condition) {
    语句;
} else {
    语句;
}
```

#### switch语句

适合值匹配，**只支持byte、short、int、char，不支持double、float、long，jdk7开始支持string**

```
switch (表达式) {
	case 值1:
		表达式;
		break;
	case ...:
		表达式;
		break;
	default:
		表达式;
}
```

case给的值不能重复，且只能是字面量，不能是变量，不写break会穿透，可以解决代码冗余

#### for语句

```java
for (初始化语句;循环条件;迭代语句){
	循环语句;
}
```

#### while语句

```java
while (循环条件){
	循环语句;
	迭代语句;
}
```

知道循环次数，用for，否则用while

#### do-while语句

```java
do{
	循环语句;
    迭代语句;
} while (循环条件)
```

一定会执行一次循环体，如抢票

#### 死循环、循环嵌套

#### 跳转控制语句

break、continue

### 随机数random

```java
import java.util.Random;

public class RandomDemo {
    public static void main(String[] args) {
        Random r = new Random();
        for (int i = 0; i < 10; i++) {
            int data = r.nextInt(10);
            System.out.print(data + " ");
        }
    }
}
```

ctrl+alt+t，可以把几行放进循环体

生成65-91之间的随机数：

```java
int number = r.nextInt(27) + 65;
```

### 数组

#### 定义及表示

存储同类型数据的内存区域（容器）

```java
int[] arr = {1, 2, 3, 4, 5};
String[] alph = {"a", "b", "c", "d", "e"};
```

#### 静态初始化数组

数组类型[] 数组名 = new 数组类型[]{元素1, 元素2, ...};

简化写法：

数组类型[] 数组名 = {元素1,元素2,...};

数组变量名存储的是数组在内存中的地址，数组是**引用类型**

#### 动态初始化数组

```java
int[] arr = new int[3]; //数字和字符默认值为0，String等引用类型默认值为null，boolean值为false
```

#### 数组访问

```java
int[] arr = {5, 2, 4, 3, 6};
for (int i = 0; i < arr.length; i++) {
    System.out.print(arr[i]+" ");
}
// 快捷键 arr.fori
```

#### 数组内存图

- 栈：方法运行时所进入的内存，变量
- 堆：new出来的东西会在这块内存中开辟空间并产生地址
- 方法区：字节码文件加载时进入的内存（.class）
- 本地方法栈
- 寄存器

#### 常见问题

浅拷贝：数组直接赋值会发生浅拷贝，只复制了地址，一个改变会影响另一个

ArrayIndexOutOfBoundsException（数组索引越界异常）

NullPointerException（空指针异常）

### 方法

一种语法结构，把一段代码封装成一个功能，以方便重复调用，让程序逻辑更清晰

```java
public static int sum(int a, int b){
    int c = a + b;
    return c;
}
```

#### 定义

修饰符 返回值类型 方法名(形参列表){

​	方法体代码

​	return 返回值;

}

没有返回值则void，也可以没有形参

#### 常见问题

1. 方法的编写顺序无所谓
2. 方法之间平级，不能嵌套

3. 传参数组，int[]

#### 内存原理

没被调用的时候在方法区的字节码文件中存放，调用时进入栈内存中运行

#### 参数传递

##### 值传递

传输实参给方法的形参（定义方法时声明的参数）时，不传输实参本身，而是传输实参变量中存储的值

##### 引用传递

传数组地址，会改变原来的值

```java
System.out.print(i == arr.length - 1 ? arr[i] : arr[i] + ",");
```

#### 方法重载

同一类中，多个方法名称相同，但形参列表（个数、类型、顺序，名称无所谓）不同，那么这些方法是重载方法。**不会冲突**。

- 可读性好，通过形参不同实现功能的差异化选择

#### 单独使用return

可以立即跳出并结束当前方法的执行

## 面向对象

对象是类的实例

### 设计对象

定义类

```java
public class 类名 {
    成员变量（属性）;
    成员方法（行为）;
    构造器;
    代码块;
    内部类;
}
```

创建对象

```java
Car c = new Car();
```

适用对象

```java
对象名.成员变量
对象名.成员方法()
```

#### 注意事项

- 首字母大写，驼峰模式
- 一个Java文件可以定义多个类，但只能有一个public类，和文件名相同（最好一个文件一个类）
- 可以有对象数组`Goods[] shopCar = new Goods[100];`

### 内存机制

堆内存中不放方法代码，而是成员方法引用地址

调用方法时，栈内存=>堆内存中的地址=>方法区=>加载到栈内存执行

垃圾回收

### 构造器

- 知道对象具体通过调用什么代码得到的
- 掌握为对象赋值的简便方法
- 为以后学习面向对象编程做支撑

定义在类中的，可以用于初始化一个类的对象，并返回对象地址

```java
public class Car {
    String name;
    double price;
    // 无参数构造器，可以不写
    public Car() {
        ...
    }
    // 有参数构造器
    public Car(String n, double p){
        name = n;
        price = p;
    }
}
Car Car1 = new Car("奔驰", 100);
```

- 默认有无参数构造器，可不写
- 一旦有了有参数构造器，无参数构造器就没有了，还要用就要手写一个

### this关键字

出现在构造器和方法中，代表当前对象的地址，可以指定访问当前对象的成员变量、成员方法

```java
public class Car {
    String name;
    double price;
    // 无参数构造器，可以不写
    public Car() {
        ...
    }
    // 有参数构造器
    public Car(String name, double price){
        this.name = name;
        this.price = price;
    }
}
```

### 三大特征

封装、继承、多态

#### 封装

原则：对象代表什么，就得封装对应的数据，并提供数据对应的行为

人画圆，画圆的行为定义在圆对象中

##### private

为了防止诸如把年龄赋值成负数，采取以下方法：

- 一般建议对成员变量使用private关键字修饰（private修饰的成员只能在当前类中访问）
- 为每个成员变量提供配套public修饰的getter，setter方法暴露其取值和赋值

```java
public class Student {
    private int age;
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        if (age >= 0 && age <= 200) {
            this.age = age;
        }
        else {
            System.out.println("wrong!");
        }
    }
}
```

#### 标准JavaBean

也可以称为实体类，其对象可以用于在程序中封装数据

- 成员变量使用private修饰
- 提供成员变量对应的setXxx/getXxx方法，**可以右键generage！！！！！**
- 必须提供一个无参构造器，有参构造器可不写

#### 成员变量和局部变量

| 区别     | 成员变量                       | 局部变量                               |
| -------- | ------------------------------ | -------------------------------------- |
| 类中位置 | 类中，方法外                   | 方法中                                 |
| 初始值   | 默认值                         | 无默认值，要先赋值                     |
| 内存位置 | 堆内存                         | 栈内存                                 |
| 生命周期 | 随着对象创建而存在，消失而消失 | 随着方法调用而存在，随着方法结束而消失 |
| 作用域   |                                | 大括号中                               |

#### 继承

extends关键字，父子关系

```java
public class Student extends People {
    
}
```

子类（派生类），父类（基类或超类）

提高代码的复用性，增强类的功能扩展性

子类相同特征放在父类中，独有属性放在子类中

##### 内存机制

堆内存中父类空间（super）和子类空间（this）

##### 特点

- 子类可以继承父类的属性和行为，但不能继承父类的构造器
- 单继承模式，一个类只能继承一个直接父类
- 不支持多继承，但是支持多层继承，就近原则
- 所有的类都是Object类的子类

子类**可以**继承父类的私有成员，但无法直接访问

父类的静态成员与子类共享，但并非继承

Object类是祖宗类，所有类都直接或间接继承Object

super 访问父类变量和方法

##### 方法重写

子类出现了和父类一样的方法声明覆盖父类的方法，就称子类这个方法是重写的方法

当子类需要父类的功能，但父类功能不能完全满足需求时

@Override重写校验注解，更安全，提高程序的可读性，更优雅

```java
class NewPhone extends Phone{
    @Override
    public void call(){
        super.call();
        System.out.println("打视频");
    }
}

class Phone{
    public void call(){
        System.out.println("打电话");
    }
}
```

- 重写方法名称和形参列表必须和被重写方法一样
- 私有方法不能重写
- 子类重写父类方法时，访问权限必须大于等于父类
- 子类不能重写父类静态方法

##### 子类构造器

子类的全部构造器默认会先访问父类的无参构造器，再执行自己，因为要先完成父类数据空间的初始化

子类构造器第一行语句默认为super()，不写也存在

子类构造器访问父类有参数构造器：

```java
public class People{
    ...
}
public class Student{
    public Student(String name, int age){
        super(name, age);
    }
}
// 外部调用
Student s = new Student("libinghan", 24);
```

##### this和super

this()访问本类构造器，如创建一些包含默认值的构造器

this()（要用到父类构造器）和super()都必须在第一行，因此不能共存

#### 多态

同类型的对象，执行同一个行为，会表现不同的行为特征

```java
父类类型 对象名称 = new 子类构造器;
接口 对象名称 = new 实现类构造器;
```

成员访问特点

- 方法调用：编译看左边，运行看右边（编译看父类里是否存在方法，运行时运行子类中的方法）
- 变量调用：编译看左边，运行看左边（父类变量）

多态的前提：有继承或实现关系，有父类引用指向子类对象，有方法重写

优势：

- 右边对象实现解耦，便于扩展和维护
- 定义方法的时候，使用父类型作为参数，该方法就可以接父类的一切子类对象，体现多态的扩展性与便利

问题：不能使用子类的独有功能

若父类传给子类，可以解决这个问题，必须强制类型转换

```java
// 子类 对象变量 = (子类)父类类型的变量
Animal a = new Tortoise();
// Tortoise t = (Tortoise) a;
if (a instanceof Tortoise) {
    Tortoise t = (Tortoise) a;
    t.layEggs();
} else {
    Dog d = (Dog) a;
    d.lookDoor();
}
```

若转型后的类型和对象真实类型不是同一种类型，会ClassCastException，建议用instanceof判断，比如父类是参数且需要调用子类独有功能时

### 常用API

#### String

String类定义的变量可以用于存储字符串，提供了很多操作字符串的功能

**String常被称为不可变字符串类型，它的对象创建后不能被更改**

String变量每次的修改都是产生并指向了新的字符串对象，原来的字符串对象没有改变，所以称不可变字符串

##### 创建字符串对象

1. String name = "java";

2. String s1 = new String();

3. String s2 = new String("java");

4. char[] chars = {'j', 'a', 'v', 'a'};

   String s3 = new String(chars);

5. byte[] bytes = {106, 97, 118, 97};

   String s4 = new String(bytes);

- ""方式创建的，在字符串常量池中存储，相同内容只会存储一份
- 构造器new创建，每次都会产生一个新对象，放在堆内存中

##### 面试题

```java
String s2 = new String("abc");
// 创建了两个对象，常量池中一个"abc"，在堆内存中new了一个字符串对象
String s1 = "abc";
// 创建了0个对象，常量池中已经有"abc"，直接指向常量池
System.out.println(s1 == s2);
// false
```

java编译优化机制

```java
String s1 = "abc";
String s2 = "a" + "b" + "C";
System.out.println(s1 == s1);
//true 因为在编译阶段编译器就把"a"+"b"+"c"合并成了"abc"
```

##### 字符串内容比较

常量池和堆中相同的字符串用"=="比较会是false，因为地址不一样。比如开始""定义的和后来输入的字符串。

使用**equals**

基本数据类型还是用==

```java
s1.equals(s2);
// 忽略大小写比较,验证码等可用
s1.equalsIgnoreCase(s2);
```

##### 常用API

- .length()

- .charAt(5) 取出第五个字符

- .toCharArray() 转成字符数组

- .substring(0, 5) 截取，包前不包后

​		.substring(4) 从4开始截取到最后

- .replace(s1, s2) 用s2替代s1，**不会改变原来的字符串**
- .contains(s) 判断字符串中是否存在s
- .startsWith(s) 判断字符串是否以s开始
- .split(s) 以s为分隔符把字符串分割成字符串数组

#### ArrayList

集合，与数组类似，也是一种容器，但数组类型长度固定

##### 特点

集合大小不固定，类型也可以选择不固定，**但是不建议**

非常适合元素不固定，要增删的场景

有很多丰富的功能

##### 创建和使用

```java
public ArrayList();
public boolean add(E e);
public void add(int index, E element);
System.out.println(ArrayList list); //直接输出集合内容，继承
```

##### 泛型

ArrayList\<E\>，是一个泛型类，在编译阶段约束集合对象只能操作某种数据类型

集合中只能存储引用类型，不支持基本数据类型

ArrayList\<int\>不行，ArrayList\<Integer\>行

`ArrayList<String> list = new ArrayList<>();`

集合内有不同类型，泛型写Object，规范

##### 常用API

- .get(3) 获取第三个元素
- .size() 获取集合元素个数

- .remove(3) 删除第三个元素，返回该元素值
- .remove(Object o) 删除元素o，返回布尔值，默认删除第一个找到的值
- .set(3, E element) 把第三个元素改成e元素，修改集合并返回修改前的值

##### 注意点

遍历删除时索引会变，要后退一步再继续。或者也可以**反序遍历**。

alt + enter 创建方法

ctrl + alt + t 创建循环

### static

静态，可以修饰成员变量和成员方法

static修饰成员变量表示该成员变量只在内存中存储一份，可以被共享访问、修改

类名.静态成员变量/对象.静态成员变量（不推荐）

#### 内存机制

加载到方法区的同时在堆内存加载静态变量区

#### static方法

**静态成员方法**（有static修饰，归属于类），建议用类名访问

**实例成员方法**（无static修饰，归属于对象），只能用对象访问

若表示对象自己的行为的，且方法中需要访问实例成员的，用实例方法

共用功能用静态方法

##### 内存机制

类和静态方法一开始就在方法区加载，调用时提到栈里跑

加载实例成员方法时，方法区加载实例方法，堆中对象引用方法区中的实例方法

##### 注意事项

- 静态方法只能访问静态成员，不能访问实例成员
- 实例方法可以访问静态成员，也可以访问实例成员
- 静态方法中不可以出现this关键字

##### 工具类Util

每个方法都以完成一个共用的功能为目的，用来给开发人员共同使用

把方法扔在一个类里，要写**静态方法**，无需创建对象，建议将构造器**私有**

##### 代码块

类下{}括起来的代码

- 静态代码块 

static{} 有static修饰，属于类，与类一起**优先加载**一次，自动触发执行

作用：初始化静态资源

案例，斗地主初始化54张牌

- 构造代码块（很少用）

{} 每次创建对象，调用构造器执行时，都会执行该代码块中的代码，并且在构造器执行前执行

作用：初始化实例资源

##### 单例模式

设计模式：最优解，有20多种设计模式

可以保证系统中，应用该模式的这个类永远只会有一个实例，一个类只能创建一个对象，例如任务管理器

- 饿汉单例设计模式

在用类获取对象的时候，对象已经提前创建好了

设计步骤：

定义一个类，把构造器私有

定义一个静态变量存储一个对象

```java
public class SingleInstance {
    // 静态成员变量存储一个对象，只会有一个对象
    // 静态成员变量和类一起加载，只在内存中加载一次
    public static SingleInstance instance = new SingleInstance();
    private SingleInstance () {
    }
}
```

- 懒汉单例模式

在真正需要该对象的时候，才去创建一个对象（延迟加载对象）

设计步骤：

定义一个类，把构造器私有

定义一个静态变量存储一个对象

提供一个返回单例对象的方法

```java
public class SingleInstance {
    private static SingleInstance instance;
    public static SingleInstance getInstance() {
        if(instance == null){
            instance = new SingleInstance();
        }
        return SingleInstance();
    }
    private SingleInstance () {
    }
}
```

### 语法

#### 包

建包

```java
package com.zhongan.javabean;
```

不同包的类需要导包

```java
import 包.类;
// 快捷键 alt+enter
```

不同类的同名包只能导入一个，另一个要全名访问（包.类）

#### 权限修饰符

private（同一类）=>缺省（同一包其他类）=>protected（不同包下的子类）=>public（不同包下的无关类）

#### final

可修饰类、方法和对象

修饰类：该类是最终类，不能被继承（工具类）

```java
public final class XxxUtil {
}
```

修饰方法：该方法是最终方法，不能被重写（@Override）

```java
public final void eat() {
}
```

修饰变量：该变量第一次赋值后，不能再被赋值

```java
public static void buy(final double discount) {
    // 不能对discount赋值了，调用方法时赋值
}
public static final String name = "libinghan"; // 常量，需要赋值
```

注意：

final修饰基本类型，变量存储的数据值不能变

final修饰引用类型，变量存储的地址值不能变，但地址指向的对象内容是可以改变的

#### 常量

```java
public static final 变量类型 变量名 = 变量值;
```

英文单词全大写，多个单词下划线连接

常量执行原理

在编译阶段会进行**宏替换**，把使用常量的地方全部替换成真实的字面量

让使用常量的程序执行性能与直接使用字面量一样

#### 枚举

java中的一种特殊类型，为了做信息的标志和信息的分类。代码可读性好，入参约束严谨，是最好的信息分类技术

```java
修饰符 enum 枚举名称{
    第一行罗列枚举实例名称
}
enum Season{
    SPING, SUMMER, AUTUMN, WINTER;
}
```

- 枚举类继承了枚举类型：java.lang.Enum

- 枚举都是最终类，不可以被继承

- 枚举类构造器是私有的，枚举对外不能创建对象

- 枚举类第一行默认罗列枚举类对象名称

```java
public enum Orientation {
    UP, DOWN, LEFT, RIGHT;
}
public class Xxx {
    ...;
    public static void xxx(String[] Args) {
        move(Orientation.UP);
    }
    public static void move(Orientation o) {
        // switch默认支持枚举
        switch(o) {
            case UP:
                ...;
                break;
            default:
                ...;
                break;
        }
    }
}
```

#### 抽象类

```java
public abstract class Animal {
    public abstract void run();
}
public class Dog extends Animal {
    @Override
    public void run() {
        ...;
    }
}
```

使用场景

- 抽象类可以理解成不完整的设计图，一般作为父类让子类继承
- 当父类知道子类一定要完成某些行为，但每个子类实现方式不同，则定义为抽象形式，声明成抽象类

特征

- 类有的成员抽象类都具备
- 抽象类不一定有抽象方法，但有抽象方法的一定是抽象类
- 一个类继承了抽象类，必须重写所有抽象方法，否则这个类也必须定义为抽象类
- 抽象类得到了抽象方法，但**失去了创建对象的能力**

final和abstract是互斥关系

##### 模板方法模式

使用场景：一个功能多人开发，大部分相同

把功能定义为一个模板方法，放在抽象类，模板方法定义通用的确定的代码，不能决定的功能让子类继承重写

模板方法（固定的）建议加final

#### 接口

```java
public interface 接口名{
    // 常量
    String name = "libinghan";
    //抽象方法
    void run();
}
```

接口是一种规范，默认都是公开的，可以不写public abstract和public static final

接口是用来被实现的，实现接口的类称为实现类，实现类可以理解为子类

```java
修饰符 class implements 接口1, 接口2, ... {
    // 重写抽象方法
}
```

类和类：单继承

类和接口：多实现

接口和接口：多继承，合并多个接口的功能，便于子类实现

##### JDK8后方法

1. 默认方法，类似普通实例方法，但必须有default修饰，需要用实现类的对象来调用
2. 静态方法，用static修饰，**只能用接口名调用**
3. 私有方法，用private修饰，只能在本类中被其他默认方法或者私有方法访问

注意事项

- 接口不能创建对象
- 一个类实现多个接口，有同样的静态方法不冲突，因为接口的静态方法只能接口自己调用
- 一个类继承了父类，又实现了接口，有同名方法时，默认用**父类**的
- 一个类实现多个接口，有同样的默认方法不冲突，这个类重写该方法即可
- 一个接口继承多个接口，如果多个接口中存在规范冲突则不能多继承

#### 内部类

定义在类里面的类，里面的类可以理解成寄生，外部类理解成宿主

使用场景：

- 一个事物内部还需要完整结构来描述，且该结构只为这个事物服务
- 可以方便访问外部成员，包括私有成员
- 提供了更好的封装性，可以用private，protected修饰

分类

- 静态内部类

  public static class

  和外部类完全一样

  创建对象格式

  ```java
  Outer.Inner 对象名 = new Outer.Inner();
  ```

  静态内部类可以访问外部类的静态成员，不可以直接访问外部类的实例成员

- 成员内部类

  无static修饰，属于外部对象

  用得比较多

  ```java
  Outer.Inner 对象名 = new Outer().new Inner();
  ```

  成员内部类可以访问外部类的静态成员，也可以直接访问外部类的实例成员

  ```java
  class People(){
      private int heartbeat = 150;
      public class Heart {
          private int heartbeat = 100;
          public void show() {
              int heartbeat = 78;
              System.out.println(heartbeat); //78
              System.out.println(this.heartbeat); //100
              System.out.println(People.this.heartbeat); //150
          }
      }
  }
  ```

  

- 局部内部类

  不常用

  放在方法、代码块、构造器等执行体中

- **匿名内部类**

  属于局部内部类，定义在方法、代码块中，方便创建子类对象，简化代码

  ```java
  // new 类|抽象类名|接口名(){
  //     重写方法;
  // };
  Employee a = new Employee() {
      public void work(){
      }
  };
  a.word();
  ```

  ```java
  public class Test {
      public static void main(String[] args) {
          Animal a = new Animal() {
             @Override
             public void run() {
                 System.out.println("Tiger runs fast");
             }
          }
          a.run();
      }
  }
  abstract class Animal {
      public abstract void run();
  }
  ```

  特点：

  - 匿名内部类没有名字
  - 写出来会产生一个匿名内部类的对象
  - 匿名内部类的对象类型相当于是当前new的类型的子类类型

### 常用API

#### Object

所有类都直接或间接继承Object类，是祖宗类，Object类方法一切子类都能用

##### toString

public String toString()

默认返回当前对象在堆内存中的地址信息：类的全限名@内存地址

父类toString()方法存在的意义就是为了**被子类重写**，以便返回对象的内容信息，而不是地址信息

快捷键：右键=>generate=>toString或者toS回车

##### equals

public Boolean equals(Object o)

默认比较当前对象与另一个对象地址是否相同，相同返回true，不同返回false

父类equals()方法存在的意义就是为了**被子类重写**，以便比较对象的内容是否相同

#### Objects

上面equals可能会出现空指针异常，用Objects.equals(s1, s2)就不报错，会比较null

public static boolean isNull(Object obj)

#### StringBuilder

可变的字符串类，看成一个对象容器，提高字符串的操作效率

是个工具，需要恢复成String类型

```java
StringBuilder sb = "hello";
sb.append(" world!");
String s = sb.toString();
```

构造器：

- public StringBuilder() 空白的
- public StringBuilder(String str)

##### 常用方法

- public StringBuilder append(任意类型) 添加数据并返回StringBuilder对象本身

​		支持链式编程 .reverse().append()....

- public StringBuilder reverse()
- public int length()
- public String toString()

用String类来操作字符串，操作时会new一个StringBuilder对象，拼接后toString()放在堆内存

StringBuilder类只会产生一个对象

#### Math

没有提供公开的构造器

| 方法名                                       | 说明           |
| -------------------------------------------- | -------------- |
| public static int abs(int a)                 | 取绝对值       |
| public static double ceil(double a)          | 上取整         |
| public static double floor(double a)         | 下取整         |
| public static int round(float a)             | 四舍五入       |
| public static int max(int a, int b)          | 取最大值       |
| public static double pow(double a, double b) | a的b次幂       |
| public static double random()                | [0, 1)取随机数 |

#### System

类名调用，不能被实例化

| 方法名                                                       | 说明                                                     |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| public static void exit(int status)                          | 终止当前运行的Java虚拟机，非零表示异常终止，零为正常终止 |
| public static long currentTimeMilllis()                      | 返回当前系统的时间毫秒值形式                             |
| public static void arraycopy(源数组，起始索引，目的地数组，起始索引，拷贝个数) | 数组拷贝                                                 |

#### BigDecimal

解决浮点型运算精度失真的问题

```java
public static BigDecimal valueOf (double val);  // 包装浮点数为BigDecimal对象，先转字符串再调用构造器
BigDecimal a = BigDecimal.valueof(0.1);
```

| 方法名                                                     | 说明 |
| ---------------------------------------------------------- | ---- |
| public BigDecimal add(BigDecimal b)                        | 加法 |
| public BigDecimal substract(BigDecimal b)                  | 减法 |
| public BigDecimal multiply(BigDecimal b)                   | 乘法 |
| public BigDecimal divide(BigDecimal b)                     | 除法 |
| public BigDecimal divide(BigDecimal b, 精确几位，舍入模式) | 除法 |

计算完之后要把数据转回原格式，用public static doubleValue(double val)方法

如果除不尽会报错，要用divide(val, scale, RoundingMode)方法，如a.divide(b, 2, RoundingMode.HALF_UP); 保留两位四舍五入

#### Date

代表当前所在系统的日期时间

构造器：

- public Date() 代表系统当前日期时间

- public Date(long time) 把时间毫秒值转为Date对象

常用方法： 

- public long getTime() 获取时间对象的毫秒值
- public void setTime(long time) 把时间毫秒值转为Date对象

#### SimpleDateFormat

可以把Date对象，时间毫秒值格式化成喜欢的时间格式，或者把字符串解析成日期对象

构造器：

- public SimpleDataFormat() 使用默认格式
- public SimpleDataFormat(String pattern) 使用指定的格式

格式化方法：

- public final String format(Date date) 日期转日期时间字符串
- public final String format(Object time) 时间毫秒值转日期时间字符串

```java
Date d = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String rs1 = sdf.format(d);
long time1 = System.currentTimeMillis() + 30 * 1000;
String rs2 = sdf.format(time1);
```

- public Date parse(String source) 解析字符串时间成日期对象

```java
String dateStr = "2022-09-04 16:49:58"
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Date d = sdf.parse(dateStr);
long time = d.getTime() + 30L * 1000;
String rs = sdf.format(time);
```

- public Boolean before(Date d)
- public Boolean after(Date d)

#### Calendar

可变日期对象，代表了系统此刻日期对应的日历对象，是抽象类，无法创建对象

| 方法名                                 | 说明                     |
| -------------------------------------- | ------------------------ |
| public int get(int field)              | 取日期中的某个字段信息   |
| public void set(int field, int value)  | 修改日期中的某个字段信息 |
| public void add(int field, int amount) | 为某个字段加减指定的值   |
| public final Date getTime()            | 拿到此刻日期对象         |
| public long getTimeInMillis()          | 拿到此刻时间毫秒值       |

```java
Calendar cal = Calendar.getInstance();
int year = cal.get(Calendar.YEAR);
cal.set(Calendar.HOUR, 12);
cal.add(Calendar.DAY_OF_YEAR, 10);
Date d = cal.getTime();
```

#### JDK8新增API

LocalDate日期，LocalTime时间，LocalDateTime日期时间，Instant时间戳，DateTimeFormatter时间格式化和解析，Duration时间间隔，Period日期间隔

##### Local

```java
LocalDate nowDate = LocalDate.now();
int year = nowDate.getYear();
int month = nowDate.getMonthValue();
int day = nowDate.getDayOfMonth();
LocalDate bt = LocalDate.of(2022, 08, 27);
// 不可变对象，每次修改返回新时间对象
LocalDate yt = nowDate.minusDays();
System.out.println(nowDate.equals(yt));
System.out.println(yt.isBefore(nowDate));
```

##### Instant

```java
Instant instant = Instant.now();
// instant.atZone(ZoneId.systemDefault()); 当前时区时间戳
Date date = Date.from(instant);
Instant instant2 = date.toInstant();
```

##### DateTimeFormatter

```java
LocalDateTime ldt = LocalDateTime.now();
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String ldtStr = ldt.format(dtf);
String ldtStr1 = dtf.format(ldt); // 都可以
LocalDateTime ldt1 = LocalDataTime.parse("2022-09-04 20:21:43", dtf);
```

##### Duration/Period

用于LocalDate中比较

```java
LocalDate today = LocalDate.now();
LocalDate btd = LocalDate.of(2022, 08, 27);
Period period = Period.between(btd, today);
Int years = period.getYears();
// Duration一般用于LocalDateTime，也可以用于时间戳
Duration duration = Duration.between(ldt1, ldt2);
Int hours = duration.toHours(); //总小时数
```

##### ChronoUnit

最全工具类，计算所有时间

```java
Int years = ChronoUnit.YEARS.between(btd, today);
```

#### 包装类

8种基本类型对应的引用类型

Byte，Short，Integer，Long，Character，Float，Double，Boolean

一切皆对象，集合和泛型也只能支持包装类型

自动装箱，自动拆箱，包装类和基本类型可以互相赋值

特有功能

- 默认值可以是null，容错率更高
- 可以把基本类型的数据转换成字符串类型，调用toString方法，不常用，可以直接加""
- 可以把字符串类型的数值转换成真实的数据类型

​		Integer.parseInt("123");

​		Integer.valueOf("123");

​		Double.parseDouble("123.45");

​		Double.valueOf("123.45");

#### 正则表达式

字符类

| 正则表达式     | 说明                     |
| -------------- | ------------------------ |
| [abc]          | 只能是a，b或c            |
| [^abc]         | 不能是a，b和c            |
| [a-zA-Z]       | 字母                     |
| [a-d[m-p]]     | a-d或m-p                 |
| [a-z&&[def]]   | 交集                     |
| [a-z&&\[^def]] | 排除def                  |
| [a-z&&\[^m-p]] | a-l或q-z                 |
| .              | 任意字符                 |
| \d             | 数字                     |
| \D             | 非数字                   |
| \s             | 空白字符，[\t\n\x0B\f\r] |
| \S             | 非空白字符               |
| \w             | 英文数字下划线           |
| \W             | 非单词字符               |

贪婪的量词

| 正则表达式 | 说明               |
| ---------- | ------------------ |
| X?         | 0或1次             |
| X*         | 0或多次            |
| X+         | 1或多次            |
| X{n}       | n次                |
| X{n, }     | 至少n次            |
| X{n, m}    | 至少n次但不超过m次 |

```java
public boolean matches(String regex);
System.out.println("a".matches("[abc]")); //true
public String replaceAll(String regex, String newStr);
public String[] split(String regex);
Pattern pattern = Pattern.compile(regex);
Matcher matcher = pattern.matcher(rs);
while (matcher.find()) {
    String rs1 = matcher.group();
    System.out.println(rs1);
}
```

#### Arrays

```java
int[] arr = {2, 1, 5, 6, 3};
System.out.println(Arrays.toString(arr));
Arrays.sort(arr);
Arrays.binarySearch(arr, 5); // 有序数组二分查找，成功则返回index，失败返回负数-(index+1)
Arrays.sort(arr, new Comparator<Integer>(){
    @Override
    public int compare(Integer o1, Integer o2) {
        return o2 - o1;
    }
}); // 降序
```

#### Lambda表达式

简化匿名内部类，只能简化函数式接口（接口中有且仅有一个抽象方法，通常会加上@FunctionalInterface注解）的匿名内部类的写法形式

```java
(匿名内部类被重写方法的形参列表)->{
    被重写方法的方法体代码
}
public class Xxx {
    public static void main(String[] args) {
        Swimming s1 = () -> {
            System.out.println("swim fast");
        };
        go(s1);
    }
    public static void go(Swimming s) {
        System.out.println("start");
        s.swim();
        System.out.println("end");
    }
}

@FunctionalInterface
interface Swimming {
    void swim();
}

```

案例

```java
Arrays.sort(arr, (Integer o1, Integer o2) -> {
    return o2 - o1;
}); // 降序
```

省略写法

- 参数类型可以不写
- 如果只有一个参数，参数类型可以省略，()也可以省略
- 如果方法体只有一行代码，可以省略大括号不写，同时要省略分号，如果是return语句，return也可以不写

```java
Arrays.sort(arr, (o1, o2) -> o2 - o1); // 降序
```

#### Collection

集合是存储对象数据的容器，大小类型不固定，适合元素增删操作

集合：单列Collection，双列Map

Collection集合体系

- List接口：ArrayList，LinkedList，添加的元素是有序、可重复、有索引
- Set接口：HashSet（LinkedHashSet有序），TreeSet，无序、无重复、无索引

```java
// 支持泛型，可以在编译阶段约束集合只能操作某种数据类型
Collection<String> lists = new ArrayList<>();
```

##### 常用API

```java
Collection<String> list = new ArrayList<>();
list.add("123");
list.clear();
System.out.println(list.isEmpty());
System.out.println(list.size());
System.out.println(list.contains("123")); // true
list.remove("123"); // 默认删除第一个
Object[] arrs = list.toArray();
list1.addAll(list2); //把list2中的元素加入list1中
```

##### 遍历

- 迭代器Iterator，不能遍历数组

```java
Iterator<String> it = lists.iterator();
String ele = it.next(); // 先取元素后移位
while (it.hasNext()){
    String ele = it.next();
}
```

- foreach/增强for循环，实现Iterator接口的类才可以使用迭代器和增强for

```java
for(String ele : list){
    System.out.println(ele);
}
```

- lambda表达式

```java
list.forEach(s->System.out.println(s));
```

### 数据结构

#### 栈

后进先出，先进后出

#### 队列

先进先出，后进后出

#### 数组

内存中的连续空间，查询速度快，删除添加效率低

#### 链表

在内存中不连续，每个节点包含数据值和下一个元素的地址

查询慢，每次要重头开始找，增删相对快（但找到慢，不稳定）

#### 二叉树

二叉查找树，先序遍历是有序的

#### 平衡二叉树

二叉查找树，让树尽可能矮小

左右两个子树高度差不超过1，任意节点的两个子树都是平衡二叉树

左左右旋，左右先左旋子树成左左再右旋，右右左旋，右左先右旋子树成右右再左旋

#### 红黑树

不通过高度平衡，而是通过红黑规则

- 非红即黑，根节点黑色
- 红节点的子节点必须是黑的
- 对每个节点，从该节点到所有叶节点的路径上，黑节点数量相同

插入逻辑

- 默认用红色效率高

- 父节点和叔节点都为红，则都改成黑，祖父节点变红，祖父为黑则变黑
- 父节点为红，叔节点为黑，则父节点变黑，祖父节点变红，再以祖父节点为支点旋转

### List集合

```java
List<String> list = new ArrayList<>();
// list集合独有方法
list.add(index, elem);
list.remove(index); //返回被删除的数据
list.get(index);
list.set(index, item); // 返回修改前的数据
```

ArrayList基于数组实现，LinkedList基于双链表实现

#### 遍历

- 迭代器 `hasNext()，next()`
- 增强for `for (String elem : list)`
- Lambda表达式 `list.forEach(s -> System.out.println(s))`
- for循环 `for (i = 0; i < list.size(); i++)`

#### ArrayList

基于数组，有索引定位，增删需要移位

第一次创建集合并添加第一个元素的时候，先创建一个长度为10的数组

每次添加一个元素，size+1，到达最后位置时扩容为之前的1.5倍

#### LinkedList

```java
// 栈
LinkedList<Integer> stack = new LinkedList<>();
stack.addFirst(1);
// stack.push(1);
stack.addFirst(2);
stack.addFirst(3); // 3, 2, 1
System.out.println(stack.getFirst()); // 3
// stack.pop();
System.out.println(stack.removeFirst()); // 3 返回删除的元素
// 队列
LinkedList<Integer> queue = new LinkedList<>();
queue.addLast(1);
queue.addLast(2);
queue.addLast(3); // 1, 2, 3
System.out.println(queue.getFirst()); // 1
System.out.println(queue.removeFirst()); // 1 返回删除的元素
```

#### 集合并发修改异常

迭代器删除时，不要用list.remove()，而应该用Iterator.remove()，删除当前元素且迭代器不后移

不能用forEach，Lambda表达式和for循环（不报错但会漏删）遍历删除

for循环可以倒着删，或者删除成功时i--，再i++

### Set集合

无序，存取顺序不一致，不重复，无索引

```java
Set<String> sets = new HashSet<>(); // 无序，不重复，无索引
Set<String> sets = new LinkedSet<>(); // 有序，不重复，无索引
Set<String> sets = new TreeSet<>(); // 可排序，不重复，无索引
```

HashSet底层采取哈希表存储数据，增删改查性能都较好

哈希表：jdk8前使用数组+链表，jdk8后使用数组+链表+红黑树

public int hashCode()：返回对象的哈希值

同一对象多次调用hashCode()返回的哈希值相同，默认情况下不同对象的哈希值不同

#### HashSet底层

jdk1.7前

- new时会先创建一个默认长度为16的数组，数组名叫table
- 元素的哈希值和数组长度求余存入对应的位置
- 当前位置为null直接存入，不为null用equals方法比较，一样不存，不一样存入，形成链表

jdk1.8开始

- 链表长度超过8时，自动转成红黑树，比较哈希值大小
- 满当前数组长度的0.75倍时扩容2倍

需要重写hashCode和equals方法（快捷键生成），来比较内容是否一样

#### LinkedHashSet

底层依然是哈希表，但每个元素额外多了一个双链表的机制记录存储的顺序

#### TreeSet

基于红黑树的数据结构实现排序，增删改查性能好，数值类型升序，字符串类型按首字符编号升序，自定义类型无法直接排序

```java
Set<Apple> apples = new TreeSet<>();
// 让自定义的类实现Comparable接口重写里面的CompareTo方法
public class Apple implements Comparable<Apple>{
    ...;
    @Override
    public int CompareTo(Apple o){
        return this.weight - o.weight; // 去掉相同元素
        return this.weight - o.weight >= 0 ? 1 : -1; // 保留相同元素
    }
}
// TreeSet集合有参构造器，可以设置Comparator接口对应的比较器对象，来定制比较规则
set<Apple> apples = new TreeSet<>((o1, o2) -> Double.compare(o1.getWeight(), o2.getWeight()));
```

#### 可变参数

用在形参中可以接收多个数据，格式：数据类型...参数名称

可以不传，传一个或者多个，也可以传一个数组，可变参数在方法内部本质上就是一个数组

一个形参列表只能有一个可变参数，可变参数必须放在形参列表的最后面

```java
public static void main(String[] args) {
    sum(10);
    sum(10, 20, 30);
    sum(new int[]{10, 20, 30});
}
public static void sum(int...num){
    ...;
}
```

#### 集合工具类Collections

不属于集合，是用来操作集合的工具类（不需要构建对象）

```java
List<Integer> nums = new ArrayList<>();
Collections.addAll(nums, 1, 2, 3);
Collections.shuffle(nums); // 打乱顺序
Collections.sort(nums);
Collections.sort(apples, (o1, o2) -> Double.compare(o1.getPrice(), o2.getPrice()));

List<Card> lastThreeCards = allCards.subList(allCards.size() - 3, allCards.size());
```

### Map集合

双列集合，每个集合包含两个数据，每个元素的格式：key=value，也被称为键值对集合，键无序不重复无索引，键和值都可以为null

Map：HashMap（LinkedHashMap 有序），HashTable（Properties），...（TreeMap 排序）

```java
Map<String, Integer> maps = new HashMap<>();
maps.put("a", 1);
maps.put("b", 2);
maps.put(null, null);
```

#### 常用API

```java
Integer a = maps.get("a"); // 有就是对应value，没有就是null
maps.remove("a"); // 返回被删除key对应的value
System.out.println(maps.containsKey("a")); // 包含就是true，不包含就是false
System.out.println(maps.containsValue(1)); // 包含就是true，不包含就是false
Set<String> keys = maps.KeySet(); // 获取全部键的集合
Collection<Integer> values = maps.values(); // 获取全部值的集合
System.out.println(maps.size()); // 集合的大小
System.out.println(maps.isEmpty()); // 判断集合是否为空
map1.putAll(map2); // 把map2中的所有元素放进map1中
```

#### 遍历

- 键找值

```java
Set<String> keys = maps.keySet();
for(String key : keys) {
    int value = maps.get(key);
}
```

- 键值对

```java
// 先把Map集合转换成Set集合，Set集合每个元素都是键值对实体类型，遍历Set集合，然后提取键以及提取值
Set<Map.Entry<String, Integer>> entries = maps.entrySet(); // 快捷键，maps.entrySet()然后alt+enter+v
for (Map.Entry<String, Integer> entry : entries) {
    String key = entry.getKey();
    int value = entry.getValue();
}
```

- Lambda表达式

```java
maps.forEach((key, value) -> System.out.print(key + "-" + value));
```

#### HashMap

无序，不重复，无索引，无需学习特有方法，和HashSet底层原理一样，都是哈希表结构

依赖hashCode和equals方法保证兼职唯一

#### LinkedHashMap

有序，不重复，无索引，哈希表加双链表

#### TreeMap

可排序，按照键数据的大小默认升序排序，只能对键排序

排序

```java
Map<Apple, String> maps = new TreeMap<>(new Comparator<Apple>(){
    @Override
    public int compare(Apple o1, Apple o2) {
        return Double.compare(o2.getPrice(), o1.getPrice());
    }
});
```

### 不可变集合

集合的数据项在创建的时候提供，并且在整个生命周期中都不可改变

如果某个数据不能被修改，把它防御性地拷贝到不可变集合中，或者当集合对象被不可信的库调用时，使用不可变集合

#### 创建不可变集合

```java
List<Integer> lists = List.of(1, 2, 3, 4);
Set<String> names = Set.of("1", "2", "3", "4");
Map<String, Integer> maps = Map.of("a", 1, "b", 2, "c", 3);
```

### Stream流

简化集合和数组操作的API

```java
names.stream().filter(s -> s.startsWith("zhang")).filter(s -> s.length == 3).forEach(s -> System.out.println(s));
```

#### 方法

- 获取Stream流

```java
// 集合，直接有stream方法
Collection<String> list = new ArrayList<>();
Stream<String> s = list.stream();
Map<String, Integer> maps = new HashMap<>();
Stream<String> keyStream = maps.keySet().stream();  // 键流
Stream<Integer> valueStream = maps.values().stream();  // 值流
Stream<Map.Entry<String,Integer>> keyAndValueStream = maps.entrySet().stream();  // 键值对流
// 数组，使用Arrays的stream方法
String[] names = {"a", "b", "c"}
Stream<String> nameStream = Arrays.stream(names);
Stream<String> nameStream2 = Stream.of(names);
```

- 中间方法

```java
List<String> list = new ArrayList<>();
list.stream().filter(s -> s.startWith("li")).limit(2).forEach(s -> System.out.println(s));  // 出入参相同，可以简化为(System.out::println)
long size = list.stream().filter(s -> s.length == 3).count();
list.stream().filter(s -> s.startWith("li")).skip(2);  // 跳过前两个
list.stream.map(s -> "li" + s);  // 加工所有元素
Stream<String> s1 = list.stream().filter(s -> s.startWith("li"));
Stream<String> s2 = list.stream().filter(s -> s.startWith("dai"));
Stream<String> s3 = Stream.concat(s1, s2);
s3.distinct().forEach(System.out::println);
Employee e = list.stream().max((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())).get(); // 可以再用map方法加工成优秀员工对象
Topperformer t = list.stream().max((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())).map(e -> new Topperformer(e.getName(), e.getSalary)).get();
list.stream().sorted((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())).skip(1).limit(list.size() - 2).forEach(e -> allMoney += e.getSalary()); // 去掉最高和最低工资再求和
```

- 终结方法

```java
forEach和count
```

#### 收集

```java
Stream<String> s1 = list.stream().filter(s -> s.startWith("li"));
List<String> liList = s1.collect(Collectors.toList()); // 流无法被收集两次
Stream<String> s2 = list.stream().filter(s -> s.startWith("li"));
Set<String> liSet = s2.collect(Collectors.toSet());
Stream<String> s3 = list.stream().filter(s -> s.startWith("li"));
Object[] arrs = s3.toArray();
```

### 泛型

可以在编译阶段约束操作的数据类型，只能支持引用数据类型

可以统一数据类型，把运行时的问题提前到编译阶段，避免了强制类型转换的异常

#### 自定义泛型类

```java
public class MyArrayList<E>{
    public void add(E e){}
    public void remove(E e){}
}
```

#### 自定义泛型方法

```java
public <T> void show(T[] arr){
    if (arr) {
        StringBuilder sb = new StringBuilder("[");
        for (int i = 0; i < arr.length; i++){
            sb.append(arr[i]).append(i == arr.length - 1 ? "" : ",");
        }
        sb.append("]");
        System.out.println(sb);
    }else{
        System.out.println(sb);
    }
}
```

#### 自定义泛型接口

泛型接口可以让实现类选择当前功能需要操作的数据类型

```java
public interface Data<E>{
    void add(E e);
    E queryById(int id);
}
public class TeacherData implements Data<Teacher>{
    @Override
    public void add(Teacher teacher){}
    @Override
    public Teacher queryById(int id){}
}
```

#### 泛型通配符

"?"在使用泛型时代表任意类型

ETKV在定义泛型时使用

ArrayList\<Cat>和ArrayList\<Animal>没有继承关系，用ArrayList<?>传参

泛型的上下限：

- ? extends Car：必须是Car或其子类
- ? super Car：必须是Car或其父类

## 异常

Throwable:

- Error（系统级别问题，JVM退出等，代码无法控制）
- Exception（java.lang包下，称为异常类，程序本身可以处理的问题）
  - RuntimeException及子类：运行时异常，编译阶段不报错（空指针异常，数组索引越界异常）
  - 其他异常：编译时异常，编译期间必须处理，否则不能通过编译（日期格式化异常），javac阶段

### 运行时异常

- 数组索引越界异常：ArrayIndexOutOfBoundsException，arr[arr.length()]

- 空指针异常：NullPointerException，调用空指针的变量会报错

  ```java
  String name = null;
  System.out.println(name.length()); // null
  System.out.println(name.length()); // 报错
  ```

- 数学操作异常：ArithmeticException

  ```java
  int a = 10 / 0;
  ```

- 类型转换异常：ClassCastException

  ```java
  Object o = 23;
  String s = (String) o; // 报错
  ```

- 数字转换异常：NumberFormatException

  ```java
  String number = "1a";
  Integer it = Integer.valueOf(number);
  ```

### 编译时异常

```java
public static void main(String[] args) throw ParseException {
    String date = "2022-09-18 00:00:00";
    SimpleDateForm sdf = new SimpleDateForm("yyyy-MM-dd HH:mm:ss");
    Date d = sdf.parse(date);
    System.out.println(d);
}
```

在编译阶段报错，提醒不要出错

### 异常默认处理流程

- 默认会在出现异常的代码处自动创建异常对象，ArithmeticException
- 异常从方法中出现的点抛出给调用者，调用者最终抛出给JVM虚拟机
- 虚拟机接收到异常对象后，先在控制台直接输出异常栈信息数据
- 直接从当前执行的异常点干掉当前程序
- 程序已死亡，后面代码不执行

#### 编译时异常

- 出现异常直接抛给调用者，调用者再抛出去throws，发生异常的方法不处理异常，抛给虚拟机会引起程序死亡

- 出现异常自己捕获处理try...catch...，在方法内部，可以将方法内部出现的异常直接捕获处理，程序可以继续执行

  ```java
  try {
      ...;
  } catch (FileNotFoundException e) { // 打印异常栈信息
      e.printStackTrace();
  } catch (ParseException e) {
      e.printStackTrace();
  }
  try {
      ...;
  } catch (Exception e) {
    e.printStackTrace();  
  };
  ```

- 前两者结合，出现异常直接抛给调用者，调用者捕获处理

#### 运行时异常

建议在最外层调用处集中捕获处理

### 自定义异常

#### 编译时异常

定义异常类继承Exception，重写构造器，存在异常时throw new自定义对象抛出

```java
public class AgeIlleagalException extends Exception {
    public AgeIlleagalException () {
        
    }
    public AgeIlleagalException (String message) {
        super(message);
    }
}
public class ExceptionDemo {
    public static void main(String[] args) {
        try {
            checkAge(-1);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    public static void checkAge(int age) throws AgeIlleagalException {
        if (age < 0 || age > 200) {
            throw new AgeIlleagalException("illeagal age: " + age);
        } else {
            System.out.println("name is leagal");
        }
    }
}
```

#### 运行时异常

继承RunTimeException，其他都一样，编译阶段不报错，不用抛出

## 日志

- 可以将系统执行的信息选择性地记录到指定位置（控制台，文件，数据库）
- 可以以开关形式控制，无需改源代码
- 多线程性能较好

Logback日志实现框架，实现了slf4j接口，分为三个技术模块

- logback-core
- logback-classic
- logback-access，与Servlet容器集成，提供HTTP访问日志功能

### logback流程

1. 在项目中新建lib文件夹，导入Logback相关jar包到该文件夹，并添加到项目依赖库

2. 将logback.xml拷贝到src目录

3. 在代码中获取日志对象

   ```java
   public static final Logger LOGGER = LoggerFactory.getLogger("Test.class");
   public static void main(String[] args) {
       LOGGER.debug("...");
       LOGGER.info("...");
       LOGGER.trace("...");
       LOGGER.error("...");
   }
   ```

### 日志级别设置

TRACE（跟踪）<DEBUG（调试）<INFO<WARN<ERROR，默认级别是debug，只输出不低于设定级别的日志信息

ALL和OFF可以控制打开和关闭全部日志信息

## File类

定位文件，删除，获取文件本身信息

| 方法名                                   | 说明                                               |
| ---------------------------------------- | -------------------------------------------------- |
| public File(String pathname)             | 根据路径创建文件对象                               |
| public File(String parent, String child) | 从父路径名字符串和子路径名字符串创建文件对象       |
| public File(File parent, String child)   | 根据父路径对应文件对象和子路径名字符串创建文件对象 |

```java
File f = new File("D:\\学习\\Java\\Java.md");
// File f = new File("D:/学习/Java/Java.md");
// 可以使用相对路径，定位模块中的文件，相对到工程下
long size = f.length(); // 文件字节大小
File f2 = new File("D:\\学习");
System.out.println(f2.exists());
```

### 常用方法

```java
System.out.println(f.getAbsolutePath()); // 取绝对路径
System.out.println(f.getPath()); // 取定义时的路径，绝对/相对路径
System.out.println(f.getName()); // 获取文件名带后缀
System.out.println(f.length()); // 文件字节大小
long time = f.lastModified();
System.out.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(time));
System.out.println(f.isFile());
System.out.println(f.isDirectory());

System.out.println(f1.createNewFile()); // false
File f2 = new File("D:/学习/java");
System.out.println(f2.mkdir()); // 创建一级目录，创建成功返回真，失败返回假
File f3 = new File("D:/学习/Java/java/java1/j1");
System.out.println(f2.mkdirs());
System.out.println(f1.delete()); // 删除文件和空文件夹

File f4 = new File("D:/学习");
String[] names = f4.list(); // 只能获取一级文件名称
for (String name : names) {
    System.out.println(name);
}
// 不存在，是文件都返回null，空文件夹返回长度为0的数组，包含隐藏内容，需要权限返回null
File[] files = f1.listFiles(); // 只能获取一级文件对象
for (File f : files) {
    System.out.println(f.getAbsolutePath());
}
Runtime r = Runtime.getRuntime();
r.exec(f.getAbsolutePath()); // 执行文件
```

## IO流

### 字符集

ASCII字符集：ASCII使用1个字节（8位）存储一个字符，总共可以表示128个字符信息

GBK（两个字节），Unicode（UTF-8）

```java
String name = "abc";
byte[] bytes = name.getBytes(); // 编码，默认utf-8，有编码类型的参数
String rs = new String(bytes); // 解码，默认utf-8，有编码类型的参数
```

### IO流

I，输入，把数据从硬盘文件读入内存；O，输出，从内存写入硬盘文件

字节流，音视频文件；字符流，文本内容

四个抽象类：字节输入流InputStream，字节输出流OutputStream，字符输入流Reader，字符输出流Writer

#### 字节输入流

```java
InputStream is = new FileInputStream("D:\\学习\\1.txt");
// int b1 = is.read(); // 读is流的一个字节，读完后is失去这个字节
// int b;
// while ((b = is.read()) != -1) {
//     System.out.print((char) b);
// }
byte[] buffer = new byte[3]; // 3个字节
int len1 = is.read(buffer); // 返回读入的字节数，buffer写入三个字节，is流去掉三个字节，可能会残留上次读的数据
// String rs1 = new String(buffer); // 可能会残留上次读的数据
String rs1 = new String(buffer, 0, len1);

byte[] buffer = new byte[3];
int len;
while ((len = is.read(buffer1)) != -1) {
    System.out.print(new String(buffer, 0, len)); // 还是会乱码
}

byte[] buffer = is.readAllBytes(); // 不会有乱码
```

#### 字节输出流

```java
OutputStream os = new FileOutputStream("D:\\学习\\out.txt");
// OutputStream os = new FileOutputStream("D:\\学习\\out.txt", true); // 追加，不覆盖
os.write("a"); // 写一个字节
os.write("\r\n".getBytes());
// os.flush(); // 写数据必须刷新数据
os.close(); // 释放资源，包含刷新，关闭后不可以使用
byte[] buffer = {'a', 96, 97};
os.write(buffer);
os.write(buffer, 0, 2);
```

#### 拷贝

```java
InputStream is = new FileInputStream("D:\\study.avi");
OutputStream os = new FileOutputStream("D:\\学习\\study.avi");
byte[] buffer = new byte[1024];
int len;
while ((len = is.read(buffer)) != -1) {
    os.write(buffer, 0, len);
}
os.close();
is.close();
```

#### 资源释放

- try...catch...finally

  ```java
  InputStream is = null;
  OutputStream os = null;
  try {
      InputStream is = new FileInputStream("D:\\study.avi");
      OutputStream os = new FileOutputStream("D:\\学习\\study.avi");
      byte[] buffer = new byte[1024];
      int len;
      while ((len = is.read(buffer)) != -1) {
          os.write(buffer, 0, len);
      }
  } catch (Exception e) {
      e.printStackTrace();
  } finally {
      if os != null {
      	os.close();
      }
      if is != null{
  	    is.close();
      }
  }
  ```

  finally代码块一定要执行，可以在代码结束之后释放资源

  ```java
  try (
  	InputStream is = new FileInputStream("D:\\study.avi");
      OutputStream os = new FileOutputStream("D:\\学习\\study.avi");
      MyConnection connection = new MyConnection();
      
  ){ 
      byte[] buffer = new byte[1024];
      int len;
      while ((len = is.read(buffer)) != -1) {
          os.write(buffer, 0, len);
      }
  } catch (Exception e) {
      e.printStackTrace();
  }
  
  class MyConnection implements AutoCloseable {
      @Override
      public void close() throws IOException {
          System.out.println("...");
      }
  }
  ```


#### 字符输入流

```java
Reader fr = new FileReader("D:\\学习\\1.txt");
// int code = fr.read(); // 每次读一个字符
// System.out.print((char)code);
int code;
while ((code == fr.read()) != -1) {
    System.out.print((char)code);
}

char[] buffer = new char[1024]; // 1K字符
int len;
while ((len = fr.read(buffer)) != -1) {
    String rs = new String(buffer, 0, len);
    System.out.print(rs);
}
```

#### 字符输出流

```java
Writer fw = new FileWriter("D:\\学习\\out.txt", true);
fw.write("a");
fw.write("李");
fw.write("\r\n");
fw.write("libinghan");
char[] chars = "abc".toCharArray();
fw.write(chars);
fw.write("libinghan", 0, 5);
fw.write(chars, 0, 5);
// fw.flush();
fw.close(); // 包含刷新
```

### 缓冲流

自带缓冲区（默认8KB），可以提高原始字节流、字符流读写数据的性能

#### 字节缓冲流

```java
try (
 InputStream is = new FileInputStream("D:\\study.avi");
    InputStream bis = new BufferedInputStream(is);
    OutputStream os = new FileOutputStream("D:\\学习\\study.avi");
    OutputStream bos = new BufferedOutputStream(os);
    
){ 
    byte[] buffer = new byte[1024];
    int len;
    while ((len = bis.read(buffer)) != -1) {
        bos.write(buffer, 0, len);
    }
} catch (Exception e) {
    e.printStackTrace();
}
```

#### 字符缓冲流

```java
Reader fr = new FileReader("D:\\study.txt");
BufferedReader br = new BufferedReader(fr);
char[] buffer = new char[1024];
int len;
// while ((len = br.read(buffer)) != -1) {
//     String rs = new String(buffer, 0, len);
//     System.out.print(rs);
// }

String line;
while ((line = br.readLine()) != null) {
    System.out.print(br.readLine());
}

Writer fw = new FileWriter("D:\\study.txt", true);
BufferedWriter bw = new BufferedWriter(fw);
bw.write("123");
bw.newLine(); // 换行
```

### 转换流

```java
InputStream is = new FileInputStream("D:\\study.txt");
Reader isr = new InputStreamReader(is);
BufferedReader br = new BufferedReader(isr, "GBK"); // 把GBK编码转换成字符输入流
String line;
while ((line = br.readLine()) != null) {
    System.out.print(br.readLine());
}

OutputStream os = new FileOutputStream("D:\\学习\\study.txt");
Writer osw = new OutputStreamWriter(os, "GBK"); // GBK的方式写字符数据
BufferedWriter bw = new BufferedWriter(osw);
bw.write("abc");
bw.close();
```

### 对象序列化

以内存为基准，把内存中的对象存储到磁盘文件中去，称为对象序列化，反之称为对象反序列化

```java
// 对象要序列化，先要实现Serializable接口，不参加序列化的字段用transient修饰，serialVersionUID序列号
Student s = new Student("libinghan", 24);
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("D:\\obj.txt"));
oos.writeObject(s);
oos.close();

ObjectInputStream ois = new ObjectInputStream(new FileInputStream("D:\\obj.txt"));
Student s = (Student) is.readObject();
```

### 打印流

方便、高效地写数据到文件

```java
PrintStream ps = new PrintStream("D:\\print.txt");
ps.println(123);
ps.println(true);
ps.println("abc");
ps.close();

// 改变输出位置，重定向
System.setOut(ps);
System.out.println(123);
```

### Properties

是一个Map集合，代表一个属性文件，可以把自己对象中的键值对信息存入属性文件中，后缀为.properties，内容为key=value

```java
Properties = properties = new Properties();
Properties.setProperty("a" : "1");
Properties.setProperty("b" : "2");
Properties.store(new FileWriter("D:\\properties.txt", "")); // 第二个参数为注释
Properties.load(new FileReader("D:\\properties.txt"));
String rs = properties.getProperty("a");
```

### commons-io

```java
IOUtils.copy(new FileInputStream("D:\\1.jpg"), new FileOutputStream("D:\\2.jpg"));
FileUtils.copyFileToDirectory(new File("D:\\1.jpg"), new File("D:\\"));
FileUtils.copyDirectoryToDirectory(new File("D:\\学习"), new File("D:\\"));
FileUtils.deleteDirectory(new File("D:\\学习"));
```

## 多线程

### 创建

#### 继承Thread

定义子类MyThread继承线程类java.lang.Thread，重写run()方法，创建MyThread对象，调用start()发方法启动线程

```java
public class ThreadDemo {
    public static void main(String[] args) {
        Thread t = new MyThread();
        t.start();
        for (int i = 0; i < 5; i++) {
            System.out.println("主线程" + i);
        }
    }
}
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程" + i);
        }
    }
}
```

需要调用start，若调用run则还是单线程，要把主线程放在子线程之后

#### 实现Runnable

定义线程任务类MyRunnable实现Runnable接口，重写run()方法，创建MyRunnable任务对象，把MyRunnable任务对象交给Thread处理，调用start()方法启动线程

```java
public class ThreadDemo {
    public static void main(String[] args) {
        Runnable target = new MyRunnable();
        Thread t = new Thread(target);
        t.start();
        for (int i = 0; i < 5; i++) {
            System.out.println("主线程" + i);
        }
    }
}
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程" + i);
    }
}
```

扩展性更强，可以继承其他类和实现其他接口，线程有执行结果不能直接返回

匿名内部类方式实现

```java
public class ThreadDemo {
    public static void main(String[] args) {
        new Thread(() -> {
        	for (int i = 0; i < 5; i++) {
            	System.out.println("子线程" + i);
    		}
        }).start();
        for (int i = 0; i < 5; i++) {
            System.out.println("主线程" + i);
        }
    }
}
```

#### 实现Callable

前两种不适合需要返回线程执行结果的业务场景

定义类实现Callable接口，重写call方法，封装要做的事，用FutureTask把Callable对象封装成线程任务对象，把线程任务对象交给Thread处理，调用Thread的start方法启动线程，执行任务，线程执行完后通过FutureTask的get方法获取任务执行的结果

```java
public class ThreadDemo {
    public static void main(String[] args) {
        Callable<String> call = new MyCallable(100);
        // FutureTask是Runnable对象，可以交给Thread，可以在线程执行完毕之后调用get方法得到结果
        FutureTask<String> f1 = new FutureTask<>(call);
        Thread t1 = new Thread(f1);
        t1.start();
        String rs = f1.get();
    }
}
class MyCallable implements Callable<String>{
    private int n;
    public MyCallable(int n) {
        this.n = n;
    }
    @Override
    public String call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            sum += i;
        }
        return sum;
    }
}
```

知识实现接口，可以继续继承类和实现接口，扩展性强，可以在线程执行完毕后获取线程执行的结果

### 常用API

```java
public class ThreadDemo {
    public static void main(String[] args) {
        Thread t1 = new MyThread();
        // Thread t1 = new MyThread("thread1");
        t1.setName("thread1");
        t1.start();
        
        Thread t2 = new MyThread();
        t2.setName("thread2");
        t2.start();
        
        // 主线程的名称叫main
        Thread m = Thread.currentThread();
        System.out.println(m.getName());
    }
}
class MyThread extends Thread {
    // 加入name有参构造器，可以不用setName
    // public MyThread() {}
    // public MyThread(String name) {
    // 	 super(name); // 使用父类构造器
    // }
    	
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            Thread.sleep(3000);
            System.out.println("子线程" + Thread.currentThread().getName() + i);
        }
    }
}
```

### 线程安全

多个线程同时操作一个共享资源的时候可能会出现业务安全问题，称为线程安全问题，如同时取钱可能余额不足

产生原因：存在多线程并发，同时访问共享资源，存在修改共享资源

### 线程同步

核心思想：加锁，把共享资源上锁，每次只能一个线程进入，访问完毕以后解锁，其他线程才能进来

#### 同步代码块

把出现线程安全的核心代码上锁，每次只能一个线程进入，执行完毕后解锁

```java
public void drawMoney(double money) {
	String name = Thread.currentThread().getName();
    synchronized (this) { // 括号内是对所有线程都是唯一的锁对象
        if (this.money >= money) {
            this.money -= money;
        } else {
            System.out.println("余额不足");
        }
    }
}
```

锁对象不能用任意唯一的对象，会影响无关线程的执行，建议使用共享资源作为锁对象

对于实例方法建议使用this作为锁对象，对于静态方法建议使用字节码（类名.class）对象作为锁对象

#### 同步方法

把出现线程安全问题的核心方法上锁，每次只能一个线程进入，执行完毕后解锁

在方法上加上synchronized修饰即可

```java
public synchronized void drawMoney(double money) {
	String name = Thread.currentThread().getName();
    if (this.money >= money) {
        this.money -= money;
    } else {
        System.out.println("余额不足");
    }
}
```

同步方法底层也是隐式加锁的，锁住整个方法，实例方法默认用this作为锁对象，静态方法默认用类名.class作为锁对象

#### Lock锁

实现类为ReentrantLock

```java
public class x {
    private final Lock lock = new ReentrantLock(); // 唯一且不可替换
    lock.lock();
    try {
        public void drawMoney(double money) {
            String name = Thread.currentThread().getName();
            if (this.money >= money) {
                this.money -= money;
            } else {
                System.out.println("余额不足");
            }
        }
    } finally {
        lock.unlock();
    }
}
```

### 线程通信

前提：多个线程操作同一个共享资源的时候需要进行通信，且要保证线程安全

Object类的等待和唤醒方法：

- void wait()，让当前线程等待并释放所占锁，直到另一个线程调用notify()方法或notifyAll()方法
- void notify()，唤醒正在等待的单个线程
- void notifyAll()，唤醒正在等待的所有线程

上述方法应该使用当前同步锁对象进行调用

取10w和存10w轮流进行：

```java
public synchronized void drawMoney(double money) {
	String name = Thread.currentThread().getName();
    if (this.money >= money) {
        this.money -= money;
        this.notifyAll();
        this.wait();
    } else {
        System.out.println("余额不足");
        this.notifyAll();
        this.wait();
    }
}

public synchronized void depositMoney(double money) {
	String name = Thread.currentThread().getName();
    if (this.money == 0) {
        this.money += money;
        this.notifyAll();
        this.wait();
    } else {
        System.out.println("余额足够");
        this.notifyAll();
        this.wait();
    }
}
```

### 线程池

ExecutorService接口，ThreadPoolExecutor实现类，Executors工具类

```java
public ThreadPoolExecutor(
    int corePoolSize,  // 核心线程
    int maximumPoolSize,  // 可支持的最大线程数
    long keepAliveTime,  // 临时线程的最大存活时间
    TimeUnit unit,  // 存活时间单位
    BlockingQueue<Runnable> workQueue,  // 任务队列
    ThreadFactory threadFactory,  // 用哪个线程工厂创建线程
    // AbortPolicy（丢弃任务，抛出RejectedExecutionException异常），DiscardPolicy（丢弃任务，不抛出异常），DiscardOldestPolicy（抛弃等待最久的任务），CallerRunsPolicy（主程序调用run方法，绕过线程池）
    RejectedExecutionHandler handler  // 线程忙，任务满的时候，处理新任务
)
```

新任务提交时发现核心线程都在忙，任务队列也满了，并且还可以创建临时线程时，才会创建临时线程

核心线程和临时线程都在忙，任务队列也满了，新的任务过来的时候才会开始任务拒绝

```java
ExecutorService pools = new ThreadPoolExecutor(3, 5, 8, TimeUnit.SECONDS, new ArrayBlockingQueue<>(6), Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());
```

#### ExecutorService

执行Runnable任务

```java
public class Test {
    public static void main(String[], args) {
        ExecutorService pools = new ThreadPoolExecutor(3, 5, 8, TimeUnit.SECONDS, new ArrayBlockingQueue<>(6), Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());
        Runnable target = new MyRunnable();
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        pool.shutdownNow(); // 立即关闭，即使任务没有完成，会丢失任务
        pool.shutdown(); // 会等待全部任务执行完毕之后再关闭
    }
}

public class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + i);
        }
    }
}
```

执行Callable任务

```java
public class Test {
    public static void main(String[], args) {
        ExecutorService pools = new ThreadPoolExecutor(3, 5, 8, TimeUnit.SECONDS, new ArrayBlockingQueue<>(6), Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());
        Future<String> f1 = pool.submit(new MyCallable(100));
        Future<String> f2 = pool.submit(new MyCallable(200));
        String rs = f1.get();
        System.out.println(f2.get());
    }
}

public class MyCallable implements Callable<String> {
    private int n;
    public MyCallable(int n) {
        this.n = n;
    }
    @Override
    public String call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            sum += i;
        }
        return Thread.currentThread().getName() + sum;
    }
}
```

#### Executors

```java
ExecutorService pool = Executors.newFixedThreadPool(3);
pool.execute(new MyRunnable());
pool.execute(new MyRunnable());
```

大型并发系统环境中可能会出现系统风险，没有限制任务队列长度或线程数量，可能出现OOM错误

### 定时器

#### Timer

```java
Timer timer = new Timer();
timer.schedule(new TimerTask() {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}, 3000, 2000); // TimerTask task, long delay, long period
```

Timer是单线程，处理多个任务顺序执行，存在延时与设定时间有出入

任务异常Timer线程死掉，影响后续任务

#### ScheduledExecutorService

```java
ScheduledExecutorService pool = Executors.newScheduledThreadPool(3);
pool.scheduleAtFixedRate(new TimerTask() {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }, 0, 2, TimeUnit.SECONDS);
}
```

### 线程生命周期

New新建，start()进入Runnable

Runnable可运行，未获得锁对象进入Blocked，获得锁对象调用wait()进入waiting，sleep(毫秒)或wait(毫秒)进入Timed Waiting，执行完毕进入Terminated

Blocked锁阻塞，获得锁对象进入Runnable

Waiting无限等待，被其他线程notify并获得锁对象进入Runnable，被其他线程notify但未获得锁对象进入Blocked

Time Waiting计时等待，sleep时间到或wait时间到并获得锁对象或wait时间没到，被其他线程notify并获得锁对象，进入Runnable，wait时间到未获得锁对象或wait时间没到，其他线程notify但未获得锁对象，进入Blocked

Terminated被终止

## 网络编程

### 三要素

#### IP地址

Internet Protocol，互联网协议地址，分配给上网地址的唯一标识

IPv4：4字节32bit，4个十进制数

IPv6：16字节128bit，8个整数，每个整数有4个十六进制位

私有地址（局域网使用），192.168.0.0~192.168.255.255，组织内部使用

公网地址

```java
InetAddress ip1 = InetAddress.getLocalHost();
System.out.println(ip1.getHostName());
System.out.println(ip1.getHostAddress());
InetAddress ip2 = InetAddress.getByName("www.baidu.com");
InetAddress ip3 = InetAddress.getByName("112.80.248.76");
System.out.println(ip3.isReachable(5000)); // true
```

#### 端口

标识正在计算机设备上运行的进程，被规定位一个16位的二进制，范围是0~65535

周知端口：0~1023，预先定义的知名应用，HTTP80，FTP21

注册端口：1024~49151

动态端口：49152~65535

#### 协议

TCP/IP模型

应用层：HTTP，FTP，DNS，SMTP

传输层：TCP（面向连接的可靠通信协议，三次握手，连接中可进行大数据量的传输，连接、发送数据都需要确认，传输完毕后还需释放已建立的连接，通信效率较低），UDP（无连接，不可靠，将数据源IP，目的地IP和端口封装成数据包，不需要建立连接，每个数据包大小在64KB内，发送方直接发送，接收方无需确认，可以广播发送，结束无需释放资源，开销小，速度快）

网络层：IP，ICMP

数据链路层+物理层：物理寻址，比特流

### UDP通信

DatagramPacket：数据包对象

DatagramSocket，发送端和接收端对象

```java
// 客户端
DatagramSocket socket = new DatagramSocket(); // 发送端自带默认端口号
byte[] buffer = "i am libinghan".getBytes();
DatagramPacket packet = new DatagramPacket(buffer, buffer.length, InetAddress.getByName("..."), 8888);
socket.send(packet);
socket.close();
// 服务端
DatagramSocket socket = new DatagramSocket(8888);
byte[] buffer = new byte[1024 * 64];
DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
socket.receive(packet);
String ip = packet.getSocketAddress().toString();
int port = packet.getPort();
int len = packet.getLength();
String rs = new String(buffer, 0, len);
System.out.println(rs);
socket.close();
```

#### 多发多收消息

```java
// 客户端
DatagramSocket socket = new DatagramSocket();
Scanner sc = new Scanner(System.in);
while (true) {
    String msg = sc.nextLine();
    if ("exit".equals(msg)) {
        socket.close();
        break;
    }
    byte[] buffer = msg.getBytes();
    DatagramPacket packet = new DatagramPacket(buffer, buffer.length, InetAddress.getByName("..."), 8888);
    socket.send(packet);
}
// 服务端
DatagramSocket socket = new DatagramSocket(8888);
byte[] buffer = new byte[1024 * 64];
DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
while (true) {
    socket.receive(packet);
    int len = packet.getLength();
    String rs = new String(buffer, 0, len);
    System.out.println(rs);
}
```

#### 广播组播

```java
// 广播
DatagramPacket packet = new DatagramPacket(buffer, buffer.length, InetAddress.getByName("255.255.255.255"), 9999);
// 组播 224.0.0.0~239.255.255.255.255
// 客户端
DatagramPacket packet = new DatagramPacket(buffer, buffer.length, InetAddress.getByName("224.0.0.1"), 9999);
// 服务端
DatagramSocket socket = new MultiCastSocket(9999);
socket.joinGroup(new InetSocketAddress(InetAddress.getByName("224.0.0.1"), 9999), NetworkInterface.getByInetAddress(InetAddress.getLocalHost()));
```

### TCP通信

客户端：创建客户端的Socket对象，请求与服务端的连接；使用Socket对象调用getOutputStream()方法得到字节输出流；使用字节输出流完成数据的发送；释放资源，关闭socket管道

服务端：注册端口，调用accept方法等待接收客户端的Socket连接请求，建立Socket通信，把字节输入流包装成缓冲字符输入流进行消息的接收，按照行读取消息

```java
// 客户端
Socket socket = new Socket("127.0.0.1", 7777); // 服务端ip，端口
OutputStream os = socket.getOutputStream();
PrintStream ps = new PrintStream(os);
ps.println("i am libinghan");
ps.flush();
// 服务端
ServerSocket serverSocket = new ServerSocket(7777);
Socket socket = serverSocket.accept();
InputStream is = socket.getInputStream();
BufferedReader br = new BufferedReader(new InputStreamReader(is));
String msg;
if ((msg == br.readLine()) != null) {
    System.out.println(socket.getRemoteSocketAddress() + msg);
}
```

#### 多发多收消息

```java
// 客户端
Socket socket = new Socket("127.0.0.1", 7777); // 服务端ip，端口
OutputStream os = socket.getOutputStream();
PrintStream ps = new PrintStream(os);
Scanner sc = new Scanner(System.in);
while (true) {
    String msg = sc.nextLine();
    ps.println(msg);
    ps.flush();
}
ps.println("i am libinghan");
ps.flush();
// 服务端
ServerSocket serverSocket = new ServerSocket(7777);
Socket socket = serverSocket.accept();
InputStream is = socket.getInputStream();
BufferedReader br = new BufferedReader(new InputStreamReader(is));
String msg;
while ((msg == br.readLine()) != null) {
    System.out.println(socket.getRemoteSocketAddress() + msg);
}
```

#### 同时处理多个客户端

```java
// 服务端
ServerSocket serverSocket = new ServerSocket(7777);
while (true) {
	Socket socket = serverSocket.accept();
    new ServerReaderThread(socket).start();
}

public class ServerReaderThread extends Thread {
    private Socket socket;
    public ServerReaderThread(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        InputStream is = socket.getInputStream();
        BufferedReader br = new BufferedReader(new InputStreamReader(is));
        String msg;
        while ((msg == br.readLine()) != null) {
            System.out.println(socket.getRemoteSocketAddress() + msg);
        }
    }
}
```

#### 线程池优化

```java
public class ServerDemo {
    public static ExecutorService pool = new ThreadPoolExecutor(3, 5, 6, TimeUnit.SECONDS, new ArrayBlockingQueue<>(2), Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());
    public static void main(String[] args) {
		ServerSocket serverSocket = new ServerSocket(7777);
    	while (true) {
        	Socket socket = serverSocket.accept();
            Runnable target = new ServerReaderRunnable(socket);
        	pool.execute(target);
    	}       
    }
}

public class ServerReaderRunnable implements Runnable {
    private Socket socket;
    public ServerReaderRunnable(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        InputStream is = socket.getInputStream();
        BufferedReader br = new BufferedReader(new InputStreamReader(is));
        String msg;
        while ((msg == br.readLine()) != null) {
            System.out.println(socket.getRemoteSocketAddress() + msg);
        }
    }
}
```

## 单元测试

针对java方法的测试，进而检查方法的正确性

只有一个main方法，如果一个方法测试失败，其他方法会受到影响，无法得到结果报告，无法实现自动化测试

JUnit可以灵活选择执行哪些测试方法，可以生成全部方法的测试报告，单元测试中的某个方法测试失败了，不会影响其他测试方法的测试

```java
public cass TestUserService {
    /**
    测试方法，必须是公开，无参数，无返回值的方法，必须用@Test注解标记
    */
    @Test // alt+enter junit
    public void testLoginName() {
        UserService userService = new UserService();
        String rs = userService.loginName("libinghan", "123456");
        Assert.assertEquals("业务失败", "登陆成功！", rs); // 断言
    }
}
```

### 常用注解

- @Test 测试方法
- @Before 修饰实例方法，在每个测试方法执行之前执行一次
- @After 修饰实例方法，在每个测试方法执行之后执行一次
- @BeforeClass 修饰静态方法，在所有测试方法执行之前执行一次
- @AfterClass 修饰静态方法，在所有测试方法执行之后执行一次

## 反射

反射是指对于任何一个Class类，运行时都可以直接得到这个类的全部成分，构造器对象Constructor，成员变量对象Field，成员方法对象Method，运行时动态获取类信息以及动态调用类中成分的能力称为Java的反射机制

```java
Class c1 = Class.forName("包名+类名");
Class c2 = Student.class;
Student s = new Student();
Class c3 = s.getClass();
```

### 获取构造器

```java
Class c = Student.class;
// 获取全部public修饰的构造器
Constructor[] constructors = c.getConstructors();
for (Constructor constructor : constructors) {
    System.out.println(constructor.getName() + constructor.getParameterCount());
}
// 获取全部构造器
Constructor[] constructors = c.getDeclaredConstructors();
// 获取某个public构造器
Constructor cons = c.getConstructor(String.class, int.class); // 有参构造器
Student s = (Student) cons.newInstance("libinghan", 24);
// 获取某个构造器
Constructor cons = c.getDeclaredConstructor(); // 无参构造器
cons.setAccessible(true); // 私有构造器，暴力反射，破坏封装性
Student s = (Student) cons.newInstance();
```

### 获取成员变量

```java
Class c = Student.class;
// 获取所有成员变量
Field[] fields = c.getDeclaredFields();
for (Field field : fields) {
    System.out.println(field.getName() + field.getType());
}
// 获取某个成员变量
Field f = c.getDeclaredField("age");
f.setAccessible(true);
Student s = new Student();
f.set(s, 24);
int age = f.get(s);
```

### 获取方法

```java
Class c = Student.class;
// 获取所有方法对象
Method[] methods = c.getDeclaredMethods();
for (Method method : methods) {
    System.out.println(method.getName() + method.getReturnType() + method.getParameterCount());
}
// 获取某个方法
Method m1 = c.getDeclaredMethod("study");
Method m2 = c.getDeclaredMethod("test", String.class);
Student s = new Student();
m1.invoke(s);
int grade = m2.invoke(s, "Math");
```

### 作用

- 绕过编译阶段为集合添加数据

  泛型只在编译阶段可以约束，编译成Class文件进入运行阶段时泛型被擦除，用反射可以为集合存入其他任意类型的元素

  ```java
  ArrayList<String> list = new ArrayList<>();
  list.add(1);
  list.add(2);
  Class c = list.getClass();
  Method add = c.getDeclaredMethod("add", Object.class);
  add.invoke(list, "libinghan");
  ```

- 通用框架的底层原理

  不清楚对象字段时获取对象的字段名称和值存储到文件中去

  ```java
  public static void save(Object obj) {
      PrintStream ps = new PrintStream("/data.txt", true);
      Class c = obj.getClass(); // 全限名
      ps.println("========" + c.getSimpleName() + "========"); // 当前类名
      Field[] fields = c.getDeclaredFields();
      for (Field field : fields) {
          String name = field.getName();
          field.setAccessible(true);
          String value = field.get(obj) + "";
          ps.println(name + "=" + value);
      }
  }
  ```

## 注解

Annotation，对Java中类，方法，成员变量做标记进行特殊处理

### 自定义注解

若注解只有一个属性则属性名可以省略

```java
public @interface MyBook {
    String name() default "佚名";
    String[] authors();
    double price();
}
// 可以标记类，方法，成员变量
@MyBook(name = "西游记", authors = {"吴承恩"}, price = 30.0)
public class AnnotationDemo {
    ...;
}
```

### 元注解

注解注解的注解

- @Target 约定自定义注解只能在哪些地方使用

  ```java
  @Target({ElementType.METHOD, ElementType.FIELD}) // TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE
  public @interface Mytest {
      ...;
  }
  ```

- @Retention 申明注解的生命周期

  ```java
  @Retention(RetentionPolicy.RUNTIME) // 在运行阶段注解也不会消失，SOURCE只作用在源码阶段，CLASS作用在源码和字节码文件阶段
  public @interface Mytest {
      ...;
  }
  ```

### 注解的解析

```java
@Test
public void parseClass() {
    Class c = BookStore.class;
    if(c.isAnnotationPresent(Book.class)) {
        Book book = (Book) c.getDeclaredAnnotation(Book.class);
        System.out.println(book.value());
        System.out.println(book.price());
        System.out.println(Arrays.toString(book.author()));
    }
}
```

## 动态代理

对对象的行为做一些辅助操作

必须存在接口，被代理对象需要实现接口，使用Proxy类提供的方法得到对象的代理对象

```java
public static Skill getProxy(Star obj) {
    return (Skill) Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(), new InvocationHandler({
        @Override
        public Object invoke(Object proxy, Method method, Object[] args) {
            ...;
            Object rs = method.invoke(obj, args);
            ...;
            return rs;
        }
    }));
}
```

在不改变源码的情况下，实现对方法功能的增强，提高了代码的复用

简化了编程工作，提高开发效率和系统可扩展性

为被代理对象所有方法做代理

灵活，支持任意接口类型的实现类对象做代理，也可以直接为接口本身做代理

## XML

eXtensible Markup Language可扩展标记语言

纯文本，默认使用utf-8编码，可嵌套，XML文件，经常被当成消息进行网络传输，或者作为配置文件用于存储系统的信息

### 语法规则

b必须存在一个根标签，有且只能有一个，必须成对，属性和标签名空格隔开，属性值用引号

注释：<!--注释内容-->

<![CDATA[内容]]>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
```

### 文档约束

dtd约束，不能约束具体的数据类型

schema约束，本身也是一个xml文件，可以约束XML文件的标签内容格式和具体的数据类型

### 解析

DOM解析，dom4j技术

Document对象，整个xml文档

Element对象，标签

Attribute对象，属性

Text对象，文本内容

```java
SAXReader saxReader = new SAXReader();
InputStream is = Dom4JDemo.class.getResourceAsStream("/xxx.xml"); // src下的文件
Document document = saxReader.read(is);
Element root = document.getRootElement();
System.out.println(root.getName());
List<Element> sonEles = root.elements(); // 子元素
for (Element sonEle : sonEles) {
    System.out.println(sonEle.getName());
}
Element userEle = root.element("user"); // 默认拿第一个子元素
System.out.println(userEle.attributeValue("id"));
System.out.println(userEle.elementText("name"));
System.out.println(userEle.elementTextTrim("name")); // 去除前后空格
Attribute idAttr = userEle.attribute("id");
System.out.println(idAttr.getName() + ":" + idAttr.getValue());
Element email = userEle.element("email");
System.out.println(email.getText());
```

### XPath

```java
List<Node> nameEles = document.selectNodes("/contactList/contact/name");
for (Node nameNode : nameNodes) {
    Element nameEle = (Element) nameNode;
    System.out.println(nameEle.getTextTrim());
}
List<Node> nameNodes = document.selectNodes("//name"); // 全局找
List<Node> nodes = document.selectNodes("//@id"); // 在全文检索属性对象
Node node = document.selectSingleNode("//name[@id]"); // 查询包含id属性的name元素
Node node = document.selectSingleNode("//name[@id = 888]");
```

## 工厂模式

设置工厂类来创建对象，可以封装对象的创建细节，实习类与类之间的解耦操作

## 装饰模式

创建一个新类包装原始类，从而提升原来类的功能

定义父类（InputStream）=>定义原始类（FileInputStream），继承父类，定义功能=>定义装饰类（BufferedInputStream），继承父类，包装原始类，增强功能

# Java面试题

## ArrayList

基于数组，需要连续内存

随机访问快（根据下标）

尾部插入删除性能好，其他地方性能差

可以利用CPU缓存，局部性原理

无参构造ArrayList，起始容量为0

每次填满了扩容1.5倍（移位和原容量相加），用新数组代替旧数组，拷贝，旧数组会被垃圾回收

addAll扩容成max(元素个数, 下次扩容大小)，不到10个则扩容成10

## failFast

一旦发现遍历的同时其他人修改，立刻抛异常，如ArrayList

## failSafe

一旦发现遍历的同时其他人修改，有应对策略，例如牺牲一致性来让遍历运行完成，如CopyOnWriteArrayList

## LinkedList

基于双向链表，无需连续内存

随机访问慢

头尾插入删除性能好

占用内存多

## HashMap

1.7：数组+链表

1.8之后：数组+链表或红黑树

### 解决哈希冲突

- 元素个数超过哈希表长度的0.75倍则扩容成两倍，桶下标重新计算

- 如果链表长度超过树化阈值（8）且数组容量大于等于64，则变成红黑树

红黑树为了避免DoS攻击，是偶然情况

### 树退化

- 扩容时拆分树，树元素个数<=6，退化为链表
- remove树节点，若root，root.left，root.right，root.left.left中有一个为null，移除之前判断，退化为链表

需要二次哈希，每个对象有hashCode()方法，HashMap中还有hash()方法

求模优化：a % b = a & (b - 1)（前提条件：b为2的n次方）

### 二次哈希

让数据分布更均匀

扩容时如何判断链表中的节点是否要移动：二次哈希值 & 原始容量，为0则不移动，不为0则移到新位置（原始位置+原始容量）

容量为2的n次方（性能更高）的问题：可能分布不均匀（比如全是偶数），可以取质数为容量（哈希分布性高）

### put流程

- HashMap是懒惰创建数组的，首次使用才创建数组
- 计算索引
- 没人占用，创建Node占位返回
- 已被占用，TreeNode走红黑树的添加或更新，普通Node走链表的添加或更新，长度超过树化阈值则树化
- 返回前检查容量是否超过阈值，超过则扩容再迁移元素

### 并发丢数据

扩容死链，数据错乱

### key

可以为null，作为key的对象，要实现hashCode和equals方法，且key的内容不能修改

String对象的hashCode()计算每次乘31，散列效果好，且可以优化成移位

## 设计模式

### 单例模式

#### 饿汉式

```java
private Singleton(){};
private static final Singleton INSTANCE = new Singleton();
public static Singleton getInstance() {
    return INSTANCE;
}
```

反射，反序列化和Unsafe会破坏单例

#### 枚举饿汉式

```java
enum Sex {
    MALE, FEMALE;
}
```

#### 懒汉式

```java
private Singleton(){};
private static Singleton INSTANCE = null;
public static synchronized Singleton getInstance() {
    if (INSTANCE == null) {
        INSTANCE = new Singleton();
    }
    return INSTANCE;
}
```

#### DCL懒汉式

```java
private Singleton(){};
private static volatile Singleton INSTANCE = null;
public static Singleton getInstance() {
    if (INSTANCE == null) {
        synchronized (Singleton.class) {
            if (INSTANCE == null) {
        		INSTANCE = new Singleton();
            }
        }
    }
    return INSTANCE;
}
```

#### 内部类懒汉式

```java
private Singleton(){};
private static class Holder {
    static Singleton INSTANCE = new Singleton();
}

public static synchronized Singleton getInstance() {
    return Holder.INSTANCE;
}
```

#### 在jdk中的体现

RunTime类（饿汉式），Console类（懒汉式），Collections（EmptyNavigableSet，EmptyEnumeration，ReverseComparator懒汉式；EmptySet，EmptyList，EmptyMap饿汉式）

## 并发

### Java线程状态

New新建，start()进入Runnable

Runnable可运行，未获得锁对象进入Blocked，获得锁对象调用wait()进入waiting，sleep(毫秒)或wait(毫秒)进入Timed Waiting，执行完毕进入Terminated

Blocked锁阻塞，获得锁对象进入Runnable

Waiting无限等待，被其他线程notify并获得锁对象进入Runnable，被其他线程notify但未获得锁对象进入Blocked

Time Waiting计时等待，sleep时间到或wait时间到并获得锁对象或wait时间没到，被其他线程notify并获得锁对象，进入Runnable，wait时间到未获得锁对象或wait时间没到，其他线程notify但未获得锁对象，进入Blocked

Terminated被终止

### 操作系统线程状态

新建，就绪，运行，阻塞，终结

### lock vs synchronized

- synchronized是关键字，源码在jvm中，用c++实现，Lock是接口，源码在jdk中，java实现

- 使用synchronized时，退出同步代码块锁会自动释放，使用Lock时，要Unlock
- 都是悲观锁，互斥，同步，锁重入
- Lock可以获取等待状态，公平锁，可打断，可超时，多条件变量
- 没有竞争时，synchronized做了很多优化，偏向锁，轻量级锁，竞争激烈时，Lock性能更好

## 垃圾回收

实现无用对象内存自动释放，减少内存碎片，加快分配速度

- 标记清除（内存碎片）
- 标记整理，存活的对象向一端靠拢，效率较低
- 标记复制，内存分为两部分，第二部分什么都不存，垃圾回收时标记完把第一部分标记的对象复制到第二部分中，第一部分全部回收（占用了额外内存）

GC要点

- 回收区域是堆内存，不包括栈，方法调用结束会自动释放方法占用内存
- 判断无用对象，可达性分析算法，三色标记法标记存活对象，回收未标记对象
- 垃圾回收器
- GC大都采用分代回收思想，新生代和老年代不同回收策略
- 规模分成Minor GC，Mixed GC和Full GC

### 分代回收

伊甸园eden，最初的对象

幸存区surviver，伊甸园内存不足后回收的幸存对象，分成from和to，采用标记复制法

老年代old，幸存区对象熬过最多15次回收后，晋升到老年代

### 三色标记

黑色-已标记

灰色-标记中

白色-未标记
