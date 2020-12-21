[toc]

# reduce()

## 概要：reduce函数会对参数序列中元素进行累积。


## 语法

```python
reduce(function, iterable[, initial])
```
- function: 函数名，有两个参数
- iterable -- 可迭代对象
- initial -- 可选，初始参数

## 含义

- 函数将一个数据集合（链表，元组等）中的所有数据进行下列操作：用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作，得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。  
- 第一次调用function时，如果提供initial参数，会以iterable中的第一个元素和initial作为参数调用function，否则会以序列iterable中的前两个元素做参数调用function。


## 使用demo

> from functool import reduce  # 导入


```python
In [17]: from functools import reduce

In [18]: a = [3,1,4,1,5,9]

In [19]: ret = reduce(lambda x,y:x+y, a)

In [20]: print(ret)
23

In [21]: ret1 = reduce(lambda x,y:x+y, a , 11)

In [22]: print(ret1)
34

In [23]: ret2 = reduce(lambda x,y:x*y, a, 5)

In [24]: print(ret2)
2700

In [25]: ret3 = reduce(lambda x,y:x*y, a)

In [26]: print(ret3)
540
```

![image](http://note.youdao.com/yws/res/232845/EF8B69A1165841D2844C60737A9E4D3F)