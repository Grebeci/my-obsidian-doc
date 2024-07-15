# 一、基础知识

### 1.1 quick start

###### 1. Jave  Platform 




![](https://www.oracle.com/ocom/groups/public/@otn/documents/digitalasset/2167990.jpg)

- **JRE ** (Java Runtime Environment) ：是Java程序的运行时环境，包含`JVM` 和运行时所需要的`核心类库`。
- **JDK**  (Java Development Kit)：是Java程序开发工具包，包含`JRE` 和开发人员使用的工具。

我们想要运行一个已有的Java程序，那么只需安装`JRE` 即可。

我们想要开发一个全新的Java程序，那么必须安装`JDK` ，其内部包含`JRE`。

关系：

![jre-jdk的关系](https://images-note-wang.oss-cn-beijing.aliyuncs.com/note/java/1561383524152.png)

### 1.2 

###### 常量

* **常量：在程序执行的过程中，其值不可以发生改变的量**

* 常量的分类：

	* 自定义常量：通过final关键字定义（后面在面向对象部分讲解）

	* 字面值常量：

|      字面值常量       |      举例      |
| :-------------------: | :------------: |
|   字符串常量 string   |  ”HelloWorld“  |
|  整数常量（默认int）  |    12，-23     |
| 浮点常量默认（double) |     12.34      |
|     字符常量char      | ‘a’，'0'，‘沙’ |
|       布尔常量        |  true，false   |
|        空常量         |      null      |

###### 数据类型

Java的数据类型分为两大类：

- **基本数据类型**：包括 `整型`（byte,short,int,long)、`浮点型`（float，double）、`字符` char、`布尔` Boolean。 
- **引用数据类型**：包括 `类`、`数组`、`接口`。 



类型转换：

隐式类型转换

显式的类型转换 `数据类型 变量名 = （数据类型）被强转数据值；`



###### 运算符

1. 算术运算符，赋值运算符，关系运算符，逻辑运算符，三目运算符。
2. 运算符的优先级



###### 流程控制

分支结构

if-else 

switch

循环结构

for

while

do-while

break,continue





# 二、数据结构

### 2.1 数组

java 原生的数据结构，内存连续，容量确定，支持索引随机访问。

定义 : 原则是必须在编译时确定大小。

访问：索引位置访问

##### 遍历：

1. 循环-for
2. 函数式

##### 默认值

整型取0 ，float取`0.0F`, double 0.0, char 0 或 '\u0000' boolean false 引用类型 false



##### 数组的操作（API)

java 标准库 ：java.util.Arrays

Stream API

第三方 ：Apache Commons Lang库中的`ArrayUtils`



###### 数组工具类Arrays

java.util.Arrays数组工具类，提供了很多静态方法来对数组进行操作，而且如下每一个方法都有各种重载形式，以下只列出int[]类型的，其他类型的数组类推：

* static int binarySearch(int[] a, int key) ：要求数组有序，在数组中查找key是否存在，如果存在返回第一次找到的下标，不存在返回负数

* static int[] copyOf(int[] original, int newLength)  ：根据original原数组复制一个长度为newLength的新数组，并返回新数组

* static int[] copyOfRange(int[] original, int from, int to) ：复制original原数组的[from,to)构成新数组，并返回新数组

* static boolean equals(int[] a, int[] a2) ：比较两个数组的长度、元素是否完全相同

* static void fill(int[] a, int val) ：用val填充整个a数组
* static void fill(int[] a, int fromIndex, int toIndex, int val)：将a数组[fromIndex,toIndex)部分填充为val 
* static void sort(int[] a) ：将a数组按照从小到大进行排序
* static void sort(int[] a, int fromIndex, int toIndex) ：将a数组的[fromIndex, toIndex)部分按照升序排列
* static String toString(int[] a) ：把a数组的元素，拼接为一个字符串，形式为：[元素1，元素2，元素3。。。]

###### 其他

数组复制

- static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)： 
	从指定源数组中复制一个数组，复制从指定的位置开始，到目标数组的指定位置结束。常用于数组的插入和删除

二维数组是用以一维数组模拟出来的，相当于一维数组中存了每个一维数组的引用。





# 三、面向对象

#### 类的结构

- 属性：对象属性，类属性；对象属性有修饰符、类型、默认值。
- 方法：修饰符，返回值类型，参数，可变参数，方法重载，递归方法
- 修饰符

#### 封装

权限修饰符

属性封装-getter setter 方法

构造器-对初始化进行封装。

#### this的含义

this代表当前对象的引用（地址值），即对象自己的引用。

* this可以用于构造器中：表示正在创建的那个实例对象，即正在new谁，this就代表谁
* this用于实例方法中：表示调用该方法的对象，即谁在调用，this就代表谁。

#### this使用格式

1、this.成员变量名

当方法的局部变量与当前对象的成员变量重名时，就可以在成员变量前面加this.，如果没有重名问题，就可以省略this.

```java
this.成员变量名；
```

2、this.成员方法

调用当前对象自己的成员方法时，都可以加"this."，也可以省略，实际开发中都省略

```java
【变量=】this.成员方法(【实参列表】);
```

3、this()或this(实参列表)

当需要调用本类的其他构造器时，就可以使用该形式。

要求：

必须在构造器的首行

如果一个类中声明了n个构造器，则最多有 n - 1个构造器中使用了"this(【实参列表】)"，否则会发生递归调用死循环

#### JavaBean

`JavaBean` 是 Java语言编写类的一种标准规范。符合`JavaBean` 的类，要求类必须是具体的和公共的，并且具有无参数的构造方法，成员变量私有化，并提供用来操作成员变量的`set` 和`get` 方法。



#### Package

声明包的语法格式

```java
package 包名;
```

导入

```java
import 包.类名;
import 包.*;
import static 包.类名.静态成员; //后面补充
```

注意：

使用java.lang包下的类，不需要import语句，就直接可以使用简名称

import语句必须在package下面，class的上面

当使用两个不同包的同名类时，例如：java.util.Date和java.sql.Date。一个使用全名称，一个使用简名称

#### static

- 类属性，类方法
- 静态导入
- 静态代码块，作用是完成类的初始化。

堆上和栈上

- 对象属性，局部变量的问题是堆和栈的问题
- 传递引用还是传递数值 ==》 堆/栈
- 局部变量的作用域问题

#### 继承

继承是is-a 关系

继承元素 

* 父类中的成员，无论是公有(public)还是私有(private)，均会被子类继承。
* 子类虽会继承父类私有(private)的成员，但子类不能对继承的私有成员直接进行访问，可通过继承的公有方法进行访问。

- 属性：如果重名，需要用 `super` , `this` 指定调用成员属性。
- 方法：重写，final 方法不能重写。
- 构造器：不继承父类的构造方法，构造方法的作用是初始化成员变量的。所以子类的初始化过程中，**必须**先执行父类的初始化动作。子类的构造方法中默认有一个`super()` ，表示调用父类的构造方法，父类成员变量初始化后，才可以给子类使用。



#### 类和实例的初始化

##### 类初始化

类被加载内存后，会在方法区创建一个Class对象（反射）来存储该类的所有信息。此时会为类的静态变量分配内存，然后为类变量进行初始化。那么，实际上，类初始化的过程时在调用一个<clinit>()方法，而这个方法是编译器自动生成的。编译器会将如下两部分的**所有**代码，**按顺序**合并到类初始化<clinit>()方法体中。

（1）静态类成员变量的显式赋值语句

（2）静态代码块中的语句

整个类初始化只会进行一次，如果子类初始化时，发现父类没有初始化，那么会先初始化父类。



##### 实例初始化

实际上我们编写的代码在编译时，会自动处理代码，整理出一个<clinit>()的类初始化方法，还会整理出一个或多个的<init>(...)实例初始化方法。一个类有几个实例初始化方法，由这个类有几个构造器决定。

实例初始化方法的方法体，由四部分构成：

（1）super()或super(实参列表)    这里选择哪个，看原来构造器首行是哪句，没写，默认就是super()

（2）非静态实例变量的显示赋值语句

（3）非静态代码块

（4）对应构造器中的代码

特别说明：其中（2）和（3）是按顺序合并的，（1）一定在最前面（4）一定在最后面



#### 抽象类

- 不能被实例化，子类必须实现抽象类的抽象方法。
- 常用在强制子类必须重写某方法，或者不想类被实例化。



#### 多态

把子类的对象绑定到父类的引用上，在运行时调用子类真正调用子类重写的方法。

这样编译时类型是父类类型，运行时类型是子类类型。

- 原则：能够调用什么方法，看编译时类型；真正运行调用方法（能调用的方法）看运行时。

把父类类型强制转为子类类型，是为了调用父类特有的方法。instanceof 判断是不是子类类型。





#### Object根父类

类 `java.lang.Object`是类层次结构的根类，即所有类的父类。每个类都使用 `Object` 作为超类。所有对象（包括数组）都实现这个类的方法。

如果一个类没有特别指定父类，那么默认则继承自Object类。



Object类的API文档，Object类当中包含的方法有11个


（1）public String toString()：

①默认情况下，toString()返回的是“对象的运行时类型 @ 对象的hashCode值的十六进制形式"

②通常是建议重写，如果在eclipse中，可以用Alt +Shift + S-->Generate toString()

③如果我们直接System.out.println(对象)，默认会自动调用这个对象的toString()

（2）public final Class<?> getClass()：获取对象的运行时类型

（3）protected void finalize()：当对象被GC确定为要被回收的垃圾，在回收之前由GC帮你调用这个方法。而且这个方法只会被调用一次。子类可以选择重写。

（4）public int hashCode()：返回每个对象的hash值。

规定：①如果两个对象的hash值是不同的，那么这两个对象一定不相等；

​	②如果两个对象的hash值是相同的，那么这两个对象不一定相等。

主要用于后面当对象存储到哈希表等容中时，为了提高性能用的。



（5）public boolean equals(Object obj)：用于判断当前对象this与指定对象obj是否“相等”

①默认情况下，equals方法的实现等价于与“==”，比较的是对象的地址值

②我们可以选择重写，重写有些要求：

A：如果重写equals，那么一定要一起重写hashCode()方法，因为规定：

​	a：如果两个对象调用equals返回true，那么要求这两个对象的hashCode值一定是相等的；

​	b：如果两个对象的hashCode值不同的，那么要求这个两个对象调用equals方法一定是false；

​	c：如果两个对象的hashCode值相同的，那么这个两个对象调用equals可能是true，也可能是false

本质上是因为容器类（Set，HashMap) 判断两个对象是否相等是先调用Hashcode，如果相等在调用equals ,所以hashcode是equals的必要不充分条件。

B：如果重写equals，那么一定要遵循如下几个原则：

​	a：自反性：x.equals(x)返回true

​	b：传递性：x.equals(y)为true, y.equals(z)为true，然后x.equals(z)也应该为true

​	c：一致性：只要参与equals比较的属性值没有修改，那么无论何时调用结果应该一致

​	d：对称性：x.equals(y)与y.equals(x)结果应该一样

​	e：非空对象与null的equals一定是false

#### 接口

结构

在JDK8之前，接口中只运行出现：

（1）公共的静态的常量：其中public static final可以省略

（2）公共的抽象的方法：其中public abstract可以省略

在JDK1.8时，接口中允许声明默认方法和静态方法，必须显式声明static ，default关键字。public可以省略。

非抽象子类实现接口：

1. 必须重写接口中**所有**抽象方法。

2. 继承了接口的默认方法，即可以直接调用，也可以重写。

3. 可以实现多个接口

接口时可以多继承的，其中default 方法可以重写。





##### 冲突问题

- 父类优先
- 钻石问题：强制重写 或者 显式选择



##### 经典接口

java.lang.Comparable

java.util.Comparator



#### 匿名内部类

语法格式

```java
new 父类(【实参列表】){
    重写方法...
}
```

```java
new 父接口(){
    重写方法...
}
```

> 匿名内部类是没有名字的类，因此在声明类的同时就创建好了唯一的对象
>
> 思考：这个对象能做什么呢？
>
> 答：（1）调用某个方法（2）赋值给父类/父接口的变量，通过多态引用使用这个对象（3）作为某个方法调用的实参



（1）一般为了构造一个特定接口的对象，目的是调用这个接口具体的实现方法。

- 例如 ： 调用java.util.Arrays数组工具类的排序方法public static void sort(Object[] a, Comparator c)对数组的元素进行排序，用匿名内部类的对象给c形参传入按照薪资比较大小的定制比较器对象。并显示排序后结果。

（2）重写某个类的方法，同时编译时类型保持一致。简单来说就是自定义某个类型的行为。



#### 内部类



#### 枚举

一种类型只有固定的，特定的几个对象。



语法格式：

```java
【修饰符】 enum 枚举类名{
    常量对象列表
}

【修饰符】 enum 枚举类名{
    常量对象列表;
    
    其他成员列表;
}
```

###### 枚举类的要求和特点：

* 枚举类的常量对象列表必须在枚举类的首行，因为是常量，所以建议大写。
* 如果常量对象列表后面没有其他代码，那么“；”可以省略，否则不可以省略“；”。
* 编译器给枚举类默认提供的是private的无参构造，如果枚举类需要的是无参构造，就不需要声明，写常量对象列表时也不用加参数。
* 如果枚举类需要的是有参构造，需要手动定义private的有参构造，调用有参构造的方法就是在常量对象名后面加(实参列表)就可以。
* 枚举类默认继承的是java.lang.Enum类，因此不能再继承其他的类型。可以实现接口。
* JDK1.5之后switch，提供支持枚举类型，case后面可以写枚举常量名。

###### 枚举类型常用方法

```java
1.toString(): 默认返回的是常量名（对象名），可以继续手动重写该方法！
2.name():返回的是常量名（对象名） 【很少使用】
3.ordinal():返回常量的次序号，默认从0开始
4.values():返回该枚举类的所有的常量对象，返回类型是当前枚举的数组类型，是一个静态方法
5.valueOf(String name)：根据枚举常量对象名称获取枚举对象
```

#### 注解

注解也是一种注释，是给编译器读取的。



一个完整的注解有三个部分：

* 注解的声明：就如同类、方法、变量等一样，需要先声明后使用
* 注解的使用：用于注解在包、类、方法、属性、构造、局部变量等上面的10个位置中一个或多个位置
* 注解的读取：有一段专门用来读取这些使用的注解，然后根据注解信息作出相应的处理，这段程序称为注解处理流程，这也是注解区别与普通注释最大的不同。

系统预定义注释

```
@Override

@Deprecated

@SuppressWarnings
```

#### 包装类

基本类型不是对象

- 泛型容器：java实现的泛型容器只能存储对象

- java5 后可以自动装箱拆箱。

	装箱：Integer.valueOf(x) 

	拆箱：x.intValue()

- 包装类 整型缓存 -128~127 值， Character缓存 0~127 Boolean true和false

##### API

1、基本数据类型和字符串之间的转换

(1) 基本类型=> 字符串， 包装类型转字符串

```
String str = 1 + ""
```

（2）把字符串转为基本数据类型

parseXxx(String s) : Xxx是包装类型名称。

2、数据类型的最大最小值

```java
Integer.MAX_VALUE和Integer.MIN_VALUE
Long.MAX_VALUE和Long.MIN_VALUE
Double.MAX_VALUE和Double.MIN_VALUE
```

3、转大小写

```java
Character.toUpperCase('x');
Character.toLowerCase('X');

```

4、转进制

```java
Integer.toBinaryString(int i) 
Integer.toHexString(int i)
Integer.toOctalString(int i)
```



# 四、常用类



## 4.1 和数学相关的类

### 1、java.lang.Math

`java.lang.Math` 类包含用于执行基本数学运算的方法，如初等指数、对数、平方根和三角函数。类似这样的工具类，其所有方法均为静态方法，并且不会创建对象，调用起来非常简单。

* `public static double abs(double a) ` ：返回 double 值的绝对值。 

* `public static double ceil(double a)` ：返回大于等于参数的最小的整数。

* `public static double floor(double a) ` ：返回小于等于参数最大的整数。

* `public static long round(double a)` ：返回最接近参数的 long。(相当于四舍五入方法)  

* public static double pow(double a,double b)：返回a的b幂次方法
* public static double sqrt(double a)：返回a的平方根
* public static double random()：返回[0,1)的随机值
* public static final double PI：返回圆周率
* public static double max(double x, double y)：返回x,y中的最大值
* public static double min(double x, double y)：返回x,y中的最小值

### 2、java.math包

#### （1）BigInteger

不可变的任意精度的整数。

运算

* BigInteger(String val) 
* BigInteger add(BigInteger val)  
* BigInteger subtract(BigInteger val)
* BigInteger multiply(BigInteger val) 
* BigInteger divide(BigInteger val) 
* BigInteger remainder(BigInteger val)
* ....

#### （2）RoundingMode枚举类

CEILING ：向正无限大方向舍入的舍入模式。 
DOWN ：向零方向舍入的舍入模式。 
FLOOR：向负无限大方向舍入的舍入模式。 
HALF_DOWN ：向最接近数字方向舍入的舍入模式，如果与两个相邻数字的距离相等，则向下舍入。 
HALF_EVEN：向最接近数字方向舍入的舍入模式，如果与两个相邻数字的距离相等，则向相邻的偶数舍入。 
HALF_UP：向最接近数字方向舍入的舍入模式，如果与两个相邻数字的距离相等，则向上舍入。 
UNNECESSARY：用于断言请求的操作具有精确结果的舍入模式，因此不需要舍入。 
UP：远离零方向舍入的舍入模式。 



#### （3）BigDecimal

不可变的、任意精度的有符号十进制数。

* BigDecimal(String val) 
* BigDecimal add(BigDecimal val) 
* BigDecimal subtract(BigDecimal val)
* BigDecimal multiply(BigDecimal val) 
* BigDecimal divide(BigDecimal val) 
* BigDecimal divide(BigDecimal divisor, int roundingMode) 
* BigDecimal divide(BigDecimal divisor, int scale, RoundingMode roundingMode) 
* BigDecimal remainder(BigDecimal val) 
* ....

### 3、java.util.Random

用于产生随机数

* boolean nextBoolean():返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 boolean 值。 

* void nextBytes(byte[] bytes):生成随机字节并将其置于用户提供的 byte 数组中。 

* double nextDouble():返回下一个伪随机数，它是取自此随机数生成器序列的、在 0.0 和 1.0 之间均匀分布的 double 值。 

* float nextFloat():返回下一个伪随机数，它是取自此随机数生成器序列的、在 0.0 和 1.0 之间均匀分布的 float 值。 

* double nextGaussian():返回下一个伪随机数，它是取自此随机数生成器序列的、呈高斯（“正态”）分布的 double 值，其平均值是 0.0，标准差是 1.0。 

* int nextInt():返回下一个伪随机数，它是此随机数生成器的序列中均匀分布的 int 值。 

* int nextInt(int n):返回一个伪随机数，它是取自此随机数生成器序列的、在 0（包括）和指定值（不包括）之间均匀分布的 int 值。 

* long nextLong():返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 long 值。 



## 4.2 字符串

### 4.2.1 什么是字符串

- 所有的字符串字面量，都是 `java.lang.String` 的实例。

- String 内部是用字符串组实现的。

	JDK1.9之前有一个char[] value数组，JDK1.9之后byte[]**数组**

- 不可变变量，值可以共享。一旦修改就会产生新对象。

	内部实现了字符串常量池，值可以共享。

- Java 语言提供对字符串串联符号（"+"）以及将其他对象转换为字符串的特殊支持（toString()方法）。
- final 声明，不能被继承。



### 4.2.2 构造字符串对象

#### 1、使用构造方法

使用构造方法创建的对象是在堆上。

* `public String() ` ：初始化新创建的 String对象，以使其表示空字符序列。
* ` String(String original)`： 初始化一个新创建的 `String` 对象，使其表示一个与参数相同的字符序列；换句话说，新创建的字符串是该参数字符串的副本。
* `public String(char[] value) ` ：通过当前参数中的字符数组来构造新的String。
* `public String(char[] value,int offset, int count) ` ：通过字符数组的一部分来构造新的String。
* `public String(byte[] bytes) ` ：通过使用平台的默认字符集解码当前参数中的字节数组来构造新的String。
* `public String(byte[] bytes,String charsetName) ` ：通过使用指定的字符集解码当前参数中的字节数组来构造新的String。

#### 2、使用静态方法

* static String copyValueOf(char[] data)： 返回指定数组中表示该字符序列的 String
* static String copyValueOf(char[] data, int offset, int count)：返回指定数组中表示该字符序列的 String
* static String valueOf(char[] data)  ： 返回指定数组中表示该字符序列的 String
* static String valueOf(char[] data, int offset, int count) ： 返回指定数组中表示该字符序列的 String
* static String valueOf(xx  value)：xx支持各种数据类型，返回各种数据类型的value参数的字符串表示形式。

#### 3、使用""+

任意数据类型与"字符串"进行拼接，结果都是字符串

##### 面试题

下面代码创建了几个对象

```mysql
String str1 = "hello";//1个，在常量池中
String str3 = new String("hello");
//str3首先指向堆中的一个字符串对象，然后堆中字符串的value数组指向常量池中常量对象的value数组
```

字符串拼接产生的对象个数

原则：

（1）常量+常量：结果是常量池

（2）常量与变量 或 变量与变量：结果是堆

（3）拼接后调用intern方法：结果在常量池

（4）concat方法拼接，哪怕是两个常量对象拼接，结果也是在堆。

#### 4、字符串对象比较

1、==：比较是对象的地址

> 只有两个字符串变量都是指向字符串的常量对象时，才会返回true

```java
String str1 = "hello";
String str2 = "hello";
System.out.println(str1 == str2);//true
    
String str3 = new String("hello");
String str4 = new String("hello");
System.out.println(str1 == str4); //false
System.out.println(str3 == str4); //false
```

2、equals：比较是对象的内容，因为String类型重写equals，区分大小写

只要两个字符串的字符内容相同，就会返回true

```java
String str1 = "hello";
String str2 = "hello";
System.out.println(str1.equals(str2));//true
    
String str3 = new String("hello");
String str4 = new String("hello");
System.out.println(str1.equals(str3));//true
System.out.println(str3.equals(str4));//true
```

3、equalsIgnoreCase：比较的是对象的内容，不区分大小写

```java
String str1 = new String("hello");
String str2 = new String("HELLO");
System.out.println(str1.equalsIgnoreCase(strs)); //true
```

4、compareTo：String类型重写了Comparable接口的抽象方法，自然排序，按照字符的Unicode编码值进行比较大小的，严格区分大小写

```java
String str1 = "hello";
String str2 = "world";
str1.compareTo(str2) //小于0的值
```

5、compareToIgnoreCase：不区分大小写，其他按照字符的Unicode编码值进行比较大小

```java
String str1 = new String("hello");
String str2 = new String("HELLO");
str1.compareToIgnoreCase(str2)  //等于0
```

##### 空串比较

1、哪些是空字符串

```java
String str1 = "";
String str2 = new String();
String str3 = new String("");
```

空字符串：长度为0

2、如何判断某个字符串是否是空字符串

```java
if("".equals(str))

if(str!=null  && str.isEmpty())

if(str!=null && str.equals(""))

if(str!=null && str.length()==0)
```



### 4.2.3 常用API

#### 1、系列1

（1）boolean isEmpty()：字符串是否为空

（2）int length()：返回字符串的长度

（3）String concat(xx)：拼接，等价于+

（4）boolean equals(Object obj)：比较字符串是否相等，区分大小写

（5）boolean equalsIgnoreCase(Object obj)：比较字符串是否相等，不区分大小写

（6）int compareTo(String other)：比较字符串大小，区分大小写，按照Unicode编码值比较大小

（7）int compareToIgnoreCase(String other)：比较字符串大小，不区分大小写

（8）String toLowerCase()：将字符串中大写字母转为小写

（9）String toUpperCase()：将字符串中小写字母转为大写

（10）String trim()：去掉字符串前后空白符



#### 2、系列2：查找

（11）boolean contains(xx)：是否包含xx

（12）int indexOf(xx)：从前往后找当前字符串中xx，即如果有返回第一次出现的下标，要是没有返回-1

（13）int lastIndexOf(xx)：从后往前找当前字符串中xx，即如果有返回最后一次出现的下标，要是没有返回-1

#### 3、系列3：字符串截取

（14）String substring(int beginIndex) ：返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符串。 

（15）String substring(int beginIndex, int endIndex) ：返回一个新字符串，它是此字符串从beginIndex开始截取到endIndex(不包含)的一个子字符串。 

#### 4、系列4：和字符相关

（16）char charAt(index)：返回[index]位置的字符

（17）char[] toCharArray()： 将此字符串转换为一个新的字符数组返回

（18）String(char[] value)：返回指定数组中表示该字符序列的 String。 

（19）String(char[] value, int offset, int count)：返回指定数组中表示该字符序列的 String。

（20）static String copyValueOf(char[] data)： 返回指定数组中表示该字符序列的 String

（21）static String copyValueOf(char[] data, int offset, int count)：返回指定数组中表示该字符序列的 String

（22）static String valueOf(char[] data, int offset, int count) ： 返回指定数组中表示该字符序列的 String

（23）static String valueOf(char[] data)  ：返回指定数组中表示该字符序列的 String

#### 5、系列5：编码与解码

（24）byte[] getBytes()：编码，把字符串变为字节数组，按照平台默认的字符编码进行编码

​	byte[] getBytes(字符编码方式)：按照指定的编码方式进行编码

（25）new String(byte[] ) 或 new String(byte[], int, int)：解码，按照平台默认的字符编码进行解码

​     new String(byte[]，字符编码方式 ) 或 new String(byte[], int, int，字符编码方式)：解码，按照指定的编码方式进行解码

#### 6、系列6：开头与结尾

（26）boolean startsWith(xx)：是否以xx开头

（27）boolean endsWith(xx)：是否以xx结尾

#### 7、系列7：正则匹配

（28）boolean matchs(正则表达式)：判断当前字符串是否匹配某个正则表达式

#### 8、系列8：替换

（29）String replace(xx,xx)：不支持正则

（30）String replaceFirst(正则，value)：替换第一个匹配部分

（31）String repalceAll(正则， value)：替换所有匹配部分

#### 9、系列9：拆分

（32）String[] split(正则)：按照某种规则进行拆分

### 4.3.4 可变字符序列

因为String对象是不可变对象，虽然可以共享常量对象，但是对于频繁字符串的修改和拼接操作，效率极低。因此，JDK又在java.lang包提供了可变字符序列StringBuilder和StringBuffer类型。

StringBuffer：老的，线程安全的（因为它的方法有synchronized修饰）

StringBuilder：线程不安全的

常用的API，StringBuilder、StringBuffer的API是完全一致的



## 日期时间API

### 10.5.1 JDK1.8之前

1、java.util.Date

new  Date()：当前系统时间

long  getTime()：返回该日期时间对象距离1970-1-1 0.0.0 0毫秒之间的毫秒值

new Date(long 毫秒)：把该毫秒值换算成日期时间对象

2、java.util.Calendar：

（1）getInstance()：得到Calendar的对象

（2）get(常量)

3、java.text.SimpleDateFormat：日期时间的格式化

y：表示年

M：月

d：天

H： 小时，24小时制

h：小时，12小时制

m：分

s：秒

S：毫秒

E：星期

D：年当中的天数

### 10.5.2 JDK1.8之后

java.time及其子包中。

1、LocalDate、LocalTime、LocalDateTime

（1）now()：获取系统日期或时间

（2）of(xxx)：或者指定的日期或时间

（3）运算：运算后得到新对象，需要重新接受

plusXxx()：在当前日期或时间对象上加xx

minusXxx() ：在当前日期或时间对象上减xx

| 方法 | **描述** |
| ---- | -------- |
| now() / now(ZoneId zone) |                                     |
| of()                                                           |静态方法，根据指定日期/时间创建对象           |
| getDayOfMonth()/getDayOfYear()                                 |获得月份天数(1-31) /获得年份天数(1-366)      |
| getDayOfWeek()                                                 |获得星期几(返回一个 DayOfWeek 枚举值)        |
| getMonth()                                                     |获得月份, 返回一个 Month 枚举值                             |
| getMonthValue() / getYear()                                    |获得月份(1-12) /获得年份                                     |
| getHours()/getMinute()/getSecond()                             |获得当前对象对应的小时、分钟、秒                             |
| withDayOfMonth()/withDayOfYear()/withMonth()/withYear()      | 将月份天数、年份天数、月份、年份修改为指定的值并返回新的对象 |
| with(TemporalAdjuster  t)                     |将当前日期时间设置为校对器指定的日期时间|
| plusDays(), plusWeeks(), plusMonths(), plusYears(),plusHours() | 向当前对象添加几天、几周、几个月、几年、几小时               |
| minusMonths() / minusWeeks()/minusDays()/minusYears()/minusHours() | 从当前对象减去几月、几周、几天、几年、几小时                 |
| plus(TemporalAmount t)/minus(TemporalAmount t)                            |添加或减少一个 Duration 或 Period|
| isBefore()/isAfter()                                           |比较两个 LocalDate|
| isLeapYear()                        |判断是否是闰年（在LocalDate类中声明）|
| format(DateTimeFormatter  t)                         |格式化本地日期、时间，返回一个字符串|
| parse(Charsequence text)                           |将指定格式的字符串解析为日期、时间|

2、DateTimeFormatter：日期时间格式化

该类提供了三种格式化方法：

预定义的标准格式。如：ISO_DATE_TIME;ISO_DATE

本地化相关的格式。如：ofLocalizedDate(FormatStyle.MEDIUM)

自定义的格式。如：ofPattern(“yyyy-MM-dd hh:mm:ss”)





# 五、异常

为什么要有异常？

在没有异常的时候，比如shell，如果遇到错误了，还会继续执行。如果想要停止在错误的地方，就需要主动检测每一条命令的状态，错误触发exit。

有了异常之后，可以在错误地方中止执行，然后打印堆栈信息。额外的，还可以进行一些必要的处理。



##### java的异常体系

异常的根类是`java.lang.Throwable`，其下有两个子类：`java.lang.Error`与`java.lang.Exception`，平常所说的异常指`java.lang.Exception`。

我们平常说的异常就是指Exception，因为这类异常一旦出现，我们就要对代码进行更正，修复程序。

##### 分类

**异常(Exception)的分类**:根据在编译时期还是运行时期去检查异常?

* **编译时期异常**:checked异常。在编译时期,就会检查,如果没有处理异常,则编译失败。(如日期格式化异常)
* **运行时期异常**:runtime异常。在运行时期,检查异常.在编译时期,运行异常不会被编译器检测到(不报错)。(如数组索引越界异常，类型转换异常)。程序员应该积极避免其出现的异常，而不是使用try..catch处理，因为这类异常很普遍，若都使用try..catch或throws处理可能会对程序的可读性和运行效率产生影响。

##### 处理

异常对象怎么生成

- 虚拟机生成
- 手动创建：Exception exception = new ClassCastException()

##### 主动抛出异常

throw new 异常类名(参数);

##### 声明异常

**声明异常**：将问题标识出来，报告给调用者。如果方法内通过throw抛出了**编译时异常**，而没有捕获处理（稍后讲解该方式），那么必须通过throws进行声明，让调用者去处理。

关键字**throws**运用于方法声明之上,用于表示当前方法不处理异常,而是提醒该方法的调用者来处理异常(抛出异常).

**声明异常格式：**

~~~
修饰符 返回值类型 方法名(参数) throws 异常类名1,异常类名2…{   }	
~~~

声明异常的代码演示：

##### 处理异常

1. 抛出给上级调用者，throws ,也就是把异常声明出来。
2. 在方法中使用try-catch的语句块来处理异常。

```
try{
     编写可能会出现异常的代码
}catch(异常类型  e){
     处理异常的代码
     //记录日志/打印异常信息/继续抛出异常
}finally{
     无论try中是否发生异常，也无论catch是否捕获异常，也不管try和catch中是否有return语句，都一定会执行
 }
  
 或
  try{
     
 }finally{
     无论try中是否发生异常，也不管try中是否有return语句，都一定会执行
 } 
```



Throwable类中定义了一些查看方法:

* `public String getMessage()`:获取异常的描述信息,原因(提示给用户的时候,就提示错误原因。


* `public void printStackTrace()`:打印异常的跟踪栈信息并输出到控制台。

​            *包含了异常的类型,异常的原因,还包括异常出现的位置,在开发和调试阶段,都得使用printStackTrace。*

return 和try-catch

- finally 无论前面是不是有return，只要触发了try，就要执行。如果finally 有return，就覆盖前面的返回值。



处理多个异常的层次

- 异常有继承关系，子类异常放在前面。防止把异常吞了。



