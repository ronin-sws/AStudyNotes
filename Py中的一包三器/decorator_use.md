[toc]

# 装饰器decorator：

## 对装饰器的理解：
- 装饰器目的: 对已有函数进行额外的功能扩展，
- 装饰器本质上一个闭包函数，也就是说他也是一个函数嵌套

- 装饰器的特点:
    1. 不修改已有函数的源代码
    2. 不修改已有函数的调用方式
    3. 给以后函数添加额外的功能

## 装饰器的应用场景：
- 引入日志
- 函数执行时间统计
- 执行函数前预备处理
- 执行函数后清理功能
- 权限校验等场景
- 缓存

## 装饰器的使用：
```python
# 定义装饰器
def decorator(func):  # 如果闭包函数的参数有且只有一个并且是函数类型，那么这个闭包函数称为装饰器
    print("装饰器执行了")
    def inner():
        # 在内部函数里面对已有函数进行装饰
        print("已添加登陆验证")
        func()
    return inner


# 装饰器的语法糖写法: @装饰器名称，装饰器的语法糖就是在装饰以后函数的时候写法更加简单
@decorator  # comment = decorator(comment) 装饰器语法糖对该代码进行了封装  comment=inner
def comment():
    print("发表评论")

# 已添加登陆验证
# 发表评论

# # 调用装饰器对已有函数进行装饰  ， comment=inner
# comment = decorator(comment)

# 调用方式不变
comment()

# 装饰器的执行时机： 当当前模块加载完成以后，装饰器会立即执行，对已有函数进行装饰
```

## 装饰器的类型
### 1.装饰无参数的函数
```python
from time import ctime, sleep

def timefun(func):
    def wrapped_func():
        print("%s called at %s" % (func.__name__, ctime()))
        func()
    return wrapped_func

@timefun
def foo():
    print("I am foo")

foo()
sleep(2)
foo()


# 上面代码理解装饰器执行行为可理解成

foo = timefun(foo)
# foo先作为参数赋值给func后,foo接收指向timefun返回的wrapped_func
foo()
# 调用foo(),即等价调用wrapped_func()
# 内部函数wrapped_func被引用，所以外部函数的func变量(自由变量)并没有释放
# func里保存的是原foo函数对象
```

### 2.装饰带有参数的装饰器
```python
# 定义装饰器
def decorator(func):
    # 使用装饰器装饰已有函数的时候，内部函数的类型和要装饰的已有函数的类型保持一致
    def inner(a, b):
        # 在内部函数对已有函数进行装饰
        print("正在努力执行加法计算")
        func(a, b)

    return inner


# 用装饰器语法糖方式装饰带有参数的函数
@decorator  #  add_num= decorator(add_num), add_num=inner
def add_num(num1, num2):
    result = num1 + num2
    print("结果为:", result)

add_num(1, 2)
```

### 3.装饰不定长参数函数 （可视为通用装饰器）
```python
def set_func(func):
	print("---开始进行装饰")
	def call_func(*args, **kwargs):
		print("---这是权限验证1----")
		print("---这是权限验证2----")
		# func(args, kwargs)  # 不行，相当于传递了2个参数 ：1个元组，1个字典
		func(*args, **kwargs)  # 拆包
	return call_func


@set_func  # 相当于 test1 = set_func(test1)
def test1(num, *args, **kwargs):
	print("-----test1----%d" % num)
	print("-----test1----" , args)
	print("-----test1----" , kwargs)

test1(100)
test1(100, 200)
test1(100, 200, 300, mm=100)

```

```python
# 该装饰器还可以成为是通用的装饰器
def decorator(func):
    # 使用装饰器装饰已有函数的时候，内部函数的类型和要装饰的已有函数的类型保持一致
    def inner(*args, **kwargs):
        # 在内部函数对已有函数进行装饰
        print("正在努力执行加法计算")

        # *args: 把元组里面的每一个元素，按照位置参数的方式进行传参
        # **kwargs: 把字典里面的每一个键值对，按照关键字的方式进行传参
        # 这里对元组和字典进行拆包，仅限于结合不定长参数的函数使用
        num = func(*args, **kwargs)
        return num

    return inner


@decorator  # show= decorator(show) show = inner
def show():
    return "哈哈"

result = show()
print(result)


# 用装饰器语法糖方式装饰带有参数的函数
@decorator  #  add_num= decorator(add_num), add_num=inner
def add_num(*args, **kwargs):
    result = 0

    # args: 元组类型
    # kwargs: 字典类型

    for value in args:
        result += value

    for value in kwargs.values():
        result += value

    return result

result = add_num(1, 2)
print("结果为:", result)


if __name__ == '__main__':
    my = {"a":1}
    print(**my)

```


### 4.装饰有返回值函数
```python
from time import ctime, sleep

def timefun(func):
    def wrapped_func():
        print("%s called at %s" % (func.__name__, ctime()))
        func()
    return wrapped_func

@timefun
def foo():
    print("I am foo")

@timefun
def getInfo():
    return '----hahah---'

foo()
sleep(2)
foo()


print(getInfo())

foo called at Fri Nov  4 21:55:35 2016
I am foo
foo called at Fri Nov  4 21:55:37 2016
I am foo
getInfo called at Fri Nov  4 21:55:37 2016
None
```

### 5.多个装饰器对同一个函数进行装饰
```
def add_qx(func):
	print("---开始进行装饰权限1的功能---")
	def call_func(*args, **kwargs):
		print("---这是权限验证1----")
		return func(*args, **kwargs)
	return call_func


def add_xx(func):
	print("---开始进行装饰xxx的功能---")
	def call_func(*args, **kwargs):
		print("---这是xxx的功能----")
		return func(*args, **kwargs)
	return call_func


@add_qx
@add_xx
def test1():
	print("------test1------")


test1()

```
### 6.同一个装饰器对多个函数进行装饰
```
def set_func(func):
	def call_func(a):
		print("---这是权限验证1----")
		print("---这是权限验证2----")
		func(a)
	return call_func


@set_func  # 相当于 test1 = set_func(test1)
def test1(num):
	print("-----test1----%d" % num)


@set_func  # 相当于 test2 = set_func(test2)
def test2(num):
	print("-----test2----%d" % num)


test1(100)
test2(200)

装饰器在调用函数之前，已经被python解释器执行了，所以要牢记当调用函数之前 其实已经装饰好了，尽管调用就可以了
```


### 7.多个装饰器的使用

```
def make_div(func):
    print("make_div装饰器执行了")

    def inner():
        # 在内部函数对已有函数进行装饰
        result = "<div>" + func() + "</div>"
        return result

    return inner


# 定义装饰器
def make_p(func):
    print("make_p装饰器执行了")

    def inner():
        # 在内部函数对已有函数进行装饰
        result = "<p>" + func() + "</p>"
        return result

    return inner

# 多个装饰器的装饰过程: 由内到外的一个装饰过程，先执行内部的装饰器，在执行外部的装饰器
# 原理剖析: content = make_div(make_p(content))
# 分步拆解: content = make_p(content),内部装饰器装完成content=make_p.inner
#  content = make_div(make_p.inner)
@make_div
@make_p
def content():
    return "人生苦短，我用python!"

# <p>人生苦短，我用python!</p>
result = content()
print(result)


```


### 8.类装饰器

```
# # 类装饰器: 使用类装饰已有函数
class MyDecorator(object):
    def __init__(self, func):
        self.__func = func

    # 实现__call__这个方法，让对象变成可调用的对象，可调用的对象能够像函数使用
    def __call__(self, *args, **kwargs):
        # 对已有函数进行封装
        print("课已讲完")
        self.__func()


@MyDecorator  # @MyDecorator => show = MyDecorator(show)
def show():
    print("快要下学啦")

# 执行show  # 执行MyDecorator类创建实例对象 -> show() => 对象()
show()



# class AAA(object):
#     pass
#
# a = AAA()
# a()

# 扩展： 函数之所有能够调用是因为函数内部使用__call__
def mytest():
    print("哈哈")

print(dir(mytest))
```

```
class Test(object):
    def __init__(self, func):
        print("---初始化---")
        print("func name is %s"%func.__name__)
        self.__func = func
    def __call__(self):
        print("---装饰器中的功能---")
        self.__func()
#说明：
#1. 当用Test来装作装饰器对test函数进行装饰的时候，首先会创建Test的实例对象
#   并且会把test这个函数名当做参数传递到__init__方法中
#   即在__init__方法中的属性__func指向了test指向的函数
#
#2. test指向了用Test创建出来的实例对象
#
#3. 当在使用test()进行调用时，就相当于让这个对象()，因此会调用这个对象的__call__方法
#
#4. 为了能够在__call__方法中调用原来test指向的函数体，所以在__init__方法中就需要一个实例属性来保存这个函数体的引用
#   所以才有了self.__func = func这句代码，从而在调用__call__方法中能够调用到test之前的函数体
@Test
def test():
    print("----test---")
test()
showpy()#如果把这句话注释，重新运行程序，依然会看到"--初始化--"


# 运行结果如下：
---初始化---
func name is test
---装饰器中的功能---
----test---

```

