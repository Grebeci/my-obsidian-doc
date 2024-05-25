程序逻辑有些是数据驱动的，有些是类型驱动的。



#### 为什么使用泛型（Generics）

> [为什么使用泛型？(Java™ 教程 > 学习 Java 语言 > 泛型（已更新） (oracle.com)](https://docs.oracle.com/javase/tutorial/java/generics/why.html)

**简记为：**

- 编译期检查  —— 类型安全 （Type Safety）
- 消除类型转换
- 代码复用。—— 实现通用的容器，适用于所有类型。



#### Generic Types ：泛型类(型)  

> [Generic Types (The Java™ Tutorials > Learning the Java Language > Generics (Updated)) ~ 泛型（Java™ 教程 > 学习 Java 语言 > 泛型（已更新） (oracle.com)](https://docs.oracle.com/javase/tutorial/java/generics/types.html)

泛型类的定义格式如下：

```
class name<T1, T2, ..., Tn> { /* ... */ }
```

- `类型参数(type parameter)` （也称为`类型变量(type variable)`）T1、T2、...和 Tn。

- 类型参数命名约定。

  ```
  E - Element (used extensively by the Java Collections Framework)
  K - Key
  N - Number
  T - Type
  V - Value
  S,U,V etc. - 2nd, 3rd, 4th types
  ```

- 类型形参（Type Parameter） ，类型实参（Type Argument）。

- 参数化类型（Parameterized Type）：对泛型的调用一般统称为参数化类型。例如 `Box<String> = new Box<>() ` 中的 `Box<String>` 就称为参数化类型。



#### 原始类型（Raw Type）

> [Raw Types (The Java™ Tutorials > Learning the Java Language > Generics (Updated)) ~ 原始类型(Java™教程>学习Java语言>泛型(更新)) (oracle.com)](https://docs.oracle.com/javase/tutorial/java/generics/rawTypes.html)

对于**泛型类**，相对于参数化类型，省略实际类型参数就称为原始类型。澄清以下概念：

- 非泛型类或接口类型不是原始类型。原始类型是在泛型类上的概念。
- 由于 JDK5 之前没有泛型，许多API类(比如Collections类)不是泛型的。所以为了向后兼容，出现了这个概念。



#### 泛型方法

泛型方法是引入自身类型参数的方法。这与声明泛型类似，但类型参数的作用域仅限于声明它的方法。允许使用静态和非静态泛型方法以及泛型类构造函数。

