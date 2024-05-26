
## 泛型通配符

> 阅读 [Java Tutorial -Generic ](https://docs.oracle.com/javase/tutorial/java/generics)  有很多不懂或者写的模糊的地方。

为什么会有泛型通配符，或者说泛型通配符使用的场景，目前理解的有：

- 简化泛型声明。
- 异质集合（Collection）中不关心每个元素的具体类型。
- 约定集合容器 只可读 / 只可写。
- 逆变和协变 。



#### case1 

原文

> You can use an upper bounded wildcard to relax the restrictions on a variable. For example, say you want to write a method that works on `List<Integer>`, `List<Double>`, *and* `List<Number>`; you can achieve this by using an upper bounded wildcard.
>
> 可以使用上界通配符来放宽对变量的限制。例如，假设你想编写一个方法，可以在`List<Integer>`、`List<Double>`和`List<Number>` 上工作;您可以通过使用上界通配符来实现这一点。

疑惑点： 即使不引入通配符，也可以实现这点

考虑以下 code

```java
public static void process(List<Number> list) {/* ...*/}
```

按照原文的意思，这个函数只能接受 `List<Number>` 类型，由于泛型的不变性，`List<Integer>` 和`List<Number>`  是没有继承关系的。

但是，借助泛型上下界，我们可以写出

```java
public static <T extends Number> void process(List<T> list) {/* ...*/}
```

同样可以在 `List<Integer>`、`List<Double>`和`List<Number>` 上工作。



按照文档要求通配符方式，则有

```java
 public void process(List<? extends Number> list) {}
```

和前一种相比：

- 声明更简化，不必引入 T。
- 这种方式还有个feature，List 只读而不能写入。

还有就是，不借助泛型通配符的方式的 `process` ， `T`在每次方法调用时会被实例化为调用它时传入的具体类型参数。



#### case2

> The `sumOfList` method returns the sum of the numbers in a list:sumOfList
>
> 方法返回列表中所有数字的和:
>
> ```java
> public static double sumOfList(List<? extends Number> list) {
>     double s = 0.0;
>     for (Number n : list)
>         s += n.doubleValue();
>     return s;
> }
> ```
>
> The following code, using a list of `Integer` objects, prints `sum = 6.0`:
>
> 下面的代码使用一个Integer对象列表，输出sum = 6.0:
>
> ```java
> List<Integer> li = Arrays.asList(1, 2, 3);
> System.out.println("sum = " + sumOfList(li));
> ```
>
> A list of `Double` values can use the same `sumOfList` method. The following code prints `sum = 7.0`:
>
> Double值的列表可以使用相同的sumOfList方法。下面的代码输出sum = 7.0:
>
> ```java
> List<Double> ld = Arrays.asList(1.2, 2.3, 3.5);
> System.out.println("sum = " + sumOfList(ld));
> ```



下面不引入通配符的方式，功能一样。

```
public static <T extends Number>  double sumOfList(List<T> list) {
    /* ...*/
}
```

不同点在于，使用 `List<? extends Number> ` 声明的，会使 `List` 成为 read-only 。



#### case3 逆变和协变

>  这是必须使用通配符的例子



> [Answer to Questions and Exercises: Generics (The Java™ Tutorials > Learning the Java Language > Generics (Updated)) ~ 问题和练习答案:泛型(Java™教程>学习Java语言>泛型(更新)) (oracle.com)](https://docs.oracle.com/javase/tutorial/java/generics/QandE/generics-answers.html)
>
> Write a generic method to find the maximal element in the range[begin, end) of a list.
>
> 编写一个泛型方法来查找列表范围[开始，结束]中的最大元素。
>
> Answer：
>
> ```java
> import java.util.*;
> 
> public final class Algorithm {
>     public static <T extends Object & Comparable<? super T>>
>         T max(List<? extends T> list, int begin, int end) {
> 
>         T maxElem = list.get(begin);
> 
>         for (++begin; begin < end; ++begin)
>             if (maxElem.compareTo(list.get(begin)) < 0)
>                 maxElem = list.get(begin);
>         return maxElem;
>     }
> }
> 
> ```
>
> 这个答案是简化过的。`T extends Object &` 没必要说明



其中 `Comparable<? super T> ` 是逆变，假设有继承关系：

```java
class Animal implements Comparable<Animal> {}

class Dog extends Animal {}

class Cat extends Animal {}
```

如果泛型写成 `T extends <Comparable<T>`，函数签名为：

```java
public static <T Comparable<T>>
        T max(List<? extends T> list, int begin, int end)
```

由于 Dog 继承自Animal ，所以也实现了 `Comparable<Animal>` ，但此时要求一个 ` Comparable<Dog>`, 根据泛型的不变性，` Comparable<Dog>` 并不是 ` Comparable<Animal>` 的子类型。



## 泛型的限制

#### case1 

不能创建类型参数的实例，但是可以实例化具体的泛型类.



Java 中有一个常见的误解，即不能直接创建类型参数的实例。然而，这种限制实际上是指你不能直接使用 `new T()` 这样的代码来创建一个泛型类型的新实例，因为类型擦除后，JVM 不知道 `T` 的具体类是什么。

但是下面code是合法的：

```java
Collection<T> result = new ArrayList<T>();
```

总结：不能直接实例化类型参数（如 `new T()`），但可以实例化具体的泛型类（如 `new ArrayList<T>()`），其中类型参数作为其类型信息的一部分。这里的类型擦除不影响 `ArrayList` 的实例化，因为 `ArrayList` 仅需要知道如何在内部处理类型为 `T` 的对象，而具体的类型处理（如元素的添加）是在运行时决定的。







