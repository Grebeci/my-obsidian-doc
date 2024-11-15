#  23【反射】

## 主要内容

* 类加载过程
* 类加载器
* 反射
* 简单工厂设计模式
* 工厂方法设计模式
* 静态代理设计模式
* 动态代理设计模式

## 教学目标

* [ ] 了解类的加载过程
* [ ] 理解类初始化过程
* [ ] 了解类加载器
* [ ] 掌握获取Class对象的四种方式
* [ ] 能够运用反射获取类型的详细信息
* [ ] 能够运用反射动态创建对象
* [ ] 能够运用反射动态获取成员变量并使用
* [ ] 能够运用反射动态获取成员方法并使用
* [ ] 能够运用反射读取注解
* [ ] 能够运用反射获取泛型父类的类型参数
* [ ] 了解简单工厂设计模式
* [ ] 了解工厂方法设计模式
* [ ] 了解静态代理设计模式
* [ ] 了解动态代理设计模式

# 第16章 反射（Reflect）

## 16.1 类加载

类在内存中的生命周期：加载-->使用-->卸载

### 16.1.1 类的加载过程

当程序主动使用某个类时，如果该类还未被加载到内存中，系统会通过加载、连接、初始化三个步骤来对该类进行初始化，如果没有意外，JVM将会连续完成这三个步骤，所以有时也把这三个步骤统称为类加载。

类的加载又分为三个阶段：

（1）加载：load

就是指将类型的clas字节码数据读入内存

（2）连接：link

①验证：校验合法性等

②准备：准备对应的内存（方法区），创建Class对象，为类变量赋默认值，为静态常量赋初始值。

③解析：把字节码中的符号引用替换为对应的直接地址引用

（3）初始化：initialize（类初始化）即执行<clinit>类初始化方法，大多数情况下，类的加载就完成了类的初始化，有些情况下，会延迟类的初始化。

![1560767438339](1560767438339.png)

### 16.1.2 类初始化

1、哪些操作会导致类的初始化？

（1）运行主方法所在的类，要先完成类初始化，再执行main方法

（2）第一次使用某个类型就是在new它的对象，此时这个类没有初始化的话，先完成类初始化再做实例初始化

（3）调用某个类的静态成员（类变量和类方法），此时这个类没有初始化的话，先完成类初始化

（4）子类初始化时，发现它的父类还没有初始化的话，那么先初始化父类

（5）通过反射操作某个类时，如果这个类没有初始化，也会导致该类先初始化

> 类初始化执行的是<clinit>()，该方法由（1）类变量的显式赋值代码（2）静态代码块中的代码构成

```java
class Father{
	static{
		System.out.println("main方法所在的类的父类(1)");//初始化子类时，会初始化父类
	}
}

public class TestClinit1 extends Father{
	static{
		System.out.println("main方法所在的类(2)");//主方法所在的类会初始化
	}
	
	public static void main(String[] args) throws ClassNotFoundException {
		new A();//第一次使用A就是创建它的对象，会初始化A类
		
		B.test();//直接使用B类的静态成员会初始化B类
		
		Class clazz = Class.forName("com.atguigu.test02.C");//通过反射操作C类，会初始化C类
	}
}
class A{
	static{
		System.out.println("A类初始化");
	}
}
class B{
	static{
		System.out.println("B类初始化");
	}
	public static void test(){
		System.out.println("B类的静态方法");
	}
}
class C{
	static{
		System.out.println("C类初始化");
	}
}
```

2、哪些使用类的操作，但是不会导致类的初始化？

（1）使用某个类的静态的常量（static  final）

（2）通过子类调用父类的静态变量，静态方法，只会导致父类初始化，不会导致子类初始化，即只有声明静态成员的类才会初始化

（3）用某个类型声明数组并创建数组对象时，不会导致这个类初始化

```java
public class TestClinit2 {
	public static void main(String[] args) {
		System.out.println(D.NUM);//D类不会初始化，因为NUM是final的
		
		System.out.println(F.num);
		F.test();//F类不会初始化，E类会初始化，因为num和test()是在E类中声明的
		
		//G类不会初始化，此时还没有正式用的G类
		G[] arr = new G[5];//没有创建G的对象，创建的是准备用来装G对象的数组对象
        //G[]是一种新的类型，是数组类想，动态编译生成的一种新的类型
        //G[].class
	}
}
class D{
	public static final int NUM = 10;
	static{
		System.out.println("D类的初始化");
	}
}
class E{
	static int num = 10;
	static{
		System.out.println("E父类的初始化");
	}
	public static void test(){
		System.out.println("父类的静态方法");
	}
}
class F extends E{
	static{
		System.out.println("F子类的初始化");
	}
}

class G{
	static{
		System.out.println("G类的初始化");
	}
}
```

### 16.1.3 类加载器

很多开发人员都遇到过java.lang.ClassNotFoundException或java.lang.NoClassDefError，想要更好的解决这类问题，或者在一些特殊的应用场景，比如需要支持类的动态加载或需要对编译后的字节码文件进行加密解密操作，那么需要你自定义类加载器，因此了解类加载器及其类加载机制也就成了每一个Java开发人员的必备技能之一。

**1、类加载器分为：**

（1）引导类加载器（Bootstrap Classloader）又称为根类加载器

	它负责加载jre/rt.jar核心库
	它本身不是Java代码实现的，也不是ClassLoader的子类，获取它的对象时往往返回null

（2）扩展类加载器（Extension ClassLoader）

	它负责加载jre/lib/ext扩展库
	它是ClassLoader的子类

（3）应用程序类加载器（Application Classloader）

	它负责加载项目的classpath路径下的类
	
	它是ClassLoader的子类

（4）自定义类加载器

	当你的程序需要加载“特定”目录下的类，可以自定义类加载器；
	当你的程序的字节码文件需要加密时，那么往往会提供一个自定义类加载器对其进行解码
	后面会见到的自定义类加载器：tomcat中

**2、Java系统类加载器的双亲委托模式**

简单描述：

	下一级的类加载器，如果接到任务时，会先搜索是否加载过，如果没有，会先把任务往上传，如果都没有加载过，一直到根加载器，如果根加载器在它负责的路径下没有找到，会往回传，如果一路回传到最后一级都没有找到，那么会报ClassNotFoundException或NoClassDefError，如果在某一级找到了，就直接返回Class对象。

应用程序类加载器  把  扩展类加载器视为父加载器，

扩展类加载器 把 引导类加载器视为父加载器。

不是继承关系，是组合的方式实现的。



## 16.2  javalang.Class类

Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为Java语言的反射机制。

要想解剖一个类，必须先要获取到该类的Class对象。而剖析一个类或用反射解决具体的问题就是使用相关API（1）java.lang.Class（2）java.lang.reflect.*。所以，Class对象是反射的根源。

### 1、哪些类型可以获取Class对象

所有Java类型

用代码示例

```java
//（1）基本数据类型和void
例如：int.class
	 void.class
//（2）类和接口
例如：String.class
	Comparable.class
//（3）枚举
例如：ElementType.class
//（4）注解
例如：Override.class
//（5）数组
例如：int[].class
```

### 2、获取Class对象的四种方式

（1）类型名.class

要求编译期间已知类型

（2）对象.getClass()

获取对象的运行时类型

（3）Class.forName(类型全名称)

 可以获取编译期间未知的类型

（4）ClassLoader的类加载器对象.loadClass(类型全名称)

可以用系统类加载对象或自定义加载器对象加载指定路径下的类型

```java
public class TestClass {
	@Test
	public void test05() throws ClassNotFoundException{
		Class c = TestClass.class;
		ClassLoader loader = c.getClassLoader();
		
		Class c2 = loader.loadClass("com.atguigu.test05.Employee");
		Class c3 = Employee.class;
		System.out.println(c2 == c3);
	}
	
	@Test
	public void test03() throws ClassNotFoundException{
		Class c2 = String.class;
		Class c1 = "".getClass();
		Class c3 = Class.forName("java.lang.String");
		
		System.out.println(c1 == c2);
		System.out.println(c1 == c3);
	}
}
```

### 3、查看某个类的类加载器对象

```java
//获取应用程序类加载器对象


//获取扩展类加载器对象


//获取根加载器对象
```



## 16.3 反射的应用

### 16.3.1 获取类型的详细信息

可以获取：包、修饰符、类型名、父类（包括泛型父类）、父接口（包括泛型父接口）、成员（属性、构造器、方法）、注解（类上的、方法上的、属性上的）

示例代码获取常规信息：

```java
public class TestClassInfo {
	public static void main(String[] args) throws NoSuchFieldException, SecurityException {
		//1、先得到某个类型的Class对象
		Class clazz = String.class;
		//比喻clazz好比是镜子中的影子
		
		//2、获取类信息
		//（1）获取包对象，即所有java的包，都是Package的对象
		Package pkg = clazz.getPackage();
		System.out.println("包名：" + pkg.getName());
		
		//（2）获取修饰符
		//其实修饰符是Modifier，里面有很多常量值
		/*
		 * 0x是十六进制
		 * PUBLIC           = 0x00000001;  1    1
		 * PRIVATE          = 0x00000002;  2	10
		 * PROTECTED        = 0x00000004;  4	100
		 * STATIC           = 0x00000008;  8	1000
		 * FINAL            = 0x00000010;  16	10000
		 * ...
		 * 
		 * 设计的理念，就是用二进制的某一位是1，来代表一种修饰符，整个二进制中只有一位是1，其余都是0
		 * 
		 * mod = 17          0x00000011
		 * if ((mod & PUBLIC) != 0)  说明修饰符中有public
		 * if ((mod & FINAL) != 0)   说明修饰符中有final
		 */
		int mod = clazz.getModifiers();
		System.out.println(Modifier.toString(mod));
		
		//（3）类型名
		String name = clazz.getName();
		System.out.println(name);
		
		//（4）父类，父类也有父类对应的Class对象
		Class superclass = clazz.getSuperclass();
		System.out.println(superclass);
		
		//（5）父接口们
		Class[] interfaces = clazz.getInterfaces();
		for (Class class1 : interfaces) {
			System.out.println(class1);
		}
		
		//（6）类的属性，  你声明的一个属性，它是Field的对象
/*		Field clazz.getField(name)  根据属性名获取一个属性对象，但是只能得到公共的
		Field[] clazz.getFields();  获取所有公共的属性
		Field clazz.getDeclaredField(name)  根据属性名获取一个属性对象，可以获取已声明的
		Field[] clazz.getDeclaredFields()	获取所有已声明的属性
		*/
		Field valueField = clazz.getDeclaredField("value");
//		System.out.println("valueField = " +valueField);
		
		Field[] declaredFields = clazz.getDeclaredFields();
		for (Field field : declaredFields) {
			//修饰符、数据类型、属性名    
			int modifiers = field.getModifiers();
			System.out.println("属性的修饰符：" + Modifier.toString(modifiers));
			
			String name2 = field.getName();
			System.out.println("属性名：" + name2);
			
			Class<?> type = field.getType();
			System.out.println("属性的数据类型：" + type);
		}
		System.out.println("-------------------------");
		//（7）构造器们
		Constructor[] constructors = clazz.getDeclaredConstructors();
		for (Constructor constructor : constructors) {
			//修饰符、构造器名称、构造器形参列表  、抛出异常列表
			int modifiers = constructor.getModifiers();
			System.out.println("构造器的修饰符：" + Modifier.toString(modifiers));
			
			String name2 = constructor.getName();
			System.out.println("构造器名：" + name2);
			
			//形参列表
			System.out.println("形参列表：");
			Class[] parameterTypes = constructor.getParameterTypes();
			for (Class parameterType : parameterTypes) {
				System.out.println(parameterType);
			}
            
            //异常列表
			System.out.println("异常列表：");
			Class<?>[] exceptionTypes = constructor.getExceptionTypes();
			for (Class<?> exceptionType : exceptionTypes) {
				System.out.println(exceptionType);
			}
		}
		System.out.println("=--------------------------------");
		//(8)方法们
		Method[] declaredMethods = clazz.getDeclaredMethods();
		for (Method method : declaredMethods) {
			//修饰符、返回值类型、方法名、形参列表 、异常列表 
			int modifiers = method.getModifiers();
			System.out.println("方法的修饰符：" + Modifier.toString(modifiers));
			
			Class<?> returnType = method.getReturnType();
			System.out.println("返回值类型:" + returnType);
			
			String name2 = method.getName();
			System.out.println("方法名：" + name2);
			
			//形参列表
			System.out.println("形参列表：");
			Class[] parameterTypes = method.getParameterTypes();
			for (Class parameterType : parameterTypes) {
				System.out.println(parameterType);
			}
			
			//异常列表
			System.out.println("异常列表：");
			Class<?>[] exceptionTypes = method.getExceptionTypes();
			for (Class<?> exceptionType : exceptionTypes) {
				System.out.println(exceptionType);
			}
		}
		
	}
}
```



### 16.3.2  创建任意引用类型的对象

两种方式：

1、直接通过Class对象来实例化（要求必须有无参构造）

2、通过获取构造器对象来进行实例化



方式一的步骤：

（1）获取该类型的Class对象（2）创建对象

```java
	@Test
	public void test2()throws Exception{
		Class<?> clazz = Class.forName("com.atguigu.test.Student");
		//Caused by: java.lang.NoSuchMethodException: com.atguigu.test.Student.<init>()
		//即说明Student没有无参构造，就没有无参实例初始化方法<init>
		Object stu = clazz.newInstance();
		System.out.println(stu);
	}
	
	@Test
	public void test1() throws ClassNotFoundException, InstantiationException, IllegalAccessException{
//		AtGuigu obj = new AtGuigu();//编译期间无法创建
		
		Class<?> clazz = Class.forName("com.atguigu.test.AtGuigu");
		//clazz代表com.atguigu.test.AtGuigu类型
		//clazz.newInstance()创建的就是AtGuigu的对象
		Object obj = clazz.newInstance();
		System.out.println(obj);
	}
```

方式二的步骤：

（1）获取该类型的Class对象（2）获取构造器对象（3）创建对象

> 如果构造器的权限修饰符修饰的范围不可见，也可以调用setAccessible(true)

示例代码：

```java
public class TestNewInstance {
	@Test
	public void test3()throws Exception{
		//(1)获取Class对象
		Class<?> clazz = Class.forName("com.atguigu.test.Student");
		/*
		 * 获取Student类型中的有参构造
		 * 如果构造器有多个，我们通常是根据形参【类型】列表来获取指定的一个构造器的
		 * 例如：public Student(int id, String name) 
		 */
		//(2)获取构造器对象
		Constructor<?> constructor = clazz.getDeclaredConstructor(int.class,String.class);
		
		//(3)创建实例对象
		// T newInstance(Object... initargs)  这个Object...是在创建对象时，给有参构造的实参列表
		Object obj = constructor.newInstance(2,"张三");
		System.out.println(obj);
	}
	
}
```

### 16.3.3 操作任意类型的属性

（1）获取该类型的Class对象
Class clazz = Class.forName("com.atguigu.bean.User");

（2）获取属性对象
Field field = clazz.getDeclaredField("username");

（3）设置属性可访问

field.setAccessible(true);

（4）创建实例对象：如果操作的是非静态属性，需要创建实例对象
Object obj = clazz.newInstance();

（4）设置属性值

field.set(obj,"chai");
（5）获取属性值
Object value = field.get(obj);

> 如果操作静态变量，那么实例对象可以省略，用null表示

示例代码：

```java
public class TestField {
	public static void main(String[] args)throws Exception {
		//1、获取Student的Class对象
		Class clazz = Class.forName("com.atguigu.test.Student");
		
		//2、获取属性对象，例如：id属性
		Field idField = clazz.getDeclaredField("id");
        
        //3、如果id是私有的等在当前类中不可访问access的，我们需要做如下操作
		idField.setAccessible(true);
		
		//4、创建实例对象，即，创建Student对象
		Object stu = clazz.newInstance();
				
		//5、获取属性值
		/*
		 * 以前：int 变量= 学生对象.getId()
		 * 现在：Object id属性对象.get(学生对象)
		 */
		Object value = idField.get(stu);
		System.out.println("id = "+ value);
		
		//6、设置属性值
		/*
		 * 以前：学生对象.setId(值)
		 * 现在：id属性对象.set(学生对象,值)
		 */
		idField.set(stu, 2);
		
		value = idField.get(stu);
		System.out.println("id = "+ value);
	}
}
```

### 16.3.4 调用任意类型的方法

（1）获取该类型的Class对象
Class clazz = Class.forName("com.atguigu.service.UserService");
（2）获取方法对象
Method method = clazz.getDeclaredMethod("login",String.class,String.class);
（3）创建实例对象
Object obj = clazz.newInstance();
（4）调用方法
Object result = method.invoke(obj,"chai","123);

> 如果方法的权限修饰符修饰的范围不可见，也可以调用setAccessible(true)
>
> 如果方法是静态方法，实例对象也可以省略，用null代替

示例代码：

```java
public class TestMethod {
	@Test
	public void test()throws Exception {
		// 1、获取Student的Class对象
		Class<?> clazz = Class.forName("com.atguigu.test.Student");
		
		//2、获取方法对象
		/*
		 * 在一个类中，唯一定位到一个方法，需要：（1）方法名（2）形参列表，因为方法可能重载
		 * 
		 * 例如：void setName(String name)
		 */
		Method method = clazz.getDeclaredMethod("setName", String.class);
		
		//3、创建实例对象
		Object stu = clazz.newInstance();
		
		//4、调用方法
		/*
		 * 以前：学生对象.setName(值)
		 * 现在：方法对象.invoke(学生对象，值)
		 */
		method.invoke(stu, "张三");
		
		System.out.println(stu);
	}
}
```

