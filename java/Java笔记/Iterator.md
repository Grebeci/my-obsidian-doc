## 迭代器

迭代器设计模式的实现，

![image-20240605025840008](/Users/admin/Downloads/02_电子教材与课件/尚硅谷_第12章_集合框架/images/algo4-Week2-Iterable.png)

只有有顺序的容器实现 Iterator 才有意义，List 用索引顺序来访问，Set 会以一种随机的方式遍历元素。

##### 迭代器的位置移动

- Iterator 的remove 方法会删除上次 next 返回时的元素。也就是当前迭代器位置之前的元素。原因是删除之前先看一下这个元素。

  下面 code 删除集合中第一个元素

  ```java
  Iterator<String> it = c.iterator();
  it.next();
  it.remove();
  ```





### 特定实现

#### ListIterator



