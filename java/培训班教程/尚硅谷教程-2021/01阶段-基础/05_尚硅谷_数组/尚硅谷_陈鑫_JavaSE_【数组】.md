# JavaSE_05【数组】

## 今日内容

- 数组概念
- 数组的定义和初始化
- 数组的索引
- 数组的长度
- 数组的遍历
- 数组内存
- 数组的相关算法

## 学习目标

- [ ] 理解容器的概念
- [ ] 掌握数组的第一种定义方式
- [ ] 掌握数组的第二种定义方式
- [ ] 掌握数组的第三种定义方式
- [ ] 使用索引访问数组的元素
- [ ] 了解数组的内存图解
- [ ] 了解空指针和越界异常
- [ ] 掌握数组的遍历
- [ ] 掌握数组最大值的获取
- [ ] 掌握数组元素的统计
- [ ] 了解数组的复制
- [ ] 了解数组的反转

# 项目一：家庭记账软件系统

```java
class FamilyAccount{
	public static void main(String[] args){
		boolean flag = true;
		
		int balance = 10000;//基本金
		String details = "收支\t账户金额\t收支金额\t说    明\n";

		while(flag){
			System.out.println("-----------------家庭收支记账软件-----------------");

			System.out.println("\t\t1 收支明细");
			System.out.println("\t\t2 登记收入");
			System.out.println("\t\t3 登记支出");
			System.out.println("\t\t4 退    出");

			System.out.print("请选择(1-4)：");
			//类似于：double d = Math.random()
			//类似于：double d = Math.sqrt(x)
			char select = Utility.readMenuSelection();//接收用户的选择
			
			//判断用户的选择，进行对应的操作
			switch(select){
				case '1':
					System.out.println(details);
					break;
				case '2':
					System.out.print("本次收入金额：");
					int money = Utility.readNumber();
					
					System.out.print("本次收入说明：");
					String info = Utility.readString();
					
					balance += money;
					//收入    1000           11000            劳务费
					details += "收入\t" + money + "\t\t" + balance + "\t\t" + info + "\n";
					break;
				case '3':
					System.out.print("本次支出金额：");
					money = Utility.readNumber();
					
					System.out.print("本次支出说明：");
					info = Utility.readString();
					
					balance -= money;
					//支出    800            10200            物业费
					details += "支出\t" + money + "\t\t" + balance + "\t\t" + info + "\n";
					break;
				case '4':
					System.out.print("确认是否退出(Y/N)：");
					char confirm = Utility.readConfirmSelection();
					if(confirm == 'Y' || confirm =='y'){
						flag = false;
					}
			}
		}
	}
}
```

# 第四章 数组

## 4.1 容器概述

### 案例分析

现在需要统计某公司员工的工资情况，例如计算平均工资、找到最高工资等。假设该公司有50名员工，用前面所学的知识，程序首先需要声明50个变量来分别记住每位员工的工资，然后在进行操作，这样做会显得很麻烦，而且错误率也会很高。因此我们可以使用容器进行操作。将所有的数据全部存储到一个容器中，统一操作。

### 容器概念

- **容器：**是将多个数据存储到一起，每个数据称为该容器的元素。
- **生活中的容器：**水杯，衣柜，教室

## 4.2 数组的概念

- **数组概念：** 数组就是用于存储数据的长度固定的容器，保证多个数据的数据类型要一致。

百度百科中对数组的定义：

所谓**数组**(array)，就是相同数据类型的元素按一定顺序排列的集合，就是把有限个类型相同的变量用一个名字命名，以便统一管理他们，然后用编号区分他们，这个名字称为**数组名**，编号称为**下标或索引**(index)。组成数组的各个变量称为数组的**元素**(element)。数组中元素的个数称为**数组的长度**(length)。

![1561452334825](1561452334825.png)

数组的特点：

1、数组的长度一旦确定就不能修改

2、创建数组时会在内存中开辟一整块连续的空间。

3、存取元素的速度快，因为可以通过[下标]，直接定位到任意一个元素。

## 4.3 数组的定义

### 方式一：静态初始化

* **格式：**

```java
数据类型[] 数组名 = {元素1,元素2,元素3...};//必须在一个语句中完成，不能分开两个语句写
```

* 举例：

定义存储1，2，3，4，5整数的数组容器

```java
int[] arr = {1,2,3,4,5};//正确

int[] arr;
arr = {1,2,3,4,5};//错误
```

### 方式二：静态初始化

* **格式：**

```java
数据类型[] 数组名 = new 数据类型[]{元素1,元素2,元素3...};
或
数据类型[] 数组名;
数组名 = new 数据类型[]{元素1,元素2,元素3...};
```

* 举例：

定义存储1，2，3，4，5整数的数组容器。

```java
int[] arr = new int[]{1,2,3,4,5};//正确

int[] arr;
arr = new int[]{1,2,3,4,5};//正确

int[] arr = new int[5]{1,2,3,4,5};//错误的，后面有{}指定元素列表，就不需要在[长度]指定长度。
```

### 方式三：动态初始化

- **格式：**

```java
 数组存储的元素的数据类型[] 数组名字 = new 数组存储的元素的数据类型[长度];
 
  或

 数组存储的数据类型[] 数组名字;
 数组名字 = new 数组存储的数据类型[长度];
```

- 数组定义格式详解：
  - 数组存储的元素的数据类型： 创建的数组容器可以存储什么数据类型的数据。
  - 元素的类型可以是任意的Java的数据类型。例如：int, String, Student等
  - [] : 表示数组。
  - 数组名字：为定义的数组起个变量名，满足标识符规范，可以使用名字操作数组。
  - new：关键字，创建数组使用的关键字。因为数组本身是引用数据类型，所以要用new创建数组对象。
  - [长度]：数组的长度，表示数组容器中可以存储多少个元素。
  - **注意：数组有定长特性，长度一旦指定，不可更改。**
    - 和水杯道理相同，买了一个2升的水杯，总容量就是2升，不能多也不能少。
- 举例：

定义可以存储5个整数的数组容器，代码如下：

```java
int[] arr = new int[5];

int[] arr;
arr = new int[5];
```

> 思考：用这种方式初始化的数组，元素有值吗？

## 4.4 数组元素的访问

- **索引：** 每一个存储到数组的元素，都会自动的拥有一个编号，从0开始，这个自动编号称为**数组索引(index)**，可以通过数组的索引访问到数组中的元素。
- **索引范围：**[0, 数组的长度-1]
- **格式：**

```java
数组名[索引]
```

- **索引访问数组中的元素：**
  - 数组名[索引]=数值，为数组中的元素赋值
  - 变量=数组名[索引]，获取出数组中的元素

```java
public static void main(String[] args) {
    //定义存储int类型数组，赋值元素1，2，3，4，5
    int[] arr = {1,2,3,4,5};
    //为0索引元素赋值为6
    arr[0] = 6;
    //获取数组0索引上的元素
    int i = arr[0];
    System.out.println(i);
    //直接输出数组0索引元素
    System.out.println(arr[0]);
}
```

## 4.5 数组的遍历

* **数组的长度属性：** 每个数组都具有长度，而且是固定的，Java中赋予了数组的一个属性，可以获取到数组的长度，语句为：`数组名.length` ，属性length的执行结果是数组的长度，int类型结果。由次可以推断出，数组的最大索引值为`数组名.length-1`。
* **数组遍历：** 就是将数组中的每个元素分别获取出来，就是遍历。遍历也是数组操作中的基石。

```java
public static void main(String[] args) {
  	int[] arr = new int[]{1,2,3,4,5};
  	//打印数组的属性，输出结果是5
  	System.out.println("数组的长度：" + arr.length);
    
    //遍历输出数组中的元素
    System.out.println("数组的元素有：");
    for(int i=0; i<arr.length; i++){
        System.out.println(arr[i]);
    }
}
```

## 4.6 数组元素的默认值

当我们使用动态初始化创建数组时：

```java
 数组存储的元素的数据类型[] 数组名字 = new 数组存储的元素的数据类型[长度];
```

此时只确定了数组的长度，那么数组的元素是什么值呢？

数组的元素有默认值：

![1561509460135](1561509460135.png)

## 4.7 数组内存图

### 4.7.1 内存概述

内存是计算机中重要的部件之一，它是与CPU进行沟通的桥梁。其作用是用于暂时存放CPU中的运算数据，以及与硬盘等外部存储器交换的数据。只要计算机在运行中，CPU就会把需要运算的数据调到内存中进行运算，当运算完成后CPU再将结果传送出来。我们编写的程序是存放在硬盘中的，在硬盘中的程序是不会运行的，必须放进内存中才能运行，运行完毕后会清空内存。

Java虚拟机要运行程序，必须要对内存进行空间的分配和管理。

### 4.7.2 Java虚拟机的内存划分

为了提高运算效率，就对空间进行了不同区域的划分，因为每一片区域都有特定的处理数据方式和内存管理方式。

![1561465258546](1561465258546.png)

| 区域名称   | 作用                                                      |
| ----------| ---------------------------------------------------------|
| 程序计数器 | 程序计数器是CPU中的寄存器，它包含每一个线程下一条要执行的指令的地址 |
| 本地方法栈 | 当程序中调用了native的本地方法时，本地方法执行期间的内存区域 |
| 方法区     | 存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。 |
| 堆内存     | 存储对象（包括数组对象），new来创建的，都存储在堆内存。      |
| 虚拟机栈   | 用于存储正在执行的每个Java方法的局部变量表等。局部变量表存放了编译期可知长度的各种基本数据类型、对象引用，方法执行完，自动释放。 |

### 4.7.3 数组在内存中的存储

#### 1、一个数组内存图

```java
public static void main(String[] args) {
  	int[] arr = new int[3];
  	System.out.println(arr);//[I@5f150435
}

```

![](数组内存图1.jpg)

> 思考：打印arr为什么是[I@5f150435，它是数组的地址吗？
>
> 答：它不是数组的地址。
>
> 问？不是说arr中存储的是数组对象的首地址吗？
>
> 答：arr中存储的是数组的首地址，但是因为数组是引用数据类型，打印arr时，会自动调用arr数组对象的toString()方法，默认该方法实现的是对象类型名@该对象的hashCode()值的十六进制值。
>
> 问？对象的hashCode值是否就是对象内存地址？
>
> 答：不一定，因为这个和不同品牌的JVM产品的具体实现有关。例如：Oracle的OpenJDK中给出了5种实现，其中有一种是直接返回对象的内存地址，但是OpenJDK默认没有选择这种方式。

#### 2、数组下标为什么是0开始

因为第一个元素距离数组首地址间隔0个单元。

#### 3、两个数组内存图

```java
public static void main(String[] args) {
    int[] arr = new int[3];
    int[] arr2 = new int[2];
    System.out.println(arr);
    System.out.println(arr2);
}

```

![](数组内存图2.jpg)

#### 4、两个变量指向一个数组

```java
public static void main(String[] args) {
    // 定义数组，存储3个元素
    int[] arr = new int[3];
    //数组索引进行赋值
    arr[0] = 5;
    arr[1] = 6;
    arr[2] = 7;
    //输出3个索引上的元素值
    System.out.println(arr[0]);
    System.out.println(arr[1]);
    System.out.println(arr[2]);
    //定义数组变量arr2，将arr的地址赋值给arr2
    int[] arr2 = arr;
    arr2[1] = 9;
    System.out.println(arr[1]);
}

```

 ![](数组内存图3.jpg)

## 4.8 数组使用过程中常见异常

### 4.8.1 数组越界异常

观察一下代码，运行后会出现什么结果。

```java
public static void main(String[] args) {
    int[] arr = {1,2,3};
    System.out.println(arr[3]);
}

```

创建数组，赋值3个元素，数组的索引就是0，1，2，没有3索引，因此我们不能访问数组中不存在的索引，程序运行后，将会抛出 `ArrayIndexOutOfBoundsException`  数组越界异常。在开发中，数组的越界异常是**不能出现**的，一旦出现了，就必须要修改我们编写的代码。

![](数组越界异常.jpg)

### 4.8.2 数组空指针异常

观察一下代码，运行后会出现什么结果。

```java
public static void main(String[] args) {
    int[] arr = {1,2,3};
    arr = null;
    System.out.println(arr[0]);
｝
```

`arr = null`这行代码，意味着变量arr将不会在保存数组的内存地址，也就不允许再操作数组了，因此运行的时候会抛出`NullPointerException` 空指针异常。在开发中，数组的越界异常是**不能出现**的，一旦出现了，就必须要修改我们编写的代码。

![](空指针异常.jpg)

**空指针异常在内存图中的表现**

![](空指针异常内存图.jpg)

观察如下代码，运行后会出现什么结果：

```java
	public static void main(String[] args){
		//声明一个String[]类型的数组，用来存储三个学生的姓名
		String[] names = new String[3];
		int count = names[0].length();
		System.out.println("第一个学生姓名的字数：" + count);
	}
```

names[0]中目前是默认值null，再用它调用方法，来求字符串的长度，会报空指针异常。

![1561467707960](1561467707960.png)

![](数组内存图4.png)

## 4.9 数组的常见操作

### 4.9.1 数组找最值

思路：

（1）先假设第一个元素最大/最小

（2）然后用max/min与后面的元素一一比较

* **实现思路：**
  * 定义变量，保存数组0索引上的元素
  * 遍历数组，获取出数组中的每个元素
  * 将遍历到的元素和保存数组0索引上值的变量进行比较
  * 如果数组元素的值大于了变量的值，变量记录住新的值
  * 数组循环遍历结束，变量保存的就是数组中的最大值

![1561468948876](1561468948876.png)

示例代码：

```java
int[] arr = {4,5,6,1,9};
//找最大值
int max = arr[0];
for(int i=1; i<arr.length; i++){
    if(arr[i] > max){
        max = arr[i];
    }
}
```

### 4.9.2 数组中找最值及其下标

情况一：找最值及其第一次出现的下标

思路：

（1）先假设第一个元素最大/最小

（2）用max/min变量表示最大/小值，用max/min与后面的元素一一比较

（3）用index时刻记录目前比对的最大/小的下标

示例代码：

```java
int[] arr = {4,5,6,1,9};
//找最大值
int max = arr[0];
int index = 0;
for(int i=1; i<arr.length; i++){
    if(arr[i] > max){
        max = arr[i];
        index = i;
    }
}
```

或

思路：

（1）先假设第一个元素最大/最小

（2）用maxIndex时刻记录目前比对的最大/小的下标，那么arr[maxIndex]就是目前的最大值

```java
int[] arr = {4,5,6,1,9};
//找最大值
int maxIndex = 0;
for(int i=1; i<arr.length; i++){
    if(arr[i] > arr[maxIndex]){
        maxIndex = i;
    }
}
System.out.println("最大值：" + arr[maxIndex]);
```

情况二：找最值及其所有最值的下标（即可能最大值重复）

思路：

（1）先找最大值

①假设第一个元素最大

②用max与后面的元素一一比较

（2）遍历数组，看哪些元素和最大值是一样的

示例代码：

```java
int[] arr = {4,5,6,1,9};
//找最大值
int max = arr[0];
for(int i=1; i<arr.length; i++){
    if(arr[i] > max){
        max = arr[i];
    }
}

//遍历数组，看哪些元素和最大值是一样的
for(int i=0; i<arr.length; i++){
    if(max == arr[i]){
        System.out.print(i+"\t");
    }
}
```



### 4.9.3 数组统计：求总和、均值、统计偶数个数等

思路：遍历数组，挨个的累加，判断每一个元素

示例代码：

```java
int[] arr = {4,5,6,1,9};
//求总和、均值
int sum = 0;//因为0加上任何数都不影响结果
for(int i=0; i<arr.length; i++){
    sum += arr[i];
}
double avg = (double)sum/arr.length;
```

示例代码2：

```java
int[] arr = {4,5,6,1,9};

//求总乘积
long result = 1;//因为1乘以任何数都不影响结果
for(int i=0; i<arr.length; i++){
    result *= arr[i];
}
```

示例代码3：

```java
int[] arr = {4,5,6,1,9};
//统计偶数个数
int even = 0;
for(int i=0; i<arr.length; i++){
    if(arr[i]%2==0){
        even++;
    }
}
```

### 4.9.4 复制

应用场景：

1、扩容

2、备份

3、截取

4、反转

示例代码：扩容，新增一个位置来存储10

```java
int[] arr = {1,2,3,4,5,6,7,8,9};

//如果要把arr数组扩容，增加1个位置
//(1)先创建一个新数组，它的长度 = 旧数组的长度+1
int[] newArr = new int[arr.length + 1];

//(2)复制元素
//注意：i<arr.length   因位arr比newArr短，避免下标越界
for(int i=0; i<arr.length; i++){
    newArr[i] = arr[i];
}

//(3)把新元素添加到newArr的最后
newArr[newArr.length-1] = 10;

//(4)如果下面继续使用arr，可以让arr指向新数组
arr = newArr;

//(4)遍历显示
for(int i=0; i<arr.length; i++){
    System.out.println(arr[i]);
}
```

示例代码：备份

```java
int[] arr = {1,2,3,4,5,6,7,8,9};

//1、创建一个长度和原来的数组一样的新数组
int[] newArr = new int[arr.length];

//2、复制元素
for(int i=0; i<arr.length; i++){
    newArr[i] = arr[i];
}

//3、遍历显示
for(int i=0; i<arr.length; i++){
    System.out.println(arr[i]);
}
```

示例代码：截取[start,end)范围

```java
int[] arr = {11,22,33,44,55,66,77,88,99};

int start = 2;
int end = 6;

//1、创建一个新数组，新数组的长度 = end-start ;
int[] newArr = new int[end-start];

//2、赋值元素
for(int i=0; i<newArr.length; i++){
    newArr[i] = arr[start + i];
}

//3、遍历显示
for(int i=0; i<newArr.length; i++){
    System.out.println(newArr[i]);
}
```

### 4.9.5 反转

方法有两种：

1、借助一个新数组

2、首尾对应位置交换

第一种方式示例代码：

```java
int[] arr = {1,2,3,4,5,6,7,8,9};

//(1)先创建一个新数组
int[] newArr = new int[arr.length];

//(2)复制元素
int len = arr.length;
for(int i=0; i<newArr.length; i++){
    newArr[i] = arr[len -1 - i];
}

//(3)舍弃旧的，让arr指向新数组
arr = newArr;//这里把新数组的首地址赋值给了arr

//(4)遍历显示
for(int i=0; i<arr.length; i++){
    System.out.println(arr[i]);
}

```

第二种方式示例代码：

**实现思想：**数组对称位置的元素互换。

![1561469467316](1561469467316.png)

```java
int[] arr = {1,2,3,4,5,6,7,8,9};

//(1)计算要交换的次数：  次数 = arr.length/2
//(2)首尾对称位置交换
for(int i=0; i<arr.length/2; i++){//循环的次数就是交换的次数
    int temp = arr[i];
    arr[i] = arr[arr.length-1-i];
	arr[arr.length-1-i] = temp;
}

//（3）遍历显示
for(int i=0; i<arr.length; i++){
    System.out.println(arr[i]);
}
```

* 实现反转，就需要将数组对称位置的元素进行交换
* 定义两个变量，保存数组的最小索引和最大索引
* 两个索引上的元素交换位置
* 最小索引++，最大索引--
* 最小索引超过了最大索引，数组反转操作结束

![1561469087319](1561469087319.png)

```java
	public static void main(String[] args){
		int[] arr = {1,2,3,4,5,6,7,8,9};

		//首尾交换
		for(int min=0,max=arr.length-1; min<max; min++,max--){
		    //首  与  尾交换
		    int temp = arr[min];
		    arr[min] = arr[max];
			arr[max] = temp;
		}

		//（3）遍历显示
		for(int i=0; i<arr.length; i++){
		    System.out.println(arr[i]);
		}
	}
```

