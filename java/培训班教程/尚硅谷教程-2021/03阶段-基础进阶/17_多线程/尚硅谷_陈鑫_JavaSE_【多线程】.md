# 17【多线程】

## 主要内容

- 多线程

## 学习目标

- [ ] 说出进程的概念
- [ ] 说出线程的概念
- [ ] 能够理解并发与并行的区别
- [ ] 能够开启新线程
- [ ] 能够描述Java中多线程运行原理
- [ ] 能够使用继承类的方式创建多线程
- [ ] 能够使用实现接口的方式创建多线程
- [ ] 能够说出实现接口方式的好处
- [ ] 能够解释安全问题的出现的原因
- [ ] 能够使用同步代码块解决线程安全问题
- [ ] 能够使用同步方法解决线程安全问题
- [ ] 能够说出线程6个状态的名称
- [ ] 能够理解线程通信概念
- [ ] 能够理解等待唤醒机制
- [ ] 能够编写单例设计模式

#  第十一章 多线程

我们在之前，学习的程序在没有跳转语句的前提下，都是由上至下依次执行，那现在想要设计一个程序，边打游戏边听歌，怎么设计？

要解决上述问题,咱们得使用多进程或者多线程来解决.

## 11.1 相关概念

### 11.1.1 并发与并行（了解）

* **并行**（parallel）：指两个或多个事件在**同一时刻**发生（同时发生）。指在同一时刻，有多条指令在多个处理器上同时执行。
* **并发**（concurrency）：指两个或多个事件在**同一个时间段内**发生。指在同一个时刻只能有一条指令执行，但多个进程的指令被快速轮换执行，使得在宏观上具有多个进程同时执行的效果。

![](imgs\并行与并发.bmp)

在操作系统中，安装了多个程序，并发指的是在一段时间内宏观上有多个程序同时运行，这在单 CPU 系统中，每一时刻只能有一个程序执行，即微观上这些程序是分时的交替运行，只不过是给人的感觉是同时运行，那是因为分时交替运行的时间是非常短的。

而在多个 CPU 系统中，则这些可以并发执行的程序便可以分配到多个处理器上（CPU），实现多任务并行执行，即利用每个处理器来处理一个可以并发执行的程序，这样多个程序便可以同时执行。目前电脑市场上说的多核 CPU，便是多核处理器，核越多，**并行**处理的程序越多，能大大的提高电脑运行的效率。

> 注意：**单核**处理器的计算机肯定是**不能并行**的处理多个任务的，只能是多个任务在单个CPU上并发运行。同理，线程也是一样的，从宏观角度上理解线程是并行运行的，但是从微观角度上分析却是串行运行的，即一个线程一个线程的去运行，当系统只有一个CPU时，线程会以某种顺序执行多个线程，我们把这种情况称之为线程调度。

### 11.1.2 线程与进程

* **程序**：为了完成某个任务和功能，选择一种编程语言编写的一组指令的集合。

* **软件**：**1个或多个**应用程序+相关的素材和资源文件等构成一个软件系统。

* **进程**：是指一个内存中运行的应用程序，每个进程都有一个独立的内存空间，进程也是程序的一次执行过程，是系统运行程序的基本单位；系统运行一个程序即是一个进程从创建、运行到消亡的过程。

* **线程**：线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。一个进程中是可以有多个线程的，这个应用程序也可以称之为多线程程序。 

  简而言之：一个软件中至少有一个应用程序，应用程序的一次运行就是一个进程，一个进程中至少有一个线程。

* 面试题：进程是操作系统调度和分配资源的最小单位，线程是CPU调度的最小单位。不同的进程之间是不共享内存的。进程之间的数据交换和通信的成本是很高。不同的线程是共享同一个进程的内存的。当然不同的线程也有自己独立的内存空间。对于方法区，堆中中的同一个对象的内存，线程之间是可以共享的，但是栈的局部变量永远是独立的。

例如：

#### 一个软件中包含多个进程

![1563267154695](1563267154695.png)

#### 每个应用程序的运行都是一个进程

我们可以再电脑底部任务栏，右键----->打开任务管理器,可以查看当前任务的进程：

![](imgs\进程概念.png)

#### 一个应用程序的多次运行，就是多个进程

![1563267431480](1563267431480.png)

#### 一个进程中包含多个线程

![1563270525077](1563270525077.png)

### 11.1.3 线程调度

- 分时调度

  所有线程轮流使用 CPU 的使用权，平均分配每个线程占用 CPU 的时间。

- 抢占式调度

  优先让优先级高的线程使用 CPU，如果线程的优先级相同，那么会随机选择一个(线程随机性)，Java使用的为抢占式调度。

  -  抢占式调度详解

    大部分操作系统都支持多进程并发运行，现在的操作系统几乎都支持同时运行多个程序。比如：现在我们上课一边使用编辑器，一边使用录屏软件，同时还开着画图板，dos窗口等软件。此时，这些程序是在同时运行，”感觉这些软件好像在同一时刻运行着“。

    实际上，CPU(中央处理器)使用抢占式调度模式在多个线程间进行着高速的切换。对于CPU的一个核而言，某个时刻，只能执行一个线程，而 CPU的在多个线程间切换速度相对我们的感觉要快，看上去就是在同一时刻运行。
    其实，多线程程序并不能提高程序的运行速度，但能够提高程序运行效率，让CPU的使用率更高。

    ![抢占式调度](抢占式调度.bmp)

## 11.2 另行创建和启动线程

当运行Java程序时，其实已经有一个线程了，那就是main线程。

![1563281796505](1563281796505.png)

那么如何创建和启动main线程以外的线程呢？

### 11.2.1 继承Thread类

Java使用`java.lang.Thread`类代表**线程**，所有的线程对象都必须是Thread类或其子类的实例。每个线程的作用是完成一定的任务，实际上就是执行一段程序流即一段顺序执行的代码。Java使用线程执行体来代表这段程序流。Java中通过继承Thread类来**创建**并**启动多线程**的步骤如下：

1. 定义Thread类的子类，并重写该类的run()方法，该run()方法的方法体就代表了线程需要完成的任务,因此把run()方法称为线程执行体。
2. 创建Thread子类的实例，即创建了线程对象
3. 调用线程对象的start()方法来启动该线程

代码如下：

测试类：

~~~java
public class Demo01 {
	public static void main(String[] args) {
		//创建自定义线程对象
		MyThread mt = new MyThread("新的线程！");
		//开启新线程
		mt.start();
		//在主方法中执行for循环
		for (int i = 0; i < 10; i++) {
			System.out.println("main线程！"+i);
		}
	}
}
~~~

自定义线程类：

~~~java
public class MyThread extends Thread {
	//定义指定线程名称的构造方法
	public MyThread(String name) {
		//调用父类的String参数的构造方法，指定线程的名称
		super(name);
	}
	/**
	 * 重写run方法，完成该线程执行的逻辑
	 */
	@Override
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println(getName()+"：正在执行！"+i);
		}
	}
}
~~~

### 11.2.2 实现Runnable接口

Java有单继承的限制，当我们无法继承Thread类时，那么该如何做呢？在核心类库中提供了Runnable接口，我们可以实现Runnable接口，重写run()方法，然后再通过Thread类的对象代理启动和执行我们的线程体run()方法

步骤如下：

1. 定义Runnable接口的实现类，并重写该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体。
2. 创建Runnable实现类的实例，并以此实例作为Thread的target来创建Thread对象，该Thread对象才是真正
   的线程对象。
3. 调用线程对象的start()方法来启动线程。
   代码如下：

```java
  public class MyRunnable implements Runnable{
  	@Override  
      public void run() {
          for (int i = 0; i < 20; i++) {
          	System.out.println(Thread.currentThread().getName()+" "+i);         
  		}       
  	}    
  }
```

```java
  public class Demo {
      public static void main(String[] args) {
          //创建自定义类对象  线程任务对象
          MyRunnable mr = new MyRunnable();
          //创建线程对象
          Thread t = new Thread(mr, "小强");
          t.start();
          for (int i = 0; i < 20; i++) {
              System.out.println("旺财 " + i);
          }
      }
  }
```

 通过实现Runnable接口，使得该类有了多线程类的特征。run()方法是多线程程序的一个执行目标。所有的多线程
代码都在run方法里面。Thread类实际上也是实现了Runnable接口的类。

在启动的多线程的时候，需要先通过Thread类的构造方法Thread(Runnable target) 构造出对象，然后调用Thread对象的start()方法来运行多线程代码。

实际上所有的多线程代码都是通过运行Thread的start()方法来运行的。因此，不管是继承Thread类还是实现
Runnable接口来实现多线程，最终还是通过Thread的对象的API来控制线程的，熟悉Thread类的API是进行多线程编程的基础。

tips:Runnable对象仅仅作为Thread对象的target，Runnable实现类里包含的run()方法仅作为线程执行体。
而实际的线程对象依然是Thread实例，只是该Thread 线程负责执行其target的run()方法。

### 11.2.3 使用匿名内部类对象来实现线程的创建和启动

```java
new Thread("新的线程！"){
	@Override
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println(getName()+"：正在执行！"+i);
		}
	}
}.start();
```

```java
		new Thread(new Runnable(){
			@Override
			public void run() {
				for (int i = 0; i < 10; i++) {
					System.out.println(Thread.currentThread().getName()+"：" + i);
				}
			}
		}).start();
```


## 11.3 Thread类

### 11.3.1 构造方法

public Thread() :分配一个新的线程对象。
public Thread(String name) :分配一个指定名字的新的线程对象。
public Thread(Runnable target) :分配一个带有指定目标新的线程对象。
public Thread(Runnable target,String name) :分配一个带有指定目标新的线程对象并指定名字。

### 11.3.2 常用方法系列1

* public void run() :此线程要执行的任务在此处定义代码。
* public String getName() :获取当前线程名称。
* public static Thread currentThread() :返回对当前正在执行的线程对象的引用。
* public final boolean isAlive()：测试线程是否处于活动状态。如果线程已经启动且尚未终止，则为活动状态。 
* public final int getPriority() ：返回线程优先级 
* public final void setPriority(int newPriority) ：改变线程的优先级

  * 每个线程都有一定的优先级，优先级高的线程将获得较多的执行机会。每个线程默认的优先级都与创建它的父线程具有相同的优先级。Thread类提供了setPriority(int newPriority)和getPriority()方法类设置和获取线程的优先级，其中setPriority方法需要一个整数，并且范围在[1,10]之间，通常推荐设置Thread类的三个优先级常量：
  * MAX_PRIORITY（10）：最高优先级 
  * MIN _PRIORITY （1）：最低优先级
  * NORM_PRIORITY （5）：普通优先级，默认情况下main线程具有普通优先级。

```java
	public static void main(String[] args) {
		Thread t = new Thread(){
			public void run(){
				System.out.println(getName() + "的优先级：" + getPriority());
			}
		};
		t.setPriority(Thread.MAX_PRIORITY);
		t.start();
		
		System.out.println(Thread.currentThread().getName() +"的优先级：" + Thread.currentThread().getPriority());
	}
```

### 11.3.3 常用方法系列2

* public void start() :导致此线程开始执行; Java虚拟机调用此线程的run方法。

* public static void sleep(long millis) :使当前正在执行的线程以指定的毫秒数暂停（暂时停止执行）。

* public static void yield()：yield只是让当前线程暂停一下，让系统的线程调度器重新调度一次，希望优先级与当前线程相同或更高的其他线程能够获得执行机会，但是这个不能保证，完全有可能的情况是，当某个线程调用了yield方法暂停之后，线程调度器又将其调度出来重新执行。

* void join() ：等待该线程终止。 

  void join(long millis) ：等待该线程终止的时间最长为 millis 毫秒。如果millis时间到，将不再等待。 

  void join(long millis, int nanos) ：等待该线程终止的时间最长为 millis 毫秒 + nanos 纳秒。 

* public final void stop()：强迫线程停止执行。 该方法具有固有的不安全性，已经标记为@Deprecated不建议再使用，那么我们就需要通过其他方式来停止线程了，其中一种方式是使用变量的值的变化来控制线程是否结束。

#### 示例代码：倒计时

```java
	public static void main(String[] args) {
		for (int i = 10; i>=0; i--) {
			System.out.println(i);
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		System.out.println("新年快乐！");
	}
```

#### 示例代码：强行加塞

```java
import java.util.Scanner;

public class TestJoin {
	public static void main(String[] args) {
		ChatThread t = new ChatThread();
		t.start();
		
		for (int i = 1; i < 10; i++) {
			System.out.println("main:" + i);
			try {
				Thread.sleep(10);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
         //当main打印到5之后，需要等join进来的线程停止后才会继续了。
			if(i==5){
				try {
					t.join();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
		}
	}
}
class ChatThread extends Thread{
	public void run(){
		Scanner input = new Scanner(System.in);
		while(true){
			System.out.println("是否结束？（Y、N）");
			char confirm = input.next().charAt(0);
			if(confirm == 'Y' || confirm == 'y'){
				break;
			}
		}
		input.close();
	}
}
```

### 11.3.4 守护线程（了解）

有一种线程，它是在后台运行的，它的任务是为其他线程提供服务的，这种线程被称为“守护线程”。JVM的垃圾回收线程就是典型的守护线程。

守护线程有个特点，就是如果所有非守护线程都死亡，那么守护线程自动死亡。

调用setDaemon(true)方法可将指定线程设置为守护线程。必须在线程启动之前设置，否则会报IllegalThreadStateException异常。

调用isDaemon()可以判断线程是否是守护线程。

```java
public class TestThread {
	public static void main(String[] args) {
		MyDaemon m = new MyDaemon();
		m.setDaemon(true);
		m.start();

		for (int i = 1; i <= 100; i++) {
			System.out.println("main:" + i);
		}
	}
}

class MyDaemon extends Thread {
	public void run() {
		while (true) {
			System.out.println("我一直守护者你...");
			try {
				Thread.sleep(1);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
```



## 11.4 线程安全

### 11.4.1 线程安全

我们通过一个案例，演示线程的安全问题：
电影院要卖票，我们模拟电影院的卖票过程。假设要播放的电影是 “葫芦娃大战奥特曼”，本次电影的座位共100个
(本场电影只能卖100张票)。
我们来模拟电影院的售票窗口，实现多个窗口同时卖 “葫芦娃大战奥特曼”这场电影票(多个窗口一起卖这100张票)
方式一：

```java
public class Ticket implements Runnable {
	private int ticket = 100;

	/*
	 * 执行卖票操作
	 */
	@Override
	public void run() {
		// 每个窗口卖票的操作
		// 窗口永远开启
		while (true) {
            if (ticket > 0) { // 有票可以卖
                    // 出票操作
                // 使用sleep模拟一下出票时间
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                // 获取当前线程对象的名字
                String name = Thread.currentThread().getName();
                System.out.println(name + "正在卖:" + ticket--);
            }
		}
	}
}
```

测试类：

```java
public class Demo {
	public static void main(String[] args) {
		// 创建线程任务对象
		Ticket ticket = new Ticket();
		// 创建三个窗口对象
		Thread t1 = new Thread(ticket, "窗口1");
		Thread t2 = new Thread(ticket, "窗口2");
		Thread t3 = new Thread(ticket, "窗口3");
		// 同时卖票
		t1.start();
		t2.start();
		t3.start();
	}
}
```

方式二：

```java
public class TestThread {
	public static void main(String[] args) {
		Ticket t1 = new Ticket("窗口一");
		Ticket t2 = new Ticket("窗口二");
		Ticket t3 = new Ticket("窗口三");
		
		t1.start();
		t2.start();
		t3.start();
	}
}

class Ticket extends Thread{
	private static int ticket = 100;

	public Ticket() {
		super();
	}

	public Ticket(String name) {
		super(name);
	}

	/*
	 * 执行卖票操作
	 */
	@Override
	public void run() {
		// 每个窗口卖票的操作
		// 窗口永远开启
		while (true) {
            if (ticket > 0) { // 有票可以卖
                // 出票操作
                // 使用sleep模拟一下出票时间
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                // 获取当前线程对象的名字
                System.out.println(getName() + "正在卖:" + ticket--);
            }
		}
	}
}
```

发现程序出现了两个问题：

1. 相同的票数,比如某张票被卖了两回。
2. 不存在的票，比如0票与-1票，是不存在的。

这种问题，几个窗口(线程)票数不同步了，这种问题称为线程不安全。

当我们使用多个线程访问**同一资源**（可以是同一个变量、同一个文件、同一条记录等）的时候，若多个线程只有读操作，那么不会发生线程安全问题，但是如果多个线程中对资源有读和写的操作，就容易出现线程安全问题。
要解决上述多线程并发访问一个资源的安全性问题:也就是解决重复票与不存在票问题，Java中提供了同步机制
(synchronized)来解决。

![1563372934332](1563372934332.png)

根据案例简述：

窗口1线程进入操作的时候，窗口2和窗口3线程只能在外等着，窗口1操作结束，窗口1和窗口2和窗口3才有机会进入代码去执行。也就是说在某个线程修改共享资源的时候，其他线程不能修改该资源，等待修改完毕同步之后，才能去抢夺CPU资源，完成对应的操作，保证了数据的同步性，解决了线程不安全的现象。

为了保证每个线程都能正常执行原子操作,Java引入了线程同步机制。
那么怎么去使用呢？有三种方式完成同步操作：

1. 同步代码块。
2. 同步方法。
3. 锁机制

### 11.4.2 同步代码块

* 同步代码块： synchronized 关键字可以用于方法中的某个区块中，表示只对这个区块的资源实行互斥访问。
  格式:

  ```java
  synchronized(同步锁){
       需要同步操作的代码
  }
  ```
  * 同步锁:
    对象的同步锁只是一个概念,可以想象为在对象上标记了一个锁.

  ​	锁对象 可以是任意类型。

  ​	多个线程对象  要使用同一把锁。
  注意:在任何时候,最多允许一个线程拥有同步锁,谁拿到锁就进入代码块,其他的线程只能在外等着(BLOCKED)。
  使用同步代码块解决代码：

  ```java
  public class Ticket implements Runnable {
  	private int ticket = 100;
  
  	/*
  	 * 执行卖票操作
  	 */
  	@Override
  	public void run() {
  		// 每个窗口卖票的操作
  		// 窗口永远开启
          while (true) {
              synchronized (this) {//这里可以选择this作为锁，是因为对于这几个线程，Ticket的this是同一个 
                  if (ticket > 0) {// 有票可以卖
                      // 出票操作
                      // 使用sleep模拟一下出票时间
                      try {
                          Thread.sleep(100);
                      } catch (InterruptedException e) {
                          e.printStackTrace();
                      }
                      // 获取当前线程对象的名字
                      String name = Thread.currentThread().getName();
                      System.out.println(name + "正在卖:" + ticket--);
                  }
  			}
          }
  	}
  }
  ```

  当使用了同步代码块后，上述的线程的安全问题，解决了。

  ```java
  class Ticket extends Thread{
  	private static int ticket = 100;
  
  	public Ticket() {
  		super();
  	}
  
  	public Ticket(String name) {
  		super(name);
  	}
  
  	/*
  	 * 执行卖票操作
  	 */
  	@Override
  	public void run() {
  		// 每个窗口卖票的操作
  		// 窗口永远开启
  		while (true) {
  			synchronized (Ticket.class) {//这里不能选用this作为锁，因为这几个线程的this不是同一个
  				if (ticket > 0) { // 有票可以卖
  	                // 出票操作
  	                // 使用sleep模拟一下出票时间
  	                try {
  	                    Thread.sleep(100);
  	                } catch (InterruptedException e) {
  	                    e.printStackTrace();
  	                }
  	                // 获取当前线程对象的名字
  	                System.out.println(getName() + "正在卖:" + ticket--);
  	            }
  			}
  		}
  	}
  }
  ```

  

### 11.4.3 同步方法

* 同步方法:使用synchronized修饰的方法,就叫做同步方法,保证A线程执行该方法的时候,其他线程只能在方法外
  等着

格式：

```java
public synchronized void method(){
    可能会产生线程安全问题的代码
}
```

同步方法的锁对象：

（1）静态方法：当前类的Class对象

（2）非静态方法：this

使用同步方法代码如下：

```java
public class Ticket implements Runnable{
	private int ticket = 100;   
	/*
	 * 执行卖票操作   
	 */  
	@Override 
	public void run() {	    
	//每个窗口卖票的操作 	        
	//窗口 永远开启 	        
		while(true){        
			sellTicket();            
		}        
	}
		        
		/*   
		 * 锁对象 是 谁调用这个方法 就是谁     
		 *   隐含 锁对象 就是  this    
		 *    	    
		 */
	    
	public synchronized void sellTicket(){
		if(ticket>0){//有票 可以卖   
		  //出票操作
		  //使用sleep模拟一下出票时间 
			try {              
				Thread.sleep(100);
			} catch (InterruptedException e) {
  				e.printStackTrace();
			}
        
            //获取当前线程对象的名字 
            String name = Thread.currentThread().getName();
            System.out.println(name+"正在卖:"+ticket‐‐);
        }
   }
}
```

```java
class Ticket extends Thread {
	private static int ticket = 100;

	public Ticket() {
		super();
	}

	public Ticket(String name) {
		super(name);
	}

	/*
	 * 执行卖票操作
	 */
	@Override
	public void run() {
		// 每个窗口卖票的操作
		// 窗口永远开启
		while (true) {
			sellTicket();
		}
	}
	//这里必须是静态方法，因为如果是非静态方法，隐含的锁对象是this，那么多个线程就不是同一个锁对象了
	//而静态方法隐含的锁对象是当前类的Class对象
	public synchronized static void sellTicket(){
		if(ticket>0){//有票可以卖 
			//出票操作
			//使用sleep模拟一下出票时间
			try {
				Thread.sleep(100);
			} catch(InterruptedException e) {
				e.printStackTrace();
			}
			//获取当前线程对象的名字
			String name = Thread.currentThread().getName();
			System.out.println(name + "正在卖：" + ticket--);
		}
	}

}
```

## 11.5 等待唤醒机制

### 11.5.1 线程间通信

**概念：**多个线程在处理同一个资源，但是处理的动作（线程的任务）却不相同。

比如：线程A用来生成包子的，线程B用来吃包子的，包子可以理解为同一资源，线程A与线程B处理的动作，一个是生产，一个是消费，那么线程A与线程B之间就存在线程通信问题。

![1563373005631](1563373005631.png)

**为什么要处理线程间通信：**

多个线程并发执行时, 在默认情况下CPU是随机切换线程的，当我们需要多个线程来共同完成一件任务，并且我们希望他们有规律的执行, 那么多线程之间需要一些协调通信，以此来帮我们达到多线程共同操作一份数据。

**如何保证线程间通信有效利用资源：**

多个线程在处理同一个资源，并且任务不同时，需要线程通信来帮助解决线程之间对同一个变量的使用或操作。 就是多个线程在操作同一份数据时， 避免对同一共享变量的争夺。也就是我们需要通过一定的手段使各个线程能有效的利用资源。而这种手段即—— **等待唤醒机制。**

### 11.5.2 等待唤醒机制

**什么是等待唤醒机制**

这是多个线程间的一种**协作**机制。谈到线程我们经常想到的是线程间的**竞争（race）**，比如去争夺锁，但这并不是故事的全部，线程间也会有协作机制。就好比在公司里你和你的同事们，你们可能存在在晋升时的竞争，但更多时候你们更多是一起合作以完成某些任务。

就是在一个线程进行了规定操作后，就进入等待状态（**wait()**）， 等待其他线程执行完他们的指定代码过后 再将其唤醒（**notify()**）;在有多个线程进行等待时， 如果需要，可以使用 notifyAll()来唤醒所有的等待线程。

wait/notify 就是线程间的一种协作机制。

**等待唤醒中的方法**

等待唤醒机制就是用于解决线程间通信的问题的，使用到的3个方法的含义如下：

1. wait：线程不再活动，不再参与调度，进入 wait set 中，因此不会浪费 CPU 资源，也不会去竞争锁了，这时的线程状态即是 WAITING。它还要等着别的线程执行一个**特别的动作**，也即是“**通知（notify）**”在这个对象上等待的线程从wait set 中释放出来，重新进入到调度队列（ready queue）中
2. notify：则选取所通知对象的 wait set 中的一个线程释放；
3. notifyAll：则释放所通知对象的 wait set 上的全部线程。

> 注意：
>
> 哪怕只通知了一个等待的线程，被通知线程也不能立即恢复执行，因为它当初中断的地方是在同步块内，而此刻它已经不持有锁，所以她需要再次尝试去获取锁（很可能面临其它线程的竞争），成功后才能在当初调用 wait 方法之后的地方恢复执行。
>
> 总结如下：
>
> - 如果能获取锁，线程就从 WAITING 状态变成 RUNNABLE（可运行） 状态；
> - 否则，线程就从 WAITING 状态又变成 BLOCKED（等待锁） 状态

**调用wait和notify方法需要注意的细节**

1. wait方法与notify方法必须要由同一个锁对象调用。因为：对应的锁对象可以通过notify唤醒使用同一个锁对象调用的wait方法后的线程。
2. wait方法与notify方法是属于Object类的方法的。因为：锁对象可以是任意对象，而任意对象的所属类都是继承了Object类的。
3. wait方法与notify方法必须要在同步代码块或者是同步函数中使用。因为：必须要通过锁对象调用这2个方法。

### 11.5.3 生产者与消费者问题

等待唤醒机制可以解决经典的“生产者与消费者”的问题。

生产者与消费者问题（英语：Producer-consumer problem），也称有限缓冲问题（英语：Bounded-buffer problem），是一个多线程同步问题的经典案例。该问题描述了两个（多个）共享固定大小缓冲区的线程——即所谓的“生产者”和“消费者”——在实际运行时会发生的问题。生产者的主要作用是生成一定量的数据放到缓冲区中，然后重复此过程。与此同时，消费者也在缓冲区消耗这些数据。该问题的关键就是要保证生产者不会在缓冲区满时加入数据，消费者也不会在缓冲区中空时消耗数据。

生产者与消费者问题中其实隐含了两个问题：

* 线程安全问题：因为生产者与消费者共享数据缓冲区，不过这个问题可以使用同步解决。
* 线程的协调工作问题：
  * 要解决该问题，就必须让生产者线程在缓冲区满时等待(wait)，暂停进入阻塞状态，等到下次消费者消耗了缓冲区中的数据的时候，通知(notify)正在等待的线程恢复到就绪状态，重新开始往缓冲区添加数据。同样，也可以让消费者线程在缓冲区空时进入等待(wait)，暂停进入阻塞状态，等到生产者往缓冲区添加数据之后，再通知(notify)正在等待的线程恢复到就绪状态。通过这样的通信机制来解决此类问题。

#### 一对一做包子吃包子问题

就拿生产包子消费包子来说等待唤醒机制如何有效利用资源：

```java
包子铺线程生产包子，吃货线程消费包子。当包子没有时（包子状态为false），吃货线程等待，包子铺线程生产包子（即包子状态为true），并通知吃货线程（解除吃货的等待状态）,因为已经有包子了，那么包子铺线程进入等待状态。接下来，吃货线程能否进一步执行则取决于锁的获取情况。如果吃货获取到锁，那么就执行吃包子动作，包子吃完（包子状态为false），并通知包子铺线程（解除包子铺的等待状态）,吃货线程进入等待。包子铺线程能否进一步执行则取决于锁的获取情况。
```

**代码演示：**

包子资源类：

```java
public class BaoZi {
     String  pier ;
     String  xianer ;
     boolean  flag = false ;//包子资源 是否准备好  包子资源状态
}
```

吃货线程类：

```java
public class ChiHuo extends Thread{
    private BaoZi bz;

    public ChiHuo(String name,BaoZi bz){
        super(name);
        this.bz = bz;
    }
    @Override
    public void run() {
        while(true){
            synchronized (bz){
                if(bz.flag == false){//没包子
                    try {
                        bz.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }                
                
                System.out.println("吃货正在吃："+bz.pier+","+bz.xianer+"包子");
                bz.flag = false;
                bz.notify();
            }
        }
    }
}
```

包子铺线程类：

```java
public class BaoZiPu extends Thread {

    private BaoZi bz;

    public BaoZiPu(String name,BaoZi bz){
        super(name);
        this.bz = bz;
    }

    @Override
    public void run() {
        int count = 0;
        //造包子
        while(true){
            //同步
            synchronized (bz){
                if(bz.flag == true){//包子资源  存在
                    try {
                        bz.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }

                // 没有包子  造包子
                System.out.println("包子铺开始做包子");
                if(count%2 == 0){
                    // 薄皮  蟹黄包
                    bz.pier = "薄皮";
                    bz.xianer = "蟹黄灌汤";
                }else{
                    // 厚皮  牛肉大葱
                    bz.pier = "厚皮";
                    bz.xianer = "牛肉大葱";
                }
                count++;

                bz.flag=true;
                System.out.println("包子造好了："+bz.pier+bz.xianer);
                System.out.println("吃货来吃吧");
                //唤醒等待线程 （吃货）
                bz.notify();
            }
        }
    }
}
```

测试类：

```java
public class Demo {
    public static void main(String[] args) {
        //等待唤醒案例
        BaoZi bz = new BaoZi();

        ChiHuo ch = new ChiHuo("吃货",bz);
        BaoZiPu bzp = new BaoZiPu("包子铺",bz);

        ch.start();
        bzp.start();
    }
}
```

执行效果：

```java
包子铺开始做包子
包子造好了：厚皮牛肉大葱
吃货来吃吧
吃货正在吃：厚皮,牛肉大葱包子
包子铺开始做包子
包子造好了：薄皮蟹黄灌汤
吃货来吃吧
吃货正在吃：薄皮,蟹黄灌汤包子
包子铺开始做包子
包子造好了：厚皮牛肉大葱
吃货来吃吧
吃货正在吃：厚皮,牛肉大葱包子
包子铺开始做包子
包子造好了：薄皮蟹黄灌汤
吃货来吃吧
吃货正在吃：薄皮,蟹黄灌汤包子
```

#### 多个厨师多个服务员问题

案例：有家餐馆的取餐口比较小，只能放10份快餐，厨师做完快餐放在取餐口的工作台上，服务员从这个工作台取出快餐给顾客。现在有多个厨师和多个服务员。

```java
package com.atguigu.test.thread;

public class TestThread {
	public static void main(String[] args) {
		Workbench bench = new Workbench();
		Cook c1 = new Cook("大拿",bench);
		Cook c2 = new Cook("吉祥",bench);
		Waiter w1 = new Waiter("翠花",bench);
		Waiter w2 = new Waiter("如意",bench);
		
		c1.start();
		c2.start();
		w1.start();
		w2.start();
	}

}
class Workbench {
	private static final int MAX_VALUE = 10;
	private int num;
	public synchronized void put() {
		while(num >= MAX_VALUE){
			try {
				this.wait();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		try {
			Thread.sleep(100);
			//加入睡眠时间是放大问题现象，去掉同步和wait等，可观察问题
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		num++;
		System.out.println(Thread.currentThread().getName()+ "厨师制作了一份快餐，现在工作台上有：" + num + "份快餐");
		this.notifyAll();
	}
	public synchronized void take() {
		while(num <=0){
			try {
				this.wait();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		try {
			Thread.sleep(100);
			//加入睡眠时间是放大问题现象，去掉同步和wait等，可观察问题
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		num--;
		System.out.println(Thread.currentThread().getName()+"服务员取走了一份快餐，现在工作台上有：" + num + "份快餐");
		this.notifyAll();
	}
}
class Waiter extends Thread{
	private Workbench workbench;
	
	public Waiter(String name,Workbench workbench) {
		super(name);
		this.workbench = workbench;
	}

	public void run(){
		for (int i = 1; i <= 10; i++) {
			workbench.take();
		}
	}
}
class Cook extends Thread{
	private Workbench workbench;
	
	public Cook(String name,Workbench workbench) {
		super(name);
		this.workbench = workbench;
	}

	public void run(){
		for (int i = 1; i <= 10; i++) {
			workbench.put();
		}
	}
}
```

## 11.6 线程生命周期

简单来说，线程的生命周期有五种状态：新建（New）、就绪（Runnable）、运行（Running）、阻塞（Blocked）、死亡（Dead）。CPU需要在多条线程之间切换，于是线程状态会多次在运行、阻塞、就绪之间切换。

![](C:/Users/17335/Desktop/xinxin-note/Day10/README.assets/1563282798989.png)

**1.** **新建**

当一个Thread类或其子类的对象被声明并创建时，新生的线程对象处于新建状。此时它和其他Java对象一样，仅仅由JVM为其分配了内存，并初始化了实例变量的值。此时的线程对象并没有任何线程的动态特征，程序也不会执行它的线程体run()。

**2.** **就绪**

但是当线程对象调用了start()方法之后，就不一样了，线程就从新建状态转为就绪状态。JVM会为其创建方法调用栈和程序计数器，当然，处于这个状态中的线程并没有开始运行，只是表示已具备了运行的条件，随时可以被调度。至于什么时候被调度，取决于JVM里线程调度器的调度。

> 注意：
>
> 程序只能对新建状态的线程调用start()，并且只能调用一次，如果对非新建状态的线程，如已启动的线程或已死亡的线程调用start()都会报错IllegalThreadStateException异常。

**3.** **运行**

如果处于就绪状态的线程获得了CPU，开始执行run()方法的线程体代码，则该线程处于运行状态。如果计算机只有一个CPU，在任何时刻只有一个线程处于运行状态，如果计算机有多个处理器，将会有多个线程并行(Parallel)执行。

当然，美好的时光总是短暂的，而且CPU讲究雨露均沾。对于抢占式策略的系统而言，系统会给每个可执行的线程一个小时间段来处理任务，当该时间用完，系统会剥夺该线程所占用的资源，让其回到就绪状态等待下一次被调度。此时其他线程将获得执行机会，而在选择下一个线程时，系统会适当考虑线程的优先级。

**4.** **阻塞**

当在运行过程中的线程遇到如下情况时，线程会进入阻塞状态：

* 线程调用了sleep()方法，主动放弃所占用的CPU资源；
* 线程试图获取一个同步监视器，但该同步监视器正被其他线程持有；
* 线程执行过程中，同步监视器调用了wait()，让它等待某个通知（notify）；
* 线程执行过程中，同步监视器调用了wait(time)
* 线程执行过程中，遇到了其他线程对象的加塞（join）；
* 线程被调用suspend方法被挂起（已过时，因为容易发生死锁）；

当前正在执行的线程被阻塞后，其他线程就有机会执行了。针对如上情况，当发生如下情况时会解除阻塞，让该线程重新进入就绪状态，等待线程调度器再次调度它：

* 线程的sleep()时间到；
* 线程成功获得了同步监视器；
* 线程等到了通知(notify)；
* 线程wait的时间到了
* 加塞的线程结束了；
* 被挂起的线程又被调用了resume恢复方法（已过时，因为容易发生死锁）；

**5.** **死亡**

线程会以以下三种方式之一结束，结束后的线程就处于死亡状态：

* run()方法执行完成，线程正常结束
* 线程执行过程中抛出了一个未捕获的异常（Exception）或错误（Error）
* 直接调用该线程的stop()来结束该线程（已过时，因为容易发生死锁）



不过在java.lang.Thread.State的枚举类中这样定义：

```java
    public enum State {
        NEW,
        RUNNABLE,
        BLOCKED,
        WAITING,
        TIMED_WAITING,
        TERMINATED;
    }
```

* 首先它没有区分：就绪和运行状态，因为对于Java对象来说，只能标记为可运行，至于什么时候运行，不是JVM来控制的了，是OS来进行调度的，而且时间非常短暂，因此对于Java对象的状态来说，无法区分。只能我们人为的进行想象和理解。

* 其次根据Thread.State的定义，阻塞状态是分为三种的：BLOCKED、WAITING、TIMED_WAITING。

![1563381997032](1563381997032.png)



## 11.7 Thread和Runnable的区别

如果一个类继承Thread，则不适合资源共享。但是如果实现了Runable接口的话，则很容易的实现资源共享。
总结：
实现Runnable接口比继承Thread类所具有的优势：

1. 适合多个相同的程序代码的线程去共享同一个资源。
2. 可以避免java中的单继承的局限性。
3. 增加程序的健壮性，实现解耦操作，代码可以被多个线程共享，代码和线程独立。
4. 线程池只能放入实现Runable或Callable类线程，不能直接放入继承Thread的类。
5. 扩充：在java中，每次程序运行至少启动2个线程。一个是main线程，一个是垃圾收集线程。因为每当使用
   java命令执行一个类的时候，实际上都会启动一个JVM，每一个JVM其实在就是在操作系统中启动了一个进
   程。

## 11.8 释放锁操作与死锁

任何线程进入同步代码块、同步方法之前，必须先获得对同步监视器的锁定，那么何时会释放对同步监视器的锁定呢？

### **1、释放锁的操作**

当前线程的同步方法、同步代码块执行结束。

当前线程在同步代码块、同步方法中遇到break、return终止了该代码块、该方法的继续执行。

当前线程在同步代码块、同步方法中出现了未处理的Error或Exception，导致当前线程异常结束。

当前线程在同步代码块、同步方法中执行了锁对象的wait()方法，当前线程被挂起，并释放锁。

### **2、不会释放锁的操作**

线程执行同步代码块或同步方法时，程序调用Thread.sleep()、Thread.yield()方法暂停当前线程的执行。

线程执行同步代码块时，其他线程调用了该线程的suspend()方法将该该线程挂起，该线程不会释放锁（同步监视器）。应尽量避免使用suspend()和resume()这样的过时来控制线程。

### 3、死锁

不同的线程分别锁住对方需要的同步监视器对象不释放，都在等待对方先放弃时就形成了线程的死锁。一旦出现死锁，整个程序既不会发生异常，也不会给出任何提示，只是所有线程处于阻塞状态，无法继续。

```java
public class TestDeadLock {
	public static void main(String[] args) {
		Object g = new Object();
		Object m = new Object();
		Owner s = new Owner(g,m);
		Customer c = new Customer(g,m);
		new Thread(s).start();
		new Thread(c).start();
	}
}
class Owner implements Runnable{
	private Object goods;
	private Object money;

	public Owner(Object goods, Object money) {
		super();
		this.goods = goods;
		this.money = money;
	}

	@Override
	public void run() {
		synchronized (goods) {
			System.out.println("先给钱");
			synchronized (money) {
				System.out.println("发货");
			}
		}
	}
}
class Customer implements Runnable{
	private Object goods;
	private Object money;

	public Customer(Object goods, Object money) {
		super();
		this.goods = goods;
		this.money = money;
	}

	@Override
	public void run() {
		synchronized (money) {
			System.out.println("先发货");
			synchronized (goods) {
				System.out.println("再给钱");
			}
		}
	}
}
```

### 4、sleep()和wait()方法的区别

（1）sleep()不释放锁，wait()释放锁

（2）sleep()指定休眠的时间，wait()可以指定时间也可以无限等待直到notify或notifyAll

（3）sleep()在Thread类中声明的静态方法，wait方法在Object类中声明

因为我们调用wait（）方法是由锁对象调用，而锁对象的类型是任意类型的对象。那么希望任意类型的对象都要有的方法，只能声明在Object类中。

