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







#### 接口签名

Collections



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







