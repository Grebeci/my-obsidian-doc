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

`Collection` :  集合层次结构的根。

`Set` ：不能包含**重复元素**的集合。该接口对数学集合抽象进行建模。

`List`：一个有序的集合(有时称为序列)。列表可以包含重复的元素。List的用户通常可以精确控制每个元素在列表中的插入位置，并且可以通过整数索引(位置)访问元素。

`Queue`：用于在处理之前保存多个元素的集合。队列必须实现 顺序 属性，排序可能是 FIFO、优先级队列。无论使用何种排序方式，队列的头部都是将被remove或poll调用删除的元素。在FIFO队列中，所有新元素都插入到队列的尾部。其他类型的队列可能使用不同的放置规则。

`Map`：一个将键映射到值的对象。Map不能包含重复的键;每个键最多只能映射到一个值。



`SortedSet`：按升序维护其元素的Set。

`SortedMap`：按升序键顺序维护其映射的Map



#### 接口

**The Collection Interface**

Collection接口用于传递需要最大通用性的对象集合。



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







