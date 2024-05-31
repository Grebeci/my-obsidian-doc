Collection 接口有方法：

```java
boolean add(E e);
boolean remove(Object o);
```

为什么这两个方法泛型不同？

> 不确定

- add 添加要检查类型，泛型的类型安全特性，remove 默认调用 equals 方法来确定移除元素，所以和类型无关。


Collection 的 API 