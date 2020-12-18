[toc]

# 生成器（generator)的用法

## 生成器的理解
- 生成器是一类特殊的迭代器
- 通俗的说，只要函数中有yield关键字，就称为是生成器，而该函数不在是函数

## 创建生成器的方法
### 方式一：将列表改为元组，即[ ] 改为（）

```python
a = [i for i in range(5)]
print(a, type(a))

b = (i for i in range(5))
print(b, type(b))

# output:
[0, 1, 2, 3, 4] <class 'list'>
<generator object <genexpr> at 0x7fd718146a40> <class 'generator'>
```


### 方式二：使用yield关键字

- yield 的作用
    - 保存当前运行状态（断点），然后暂停执行，即将生成器（函数）挂起
    - 将yield关键字后面表达式的值作为返回值返回，此时可以理解为起到了return的作用
    - 用for循环调用generator时，拿不到generator的return语句的返回值。如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中：


```python
def fib(n):
    current = 0
    num1, num2 = 0, 1
    while current < n:
        num = num1
        num1, num2 = num2, num1+num2
        current += 1
        yield num
    return 'done'

f = fib(5)
while True:
    try:
        ret = next(f)
        print(ret,end=' ')
    except StopIteration as e:
        print(e.value)
        break
```



## 唤醒生成器的方式
### next()方式
- next()函数让生成器从断点处继续执行
>Python3中的生成器可以使用return返回最终运行的返回值，而Python2中的生成器不允许使用return返回一个返回值（即可以使用return从生成器中退出，但return后不能有任何表达式）。

### send()方式
- send()函数的唤醒的一个好处是可以在唤醒的同时向断点处传入一个附加数据。

```python
def create_num(all_num):
    a, b = 0, 1
    current_num = 0
    while current_num < all_num:
        ret = yield a
        print(">>>ret>>>>", ret)
        a, b = b, a+b
        current_num += 1

obj = create_num(10)

# obj.send(None)  # send一般不会放到第一次启动生成器，如果非要这样做 那么传递None
ret = next(obj)
print(ret)
# send的结果是下一次调用yield时 yield后面的值
ret = obj.send("dddddd")
print(ret)

```
