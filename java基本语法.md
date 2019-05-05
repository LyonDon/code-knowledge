

[TOC]


javase：桌面级开发工具
javaee：企业级开发工具，包含javase
javame：嵌入式系统开发
***

### 面向对象程序设计基本原则

1.	消除代码复制
2.	设计类时，尽量让自己的所有成员变量都是私有的
3.	类和类之间的关系称作耦合，耦合越低越好，保持距离是形成良好代码的关键

一种跨平台语言,既是编译型，又是解释型

<h5 id="0">代码编写规范：</h5>

*	每个变量的声明尽量单独占一行
*	关键字之间只认一个空格
*	关键方法要加注释
*	注意使用半角英文符号
*	标识符（严格区分大小写）
	*	数字（不能放在最前面），字母，_，4
	*	不推荐使用中文命名任何的标识符

![标识符与关键字.jpg](.\Photo\标识符与关键字.jpg)

***

##运算符表达式与语句

a\++与\++a的区别：

*	a\++：先+再赋值
*	\++a：先赋值再+

![搭建开发环境.jpg](.\Photo\搭建开发环境.jpg)

程序调试

*	单步跳过：运行完这行代码后，继续运行之后的代码 
*	单步跳入：运行这行代码时，观察代码的运行情况 

注释（文档注释）

*API其实就是由文档注释自动生成的*

### 变量（可以通过赋值更改变量的值）

整型，长整型，字符型，布尔型

变量声明

	数据类型 变量名称=变量值

变量声明尽可能靠近变量第一次使用的地方

### 常量

常量声明

~~~java
	final(final是声明常量的关键字)数据类型 常量名称=常量值
~~~

习惯上，常量名全部用大写

***
## 基本数据类型

### 整型

类型|内存空间
---|---
byte|1字节
short|2字节
int（默认类型）|4字节
long|8字节

++给long赋值的时候要加上L或l的后缀，否则可能造成精度的丢失++

使用不同进制赋值

*	10进制：直接int
*	8进制：以0开头
*	16进制：以0x或者0X开头

### 浮点型

类型|内存空间
---|---
float|4字节
double（默认类型）|8字节

++给float赋值的时候，要加上F后缀，否则可能报错++

float型的值一般就直接在后面加上F

java浮点值是近似值，不精确，可以使用java提供的四舍五入方法Math.round()

### 字符型

概念：用单引号包含的可打印的单个字符

char可以有两种赋值方式

~~~java
char a1='汉';
char a2=27721;
//两种赋值方式相同，前一种是直接赋予字符，后一种是赋予ASCII码
~~~

转义字符

*\r和\n的区别：\r是回车，使光标回到行首；\n是换行，使光标下移一行*

### 布尔类型

boolean
只有true和false两种类型

### 数据类型转换

小类型向大类型转换

*	隐式转换（自动转换）

*	显示转换（强制转换）

	*	语法 （类型名）要转换的值
	>		int a=100;
	>		byte b=(byte)a;

大类型向小类型转换

*	显示转换（强制转换）

其中，char类型和int类型可以相互兼容

## 引用类型

java是强类型语言

*	基本类型

	*	boolean型

	*	数值类型

		*	基本类型（byte，short，int，long，char）

		*	浮点类型（double，float）

*	引用类型

	*	其他

* * *

## Java类包

不能调用两个包下的同名类

## 访问控制

访问控制修饰符

![1534835098.png](.\Photo\1534835098.png)

### final关键字

*	final类

	*	~~~java
		final class类名{}
		~~~
		**final类不能被继承**

*	final方法（不能被重写）

*	final常量（不能被修改）

## 内部类

在类体里面定义了一个类，这个类叫做内部类

*	成员内部类（定义在类内的内部类）

*	局部内部类（定义在成员方法内的内部类）

*	匿名内部类（在使用过程中才进行编写的内部类，没有名字）

*	静态内部类（提供一个调试的功能，可以将main方法写在静态内部类中）

### 内部类的继承

~~~java
class ClassA{
	class ClassB{
    }
}

class OutputInnerClass extends ClassA.ClassB{
}
~~~


内部类能直接访问外部的全部资源

*	包括任何私有的成员
*	外部是函数时，只能访问那个函数里面final的变量

### 包装类

*	包装类的变量可以转换为其对应基础类型的变量
*	也可以使用valueOf创建对象
*	数据类型通过包装类转换为对象供java进行处理

#### Integer

用于封装int类型数据

~~~java
Integer(int number)
Integer(String str)
Integer.valueOf(String str)
~~~

#### Double

~~~java
Double(int number)
Double(String str)
DOuble.valueOf(数字或字符串)
~~~

++如果基本的整数和浮点数不能满足精度需求，可以使用java.math包中的BigInteger类（实现任意精度的整数运算）和BigDecimal类（任意精度的浮点数运算），但是要使用add和multiply方法，同时，使用valueof方法可以将普通的数值转换为大数值++

#### Boolean

~~~java
Boolean(Boolean value)
Boolean(String str)
~~~

Boolean值默认为false

#### Character

~~~java
Character(char value)
~~~

### Number类

doubleValue
intValue
byteValue
floatValue

### 自动装箱和自动拆箱

装箱和拆箱就是包装类和基本数据类型的相互转换

*	一般方法
	~~~java
    Integer i=new Integer(100);//装箱
    int i=num.intValue(100);//拆箱
    ~~~

*	自动方法
	~~~java
	Integer num=100;//装箱
    int i=new Integer(100);//拆箱
    ~~~

特殊情况

*JVM会将相等的byte值和Boolean值保存在同一个对象*

### Math类

lava.lang.Math

*	java.lang包不需要引用，系统会自动调用
*	Math类提供的方法都是静态方法

~~~java
Math.round(x)=(int)Math.floor(x+0.5F);
//Math.floor向下取整
//Math.round四舍五入
~~~

### Math.random

返回一个大于0小于1的随机double值

### 随机数 Random类

~~~java
Random r=new Random();//实例化对象
//方法
nextInt(int n);//返回随机生成的int值，在0到n之间，小于n
nextLong();
nextDOuble();
nextFloat();
NextBoolean();
~~~

### Date类

java.util.Date

~~~java
Date date=new Date()
Date date=new Date(long time)
~~~

### Dateformat类

输出指定格式的时间

~~~java
Dateformat format=new SimpleDateFormat("yyyy-MM-dd");//使用子类进行实例化
~~~

getTimeZone方法：获取时区
setTimeZone方法：设置时区

### Calendar类

*垃圾！没有12月，是从0月开始！垃圾垃圾垃圾！*

~~~java
Calendar rightNow=Calendar.getInstance();

set():设置日历字段
get()：获取日历字段
add()/roll():添加时间量，前者会自动向前进位，后者不会进位
~~~

### 常量

创建枚举

~~~java
public enum 枚举类名{//enum代表一个枚举类型，类似class，interface
		枚举1，//默认为final public static类型
        枚举2，
        枚举3，
		……
}
~~~

不会存在像普通常量一样多个常量引用同一个值的问题

#### 枚举类型的常用方法

*	values():可将枚举类型的成员以数组的形式返回

	*	~~~java
	Constants enumArray[]=Constants.values();
		~~~

*	valueof():可将普通字符串转换为枚举实例

	*	~~~java
	public enum Constants{
    	Constants_A;
    }
    Constants c=Constants.valueOf("constants_A")//将字符串转换为枚举类型。创建的值一定要真实存在
		~~~

*	compareTo()：比较两个枚举对象在定义时的顺序

*	ordinal()：用于得到枚举成员的位置索引

### 创建枚举的类成员

*	枚举类型的常量成员必须在其他成员之前定义，否则这个枚举类型不会产生对象
*	构造方法必须是private修饰或者无修饰符

private:

只有这个类的内部可以访问

*	类内部指的是类的成员函数和定义初始化
*	这个限制是对类的而不是对对象的

public：

都可以访问


不带public和private

同一个包内可以访问



++一个.java文件是一个编译单元++

**注意：在枚举中添加了属性或者方法的话，在最后一个枚举成员之后要加上；**

####  枚举实现接口

~~~java
public enum 枚举名称 implements 接口名称{}
~~~

枚举类型可以实现一个或多个接口，但是不能继承类。枚举默认继承java.lang.Enum类

如果枚举成员没有实现接口的抽象方法，则需要在枚举下来调用实现父类的抽象方法，这样的话，之前实验抽象方法的枚举成员就相当于是重写了这个方法

#### 泛型

可以定义安全的数据类型

~~~java
类名<类型参数1，类型参数2，……，类型参数n>{}
~~~

泛型继承类或接口

**泛型仅仅是java的语法糖，不会影响java虚拟机生成的汇编代码，编译阶段，JVM就会将泛型的类型擦除，所以泛型不会对执行速度产生什么影响**

~~~java
A<T extends anyClass>a;
~~~

泛型通配符

*是在声明对象时使用，而不是在创建类的时候使用*

限制泛型类型，并可以限制泛型对象的使用

~~~java
A<?>a;
A<? extends anyClass> a;//使用extends关键字指定了上界
A<? super anyClass> a;//使用super关键字指定了下界
~~~

继承泛型类和泛型接口

~~~java
class ExtendClass<T>{
}//定义泛型

class SubClass<T> extends ExtendClass<T>{
}//继承泛型

interface TestInterface<T>{
}//定义泛型接口

class SubClass2<T> implements TestInterface<T>{
}//实现泛型接口
~~~

继承泛型的四种情况

~~~java
//第一种情况
abstract class Father<T1,T2>{}

//全部继承
class Child<T1,T2,T3> ectends Father<T1,T2>{}//子类可以在继承父类泛型的同时，拥有自己的泛型

//部分继承
class Child<T1,A,B> extends Father<T1,String>{}//父类实现了一个泛型（对父类的一个泛型实例化了），子类可以拥有多个泛型

//实现父类泛型
class Child<A,B> extends Father<Integer,String>{}//子类在继承父类泛型的同时，将父类的所有泛型全部直接实现了，子类的泛型就全部是自己独有的

//不实现父类的泛型
class Child extends Father(){}
~~~

## System.out和System.err的区别



前者能重定向到别的输出流，可能被缓冲，标准的输出流，黑色字体

后者只能实现打印，不会被缓冲，错误的输出流,红色字体

