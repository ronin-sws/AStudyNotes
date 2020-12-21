[toc]

# numpy

#### 创建数组

```python
list_1 = [1,2,3,4,5]
list_2 = [6,7,8,9,10]
a = np.array(list_1) # 创建一维数组（方法一）
b = np.arange(temp1,temp2,temp3) # 创建一维数组（方法二） temp1:起始，temp2:终止，temp3:步长
c = np.array([list_1,list_2]) # 创建二维数组
d = np.zeros(5) # 创建全0一维矩阵（数组） 数据类型是float64
e = np.zeros([2,3]) # 创建全0二维矩阵（数组） 数据类型是float64
f = np.eye(5) # 5*5的单位矩阵（数组） 数据类型是float64
np.arange(12).reshape(3, 4)

```

#### 数组的属性

```python
# 查看数组的类型
type(a)

# 查看数据的类型
a.dtype

# 查看数组的形状
a.shape

# 查看数组的元素个数
a.size
```

#### 数组的访问

```python
# 一维数组
a = np.arange(1,10) # out:array([1,2,3,4,5,6,7,8,9])
a[1] # out:2
a[1:5] # out:array([2,3,4,5])

# 二维数组
b = np.array([[1,2,3],
              [4,5,6]])  # [1,2,3]是第0行，[4，5，6]是第一行
b[1][0] # out:4  表示访问第一行第0个元素（访问方式一）
b[1, 0] # out:4 （访问方式二）

c = np.array([[1,2,3],
             [4,5,6],
             [7,8,9]])
c[:2,1:] # 表示 ：2行数，1： 列数
# out: 
array([[2,3],
       [5,6]]) 
```



#### 指定创建数组的数据类型

```python
a = np.array([1,0,1,0], dtype=np.bool)  # 或者使用dtype='?'

# 修改数组的数据类型
a.astype("i1") # 或者使用a.astype(np.int8)
```

#### 修改浮点型的小数位数

```python
np.round(b, 2)  # b是浮点型数组
```

#### 数组的形状

```python
# 查看数组的形状,得到的结果是一个元组，
a.shape  # fg:(2,4)表示2（行）*4（列）的数组
    
# 获取数组的行数
a.shape[0]
# 获取数组的列数
a.shape[1]

# 修改数组的形状
b = a.reshape(2, 6)

# 把数组转化为一维数组
a.flatten()

```

#### 数组和数组的计算

```python
# 广播机制
a + 1
a * 3
```

如果两个数组的后缘维度（即从末尾开始算起的维度）的轴长度相符或其中一方的长度为1，则认为它们是广播兼容的。广播会在缺失和（或）长度为1的维度上进行。

可以把维度指的是shape所对应的数字。

也就是说两个数组shape得到的元组的后2位要相同，或者后2位中一个相同，另外一个数字有一个数组为1

#### numpy读取数据

```python
np.loadtxt(fname,dtype=np.float,delimiter=None,skiprows=0,usecols=None,unpack=False)
# 例如：
np.loadtxt(US_video_data_numbers_path, delimiter=",", dtype=int,unpack=1)
```

| 参数      | 解释                                                         |
| --------- | ------------------------------------------------------------ |
| frame     | 文件、字符串或产生器，可以是.gz或bz2压缩文件                 |
| dtype     | 数据类型，可选，csv的字符串以什么数据类型读入数组中，默认np.float |
| delimiter | 分割字符串，默认是任何空格，改为逗号                         |
| skiprows  | 跳过前X行，一般跳过第一行表头                                |
| usecols   | 读取指定的列，索引，元组类型                                 |
| unpack    | 如果True，读入属性将分别写入不同数组变量，False读入数据只写入一个数组变量，默认False。相当于转置的效果 |

#### numpy中的转置

转置是一种变换，对于numpy中的数组来说，就是在对角线方向交换数据，目的也是为了更方便的去处理数据。

```python
# 转置的3种方法
t.transpose()
t.swapaxes(1,0)
t.T
```

#### numpy的索引和切片

```python
a[1]  # 取第2行
a[1:3]  # 取第2到第3行
a[:, 2]  # 取第3列
a[:, 2:4]  # 取第3到第4列
a[[1,3], :]  # 分别取第2行和第3行
a[:[2,4]]  # 分别取第3列和第4列
a[[1:3],[2,4]]  # 取第2行第3行和第3列第4列的交集
a[:,2:8:2]  # 取第3列到第8列，步长为2
a[:,2:4] = 0  # 把第3列到第4列的值设置为0
```

#### numpy中布尔索引

```python
t = np.arange(24).reshape(4,6)
print(t < 10)
>>>
array([[ True,  True,  True,  True,  True,  True],
       [ True,  True,  True,  True, False, False],
       [False, False, False, False, False, False],
       [False, False, False, False, False, False]])
# 以上是t<10的输出结果

t[t<10]=0
print(t)
>>>
[[ 0  0  0  0  0  0]
 [ 0  0  0  0 10 11]
 [12 13 14 15 16 17]
 [18 19 20 21 22 23]]
```

#### numpy中的三元运算符

把t中小于10的数字替换为0，把大于10的替换为10

```python
t = np.where(t<10,0,10)
print(t)
>>>
[[ 0  0  0  0  0  0]
 [ 0  0  0  0 10 10]
 [10 10 10 10 10 10]
 [10 10 10 10 10 10]]
```

#### numpy中的clip（裁剪）

把t中小于10的数字替换为0，大于20的数字替换为20，其他数字不变

```python
t = t.clip(10, 20)
print(t)
>>>
[[10 10 10 10 10 10]
 [10 10 10 10 10 11]
 [12 13 14 15 16 17]
 [18 19 20 20 20 20]]
```

#### numpy中的nan和inf

nan(NAN,Nan):not a number表示不是一个数字

##### 什么时候会出现numpy中的nan：

- 当我们读取本地的文件为float的时候，如果有缺失，就会出现nan
- 当做了一个不合适的计算的时候（比如无穷大（inf）减去无穷大）

inf(-inf,inf):infinity，inf表示正无穷，-inf表示负无穷

##### 什么时候会出现inf(-inf,inf):

- 比如一个数字除以0，（python中直接回报错，numpy中是一个inf或者-inf）

##### inf和nan的type类型都是float

##### nan中的注意点：

- 两个nan是不相等的，即np.nan != np.nan
- 可以利用以上的特性，判断数组中nan的个数
  - np.count_nonzero(t != t)
- 如果判断一个数字是否为nan呢？通过np.isnan(a)来判断，返回bool类型，比较把nan替换为0
  - t[np.isnan(t)] = 0
- nan和任何值计算都为nan

#### numpy中常用统计函数

- 求和：t.sum(axis=None)
- 均值：t.mean(a,axis=None) 受离群点的影响较大
- 中值：np.median(t,axis=None)
- 最大值：t.max(axis=None)
- 最小值：t.min(axis=None)
- 极值：np.ptp(t,axis=None) 即最大值和最小值只差
- 标准差： t.std(axis=None)

默认返回多维数组的全部的统计结果，如果指定axis则返回一个当前轴上的结果

标准差：是一组数据平均值分散程度的一种度量。一个较大的标准差，代表大部分数值和其平均值之间差异较大；一个较小的标准差，代表这些数值较接近平均值反映出数据的波动稳定情况，越大表示波动越大，越不稳定。

#### ndarry缺失值填充均值

```python
t = np.array([[  0.,   1.,   2.,   3.,   4.,   5.],
       [  6.,   7.,  np.nan,   9.,  10.,  11.],
       [ 12.,  13.,  14.,  np.nan,  16.,  17.],
       [ 18.,  19.,  20.,  21.,  22.,  23.]])

def fill_nan_by_column_mean(t):
  for i in range(t.shape[1]):
    nan_num = np.count_nonzero(t[:, i][t[:, i] != t[:, i]])  # 计算非nan的个数
    if nan_num > 0:  # 存在的nan值
      now_col = t[:, i]
      now_col_not_nan = now_col[np.isnan(now_col) == False].sum()  # 求和
      now_col_mean = now_col_not_nan / (t.shape[0] - nan_num)  # 和/个数
      now_col[np.isnan(now_col)] = now_col_mean  # 赋值给now_col
      t[:, i] = now_col  # 赋值给t，即更新t的当前列
    
fill_nan_by_column_mean(t)
print(t)
```

#### 数组的拼接

```python
np.vstack((t1,t2))  # 竖直拼接
np.hstack((t1,t2))  # 水平拼接
```

#### 数组的行列交换

```python
t[[1,2],:] = t[[2,1],:]  # 行交换
t[:,[0,2]] = t[:,[2,0]]  # 列交换
```

#### numpy更多好用的方法

1. 获取最大值最小值的位置
   1. np.argmax(t,axis=0)
   2. np.argmin(t,axis=1)
2. 创建一个全0的数组：np.zeros((3,4))
3. 创建一个全1的数组：np.ones((3,4))
4. 创建一个对角线为1的正方形数组（方阵）：np,eye(3)

#### 生成随机数

![](C:\Users\Web_01\Desktop\文档\numpy和pandas\numpy生成随机数.png)

#### numpy的注意点copy和view

1. a=b完全不复制，a和b相互影响
2. a=b[:]，视图的操作，一种切片，会创建新的对象a，但是a的数据完全由b保管，他们两个的数据变化是一致的。
3. a=b.copy()，复制，a和b互不影响。

