## 迭代器

迭代器设计模式的实现，

`Iterable`

```java
public interface Iterable<T> { 
    Iterator<T> iterator();
}
```

约定：实现了 Iterable 接口就可以使用迭代器遍历。也可以使用foreach遍历。

`Iterable`

```
```



