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

```java
public interface Iterator<E> {
    boolean hasNext();

    E next();
    
    // optional; 如有必要
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }

   
    default void forEachRemaining(Consumer<? super E> action) {
        Objects.requireNonNull(action);
        while (hasNext())
            action.accept(next());
    }
}
```



Client 代码

```
for (Object o : collection)
    System.out.println(o);
    
for (Object o : collection)
    System.out.println(o);
```



cha r


[
]()

