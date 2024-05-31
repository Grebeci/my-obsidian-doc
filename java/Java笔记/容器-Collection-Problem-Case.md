Collection 接口有方法：

```java
boolean add(E e);
boolean remove(Object o);
```


1. 为什么这两个方法泛型不同？

answer：

> 不确定答案

- add 添加要检查类型，泛型的类型安全特性，remove 默认调用 equals 方法来确定移除元素，所以和类型无关。

2. 在 java Collection API中，List 和 SortedSet接口都有 `Sublist` 这部分 API，