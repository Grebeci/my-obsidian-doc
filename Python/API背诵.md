## 大原则

##### 1. 缩进

- 用缩进代替 `{}`， 且不必要的缩进是语法错误。

##### 2. 动态

- 类没有类似java的属性字段，可以在运行时动态的添加和修改属性。为了让类的实例在任何时候都具有完整的属性集，建议在 `__init__` 指定所有属性。







###### 字符串

| 方法                             | 描述       |
| -------------------------------- | ---------- |
| `Title()`                        | 首字母大写 |
| +                                | 字符串拼接 |
| `lstrip()`  `rstrip()` `strip()` | 去除空格   |
| upper lower                      | 大小写     |
|                                  |            |



字符串拼接

- `+` : 拼接字符串
- 

字面量表示

单引号，或者双引号，三引号





###### 整数运算

python3 两个整数除法，结果是浮点数。



## 数据结构

python3 内置的原生的数据结构

#### list

- 随机访问，索引从0开始，索引为负数表示从后往前访问。
- list 可变的容器。

##### operator

assignment

```python
fruits = ['apple', 'orange', 'banana']
print(fruits)  # ['apple','orange','banana']


```



access

```python
print(fruits[0])
print(fruits[-1])

fruits[0] = 'peach'
fruits[-1] = 'nut'
```



Add Element

```python
# 末尾追加
fruits.append("peanut")

# 添加元素到指定索引位置
fruits.insert(0, "tomato")
```



Delete Element

```python
# 删除索引位置的元素
del fruits[0]

#从末尾弹出元素
fruit = fruits.pop()
# 指定索引
fruits.pop(0)

# by value
fruits.remove("nut")
```



length

```
len(fruits)
```



Sort

```python
# 改变list元素的顺序，有副作用
fruits.sort()

# 没有副作用的
fruits_sorted = sorted(fruits)

```



reverse

```python
fruits.reverse()
```



traverse

for loop

```
for fruit in fruits:
    print(fruit)
```





build-in functions



Generate list

`range(start,stop,step)`  ： 全部参数。

`range(stop)` : 生成 [0, stop-1] 的序列。

`range(start,stop)` : 指定start，默认 step = 1 



```
for value in range(1, 5):
    print(value)
```



处理数字列表的函数

min, max, sum



##### List Comprehension

python的语法糖。凡是符合 遍历list，处理元素，生成新list，都可以用这个语法。

```python
squares = [i ** 2 for i in range(10)]
```



##### Slice

python的语法糖。可以方便的得到一个list的副本。

```python
fruits[0:3]
# 省略start，默认开始
fruits[:4] 
#省略stop，默认末尾
fruits[2:]
#都缺省，0-length-1
fruits[:]

fruits[-3:]
```





#### tuple

- 元组是不可变的容器。
- 支持切片操作



```
dimensions = (200, 50, 10)
```



#### dict

key-value 键值对。无序的哈希表。

- 描述一个Entry的属性信息。
- 

Assignment

```
person = {'name': 'alice', 'age': 18}
empty = {}
```

access

```
person['name']
```



add 

```
person['job'] = 'programmer'
```



delete 

```python
del person['name']
```



###### traverse



for-loop

```python
for key, value in person.items():
    print(key + ":" + value)
    
for key in person.keys():
    print(key)
    
for value in person.values():
    print(value)
```







## 分支判断

###### 布尔表达式

- 表达式的值是 `True` 或者 `False`。
- 逻辑操作符是  and， or
- 相等运算符 `=` `!=` `in` `not in` 



```python
if condition_test:
    do_something 
```



```python
if condition_test:
    do_something
else:
	do_something
```



```python
if condition_test:
    do_something
elif condition_test:
	do_something
else:
	do_something
```



- 条件表达式不需要用 `()`



## IO

interactive IO

```
input(__prompt)
```



## Loop

for-loop

while 循环

```python
current_number = 1
while current_number <= 5:
    print(current_number)
    current_number =+ 1
```

continue

break



## Function

带名字的代码块

###### 定义

```python
def greet_user():
    """docstring"""
    print("hello")
```



###### 传参

- 位置实参

- 关键字实参

- 默认值：形参可以有默认值。

- 可变参数

	`*args` : 实参任意多个，被封装成一个元组传递给 `args`

	`*kwargs` 实参是任意个关键字实参，被封装为一个 dict 传递给 kwargs

```python
def fun(*args, **kwargs):
    print(args)
    print(kwargs)
```





## 面向对象

Class 



```python
class Dog:
    """A simple attempt to model a dog."""

    def __int__(self, name, age):
        self.name = name
        self.age = age

    def sit(self):
        print(self.name.title() + " is now sitting.")

    def roll_over(self):
        print(self.name.title() + " rolled over!")
```



- `__init__` : 构造器，实例化对象自动调用。
- 每个方法形参都有 `self` , 实参不朽要传递他，self指向实例本身的引用。



创建类的实例

```
my_dog = Dog('willie', 6)

# access attributes
my_dog.name

# modify attributes
my_dog.age = 10
```



- 构造出来的每个实例的属性都必须有默认值，可以在 `__init__` 指定，也可以在构造实例指定。



继承

```python
class Base:
    def __init__(self):
        self.x = 1

class Child(Base):
    def __init__(self):
        super().__init__()
        self.y = 2

```

- 子类 `__init__` 显示调用 super，获得父类的属性。
