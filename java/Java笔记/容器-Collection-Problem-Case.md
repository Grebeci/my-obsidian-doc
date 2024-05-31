Collection 接口有方法：

```java
boolean add(E e);
boolean remove(Object o);
```


1. 为什么这两个方法泛型不同？

answer：

> 不确定答案

- add 添加要检查类型，泛型的类型安全特性，remove 默认调用 equals 方法来确定移除元素，所以和类型无关。

2. 在 java Collection API中，List 和 SortedSet接口都有 `Sublist` 这部分 API，建议慎重使用，原因

   - List 的 Sublist 和 SortedSet 的 Sublist的行为不一致

   参考：

   [List - sublist](https://docs.oracle.com/javase/tutorial/collections/interfaces/list.html#:~:text=Although%20the%20subList,though%20not%20concurrently))

   [SortedSet - sublist](https://docs.oracle.com/javase/tutorial/collections/interfaces/sorted-set.html#:~:text=%E5%85%B6%E8%BF%9B%E8%A1%8C%E6%8E%92%E5%BA%8F%E3%80%82-,Range%2Dview%20Operations,-Range%2Dview%E6%93%8D%E4%BD%9C)

   