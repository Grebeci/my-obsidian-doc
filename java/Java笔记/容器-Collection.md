参考自：

[Trail: Collections (The Java™ Tutorials)](https://docs.oracle.com/javase/tutorial/collections/index.html)

进度：


-----



原则：

- Don't use a library until you understand its API.

  





使用集合接口的基本原则：

- 是否重复
- 是否有序



#### 继承关系图

![Two interface trees, one starting with Collection and including Set, SortedSet, List, and Queue, and the other starting with Map and including SortedMap.](https://docs.oracle.com/javase/tutorial/figures/collections/colls-coreInterfaces.gif)

#### 接口签名



Collection Interface.   

```java
public interface Collection<E> extends Iterable<E> 
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







