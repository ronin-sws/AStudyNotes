### [toc] 

# collections模块

## 写在前面

- collections模块是在python内置数据类型（list, dict, set, tuple）的基础上，提供了性能更高的一些容器集，即额外的几种数据类型：
    
    - namedtuple(): 生成可以使用名字来访问元素内容的tuple子类
    - defaultdict(): 带有默认值的字典
    - deque(): 双端队列，可以快速的从队列的两端追加和推出元素
    - Counter(): 计数器，主要用来计数
    - OrderedDict(): 有序字典
    - ChainMap(): 用来合并两个或多个字典，当查询时，按顺序方式查询

![image](http://note.youdao.com/yws/res/236114/1828D636B0DC49B3A54DC7AA74022866)


## 常用方法：
- tuple():
> 为后面的namedtuple做铺垫

```
tuple(iterable)  # 将可迭代序列转化为元组
```

```python
In [6]: tuple([1,2,3,4])
Out[6]: (1, 2, 3, 4)

In [7]: tuple({"a":1, "b":2})
Out[7]: ('a', 'b')

In [8]: tuple((1,2,3,4))
Out[8]: (1, 2, 3, 4)
```

![image](http://note.youdao.com/yws/res/236138/94149FC8FC3041E8B55039A559BA0A9F)


![image](http://note.youdao.com/yws/res/236142/D2A3386DA33247F4B52894A1CAC06F81)


### namedtuple()

- namedtuple主要用来产生可以使用名称来访问元素的数据对象，在访问一些tuple类型的数据效果更明显


```python
from collections import namedtuple

# 面对对象的方式
class User:
    def __init__(self, name, age, height):
        pass

# 新的方式（在创建简单的对象时，比面对对象的方式更节省空间）
# 案例一：传元组
User = namedtuple("User", ["name", "age","like"])
tmptup = ('wss',25)
user = User(*tmptup, 'readbook')
print(user)
print("*"*15)
User = namedtuple("User", ("name", "age"))
# 元组
tmptuple = ('wss',25)
user = User._make(tmptuple)
print(user)
print("*"*15)
# 列表
tmplist = ['wss',25]
user = User._make(tmplist)
print(user)
print("*"*15)
# 转换为字典
user_dict=user._asdict()
print(user_dict)


# 案例二
websites = [
    ('Sohu', 'http://www.google.com/', u'张朝阳'),
    ('Sina', 'http://www.sina.com.cn/', u'王志东'),
    ('163', 'http://www.163.com/', u'丁磊')
]

Website = namedtuple('Website', ['name', 'url', 'founder'])

for website in websites:
    website = Website._make(website)
    print(website)

```
- 输出结果
![image](http://note.youdao.com/yws/res/236157/257038A839BF4D86BB8BEC64E8F3D612)

---
### defaultdict()

- 在dict的基础上进行了优化，原生的dict，用d[key]的方式访问当key,不存在时，会抛出KeyError
- 使用defaultdict,只要传入一个默认的工厂方法，当请求一个不存在的key时，会自动调用工厂方法使其结果作为key的默认值，从而避免异常抛出
- 通俗的说：key存在返回对应的value，key 不存在，就返回传入的默认值

```python
from collections import defaultdict

d = defaultdict(int)  # 默认值0
str = 'wfdsfewfeswfs'
for s in str:
    d[s] += 1
print(d)
print(d['w'])
print(d['f'])
print(d['u'])  

d2 = defaultdict(list)  # 默认值[]
print(d2['z'])
d3 = defaultdict(lambda : 5) # 自定义默认值
print(d3['z'])
```
![image](http://note.youdao.com/yws/res/236178/8B9ACFC0921C4334AA6153539E0FE46D)


```
def generate_default():
    return {
        "a":1, 
        "b":3
        }
tempd = defaultdict(generate_default)
print(tempd['z'])
```
![image](http://note.youdao.com/yws/res/236180/80EC9659110E4A5AA0C6AFB2E1F7B10D)

```
members = [
    # Age, name
    ['male', 'John'],
    ['male', 'Jack'],
    ['female', 'Lily'],
    ['male', 'Pony'],
    ['female', 'Lucy'],
]

result = defaultdict(list)
for sex, name in members:
    result[sex].append(name)

print(result) 
```

![image](http://note.youdao.com/yws/res/236185/2CDADBD5B3424EAE9AF7684B3646DA93)

---
### deque()
- deque是 double-ended queue的缩写，双端队列，它最大的好处就是实现了从队列 头部快速增加和取出对象: .popleft(), .appendleft() 。
- 使用list存储数据时，按索引访问元素很快，但是插入和删除元素就很慢了，因为list是线性存储，数据量大的时候，插入和删除效率很低。
- que是为了高效实现插入和删除操作的双向列表，适合用于队列和栈：


```
from collections import deque


q = deque(['a','b','c'])
q.append('w')
q.appendleft('x')
print(q)
# output: deque(['x', 'a', 'b', 'c', 'w'])
```

```
# -*- coding: utf-8 -*-
"""
下面这个是一个有趣的例子，主要使用了deque的rotate方法来实现了一个无限循环
的加载动画
"""
import sys
import time
from collections import deque

# 案例
fancy_loading = deque('>--------------------')

while True:
    print('\r%s' % ''.join(fancy_loading))
    fancy_loading.rotate(1)  # rotate方法用于旋转，如果参数大于0，表示将队列右端的n个元素移动到左端，否则相反
    sys.stdout.flush()
    time.sleep(0.08)
```
![image](http://note.youdao.com/yws/res/236196/12866FA2B75444D3BDE34ABDBE57C211)

---
### OrderedDict()
- python2，dict是无序的
- python3中，dict和OrderedDict都是有序的，
- 使用dict时，Key是无序的。在对dict做迭代时，我们无法确定Key的顺序。如果要保持Key的顺序，可以用OrderedDict：
- OrderedDict的Key会按照插入的顺序排列，不是Key本身排序：
- OrderedDict可以实现一个FIFO（先进先出）的dict，当容量超出限制时，先删除最早添加的Key：


```
from collections import OrderedDict
d = dict([('a', 1), ('b', 2), ('c', 3)])
print(d) # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
print(od) # OrderedDict的Key是有序的


od2 = OrderedDict()
od2['z'] = 1
od2['y'] = 2
od2['x'] = 3
print(od2.keys())
od2.move_to_end('z')  # 将指定的key移动到最后面
print(od2
```

![image](http://note.youdao.com/yws/res/236222/B08912DFDBA34969BE2091E60B07B23D)

- 参考案例

```
from collections import OrderedDict

class LastUpdatedOrderedDict(OrderedDict):
    #实现一个FIFO（先进先出）的dict，当容量超出限制时，先删除最早添加的Key：
    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity

    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print 'remove:', last
        if containsKey:
            del self[key]
            print 'set:', (key, value)
        else:
            print 'add:', (key, value)
        OrderedDict.__setitem__(self, key, value)
```
---
### Counter()

- 是一个简单的计数器，可用于统计字符串、列表等的元素个数。

```python
from collections import Counter

str1 = "sdfwetsfswfwef"
c1 = Counter(str1)
print(c1)
# print(c1.most_common(3))
# print(isinstance(c1, dict))
str2 = 'dfewafsfes'
c2 = Counter(str2)
print(c2)
c = c1 + c2
print(c)
c = c1 - c2
print(c)
c = c1 & c2
print(c)
c = c1 | c2
print(c)
```

![image](http://note.youdao.com/yws/res/236229/C22EA60422F1409C900BC43EE4E8614E)

---
### ChainMap()

- 可以用来合并两个或者更多个字典，当查询的时候，从前往后依次查询。
- 注意点就是当对 ChainMap 进行修改的时候总是只会对第一个字典进行修改

>从原理上面讲，ChainMap 实际上是把放入的字典存储在一个列表中，当进行字典的增加删除等操作只会在第一个字典上进行，当进行查找的时候会依次查找，new_child()方法实质上是在列表的第一个元素前放入一个字典并返回这个类似列表的对象，默认是 {}，而 parents 是去掉了列表开头的元素。


```python
from collections import ChainMap
a = {"x":1, "z":3}
b = {"y":2, "z":4}
c = ChainMap(a,b)
print(c)
print("x: {}, y: {}, z: {}".format(c["x"], c["y"], c["z"]))
print("*"*20)
temp1 = c.maps
print(temp1, type(temp1))
print("*"*20)
temp2 = c.parents
print(temp2, type(temp2))
```

![image](http://note.youdao.com/yws/res/236240/C72E7A6F25A64ABAB1CF37715DC3A0CF)