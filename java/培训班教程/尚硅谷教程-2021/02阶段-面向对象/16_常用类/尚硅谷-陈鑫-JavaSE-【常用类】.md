# 16【常用类】

## 主要内容

* String类
* StringBuffer和StringBuilder
* BigInteger和BigDecimal
* Java8之前的日期时间API
* Java8的日期时间API

## 学习目标

* [ ] 能够熟练使用String的常用方法
* [ ] 能够区别String、StringBuffer、StringBuilder的不同
* [ ] 能够选择合适日期时间API处理日期时间
* [ ] 了解BigInteger和BigDecimal

# 第十章 常用类（续）

## 10.2 和数学相关的类

### 1、java.lang.Math

之前介绍过了

### 2、java.math包

#### （1）BigInteger

不可变的任意精度的整数。

* BigInteger(String val) 
* BigInteger add(BigInteger val)  
* BigInteger subtract(BigInteger val)
* BigInteger multiply(BigInteger val) 
* BigInteger divide(BigInteger val) 
* BigInteger remainder(BigInteger val)
* ....

```java
	@Test
	public void test01(){
//		long bigNum = 123456789123456789123456789L;
		
		BigInteger b1 = new BigInteger("123456789123456789123456789");
		BigInteger b2 = new BigInteger("78923456789123456789123456789");
		
//		System.out.println("和：" + (b1+b2));//错误的，无法直接使用+进行求和
		
		System.out.println("和：" + b1.add(b2));
		System.out.println("减：" + b1.subtract(b2));
		System.out.println("乘：" + b1.multiply(b2));
		System.out.println("除：" + b2.divide(b1));
		System.out.println("余：" + b2.remainder(b1));
	}
```

#### （2）RoundingMode枚举类

CEILING ：向正无限大方向舍入的舍入模式。 
DOWN ：向零方向舍入的舍入模式。 
FLOOR：向负无限大方向舍入的舍入模式。 
HALF_DOWN ：向最接近数字方向舍入的舍入模式，如果与两个相邻数字的距离相等，则向下舍入。 
HALF_EVEN：向最接近数字方向舍入的舍入模式，如果与两个相邻数字的距离相等，则向相邻的偶数舍入。 
HALF_UP：向最接近数字方向舍入的舍入模式，如果与两个相邻数字的距离相等，则向上舍入。 
UNNECESSARY：用于断言请求的操作具有精确结果的舍入模式，因此不需要舍入。 
UP：远离零方向舍入的舍入模式。 

![1563118999889](1563118999889.png)

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

```java
	@Test
	public void test02(){
		/*double big = 12.123456789123456789123456789;
		System.out.println("big = " + big);*/
		
		BigDecimal b1 = new BigDecimal("123.45678912345678912345678912345678");
		BigDecimal b2 = new BigDecimal("7.8923456789123456789123456789998898888");
		
//		System.out.println("和：" + (b1+b2));//错误的，无法直接使用+进行求和
		
		System.out.println("和：" + b1.add(b2));
		System.out.println("减：" + b1.subtract(b2));
		System.out.println("乘：" + b1.multiply(b2));
		System.out.println("除：" + b1.divide(b2,20,RoundingMode.UP));//divide(BigDecimal divisor, int scale, int roundingMode)
		System.out.println("除：" + b1.divide(b2,20,RoundingMode.DOWN));//divide(BigDecimal divisor, int scale, int roundingMode)
		System.out.println("余：" + b1.remainder(b2));
	}
```

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

```java
	@Test
	public void test03(){
		Random r = new Random();
		System.out.println("随机整数：" + r.nextInt());
		System.out.println("随机小数：" + r.nextDouble());
		System.out.println("随机布尔值：" + r.nextBoolean());
	}
```

## 10.3 字符串

`java.lang.String` 类代表字符串。Java程序中所有的字符串文字（例如`"abc"` ）都可以被看作是实现此类的实例。字符串是常量；它们的值在创建之后不能更改。字符串缓冲区支持可变的字符串。因为 String 对象是不可变的，所以可以共享。

`String` 类包括的方法可用于检查序列的单个字符、比较字符串、搜索字符串、提取子字符串、创建字符串副本并将所有字符全部转换为大写或小写。 

Java 语言提供对字符串串联符号（"+"）以及将其他对象转换为字符串的特殊支持（toString()方法）。

### 10.3.1 字符串的特点

1、字符串String类型本身是final声明的，意味着我们不能继承String。

2、字符串的对象也是不可变对象，意味着一旦进行修改，就会产生新对象

> 我们修改了字符串后，如果想要获得新的内容，必须重新接受。
>
> 如果程序中涉及到大量的字符串的修改操作，那么此时的时空消耗比较高。可能需要考虑使用StringBuilder或StringBuffer的可变字符序列。

3、String对象内部是用字符数组进行保存的

> JDK1.9之前有一个char[] value数组，JDK1.9之后byte[]数组

`"abc"` 等效于 `char[] data={ 'a' , 'b' , 'c' }`。

```java
例如： 
String str = "abc";

相当于： 
char data[] = {'a', 'b', 'c'};     
String str = new String(data);
// String底层是靠字符数组实现的。
```

4、String类中这个char[] values数组也是final修饰的，意味着这个数组不可变，然后它是private修饰，外部不能直接操作它，String类型提供的所有的方法都是用新对象来表示修改后内容的，所以保证了String对象的不可变。

5、就因为字符串对象设计为不可变，那么所以字符串有常量池来保存很多常量对象

常量池在方法区。

如果细致的划分：

（1）JDK1.6及其之前：方法区

（2）JDK1.7：堆

（3）JDK1.8：元空间

```java
String s1 = "abc";
String s2 = "abc";
System.out.println(s1 == s2);
// 内存中只有一个"abc"对象被创建，同时被s1和s2共享。
```

### 10.3.2 构造字符串对象

#### 1、使用构造方法

* `public String() ` ：初始化新创建的 String对象，以使其表示空字符序列。
* ` String(String original)`： 初始化一个新创建的 `String` 对象，使其表示一个与参数相同的字符序列；换句话说，新创建的字符串是该参数字符串的副本。
* `public String(char[] value) ` ：通过当前参数中的字符数组来构造新的String。
* `public String(char[] value,int offset, int count) ` ：通过字符数组的一部分来构造新的String。
* `public String(byte[] bytes) ` ：通过使用平台的默认字符集解码当前参数中的字节数组来构造新的String。
* `public String(byte[] bytes,String charsetName) ` ：通过使用指定的字符集解码当前参数中的字节数组来构造新的String。

构造举例，代码如下：

```java
//字符串常量对象
String str = "hello";

// 无参构造
String str1 = new String（）；

//创建"hello"字符串的副本
String str2 = new String("hello");

//通过字符数组构造
char chars[] = {'a', 'b', 'c','d','e'};     
String str3 = new String(chars);
String str4 = new String(chars,0,3);

// 通过字节数组构造
byte bytes[] = { 97, 98, 99 };     
String str5 = new String(bytes);
String str6 = new String(bytes,"GBK");
```

#### 2、使用静态方法

* static String copyValueOf(char[] data)： 返回指定数组中表示该字符序列的 String
* static String copyValueOf(char[] data, int offset, int count)：返回指定数组中表示该字符序列的 String
* static String valueOf(char[] data)  ： 返回指定数组中表示该字符序列的 String
* static String valueOf(char[] data, int offset, int count) ： 返回指定数组中表示该字符序列的 String
* static String valueOf(xx  value)：xx支持各种数据类型，返回各种数据类型的value参数的字符串表示形式。

```java
	public static void main(String[] args) {
		char[] data = {'h','e','l','l','o','j','a','v','a'};
		String s1 = String.copyValueOf(data);
		String s2 = String.copyValueOf(data,0,5);
		int num = 123456;
		String s3 = String.valueOf(num);
		System.out.println(s1);
		System.out.println(s2);
		System.out.println(s3);
	}
```

#### 3、使用""+

任意数据类型与"字符串"进行拼接，结果都是字符串

```java
	public static void main(String[] args) {
		int num = 123456;
		String s = num + "";
		System.out.println(s);
		
		Student stu = new Student();
		String s2 = stu + "";//自动调用对象的toString()，然后与""进行拼接
		System.out.println(s2);
	}
```

### 10.3.3 字符串的对象的个数

1、字符串常量对象

```java
String str1 = "hello";//1个，在常量池中
```

2、字符串的普通对象和常量对象一起

```java
String str3 = new String("hello");
//str3首先指向堆中的一个字符串对象，然后堆中字符串的value数组指向常量池中常量对象的value数组
```

### 10.3.4 字符串对象的内存分析

```
String s; // 栈上

String s = null; 

String s = "";
String s = new String();
String s = new String("");

String s = "abc";
String s = new String("abc");

char[] arr = {'a','b'};
String s = new String(arr);


char[] arr = {'a','b','c','d','e'};
String s = new String(arr,0,3);
```

### 10.3.5 字符串拼接问题

#### 1、拼接结果的存储和比较问题

原则：

（1）常量+常量：结果是常量池

（2）常量与变量 或 变量与变量：结果是堆

（3）拼接后调用intern方法：结果在常量池

```java
	@Test
	public void test06(){
		String s1 = "hello";
		String s2 = "world";
		String s3 = "helloworld";
		
		String s4 = (s1 + "world").intern();//把拼接的结果放到常量池中
		String s5 = (s1 + s2).intern();
		
		System.out.println(s3 == s4);//true
		System.out.println(s3 == s5);//true
	}
	
	@Test
	public void test05(){
		final String s1 = "hello";
		final String s2 = "world";
		String s3 = "helloworld";
		
		String s4 = s1 + "world";//s4字符串内容也helloworld，s1是常量，"world"常量，常量+ 常量 结果在常量池中
		String s5 = s1 + s2;//s5字符串内容也helloworld，s1和s2都是常量，常量+ 常量 结果在常量池中
		String s6 = "hello" + "world";//常量+ 常量 结果在常量池中，因为编译期间就可以确定结果
		
		System.out.println(s3 == s4);//true
		System.out.println(s3 == s5);//true
		System.out.println(s3 == s6);//true
	}
	
	@Test
	public void test04(){
		String s1 = "hello";
		String s2 = "world";
		String s3 = "helloworld";
		
		String s4 = s1 + "world";//s4字符串内容也helloworld，s1是变量，"world"常量，变量 + 常量的结果在堆中
		String s5 = s1 + s2;//s5字符串内容也helloworld，s1和s2都是变量，变量 + 变量的结果在堆中
		String s6 = "hello" + "world";//常量+ 常量 结果在常量池中，因为编译期间就可以确定结果
		
		System.out.println(s3 == s4);//false
		System.out.println(s3 == s5);//false
		System.out.println(s3 == s6);//true
	}
```

![1562946547647](1562946547647.png)

![1562946558630](1562946558630.png)

#### 2、拼接效率问题

```java
public class TestString {
	public static void main(String[] args) {
		String str = "0";
		for (int i = 0; i <= 5; i++) {
			str += i;  
		}
		System.out.println(str);
	}
}
```

![1562946595771](1562946595771.png)

不过现在的JDK版本，都会使用可变字符序列对如上代码进行优化，我们反编译查看字节码：

```cmd
javap -c TestString.class
```

![1563106868437](1563106868437.png)

#### 3、两种拼接

```java
public class TestString {
	public static void main(String[] args) {
		String str = "hello";
		String str2 = "world";
		String str3 ="helloworld";
		
		String str4 = "hello".concat("world");
		String str5 = "hello"+"world";
		
		System.out.println(str3 == str4);//false
		System.out.println(str3 == str5);//true
	}
}
```

> concat方法拼接，哪怕是两个常量对象拼接，结果也是在堆。

### 10.3.6  字符串对象的比较

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

### 10.3.7 空字符的比较

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

### 10.3.8 字符串的常用方法

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

```java
	@Test
	public void test01(){
		//将用户输入的单词全部转为小写，如果用户没有输入单词，重新输入
		Scanner input = new Scanner(System.in);
		String word;
		while(true){
			System.out.print("请输入单词：");
			word = input.nextLine();
			if(word.trim().length()!=0){
				word = word.toLowerCase();
				break;
			}
		}
		System.out.println(word);
	}

	@Test
	public void test02(){
        //随机生成验证码，验证码由0-9，A-Z,a-z的字符组成
		char[] array = new char[26*2+10];
		for (int i = 0; i < 10; i++) {
			array[i] = (char)('0' + i);
		}
		for (int i = 10,j=0; i < 10+26; i++,j++) {
			array[i] = (char)('A' + j);
		}
		for (int i = 10+26,j=0; i < array.length; i++,j++) {
			array[i] = (char)('a' + j);
		}
		String code = "";
		Random rand = new Random();
		for (int i = 0; i < 4; i++) {
			code += array[rand.nextInt(array.length)];
		}
		System.out.println("验证码：" + code);
		//将用户输入的单词全部转为小写，如果用户没有输入单词，重新输入
		Scanner input = new Scanner(System.in);
		System.out.print("请输入验证码：");
		String inputCode = input.nextLine();
		
		if(!code.equalsIgnoreCase(inputCode)){
			System.out.println("验证码输入不正确");
		}
	}
```

#### 2、系列2：查找

（11）boolean contains(xx)：是否包含xx

（12）int indexOf(xx)：从前往后找当前字符串中xx，即如果有返回第一次出现的下标，要是没有返回-1

（13）int lastIndexOf(xx)：从后往前找当前字符串中xx，即如果有返回最后一次出现的下标，要是没有返回-1

```java
	@Test
	public void test01(){
		String str = "尚硅谷是一家靠谱的培训机构，尚硅谷可以说是IT培训的小清华，JavaEE是尚硅谷的当家学科，尚硅谷的大数据培训是行业独角兽。尚硅谷的前端和运维专业一样独领风骚。";
		System.out.println("是否包含清华：" + str.contains("清华"));
		System.out.println("培训出现的第一次下标：" + str.indexOf("培训"));
		System.out.println("培训出现的最后一次下标：" + str.lastIndexOf("培训"));
	}
```

#### 3、系列3：字符串截取

（14）String substring(int beginIndex) ：返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符串。 

（15）String substring(int beginIndex, int endIndex) ：返回一个新字符串，它是此字符串从beginIndex开始截取到endIndex(不包含)的一个子字符串。 

```java
	@Test
	public void test01(){
		String str = "helloworldjavaatguigu";
		String sub1 = str.substring(5);
		String sub2 = str.substring(5,10);
		System.out.println(sub1);
		System.out.println(sub2);
	}

	@Test
	public void test02(){
		String fileName = "快速学习Java的秘诀.dat";
		//截取文件名
		System.out.println("文件名：" + fileName.substring(0,fileName.lastIndexOf(".")));
		//截取后缀名
		System.out.println("后缀名：" + fileName.substring(fileName.lastIndexOf(".")));
	}
```

#### 4、系列4：和字符相关

（16）char charAt(index)：返回[index]位置的字符

（17）char[] toCharArray()： 将此字符串转换为一个新的字符数组返回

（18）String(char[] value)：返回指定数组中表示该字符序列的 String。 

（19）String(char[] value, int offset, int count)：返回指定数组中表示该字符序列的 String。

（20）static String copyValueOf(char[] data)： 返回指定数组中表示该字符序列的 String

（21）static String copyValueOf(char[] data, int offset, int count)：返回指定数组中表示该字符序列的 String

（22）static String valueOf(char[] data, int offset, int count) ： 返回指定数组中表示该字符序列的 String

（23）static String valueOf(char[] data)  ：返回指定数组中表示该字符序列的 String

```java
	@Test
	public void test01(){
		//将字符串中的字符按照大小顺序排列
		String str = "helloworldjavaatguigu";
		char[] array = str.toCharArray();
		Arrays.sort(array);
		str = new String(array);
		System.out.println(str);
	}
	
	@Test
	public void test02(){
		//将首字母转为大写
		String str = "jack";
		str = Character.toUpperCase(str.charAt(0))+str.substring(1);
		System.out.println(str);
	}
```

#### 5、系列5：编码与解码

（24）byte[] getBytes()：编码，把字符串变为字节数组，按照平台默认的字符编码进行编码

​	byte[] getBytes(字符编码方式)：按照指定的编码方式进行编码

（25）new String(byte[] ) 或 new String(byte[], int, int)：解码，按照平台默认的字符编码进行解码

​           new String(byte[]，字符编码方式 ) 或 new String(byte[], int, int，字符编码方式)：解码，按照指定的编码方式进行解码

```java
	/*
	 * GBK，UTF-8，ISO8859-1所有的字符编码都向下兼容ASCII码
	 */
	public static void main(String[] args) throws Exception {
		String str = "中国";
		System.out.println(str.getBytes("ISO8859-1").length);// 2
		// ISO8859-1把所有的字符都当做一个byte处理，处理不了多个字节
		System.out.println(str.getBytes("GBK").length);// 4 每一个中文都是对应2个字节
		System.out.println(str.getBytes("UTF-8").length);// 6 常规的中文都是3个字节

		/*
		 * 不乱码：（1）保证编码与解码的字符集名称一样（2）不缺字节
		 */
		System.out.println(new String(str.getBytes("ISO8859-1"), "ISO8859-1"));// 乱码
		System.out.println(new String(str.getBytes("GBK"), "GBK"));// 中国
		System.out.println(new String(str.getBytes("UTF-8"), "UTF-8"));// 中国
	}
```

##### 字符编码发展

##### **ASCII码**

计算机一开始发明的时候是用来解决数字计算的问题，后来人们发现，计算机还可以做更多的事，例如文本处理。但由于计算机只识“数”，因此人们必须告诉计算机哪个数字来代表哪个特定字符，例如65代表字母‘A’，66代表字母‘B’，以此类推。但是计算机之间字符-数字的对应关系必须得一致，否则就会造成同一段数字在不同计算机上显示出来的字符不一样。因此美国国家标准协会ANSI制定了一个标准，规定了常用字符的集合以及每个字符对应的编号，这就是ASCII字符集（Character Set），也称ASCII码。

那时候的字符编解码系统非常简单，就是简单的查表过程。其中：

* 0～31及127(共33个)是控制字符或通信专用字符（其余为可显示字符），如控制符：LF（换行）、CR（回车）、FF（换页）、DEL（删除）、BS（退格)
* 32～126(共95个)是字符(32是空格），其中48～57为0到9十个阿拉伯数字。
* 65～90为26个大写英文字母，97～122号为26个小写英文字母，其余为一些标点符号、运算符号等。

##### **OEM字符集的衍生**

当计算机开始发展起来的时候，人们逐渐发现，ASCII字符集里那可怜的128个字符已经不能再满足他们的需求了。人们就在想，一个字节能够表示的数字（编号）有256个，而ASCII字符只用到了0x00~0x7F，也就是占用了前128个，后面128个数字不用白不用，因此很多人打起了后面这128个数字的主意。可是问题在于，很多人同时有这样的想法，但是大家对于0x80-0xFF这后面的128个数字分别对应什么样的字符，却有各自的想法。这就导致了当时销往世界各地的机器上出现了大量各式各样的OEM字符集。不同的OEM字符集导致人们无法跨机器交流各种文档。例如职员甲发了一封简历résumés给职员乙，结果职员乙看到的却是r?sum?s，因为é字符在职员甲机器上的OEM字符集中对应的字节是0x82，而在职员乙的机器上，由于使用的OEM字符集不同，对0x82字节解码后得到的字符却是?。

##### **多字节字符集（MBCS）和中文字符集**

上面我们提到的字符集都是基于单字节编码，也就是说，一个字节翻译成一个字符。这对于拉丁语系国家来说可能没有什么问题，因为他们通过扩展第8个比特，就可以得到256个字符了，足够用了。但是对于亚洲国家来说，256个字符是远远不够用的。因此这些国家的人为了用上电脑，又要保持和ASCII字符集的兼容，就发明了多字节编码方式，相应的字符集就称为多字节字符集（Muilti-Bytes Charecter Set）。例如中国使用的就是双字节字符集编码。

例如目前最常用的中文字符集GB2312，涵盖了所有简体字符以及一部分其他字符；GBK（K代表扩展的意思）则在GB2312的基础上加入了对繁体字符等其他非简体字符。这两个字符集的字符都是使用1-2个字节来表示。Windows系统采用936代码页来实现对GBK字符集的编解码。在解析字节流的时候，如果遇到字节的最高位是0的话，那么就使用936代码页中的第1张码表进行解码，这就和单字节字符集的编解码方式一致了。如果遇到字节的最高位是1的话，那么就表示需要两个字节值才能对应一个字符。

![1563199557136](1563199557136.png)

##### **ANSI标准、国家标准、ISO标准**

不同ASCII衍生字符集的出现，让文档交流变得非常困难，因此各种组织都陆续进行了标准化流程。例如美国ANSI组织制定了ANSI标准字符编码（注意，我们现在通常说到ANSI编码，通常指的是平台的默认编码，例如英文操作系统中是ISO-8859-1，中文系统是GBK），ISO组织制定的各种ISO标准字符编码，还有各国也会制定一些国家标准字符集，例如中国的GBK，GB2312和GB18030。

操作系统在发布的时候，通常会往机器里预装这些标准的字符集还有平台专用的字符集，这样只要你的文档是使用标准字符集编写的，通用性就比较高了。例如你用GB2312字符集编写的文档，在中国大陆内的任何机器上都能正确显示。同时，我们也可以在一台机器上阅读多个国家不同语言的文档了，前提是本机必须安装该文档使用的字符集。

##### **Unicode的出现**

虽然通过使用不同字符集，我们可以在一台机器上查阅不同语言的文档，但是我们仍然**无法解决一个问题：如果一份文档中含有不同国家的不同语言的字符，那么无法在一份文档中显示所有字符**。为了解决这个问题，我们需要一个全人类达成共识的巨大的字符集，这就是Unicode字符集。

Unicode字符集涵盖了目前人类使用的所有字符，并为每个字符进行统一编号，分配唯一的字符码（Code Point）。Unicode字符集将所有字符按照使用上的频繁度划分为17个层面（Plane），每个层面上有216=65536个字符码空间。其中第0个层面BMP，基本涵盖了当今世界用到的所有字符。其他的层面要么是用来表示一些远古时期的文字，要么是留作扩展。我们平常用到的Unicode字符，一般都是位于BMP层面上的。目前Unicode字符集中尚有大量字符空间未使用。

Unicode同样也不完美，这里就有三个的问题，一个是，我们已经知道，英文字母只用一个字节表示就够了，第二个问题是如何才能区别Unicode和ASCII？计算机怎么知道两个字节表示一个符号，而不是分别表示两个符号呢？第三个，如果和GBK等双字节编码方式一样，用最高位是1或0表示两个字节和一个字节，就少了很多值无法用于表示字符，不够表示所有字符。Unicode在很长一段时间内无法推广，直到互联网的出现，为解决Unicode如何在网络上传输的问题，于是面向传输的众多 UTF（UCS Transfer Format）标准出现了，顾名思义，UTF-8就是每次8个位传输数据，而UTF-16就是每次16个位。UTF-8就是在互联网上使用最广的一种Unicode的实现方式，这是为传输而设计的编码，并使编码无国界，这样就可以显示全世界上所有文化的字符了。

UTF-8最大的一个特点，就是它是一种变长的编码方式。它可以使用1~4个字节表示一个符号。从unicode到uft-8并不是直接的对应，而是要过一些算法和规则来转换（**即Uncidoe字符集≠UTF-8编码方式**）。

并不是直接的对应，而是要过一些算法和规则来转换（**即Uncidoe字符集≠UTF-8编码方式**）。

Unicode符号范围         | UTF-8编码方式

(十六进制)                      |        （二进制）

—————————————————————–

0000 0000-0000 007F | 0xxxxxxx（兼容原来的ASCII）

0000 0080-0000 07FF | 110xxxxx 10xxxxxx

0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx

0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx

![1563199860263](1563199860263.png)

因此，Unicode只是定义了一个庞大的、全球通用的字符集，并为每个字符规定了唯一确定的编号，具体存储成什么样的字节流，取决于字符编码方案。推荐的Unicode编码是UTF-16和UTF-8。

早期字符编码、字符集和代码页等概念都是表达同一个意思。例如GB2312字符集、GB2312编码，936代码页，实际上说的是同个东西。

但是对于Unicode则不同，Unicode字符集只是定义了字符的集合和唯一编号，Unicode编码，则是对UTF-8、UCS-2/UTF-16等具体编码方案的统称而已，并不是具体的编码方案。所以当需要用到字符编码的时候，你可以写gb2312，codepage936，utf-8，utf-16，但请不要写Unicode。

#### 6、系列6：开头与结尾

（26）boolean startsWith(xx)：是否以xx开头

（27）boolean endsWith(xx)：是否以xx结尾

```java
	@Test
	public void test2(){
		String name = "张三";
		System.out.println(name.startsWith("张"));
	}
	
	@Test
	public void test(){
		String file = "Hello.txt";
		if(file.endsWith(".java")){
			System.out.println("Java源文件");
		}else if(file.endsWith(".class")){
			System.out.println("Java字节码文件");
		}else{
			System.out.println("其他文件");
		}
	}
```

#### 7、系列7：正则匹配

（28）boolean matchs(正则表达式)：判断当前字符串是否匹配某个正则表达式

##### 常用正则表达式：

##### 字符类

`[abc]`：`a`、`b` 或 `c`（简单类）

`[^abc]`：任何字符，除了 `a`、`b` 或 `c`（否定）

`[a-zA-Z]`：`a` 到 `z` 或 `A` 到 `Z`，两头的字母包括在内（范围）

##### 预定义字符类

`.`：任何字符（与[行结束符](#lt)可能匹配也可能不匹配）

`\d`：数字：`[0-9]`

`\D`：非数字： `[^0-9]`

`\s`：空白字符：`[ \t\n\x0B\f\r]`

`\S`：非空白字符：`[^\s]`

`\w`：单词字符：`[a-zA-Z_0-9]`

`\W`：非单词字符：`[^\w]`

##### 边界匹配器

`^`：行的开头

`$`：行的结尾

##### Greedy 数量词

*X*`?`：*X*，一次或一次也没有

*X*`*`：*X*，零次或多次

*X*`+`：*X*，一次或多次

*X*`{`*n*`}`：*X*，恰好 *n* 次

*X*`{`*n*`,}`：*X*，至少 *n* 次

*X*`{`*n*`,`*m*`}`：*X*，至少 *n* 次，但是不超过 *m* 次

##### Logical 运算符

*XY*：*X* 后跟 *Y*

*X*`|`*Y*：*X* 或 *Y*

`(`*X*`)`：X，作为捕获组

```java
	@Test
	public void test1(){
		//简单判断是否全部是数字，这个数字可以是1~n位
		String str = "12a345";
		
		//正则不是Java的语法，它是独立与Java的规则
		//在正则中\是表示转义，
		//同时在Java中\也是转义
		boolean flag = str.matches("\\d+");
		System.out.println(flag);
	}
	
	@Test
	public void test2(){
		String str = "123456789";
		
		//判断它是否全部由数字组成，并且第1位不能是0，长度为9位
		//第一位不能是0，那么数字[1-9]
		//接下来8位的数字，那么[0-9]{8}+
		boolean flag = str.matches("[1-9][0-9]{8}+");
		System.out.println(flag);
	}
```

1.验证用户名和密码，第一个字必须为字母，一共6~16位字母数字下划线组成：（"^[a-zA-Z]\w{5,15}$"）

2.验证电话号码：（"^(\d{3,4}-)\d{7,8}$"）正确格式：xxx/xxxx-xxxxxxx/xxxxxxxx

3.验证手机号码：("^1[3|4|5|7|8][0-9]{9}$")

4.验证身份证号（15位）：("\d{14}[[0-9],0-9xX]")，（18位）：("\d{17}(\d|X|x)")

5.验证Email地址：("^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$")

6.只能输入由数字和26个英文字母组成的字符串：("^[A-Za-z0-9]+$")

7.整数或者小数：("^[0-9]+([.][0-9]+){0,1}$")

#### 8、系列8：替换

（29）String replace(xx,xx)：不支持正则

（30）String replaceFirst(正则，value)：替换第一个匹配部分

（31）String repalceAll(正则， value)：替换所有匹配部分

```java
	@Test
	public void test4(){
		String str = "hello244world.java;887";
		//把其中的非字母去掉
		str = str.replaceAll("[^a-zA-Z]", "");
		System.out.println(str);
	}
```

#### 9、系列9：拆分

（32）String[] split(正则)：按照某种规则进行拆分

```java
java
@Test
public void test4(){
	String str = "张三.23|李四.24|王五.25";
	//|在正则中是有特殊意义，我这里要把它当做普通的|
	String[] all = str.split("\\|");
	
	//转成一个一个学生对象
	Student[] students = new Student[all.length];
	for (int i = 0; i < students.length; i++) {
		//.在正则中是特殊意义，我这里想要表示普通的.
		String[] strings = all[i].split("\\.");//张三,  23
		String name = strings[0];
		int age = Integer.parseInt(strings[1]);
		students[i] = new Student(name,age);
	}
	
	for (int i = 0; i < students.length; i++) {
		System.out.println(students[i]);
	}
	
}

@Test
public void test3(){
	String str = "1Hello2World3java4atguigu5";
	str = str.replaceAll("^\\d|\\d$", "");
	String[] all = str.split("\\d");
	for (int i = 0; i < all.length; i++) {
		System.out.println(all[i]);
	}
}

@Test
public void test2(){
	String str = "1Hello2World3java4atguigu";
	str = str.replaceFirst("\\d", "");
	System.out.println(str);
	String[] all = str.split("\\d");
	for (int i = 0; i < all.length; i++) {
		System.out.println(all[i]);
	}
}
```


​	@Test
​	public void test1(){
​		String str = "Hello World java atguigu";
​		String[] all = str.split(" ");
​		for (int i = 0; i < all.length; i++) {
​			System.out.println(all[i]);
​		}
​	}

### 10.3.9  相关面试题

1、面试题：字符串的length和数组的length有什么不同？

字符串的length()，数组的length属性

2、字符串对象不可变

![1563201668749](1563201668749.png)

```java
class TEXT{
	public int num;
	public String str;
	
	public TEXT(int num, String str){
		this.num = num;
		this.str = str;
	}
}
public class Class4 {
    //tIn是传对象的地址，修改形参的属性，会影响实参
    //intIn是传数据，基本数据类型的形参修改和实参无关
    //Integer和String对象不可变
	public static void f1(TEXT tIn, int intIn, Integer integerIn, String strIn){
		tIn.num =200;
		tIn.str = "bcd";//形参和实参指向的是同一个TEXT的对象，修改了属性，就相当于修改实参对象的属性
		intIn = 200;//基本数据类型的形参是实参的“副本”，无论怎么修改和实参都没关系
		integerIn = 200;//Integer对象和String对象一样都是不可变，一旦修改都是新对象，和实参无关
		strIn = "bcd";
	}
	public static void main(String[] args) {
		TEXT tIn = new TEXT(100, "abc");//tIn.num = 100, tIn.str="abc"
		int intIn = 100;
		Integer integerIn = 100;
		String strIn = "abc";
		
		f1(tIn,intIn,integerIn,strIn);  //200，bcd，100，100，abc
		
		System.out.println(tIn.num + tIn.str + intIn + integerIn + strIn);
		//200 + bcd + 100 + 100 + abc
	}
}
```

### 10.3.10 练习

1、模拟一个trim方法，去除字符串两端的空格。

```java
	public static void main(String[] args) {
		String str ="    he   llo   ";
		System.out.println(myTrim(str));
		System.out.println(myTrim2(str));
		System.out.println(myTrim3(str));
	}
	
	public static String myTrim3(String str){
		//利用正则表达式
		//^表示开头    \s表示  空白符   *表示0次或多次     |表示或者    $表示结尾
		return str.replaceAll("(^\\s*)|(\\s*$)", "");
	}
	
	public static String myTrim2(String str){
		while(str.startsWith(" ")){
			str = str.replaceFirst(" ", "");
		}
		while(str.endsWith(" ")){
			str = str.substring(0, str.length()-1);
		}
		return str;
	}
	
	public static String myTrim(String str){
		char[] array = str.toCharArray();
		int start =0;
		for(int i=0;i<array.length;i++){
			if(array[i] == ' '){
				start++;
			}else{
				break;
			}
		}
		int end = array.length-1;
		for(int i=end;i>=0;i--){
			if(array[i] == ' '){
				end--;
			}else{
				break;
			}
		}
		
		String result = str.substring(start,end+1);
		
		return result;
	}
```



2、将一个字符串进行反转。将字符串中指定部分进行反转。比如将“abcdefgho”反转为”abfedcgho”

```java
	public static void main(String[] args) {
		String str ="abcdefgho";
		System.out.println(str);
		System.out.println(reverse(str,2,5));

	}
	
	//从第start个字符，到第end个字符
	public static String reverse(String str,int start,int end){
		char[] array = str.toCharArray();
		for(int i = start,j=end;i< j;i++,j--){
			char temp =array[i];
			array[i]=array[j];
			array[j]=temp;
		}
		String s = new String(array);
		return s;
	}
```



3、获取一个字符串在另一个字符串中出现的次数。

​     比如：获取"ab"在 “abababkkcadkabkebfkabkskab”中出现的次数

```java
	public static void main(String[] args) {
		String str1="ab";
		String str2="abababkkcadkabkebfkabkskab";
		System.out.println(count(str1,str2));
	}
	
	public static int count(String str1,String str2){
		int count =0;
		do{
			int index = str2.indexOf(str1);
			if(index !=-1){
				count++;
				str2 = str2.substring(index + str1.length());
			}else{
				break;
			}
			
		}while(true);
		return count;
	}
```



4、获取两个字符串中最大相同子串。比如：

   str1 = "abcwerthelloyuiodef“;str2 = "cvhellobnm"

   提示：将短的那个串进行长度依次递减的子串与较长的串比较。

```java
	public static void main(String[] args) {
		String str=findMaxSubString("abcwerthelloyuiodef","cvhellobnm");
		System.out.println(str);
	}
	
	//提示：将短的那个串进行长度依次递减的子串与较长的串比较。
	public static String findMaxSubString(String str1,String str2){
		String result="";
		
		String mixStr = str1.length()<str2.length()?str1:str2;
		String maxStr = str1.length()>str2.length()?str1:str2;
		
		//外循环控制从左到右的下标，内循环控制从右到左的下标
		for(int i=0;i<mixStr.length();i++){
			for(int j=mixStr.length();j>=i;j--){
				String str=mixStr.substring(i, j);
				if(maxStr.contains(str)){
					//找出最大相同子串
					if(result.length()<str.length()){
						result = str;
					}
				}
			}
		}
		return result;
	}
```



## 10.4 可变字符序列

因为String对象是不可变对象，虽然可以共享常量对象，但是对于频繁字符串的修改和拼接操作，效率极低。因此，JDK又在java.lang包提供了可变字符序列StringBuilder和StringBuffer类型。

StringBuffer：老的，线程安全的（因为它的方法有synchronized修饰）

StringBuilder：线程不安全的



常用的API，StringBuilder、StringBuffer的API是完全一致的

（1）append(xx)：拼接，追加

（2）insert(int index, xx)：插入

（3）delete(int start, int end)

deleteCharAt(int index)

（4）set(int index, xx)

（5）reverse()：反转

....  替换、截取、查找...

```java
	@Test
	public void test6(){
		StringBuilder s = new StringBuilder("helloworld");
		s.setLength(30);
		System.out.println(s);
	}
	@Test
	public void test5(){
		StringBuilder s = new StringBuilder("helloworld");
		s.setCharAt(2, 'a');
		System.out.println(s);
	}
	
	
	@Test
	public void test4(){
		StringBuilder s = new StringBuilder("helloworld");
		s.reverse();
		System.out.println(s);
	}
	
	@Test
	public void test3(){
		StringBuilder s = new StringBuilder("helloworld");
		s.delete(1, 3);
		s.deleteCharAt(4);
		System.out.println(s);
	}
	
	
	@Test
	public void test2(){
		StringBuilder s = new StringBuilder("helloworld");
		s.insert(5, "java");
		s.insert(5, "chailinyan");
		System.out.println(s);
	}
	
	@Test
	public void test1(){
		StringBuilder s = new StringBuilder();
		s.append("hello").append(true).append('a').append(12).append("atguigu");
		System.out.println(s);
		System.out.println(s.length());
	}
```

#### 效率测试

```java
/*
 * Runtime：JVM运行时环境
 * Runtime是一个单例的实现
 */
public class TestTime {
	public static void main(String[] args) {
//		testStringBuilder();
		testStringBuffer();
//		testString();
	}
	public static void testString(){
		long start = System.currentTimeMillis();
		String s = new String("0");
		for(int i=1;i<=10000;i++){
			s += i;
		}
		long end = System.currentTimeMillis();
		System.out.println("String拼接+用时："+(end-start));//444
		
		long memory = Runtime.getRuntime().totalMemory() - Runtime.getRuntime().freeMemory();
        System.out.println("String拼接+memory占用内存: " + memory);//53185144字节
	}
	public static void testStringBuilder(){
		long start = System.currentTimeMillis();
		StringBuilder s = new StringBuilder("0");
		for(int i=1;i<=10000;i++){
			s.append(i);
		}
		long end = System.currentTimeMillis();
		System.out.println("StringBuilder拼接+用时："+(end-start));//4
		long memory = Runtime.getRuntime().totalMemory() - Runtime.getRuntime().freeMemory();
        System.out.println("StringBuilder拼接+memory占用内存: " + memory);//1950488
	}
	public static void testStringBuffer(){
		long start = System.currentTimeMillis();
		StringBuffer s = new StringBuffer("0");
		for(int i=1;i<=10000;i++){
			s.append(i);
		}
		long end = System.currentTimeMillis();
		System.out.println("StringBuffer拼接+用时："+(end-start));//7
		long memory = Runtime.getRuntime().totalMemory() - Runtime.getRuntime().freeMemory();
        System.out.println("StringBuffer拼接+memory占用内存: " + memory);//1950488
	}
}
```





## 10.5 日期时间API

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

```java
	@Test
	public void test10() throws ParseException{
		String str = "2019年06月06日 16时03分14秒 545毫秒  星期四 +0800";
		SimpleDateFormat sf = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒 SSS毫秒  E Z");
		Date d = sf.parse(str);
		System.out.println(d);
	}
	
	@Test
	public void test9(){
		Date d = new Date();

		SimpleDateFormat sf = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒 SSS毫秒  E Z");
		//把Date日期转成字符串，按照指定的格式转
		String str = sf.format(d);
		System.out.println(str);
	}
	
	@Test
	public void test8(){
		String[] all = TimeZone.getAvailableIDs();
		for (int i = 0; i < all.length; i++) {
			System.out.println(all[i]);
		}
	}
	
	@Test
	public void test7(){
		TimeZone t = TimeZone.getTimeZone("America/Los_Angeles");
		
		//getInstance(TimeZone zone)
		Calendar c = Calendar.getInstance(t);
		System.out.println(c);
	}
	
	@Test
	public void test6(){
		Calendar c = Calendar.getInstance();
		System.out.println(c);
		
		int year = c.get(Calendar.YEAR);
		System.out.println(year);
		
		int month = c.get(Calendar.MONTH)+1;
		System.out.println(month);
		
		//...
	}
	
	@Test
	public void test5(){
		long time = Long.MAX_VALUE;
		Date d = new Date(time);
		System.out.println(d);
	}
	
	@Test
	public void test4(){
		long time = 1559807047979L;
		Date d = new Date(time);
		System.out.println(d);
	}
	@Test
	public void test3(){
		Date d = new Date();
		long time = d.getTime();
		System.out.println(time);//1559807047979
	}
	
	@Test
	public void test2(){
		long time = System.currentTimeMillis();
		System.out.println(time);//1559806982971
		//当前系统时间距离1970-1-1 0:0:0 0毫秒的时间差，毫秒为单位
	}
	
	@Test
	public void test1(){
		Date d = new Date();
		System.out.println(d);
	}
```



### 10.5.2 JDK1.8之后

java.time及其子包中。

1、LocalDate、LocalTime、LocalDateTime

（1）now()：获取系统日期或时间

（2）of(xxx)：或者指定的日期或时间

（3）运算：运算后得到新对象，需要重新接受

plusXxx()：在当前日期或时间对象上加xx

minusXxx() ：在当前日期或时间对象上减xx

 方法                                                          **描述**                                                     
 now() / now(ZoneId zone)                                      静态方法，根据当前时间创建对象/指定时区的对象                
 of()                                                          静态方法，根据指定日期/时间创建对象                          
 getDayOfMonth()/getDayOfYear()                                获得月份天数(1-31) /获得年份天数(1-366)                      
 getDayOfWeek()                                                获得星期几(返回一个 DayOfWeek 枚举值)                        
 getMonth()                                                   获得月份, 返回一个 Month 枚举值                              
 getMonthValue() / getYear()                                   获得月份(1-12) /获得年份                                     
 getHours()/getMinute()/getSecond()                            获得当前对象对应的小时、分钟、秒                             
 withDayOfMonth()/withDayOfYear()/withMonth()/withYear()      | 将月份天数、年份天数、月份、年份修改为指定的值并返回新的对象 |
 with(TemporalAdjuster  t)                                     将当前日期时间设置为校对器指定的日期时间                     
 plusDays(), plusWeeks(), plusMonths(), plusYears(),plusHours() | 向当前对象添加几天、几周、几个月、几年、几小时               |
 minusMonths() / minusWeeks()/minusDays()/minusYears()/minusHours() | 从当前对象减去几月、几周、几天、几年、几小时                 |
 plus(TemporalAmount t)/minus(TemporalAmount t)                添加或减少一个 Duration 或 Period                            
 isBefore()/isAfter()                                          比较两个 LocalDate                                           
 isLeapYear()                                                  判断是否是闰年（在LocalDate类中声明）                        
 format(DateTimeFormatter  t)                                  格式化本地日期、时间，返回一个字符串                         
 parse(Charsequence text)                                      将指定格式的字符串解析为日期、时间                           

2、DateTimeFormatter：日期时间格式化

该类提供了三种格式化方法：

预定义的标准格式。如：ISO_DATE_TIME;ISO_DATE

本地化相关的格式。如：ofLocalizedDate(FormatStyle.MEDIUM)

自定义的格式。如：ofPattern(“yyyy-MM-dd hh:mm:ss”)

```java
	@Test
	public void test10(){
		LocalDateTime now = LocalDateTime.now();
		
//		DateTimeFormatter df = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG);//2019年6月6日 下午04时40分03秒
		DateTimeFormatter df = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT);//19-6-6 下午4:40
		String str = df.format(now);
		System.out.println(str);
	}
	@Test
	public void test9(){
		LocalDateTime now = LocalDateTime.now();
		
		DateTimeFormatter df = DateTimeFormatter.ISO_DATE_TIME;//2019-06-06T16:38:23.756
		String str = df.format(now);
		System.out.println(str);
	}
	
	@Test
	public void test8(){
		LocalDateTime now = LocalDateTime.now();
		
		DateTimeFormatter df = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH时mm分ss秒  SSS毫秒  E 是这一年的D天");
		String str = df.format(now);
		System.out.println(str);
	}
	
	@Test
	public void test7(){
		LocalDate now = LocalDate.now();
		LocalDate before = now.minusDays(100);
		System.out.println(before);//2019-02-26
	}
	
	@Test
	public void test06(){
		LocalDate lai = LocalDate.of(2019, 5, 13);
		LocalDate go = lai.plusDays(160);
		System.out.println(go);//2019-10-20
	}
	
	@Test
	public void test05(){
		LocalDate lai = LocalDate.of(2019, 5, 13);
		System.out.println(lai.getDayOfYear());
	}
	
	
	@Test
	public void test04(){
		LocalDate lai = LocalDate.of(2019, 5, 13);
		System.out.println(lai);
	}
	
	@Test
	public void test03(){
		LocalDateTime now = LocalDateTime.now();
		System.out.println(now);
	}
	
	@Test
	public void test02(){
		LocalTime now = LocalTime.now();
		System.out.println(now);
	}
	
	@Test
	public void test01(){
		LocalDate now = LocalDate.now();
		System.out.println(now);
	}
```







