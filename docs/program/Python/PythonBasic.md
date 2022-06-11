

# Python基础知识

## 1. 数据类型

Python 中的变量赋值不需要类型声明。 

Python有五个标准的数据类型：Numbers（数字）、String（字符串）、List（列表）、Tuple（元组）、Dictionary（字典）

多个变量赋值

```python
a, b, c = 1, 2, "john"
```

截取字符串

```python
s = 'abcdef'
s[1:5] #读取左闭右开区间的内容 bcde
```

列表是 Python 中使用最频繁的数据类型，使用[]标识

```python
list = [ 'runoob', 786 , 2.23, 'john', 12]
list[0] #第一个元素，追加使用append,可以插入元素，使用len()获取元素个数
```

元组用 **()** 标识。内部元素用逗号隔开。但是元组不能二次赋值，相当于只读列表 

 如果要定义一个空的tuple，可以写成`()`

 但是，要定义一个只有1个元素的tuple，如果你这么定义： 

```python
t = (1)
t
1
```

定义的不是tuple，是`1`这个数！这是因为括号`()`既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是`1`。

所以，只有1个元素的tuple定义时必须加一个逗号`,`，来消除歧义

```python
tuple = ('runoob', 786 , 2.23, 'john', 12)
```

字典(dictionary)是除列表以外python之中最灵活的内置数据结构类型。列表是有序的对象集合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。字典用"{ }"标识。字典由索引(key)和它对应的值value组成。

```python
dict = {'name': 'runoob','code':6734, 'dept': 'sales'}
# print dict['one'] 
```

数据类型转换

```
str(x)、tuple(s)
```

## 2. 运算符

逻辑运算符：

| 运算符 | 逻辑表达式 | 描述                                                         | 实例                    |
| :----- | :--------- | :----------------------------------------------------------- | :---------------------- |
| and    | x and y    | 布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。 | (a and b) 返回 20。     |
| or     | x or y     | 布尔"或" - 如果 x 是非 0，它返回 x 的计算值，否则它返回 y 的计算值。 | (a or b) 返回 10。      |
| not    | not x      | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not(a and b) 返回 False |

成员运算符：

| 运算符 | 描述                                                    | 实例                                              |
| :----- | :------------------------------------------------------ | :------------------------------------------------ |
| in     | 如果在指定的序列中找到值返回 True，否则返回 False。     | x 在 y 序列中 , 如果 x 在 y 序列中返回 True。     |
| not in | 如果在指定的序列中没有找到值返回 True，否则返回 False。 | x 不在 y 序列中 , 如果 x 不在 y 序列中返回 True。 |

身份运算符

| 运算符 | 描述                                        | 实例                                                         |
| :----- | :------------------------------------------ | :----------------------------------------------------------- |
| is     | is 是判断两个标识符是不是引用自一个对象     | **x is y**, 类似 **id(x) == id(y)** , 如果引用的是同一个对象则返回 True，否则返回 False |
| is not | is not 是判断两个标识符是不是引用自不同对象 | **x is not y** ， 类似 **id(a) != id(b)**。如果引用的不是同一个对象则返回结果 True，否则返回 False。 |

循环遍历

```python
fruits = ['banana', 'apple',  'mango']
for fruit in fruits:
    print('当前水果 :', fruit)
```

## 3. 函数

函数代码块以 **def** 关键词开头，后接函数标识符名称和圆括号**()**。

## 4. 面向对象

面向对象案例：

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def printPerson(self):
        print(self.name, self.age)

p = Person("zhangsan", 78)
p.printPerson()
```

Python的内置类属性

> `__dict__` : 类的属性（包含一个字典，由类的数据属性组成）
>
> `__doc__` :类的文档字符串
>
> `__name__`: 类名
>
> `__module__`: 类定义所在的模块（类的全名是`__main__`.className，如果类位于一个导入模块mymod中，那么className.`__module__` 等于 mymod）
>
> `__bases__` : 类的所有父类构成元素（包含了一个由所有父类组成的元组）

Python中`__`和`_`的含义

`__`name`__`的理解

