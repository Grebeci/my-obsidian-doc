## 参考自：

[Trail: Collections (The Java™ Tutorials)](https://docs.oracle.com/javase/tutorial/collections/index.html)

官方文档上是按照功能罗列出 API 的，这和其他教程按照接口，类介绍组织方式不同。如果从介绍类开始，那么就会陷入复杂的继承关系、并发集合、工具类这些没有层次的知识点上。因为如果研究一个一个类或接口（例如从 ArrayList）就会陷入N个API罗列中，不知道这些API为什么在这个地方，为什么这么设计。

而如果从集合的设计角度出发，进行整体认识，官方教程线熟悉六大接口，然后是实现，然后是算法（工具类）。这样会有一个大致的轮廓，特别是先讲接口，再介绍实现，这个方式对于一些冷门、特定场合的API 不会放过多精力。而且有一个整体记忆的概念。

总结：官方文档写的太好了！。



## 进度：

- 官方文档：[Trail: Collections (The Java™ Tutorials) (oracle.com)](https://docs.oracle.com/javase/tutorial/collections/index.html)

  - [ ] [**interoperability**](https://docs.oracle.com/javase/tutorial/collections/interoperability/index.html) 

  - [ ] [Lesson: Aggregate Operations (The Java™ Tutorials > Collections) (oracle.com) ](https://docs.oracle.com/javase/tutorial/collections/streams/index.html) 

    Stream 流相关，这个官方太简练了，因为牵涉到函数式编程，lambda 设计，还有一些算子设计，并发设计，所以要等完全理解函数式编程再重头来看。

  - [ ] [自定义实现](https://docs.oracle.com/javase/tutorial/collections/custom-implementations/index.html) ：可能扩展集合框架近期不会使用，先放着。 
  
- 书籍：Core-Java

  - 





-----



## 原则：

- Don't use a library until you understand its API.

  

选择集合接口类型  的基本原则：

- 是否重复
- 是否有序



## Overview

#### 继承关系图

![Two interface trees, one starting with Collection and including Set, SortedSet, List, and Queue, and the other starting with Map and including SortedMap.](https://docs.oracle.com/javase/tutorial/figures/collections/colls-coreInterfaces.gif)



#### 接口类型介绍

[介绍](https://docs.oracle.com/javase/tutorial/collections/interfaces/collection.html)

`Collection` :  Collection是集合层次结构的根接口，表示一组对象的集合。集合可能允许重复元素，也可能不允许；可能有序，也可能无序。此接口通常用于在需要最大通用性时传递和操作集合。

`Set` ：不能包含**重复元素**的集合。该接口对数学集合抽象进行建模。

`List`：一个有序的集合(有时称为序列)。列表可以包含重复的元素。List的用户通常可以精确控制每个元素在列表中的插入位置，并且可以通过整数索引(位置)访问元素。

`Queue`：用于在处理之前保存多个元素的集合。队列必须实现 顺序 属性，排序可能是 FIFO、优先级队列。无论使用何种排序方式，队列的头部都是将被 `remove` 或 `poll` 调用删除的元素。在FIFO队列中，所有新元素都插入到队列的尾部。其他类型的队列可能使用不同的放置规则。

`Map`：一个将键映射到值的对象。Map不能包含重复的键;每个键最多只能映射到一个值。

`SortedSet`：按升序维护其元素的Set。

`SortedMap`：按升序键顺序维护其映射的Map



## 接口

### **The Collection Interface**

**默认约定：**

所有通用集合实现都有一个接受 `Collection` 参数的构造函数。用来转换集合类型。例如：ArrayList 有构造函数

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
| `Iterator<E> iterator()`                                | iterator over all items in the collection             |
| `int size()`                                            | Number of items                                       |
| `boolean isEmpty()`                                     | is the collection empty?                              |
|                                                         |                                                       |
| `boolean add(E e)`                                      | Adds an element                                       |
| `boolean remove(Object o)`                              | Removes an element                                    |
| `boolean contains(Object o)`                            | does the list contain the given item?                 |
| `void clear()`                                          | Removes all elements                                  |
|                                                         |                                                       |
| `boolean addAll(Collection<? extends E> c)`             | Adds all elements from another collection             |
| `boolean removeAll(Collection<?> c)`                    | Removes all elements present in another collection    |
| `boolean containsAll(Collection<?> c)`                  | Checks if contains all elements of another collection |
| `default boolean removeIf(Predicate<? super E> filter)` | Removes elements matching a predicate                 |
| `boolean retainAll(Collection<?> c)`                    | Retains only elements present in another collection   |
|                                                         |                                                       |
| `Object[] toArray()`                                    | Converts to an array                                  |
| `<T> T[] toArray(T[] a)`                                | Converts to an array of specified type                |
| `<T> T[] toArray(IntFunction<T[]> generator)`           | Converts to an array using a generator                |
|                                                         |                                                       |
| `Stream<E> stream()`                                    | Returns a sequential Stream                           |
| `Stream<E> parallelStream()`                            | Returns a possibly parallel Stream                    |
|                                                         |                                                       |
| `boolean equals(Object o)`                              |                                                       |
| `int hashCode()`                                        |                                                       |

##### 1.  toArray

下面三个API都会创建一个新的数组。此API充当 Array 和 Collection 的转换桥梁。

```java
Object[] toArray();  // 不关心类型
<T> T[] toArray(T[] a);  
default <T> T[] toArray(IntFunction<T[]> generator) {return toArray(generator.apply(0));}
```

`<T> T[] toArray(T[] a);  `  ：此`api` 会根据参数提供的数组大小有不同的行为。使用时候注意看文档。推荐用法

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

这些API都是对单个元素的操作，并且可能会调用元素的 `equals` 方法。

```java
boolean add(E e);
```

- 如果集合因此次调用而发生了变化（例如，元素被成功添加），则返回true。如果集合不允许重复并且已经包含了指定的元素，则返回false。

```java
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

Remove 是在迭代期间修改集合的唯一安全方法;如果在迭代进行过程中以任何其他方式修改底层集合，则未指定行为。

下面方法是使用 Iterator 筛选任意Collection—即遍历集合以删除特定元素。

```java
static void filter(Collection<?> c) {
    for (Iterator<?> it = c.iterator(); it.hasNext(); )
        if (!cond(it.next()))
            it.remove();
}
```



##### 要点

- 增删查改返回值都是 `Boolean`，且在不同的实现有不同的语义。



### The Set Interface

散列表（Hashtable）在 Java 的实现，使用链表数组实现。

- 散列表由于不关心顺序，查找可以达到 `O(1)` 性能。
- 散列冲突不可避免，当由于桶太慢，导致散列冲突，会再散列。

##### 使用原则

- 原则1 ： 尽量使用接口类型 （例如 Set）而不是实现类型引用Collection。

Java平台包含三种通用的Set实现:HashSet、TreeSet和LinkedHashSet。不关心顺序就使用 `HashSet`，性能最高。如果关心顺序，有两种实现 `LinkedHashSet`，`TreeSet` 。 `LinkedHashSet` 是根据插入顺序对元素排序，TreeSet 是使用元素的值对其进行排序，有自然排序（对应Comparable 和 Comparator），使用红黑树实现。

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

- `s1.containsAll(s2)` — $ s2 \subseteq s1 $
- `s1.addAll(s2)` — $ s1 \cup s2 $
- `s1.retainAll(s2)` — $ s1 \cap s2 $
- `s1.removeAll(s2)` —  $s1 - s2 $

##### API

- Set 方法和Collection方法基本一致。
- 添加重复的元素，add会返回 Flase。
- 判断重复使用的方法是 `equals`，但是 TreeMap 使用 Comparable 或者 Comparator 的方法判断是否相等。

**Set集合特有的方法**

Set 接口定义了一个集合，它不包含重复的元素。该接口提供了基本的集合操作，并对所有实现类进行了一些额外的约定。

```java
public interface Set<E> extends Collection<E>
```

| Function Signature                                       | Function Description                     |
| -------------------------------------------------------- | ---------------------------------------- |
| `static <E> Set<E> of()`                                 | 返回一个包含零个元素的不可修改集合。     |
| ....                                                     | of 参数不定。                            |
| `static <E> Set<E> of(E... elements)`                    | 返回一个包含多个元素的不可修改集合。     |
| `static <E> Set<E> copyOf(Collection<? extends E> coll)` | 返回一个包含指定集合元素的不可修改集合。 |

上面这些方法都会返回不可变集合。



`java.util.TreeSet<E>`

| 方法                                        | 描述                                               |
| ------------------------------------------- | -------------------------------------------------- |
| `TreeSet()`                                 | 构造一个新的空集合                                 |
| `TreeSet(Comparator<? super E> comparator)` | 构造一个空集合，该集合根据指定的比较器排序         |
| `TreeSet(Collection<? extends E> elements)` | 构造一个包含指定集合元素的集合，元素按自然排序     |
| `TreeSet(SortedSet<E> s)`                   | 构造一个包含指定有序集的新集合，元素保持原有的顺序 |

`java.util.SortedSet<E>`

| 方法           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| `comparator()` | 返回用于比较集合元素的比较器，如果使用元素的自然顺序，则返回 `null` |
| `first()`      | 返回当前集合中最小的元素                                     |
| `last()`       | 返回当前集合中最大的元素                                     |

`java.util.NavigableSet<E>`

| 方法                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| `higher(E value)`      | 返回比指定元素大的最小元素，如果不存在这样的元素则返回 `null` |
| `lower(E value)`       | 返回比指定元素小的最大元素，如果不存在这样的元素则返回 `null` |
|                        |                                                              |
| `ceiling(E value)`     | 返回大于或等于指定元素的最小元素，如果不存在这样的元素则返回 `null` |
| `floor(E value)`       | 返回小于或等于指定元素的最大元素，如果不存在这样的元素则返回 `null` |
|                        |                                                              |
| `pollFirst()`          | 移除并返回当前集合中最小的元素，如果集合为空则返回 `null`    |
| `pollLast()`           | 移除并返回当前集合中最大的元素，如果集合为空则返回 `null`    |
|                        |                                                              |
| `descendingIterator()` | 返回一个逆序迭代器用于遍历集合                               |



##### 坑

修改 HashSet中的可变对象带来的问题，请看下列面试题

```java
HashSet set = new HashSet();
Person p1 = new Person(1001,"AA");
Person p2 = new Person(1002,"BB");

set.add(p1);
set.add(p2);
p1.name = "CC";
set.remove(p1);
System.out.println(set);

set.add(new Person(1001,"CC"));
System.out.println(set);

set.add(new Person(1001,"AA"));
System.out.println(set);

//其中Person类中重写了hashCode()和equal()方法
```

- Hashcode 如何实现的，每一步发生了什么？



### The List Interface

顺序表的实现

有序，可重复序列。两个通用实现，ArrayList通常是性能更好的实现，而LinkedList在某些情况下提供更好的性能。

- 位置访问：get、set
- 搜索：indexOf 、lastIndexOf
- 迭代：扩展 Iterator 语义，以利用列表的顺序特性。`listIterator` 方法提供了这种行为。
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

一些从 Collection 继承的接口在 List 上会有特定的行为。

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

Arrays类有一个名为asList的静态工厂方法，该方法允许将数组视为List。 生成的List不是一个通用的List实现，因为它没有实现(可选的)添加和删除操作:数组不能调整大小。

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

### The Map Interface

我们知道某些健的信息，要查找与之相关的元素（对象）。映射（Map）数据结构为此而设计。

- 也可以看成对 数学函数的建模。

**约定**

- `HashSet` 是散列映射，`TreeMap` 对键进行了排序，是搜索树的实现。
- 查找 Map 不存在的键，返回 `NULL`。重复的键值会进行覆盖，只保留最近的映射。

##### API

Map接口包括用于基本操作(如put、get、remove、containsKey、containsValue、size和empty)、批量操作(如putAll和clear)和集合视图(如keySet、entrySet和values)的方法。

```java
public interface Map<K, V>
```

| Function Signature                                           | Function Description                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `V get(Object key)`                                          | 获取与键对象相对应的值；返回与键对象相对应的值，如果在映射中找不到该键则返回null。 |
| `V put(K key, V value)`                                      | 将键对象与值对象关联到映射中。如果该键已经有映射存在，新的对象将替换旧的对象并返回旧对象。这个方法返回被替换的旧对象的值。如果这个键以前没有映射存在则返回null。返回值为null，但并不意味着null。 |
| `V remove(Object key)`                                       | 如果存在，则移除指定键的映射关系。                           |
| `default V replace(K key, V value)`                          | 仅当指定键当前已关联某个值时，才替换与其关联的值。           |
| `boolean containsKey(Object key)`                            | 如果在映射中已经有与键对象相对应的映射，返回true。           |
| `boolean containsValue(Object value)`                        | 如果在映射中已经有与值对象相对应的映射，返回true。           |
|                                                              |                                                              |
| `default V getOrDefault(Object key, V defaultValue)`         | 获取与键对象相对应的值；如果在映射中找不到该键则返回defaultValue。如果 键关联的值为 NULL，也会返回默认值。 |
| `default V putIfAbsent(K key, V value)`                      | 如果指定的键尚未与值关联(或映射为null)，则将其与给定值关联并返回null，否则返回当前值。如果不存在映射，就关联。 |
| `default boolean remove(Object key, Object value)`           | 仅当当前键-值对与指定的值匹配时才移除指定键的映射关系。      |
| `default boolean replace(K key, V oldValue, V newValue)`     | 仅当当前键-值对与指定的值匹配时，才用新的值替换当前值。      |
|                                                              |                                                              |
| `void putAll(Map<? extends K, ? extends V> m)`               | 将一个映射中的所有键值映射复制到本映射中。                   |
| `void clear()`                                               | 移除所有的映射关系（可选操作）。                             |
| `Set<K> keySet()`                                            | 返回一个包含映射中所有键的 `Set` 视图。                      |
| `Collection<V> values()`                                     | 返回一个包含映射中所有值的 `Collection` 视图。               |
| `Set<Map.Entry<K, V>> entrySet()`                            | 返回一个包含映射中所有键值对的 `Set` 视图。                  |
|                                                              |                                                              |
| `boolean equals(Object o)`                                   | 比较指定对象与此映射是否相等。                               |
| `boolean isEmpty()`                                          | 如果映射为空，则返回 `true`。                                |
| `int size()`                                                 | 返回键-值映射的数量。                                        |
| `int hashCode()`                                             | 返回此映射的哈希码值。                                       |
|                                                              |                                                              |
| `default void forEach(BiConsumer<? super K, ? super V> action)` | 对映射中的每个键值对应用指定的操作。                         |
|                                                              |                                                              |
| 这些方法不太常用，但是表达能力很强。                         |                                                              |
| `default V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)` | 如果 key 与一个非 null 值 V 关联，将函数应用到 V 和 value，将 key 与 value 关联，或者结果为 null，则删除这个 key。返回 get(key)。 |
| `default V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)` | 将函数应用到 key 和 get(key)。将 key 与结果关联，或者如果结果为 null，则删除这个键。返回 get(key)。 |
| `default V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)` | 如果 key 与一个非 null 值 V 关联，将函数应用到 key 和 V，将 key 与结果关联，或者如果结果为 null，则删除这个键。返回 get(key)。 |
| `default V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)` | 将函数应用到 key，除非 key 与一个非 null 值关联。将 key 与结果关联，或者如果结果为 null，则删除这个键。返回 get(key)。 |
| `default void replaceAll(BiFunction<? super K, ? super V, ? extends V> function)` | 在所有映射项上应用函数。将键与非 null 结果关联，对于 null 结果，则将相应的键删除。 |
|                                                              |                                                              |
| `static <K, V> Map<K, V> of()`                               | 返回一个包含零个映射的不可修改的空 `Map`。                   |
| `static <K, V> Map<K, V> of(K k1, V v1)`                     | 返回一个包含单个映射的不可修改的 `Map`。                     |
| `static <K, V> Map<K, V> ofEntries(Entry<? extends K, ? extends V>... entries)` | 返回一个包含指定条目的不可修改的 `Map`。                     |
| `static <K, V> Map<K, V> copyOf(Map<? extends K, ? extends V> map)` | 返回一个包含给定映射条目的不可修改的 `Map`。                 |

##### 构造函数

`java.util.HashMap<K, V>`

- **`HashMap()`**
- **`HashMap(int initialCapacity)`**
- **`HashMap(int initialCapacity, float loadFactor)`**
  用给定的容量和负载因子构造一个空的散列表（负载因子是一个0.0到1.0之间的数值。这个负载因子是散列表在其容量自动增加之前可以达到多满的一种度量。一旦达到这个比例，就会将散列表的容量翻倍。默认的负载因子是0.75。

`java.util.TreeMap<K, V>`

- **`TreeMap()`**
  构造一个按照其键的自然顺序进行排序的新的空映射。

- **`TreeMap(Comparator<? super K> c)`**
  构造一个空映射，其使用指定比较器对其键进行排序。

- **`TreeMap(Map<? extends K, ? extends V> entries)`**
  构造一个空映射，按照指定映射中的所有键值对进行初始化。

- **`TreeMap(SortedMap<? extends K, ? extends V> entries)`**
  构造一个空映射，按照指定有序映射中的所有键值对进行初始化，维持原有的键的排序顺序。



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

在 Java Collection Framework 中，不认为 Map是一个集合。也就是 Map 不是 `List<(key, value)>`。所以有下列 API 的转换

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



### **The SortedSet Interface**

##### Standard Constructors

注意：

`TreeSet`的标准构造函数在处理继承自`SortedSet`的集合时，因默认使用自然排序而忽略原有比较器，导致可能需要选择性使用不同构造函数以保持排序标准的一致性。

## 实现

[Overview- 对 集合框架 的实现架构做了概述](https://docs.oracle.com/javase/tutorial/collections/implementations/index.html#:~:text=Lesson%3A-,Implementations,-%E6%95%99%E8%AE%AD%3A%E5%AE%9E%E7%8E%B0)

- **General-purpose implementations** 

  通用实现，非同步（线程安全）的，原则是：一般来说，不让用户为他们不使用的功能付费是良好的API设计实践。这句话意思是不应该强制所有使用集合的场景都承担同步的开销，特别是在许多情况下同步是不必要的。

- **Concurrent implementations**：这些实现是`java.util.concurrent`包的一部分

- **Wrapper implementations** ：包装器实现与其他类型的实现(通常是通用的实现)结合使用，以提供添加的或受限的功能。 例如，如果需要线程安全的集合，那么在包装器实现一节中描述的同步包装器允许将任何集合转换为同步集合。

- **Convenience implementations**

  便利实现是小型实现，通常通过静态工厂方法提供，它为特殊集合(例如，单例集)提供方便、高效的通用实现替代方案



对于一个接口的具体实现，一般会有三类实现，通用实现，特殊实现（特定场景），并发实现。但是无锁数据结构似乎没有。



#### Set 的实现

- 是否考虑顺序？`TreeSet`和`LinkedHashSet`
- 特定的实现，[`EnumSet`](https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html) and [`CopyOnWriteArraySet`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArraySet.html).

`CopyOnWriteArraySet` :  读多写少，写时复制（Copy-on-Write），无锁且线程安全。迭代读取快照。



#### List 的实现

`ArrayList` 和 `LinkedList` ，具体使用哪种需要考虑因素：1. 具体的添加删除查询的操作，2. 元素的位置。

一般来说，如果你经常在List的开头添加元素，或者迭代List以从内部删除元素，你应该考虑使用LinkedList。这些操作在LinkedList中需要常量时间，在ArrayList中需要线性时间。位置访问在LinkedList中需要线性时间，在ArrayList中需要常量时间。

[`CopyOnWriteArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArrayList.html) ： copy-on-write 的实现。



Arrays.toList() 实现，非常特殊，需要注意。



#### Map 实现

- 考虑顺序？顺序类型：插入顺序，访问顺序，键值顺序。

特殊实现： [`EnumMap`](https://docs.oracle.com/javase/8/docs/api/java/util/EnumMap.html), [`WeakHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/WeakHashMap.html) and [`IdentityHashMap`](https://docs.oracle.com/javase/8/docs/api/java/util/IdentityHashMap.html). 

WeakHashMap ，IdentityHashMap 在特定场景用到的数据结构。

并发实现：ConcurrentHashMap

#### Queue 的实现

- LinkedList实现了Queue接口，为添加、轮询等提供先进先出(FIFO)队列操作。
- PriorityQueue类是基于堆数据结构的优先级队列。

#### Wrapper的实现

包装器实现将其所有实际工作委托给指定的集合，但在该集合提供的功能之上添加额外的功能。这是decorator设计模式的示例。

这些实现是匿名的;该库不是提供一个公共类，而是提供一个静态工厂方法。所有这些实现都可以在Collections类中找到，该类仅由静态方法组成。

对六大接口包装，[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html), [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html), [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html), [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html), [`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html), and [`SortedMap`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html) 

- 同步包装

  ```java
  public static <T> Collection<T> synchronizedCollection(Collection<T> c);
  ....
  public static <T> Xxx<T> synchronizedXxx(Xxx<T> c);  
  
  // Xxx = (Collection、Set、List、Map、SortedSet和SortedMap)
  ```

- Unmodifiable Wrappers ： 不可修改

  ```java
  public static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c);
  ....
  public static <T> Xxx<T> unmodifiableXxx(Xxx<? extends T> s);
  // Xxx = (Collection、Set、List、Map、SortedSet和SortedMap)
  ```

#### Convenience Implementations

##### 1. `Arrays.asList()`

这是数组向Collection转换的api。返回的集合不能修改。

- 固定大小

  ```java
  List<String> list = Arrays.asList(new String[size]);
  ```

- 不固定

  ```java
  List<String> list = new ArrayList<>(arrays.asList(new String[size]));
  ```

##### 2. Collections.nCopies

返回**不可变**集合列表

```java
public static <T> List<T> nCopies(int n, T o)
```

idiom：初始包含1,000个空元素的ArrayList。

```
List<Type> list = new ArrayList<Type>(Collections.nCopies(1000, (Type)null));
```



##### 3. Collections.singleton(e)

```java
 public static <T> Set<T> singleton(T o) {/**/}
```



idiom：从集合中删除所有出现的指定元素。

```java
c.removeAll(Collections.singleton(e));
```



##### 4. emptyXxx

[`emptySet`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptySet--), [`emptyList`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptyList--), and [`emptyMap`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#emptyMap--).

返回的是不可变集合。



## Algorithms

Collections 类提供静态工具方法，其第一个参数是要对其执行操作的集合。

- sorting

  - 归并排序，任何情况下都保持nlogn的复杂度，且稳定。

- Shuffling

  每个元素相同概率

- List ：（reverse，fill、copy，swap  ） ，Collection（addAll）

- Searching

  binarySearch，注意没查到的返回值。

  [二分查找的返回值](https://docs.oracle.com/javase/tutorial/collections/algorithms/index.html#searching)

- Composition（构成）

  - frequency
  - disjoint

- 极值：max/min

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



