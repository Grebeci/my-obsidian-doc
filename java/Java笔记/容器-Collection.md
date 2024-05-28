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



#### 接口

**The Collection Interface**

默认约定：所有通用集合实现都有一个接受 `Collection` 参数的构造函数。用来转换集合类型。例如：ArrayList 有构造函数

```java
public ArrayList(Collection<? extends E> c) { /***/ }
```

**接口签名**

```java
public interface Collection<E> extends Iterable<E>
```

**API**

| Function Signature                              | Function Description                                  |
| ----------------------------------------------- | ----------------------------------------------------- |
| `int size()`                                    | Number of items                                       |
| `boolean isEmpty()`                             | is the collection empty?                              |
| `boolean contains(Object o)`                    | does the list contain the given item?                 |
| `Iterator<E> iterator()`                        | iterator over all items in the collection             |
| `Object[] toArray()`                            | Converts to an array                                  |
| `<T> T[] toArray(T[] a)`                        | Converts to an array of specified type                |
| `<T> T[] toArray(IntFunction<T[]> generator)`   | Converts to an array using a generator                |
| `boolean add(E e)`                              | Adds an element                                       |
| `boolean remove(Object o)`                      | Removes an element                                    |
| `boolean containsAll(Collection<?> c)`          | Checks if contains all elements of another collection |
| `boolean addAll(Collection<? extends E> c)`     | Adds all elements from another collection             |
| `boolean removeAll(Collection<?> c)`            | Removes all elements present in another collection    |
| `boolean removeIf(Predicate<? super E> filter)` | Removes elements matching a predicate                 |
| `boolean retainAll(Collection<?> c)`            | Retains only elements present in another collection   |
| `void clear()`                                  | Removes all elements                                  |
| `boolean equals(Object o)`                      | Compares for equality                                 |
| `int hashCode()`                                | Returns the hash code value                           |
| `Spliterator<E> spliterator()`                  | Creates a Spliterator                                 |
| `Stream<E> stream()`                            | Returns a sequential Stream                           |
| `Stream<E> parallelStream()`                    | Returns a possibly parallel Stream                    |



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

















List interface. java.util.List is API for an sequence of items

```java
public interface List<Item> implements Iterable<Item>
```

|              |                 |
| ------------ | --------------- |
| `int size()` | number of items |
|              |                 |
|              |                 |
|              |                 |
|              |                 |
|              |                 |
|              |                 |
|              |                 |
|              |                 |





<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js" async></script>
<script type="math/tex">
\begin{array}{|c|c|p{8cm}|}
\hline
\textbf{Method} & \textbf{Return Type} & \textbf{Description} \\
\hline
\texttt{Collection()} & & Initialize a new collection \\
\texttt{isEmpty()} & \texttt{boolean} & Check if the collection is empty \\
...
\hline
\end{array}
</script>







