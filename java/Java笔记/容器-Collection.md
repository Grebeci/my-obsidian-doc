参考自：

[Trail: Collections (The Java™ Tutorials)](https://docs.oracle.com/javase/tutorial/collections/index.html)

进度：




-----



原则：

- Don't use a library until you understand its API.

  

选择集合接口类型  的基本原则：

- 是否重复
- 是否有序





#### 继承关系图

![Two interface trees, one starting with Collection and including Set, SortedSet, List, and Queue, and the other starting with Map and including SortedMap.](https://docs.oracle.com/javase/tutorial/figures/collections/colls-coreInterfaces.gif)



#### 接口类型介绍

[介绍](https://docs.oracle.com/javase/tutorial/collections/interfaces/collection.html)

`Collection` :  Collection是集合层次结构的根接口，表示一组对象的集合。集合可能允许重复元素，也可能不允许；可能有序，也可能无序。此接口通常用于在需要最大通用性时传递和操作集合。

`Set` ：不能包含**重复元素**的集合。该接口对数学集合抽象进行建模。

`List`：一个有序的集合(有时称为序列)。列表可以包含重复的元素。List的用户通常可以精确控制每个元素在列表中的插入位置，并且可以通过整数索引(位置)访问元素。

`Queue`：用于在处理之前保存多个元素的集合。队列必须实现 顺序 属性，排序可能是 FIFO、优先级队列。无论使用何种排序方式，队列的头部都是将被remove或poll调用删除的元素。在FIFO队列中，所有新元素都插入到队列的尾部。其他类型的队列可能使用不同的放置规则。

`Map`：一个将键映射到值的对象。Map不能包含重复的键;每个键最多只能映射到一个值。



`SortedSet`：按升序维护其元素的Set。

`SortedMap`：按升序键顺序维护其映射的Map



## 接口

#### **The Collection Interface**

默认约定：所有通用集合实现都有一个接受 `Collection` 参数的构造函数。用来转换集合类型。例如：ArrayList 有构造函数

```java
public ArrayList(Collection<? extends E> c) { /***/ }
```

**接口签名**

```java
public interface Collection<E> extends Iterable<E>
```

**API**

| Function Signature                                      | Function Description                                  |
| ------------------------------------------------------- | ----------------------------------------------------- |
| `int size()`                                            | Number of items                                       |
| `boolean isEmpty()`                                     | is the collection empty?                              |
| `boolean contains(Object o)`                            | does the list contain the given item?                 |
| `Iterator<E> iterator()`                                | iterator over all items in the collection             |
| `Object[] toArray()`                                    | Converts to an array                                  |
| `<T> T[] toArray(T[] a)`                                | Converts to an array of specified type                |
| `<T> T[] toArray(IntFunction<T[]> generator)`           | Converts to an array using a generator                |
| `boolean add(E e)`                                      | Adds an element                                       |
| `boolean remove(Object o)`                              | Removes an element                                    |
| `boolean containsAll(Collection<?> c)`                  | Checks if contains all elements of another collection |
| `boolean addAll(Collection<? extends E> c)`             | Adds all elements from another collection             |
| `boolean removeAll(Collection<?> c)`                    | Removes all elements present in another collection    |
| `default boolean removeIf(Predicate<? super E> filter)` | Removes elements matching a predicate                 |
| `boolean retainAll(Collection<?> c)`                    | Retains only elements present in another collection   |
| `void clear()`                                          | Removes all elements                                  |
| `boolean equals(Object o)`                              |                                                       |
| `int hashCode()`                                        |                                                       |
| `Spliterator<E> spliterator()`                          | Creates a Spliterator                                 |
| `Stream<E> stream()`                                    | Returns a sequential Stream                           |
| `Stream<E> parallelStream()`                            | Returns a possibly parallel Stream                    |



##### 1.  toArray

下面三个API都会创建一个新的数组。此API充当 Array 和 Collection 的转换桥梁。

```java
Object[] toArray();  // 不关心类型
<T> T[] toArray(T[] a);  
default <T> T[] toArray(IntFunction<T[]> generator) {return toArray(generator.apply(0));}
```

`<T> T[] toArray(T[] a);  `  ：此`api` 会根据提供的数组大小有不同的行为。使用时候注意看文档。推荐用法

```java
String[] y = new String[SIZE];
      ...
 y = x. toArray(y);
```

或者

```java
Collection<String> collection = ... // 你的集合
String[] array = collection.toArray(new String[0]);
```

或者

```java
String[] y = x. toArray(String[]::new);
```



##### 2. add / remove （an Element）

这些API都是对单个元素的操作，并且会调用元素的 `equals` 方法。

```
boolean add(E e);
```

- 如果集合因此次调用而发生了变化（例如，元素被成功添加），则返回true。如果集合不允许重复并且已经包含了指定的元素，则返回false。

```
boolean remove(Object o);
```

- 只会移除集合中第一个匹配的元素
- 如果由于此调用而删除了元素，则返回True。



##### 3. Bulk Operations

containsAll、addAll、removeAll，retainAll，clear 等都会修改底层集合，所以是线程不安全的。

作为批量操作强大功能的一个简单示例，考虑以下从Collection c中删除指定元素e的所有实例的习惯用法。

```
c.removeAll(Collections.singleton(e));
```

##### 4. Traversing Collections

迭代器模式

```java
public interface Iterator<E> {
    boolean hasNext();
    E next();
    void remove(); //optional
}

public interface Iterable<E>
{
 Iterator<Item> iterator();
}
```

Remove是在迭代期间修改集合的唯一安全方法;如果在迭代进行过程中以任何其他方式修改底层集合，则未指定行为。

下面方法是使用 Iterator 筛选任意Collection—即遍历集合以删除特定元素。

```java
static void filter(Collection<?> c) {
    for (Iterator<?> it = c.iterator(); it.hasNext(); )
        if (!cond(it.next()))
            it.remove();
}
```



#### The Set Interface

Java平台包含三种通用的Set实现:HashSet、TreeSet和LinkedHashSet。将元素存储在哈希表中的HashSet是性能最好的实现; 但是，它不能保证迭代的顺序。TreeSet将元素存储在红黑树中，并根据元素的值对其进行排序;它比HashSet慢得多。LinkedHashSet是作为一个散列表实现的，其中贯穿着一个链表，它根据元素插入集合的顺序(插入顺序)对元素排序。LinkedHashSet使其客户端免受HashSet提的未指定的、通常是混乱的排序的影响，其成本仅略高。



##### idiom

假设您有一个Collection c，并且希望创建另一个包含相同元素但消除所有重复元素的Collection。下面的一行代码达到了目的。

```java
Collection<Type> noDups = new HashSet<Type>(c);
```

或者借助 Aggregate Operation：

```java
c.stream()
.collect(Collectors.toSet()); // no duplicates
```

它将一个名称集合累积到一个TreeSet中:

```
Set<String> set = people.stream()
.map(Person::getName)
.collect(Collectors.toCollection(TreeSet::new));
```



Set 接口中的 `Bulk` Operator  方法对应了标准的集合的代数操作。



##### 原则

- 原则1 ： 尽量使用接口类型 （例如 Set）而不是实现类型引用Collection。

##### Set集合特有的方法

Set 接口定义了一个集合，它不包含重复的元素。该接口提供了基本的集合操作，并对所有实现类进行了一些额外的约定。

```java
public interface Set<E> extends Collection<E>
```

| Function Signature                                           | Function Description                     |
| ------------------------------------------------------------ | ---------------------------------------- |
| `static <E> Set<E> of()`                                     | 返回一个包含零个元素的不可修改集合。     |
| `static <E> Set<E> of(E e1)`                                 | 返回一个包含一个元素的不可修改集合。     |
| `static <E> Set<E> of(E e1, E e2)                            | 返回一个包含两个元素的不可修改集合。     |
| ......                                                       | .......                                  |
| `static <E> Set<E> of(E e1, E e2, E e3, E e4, E e5, E e6, E e7, E e8, E e9, E e10)` | 返回一个包含十个元素的不可修改集合。     |
| `static <E> Set<E> of(E... elements)`                        | 返回一个包含多个元素的不可修改集合。     |
| `static <E> Set<E> copyOf(Collection<? extends E> coll)`     | 返回一个包含指定集合元素的不可修改集合。 |

上面这些方法都会返回不可变集合。



#### The List Interface

有序，可重复序列。两个通用实现，ArrayList通常是性能更好的实现，而LinkedList在某些情况下提供更好的性能。

- 位置访问：get、set
- 搜索：indexOf 、lastIndexOf
- 迭代：扩展Iterator语义，以利用列表的顺序特性。listIterator方法提供了这种行为。
- 范围视图：subblist方法在列表上执行任意范围操作。



##### 构造：

```java
List<Type> list = new ArrayList<Type>(list);

List<Type> list = new ArrayList<Type>();

List<String> list = people.stream()
.map(Person::getName)
.collect(Collectors.toList());
```



##### Positional Access Operations

基本的位置访问操作有get、set、add和remove。(set和remove操作返回被覆盖或删除的旧值。)其他操作(indexOf和lastIndexOf)返回列表中指定元素的第一个或最后一个索引。

一些从 Collection继承的接口在 List 上会有特定的行为。

```java
public interface List<E> extends Collection<E>
```

| Function Signature               | Function Description                     |
| -------------------------------- | ---------------------------------------- |
| `boolean add(E e)`               | 将指定元素添加到列表末尾。               |
| `boolean remove(Object o)`       | 移除列表中首次出现的指定元素。           |
| `E get(int index)`               | 返回指定位置的元素。                     |
| `E set(int index, E element)`    | 用指定元素替换列表中指定位置的元素。     |
| `void add(int index, E element)` | 在列表的指定位置插入指定元素。           |
| `E remove(int index)`            | 移除列表中指定位置的元素。               |
| `int indexOf(Object o)`          | 返回列表中首次出现的指定元素的索引。     |
| `int lastIndexOf(Object o)`      | 返回列表中最后一次出现的指定元素的索引。 |

**注：**

Arrays类有一个名为asList的静态工厂方法，该方法允许将数组视为List。此方法不复制数组。List中的更改贯穿写入数组，反之亦然。生成的List不是一个通用的List实现，因为它没有实现(可选的)添加和删除操作:数组不能调整大小。



##### Range-View Operation

子列表 API

```java
List<E> subList(int fromIndex, int toIndex);
```

- 左闭右开
- 对 sublist 的更改都会反映在原列表中。
- 这些 API 使用务必保证其正确性。



**不可变集合**

和 Set 类似，List也有一些方法返回不可变集合。

```java
static <E> List<E> of()
static <E> List<E> of(E e1) 
...
static <E> List<E> of(E... elements)
static <E> List<E> copyOf(Collection<? extends E> coll) 
```



#### The Map Interface

Map是一个将键映射到值的对象。一个映射不能包含重复的键:每个键最多只能映射到一个值。它对数学函数抽象进行建模。Map接口包括用于基本操作(如put、get、remove、containsKey、containsValue、size和empty)、批量操作(如putAll和clear)和集合视图(如keySet、entrySet和values)的方法。



##### Bulk Operations

`putAll` 的一个例子

假设使用Map来表示属性-值对的集合;putAll操作与Map转换构造函数相结合，提供了一种使用默认值实现属性映射创建的简便方法。下面是演示此技术的静态工厂方法。

```java
static <K, V> Map<K, V> newAttributeMap(Map<K, V>defaults, Map<K, V> overrides) {
    Map<K, V> result = new HashMap<K, V>(defaults);
    result.putAll(overrides);
    return result;
}
```

使用`Map`来表示属性-值对可以让你灵活地存储和管理配置数据。通过`putAll()`方法，你可以非常方便地将一组默认配置（存储在一个`Map`中）和一组特定的覆盖配置（存储在另一个`Map`中）合并。



##### Collection Views

下面API 将 `Map` 视为 `Collection`

```java
Set<K> keySet();
Collection<V> values();
Set<Map.Entry<K, V>> entrySet();
```

注意： values 返回的不是 `Set`，允许可重复元素的集合。

```java
for (Map.Entry<KeyType, ValType> e : m.entrySet())
    System.out.println(e.getKey() + ": " + e.getValue());
```



#### **The SortedSet Interface**

##### Standard Constructors

注意：

`TreeSet`的标准构造函数在处理继承自`SortedSet`的集合时，因默认使用自然排序而忽略原有比较器，导致可能需要选择性使用不同构造函数以保持排序标准的一致性。





## 实现

[Overview- 对 集合框架 的实现架构做了概述](https://docs.oracle.com/javase/tutorial/collections/implementations/index.html#:~:text=Lesson%3A-,Implementations,-%E6%95%99%E8%AE%AD%3A%E5%AE%9E%E7%8E%B0)

- **General-purpose implementations** 

  通用实现，非同步（线程安全）的，原则是：一般来说，不让用户为他们不使用的功能付费是良好的API设计实践。这句话意思是不应该强制所有使用集合的场景都承担同步的开销，特别是在许多情况下同步是不必要的。

- ***Concurrent implementations**：这些实现是java.util.concurrent包的一部分

- **Wrapper implementations** ：包装器实现与其他类型的实现(通常是通用的实现)结合使用，以提供添加的或受限的功能。 例如，如果需要线程安全的集合，那么在包装器实现一节中描述的同步包装器允许将任何集合转换为同步集合。

- **Convenience implementations**

  便利实现是小型实现，通常通过静态工厂方法提供，它为特殊集合(例如，单例集)提供方便、高效的通用实现替代方案



对于一个接口的具体实现，一般会有三类实现，通用实现，特殊实现（特定场景），并发实现。但是无锁数据结构似乎没有。



#### Set 的实现

- 是否考虑顺序？
- 特定的实现，[`EnumSet`](https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html) and [`CopyOnWriteArraySet`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArraySet.html).

`CopyOnWriteArraySet` :  读多写少，写时复制（Copy-on-Write），无锁且线程安全。迭代读取快照。



#### List 的实现

ArrayList和LinkedList ，具体使用哪种，需要考虑，具体的添加删除查询操作，元素的位置。

一般来说，如果你经常在List的开头添加元素，或者迭代List以从内部删除元素，你应该考虑使用LinkedList。这些操作在LinkedList中需要常量时间，在ArrayList中需要线性时间。位置访问在LinkedList中需要线性时间，在ArrayList中需要常量时间。



[`CopyOnWriteArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArrayList.html) ： copy-on-write 的实现。



#### Map 实现

- 考虑顺序，



## Algorithms



## Stream

这些 API 和 函数式 编程有密切的关系，但是，对于他们的实现目前不太熟悉，根据原则，不熟悉的API慎重使用，故在没有完全理解这些API 实现后不能使用。

有以下主题需要熟悉

- 集合的 Aggregate Operation 

- **Parallelism** 集合的具体使用。

- 集合的线程安全

- side effect （副作用）

- Laziness

- Collectors 具体实现

  

进度

- [ ] lambda 表达式熟悉
- [ ] Stream 流具体实现
  - Collectors 的具体实现
  - 关于 side effect 、laziness 、thread-safe ，Parallelism







