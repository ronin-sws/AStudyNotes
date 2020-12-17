# pandas

numpy能够帮助我们处理数值，但是pandas除了处理数值之外（基于numpy），还能够帮助我们处理其他类型的数据

#### pandas之Series创建

```python
# 方法一
import string
t = pd.Series(np.arange(10), index=list(string.ascii_uppercase[:10]))

# 方法二
a = {string.ascii_uppercase[i]:i for i in range(10)}
pd.Series(a)
>>>
A    0
B    1
C    2
D    3
E    4
F    5
G    6
H    7
I    8
J    9
dtype: int64
  
# 重新指定索引
pd.Series(a, index=list(string.ascii_uppercase[5:15]))
>>>
F    5.0
G    6.0
H    7.0
I    8.0
J    9.0
K    NaN
L    NaN
M    NaN
N    NaN
O    NaN
dtype: float64
# 重新给其指定其他的索引之后，如果能够对应上，就取其值，如果不能，就为Nan
# 为什么类型为float呢？
numpy中nan为float，pandas会自动根据数据类更改series的dtype类型
pandas修改dtype和numpy的方法一样
```

#### pandas之Series切片和索引

切片：直接传入start end或者步长即可

索引：一个的时候直接传入序号或者index，多个的时候传入序号或者index的列表

```python
t = pd.Series(np.arange(10), index=list("ABCDEFGHIJ"))
  
t[2:10:2]  # 切片 步长为2
t[1]  # 取索引
t[[2,3,6]]  # 取索引，索引分别为2，3，6
t[t>4]  # 值大于4的所有数据
t["F"]  # 索引为F的数据
t[["A", "F", "g"]]  # 索引为A，F，g的数据，因为索引g没有，所以取出的值为NaN
```

#### pandas之Series的索引和值

Series对象本质上由两个数组构成，一个数组构成对象的键（index, 索引），一个数组构成对象的值（values），键->值

```python
t.index
>>>
Index(['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J'], dtype='object')

t.values
>>>
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

type(t.index)
>>>
pandas.core.indexes.base.Index

type(t.values)
>>>
numpy.ndarray
```

ndarray的很多方法都可以运用于series类型，比如argmax，clip

series具有where方法，但是结果和ndarray不同

#### Series的tolist()方法和unique()方法

```python
df = pd.Series([1,2,1,3,2,1])
df.tolist()
>>>
[1, 2, 1, 3, 2, 1]

df = pd.Series([1,2,1,3,2,1])
df.unique()
>>>
array([1, 2, 3])
```

#### pandas之读取外部数据

```python
pd.read_csv
pd.read_sql(sql_sentence, connection)
```

#### pandas之DataFrame的创建

```python
# 方法一
t = pd.DataFrame(np.arange(12).reshape(3,4),index=list("ABC"),columns=list("WXYZ"))

# 方法二
t_dict = {
  "name":["zhangsan", "lisi"], 
  "age":["12", "20"], 
}
pd.DataFrame(t_dict)

# 方法三
t_dict = [
  {"name": "zhangsan", "age": 12},
  {"name": "lisi", "age": 20},
]
pd.DataFrame(t_dict)
```

#### DataFrame的基础属性和整体情况查询

```python
# 基础属性
df.shape  # 行数 列数
df.dtypes  # 列数据类型
df.ndim  # 数据维度
df.index  # 行索引
df.colums  # 列索引
df.values  # 对象值，二维ndarray数组

# 整体情况查询
df.head(3)  # 显示头部几行，默认5行
de.tail(3)  # 显示尾部几行，默认5行
df.info()  # 相关信息概览：行数，列数，列索引，列非空值个数，列类型，内存占用
df.describe()  # 快速综合统计结果：计数，均值，标准差，最大值，四分位数，最小值
```

#### 对某一列进行排序

```python
# 对Count_AnimalName列进行排序，ascending=False表示从大到小排序
df.sort_values(by="Count_AnimalName", ascending=False)
```

#### DataFrame之取行或者列

```python
df = pd.DataFrame(np.arange(56).reshape(7,8), index=list("ABCDEFG"), columns=list("STUVWXYZ"))
# 取第X列
df["X"]
# 取第X、Z列
df[["X", "Z"]]
# 取第3到第6行
df[2:6]
# 取第3到第6行和第X列
df[2:6]["X"]
```

#### DataFrame之loc和iloc

- df.loc 通过标签索引行数据
- Df.iloc 通过位置获取行数据

```python
# 取A行，W列交集的数据
df.loc["A", "W"]
# 取A行和W、Z交集的数据
df.loc["A", ["W", "Z"]]
# 取A、C行和W、Z交集的数据
df.loc[["A", "c"], ["W", "Z"]]
# 取A行及A行之后所有行和W、Z的交集
df.loc["A": ["W", "Z"]]
# 取A行到C行和W、Z的交集
df.loc["A":"C",["W","Z"]]

# 取第2行到第3行和第3列、第4列的交集
df.iloc[1:3, [2,3]]
# 取第2行到第3行和第2列到第3列的交集
df.iloc[1:3,1:3]
```

赋值更改数据的过程

```python
df.loc["A", "Y"] = 100
df.iloc[1:2,0:2] = 200
```

#### DataFrame之布尔索引

```python
# 找到使用次数超过800的狗的名字
df[df["Count_AnimalName"]>800]

# 找到所有使用次数超过700并且名字的字符串的长度大于4的狗的名字
df[(df["Row_Labels"].str.len()>4)&(df["Count_AnimalName"]>700)]
# &符号表示：且
# |符号表示：或
```

#### pandas之字符串方法

![image](http://note.youdao.com/yws/res/235399/4A14DDCF43E94D158A61DF4B6FEE09C9)

#### 缺失数据的处理

```python
# 判断数据是否为NaN
pd.isnull(df)  # 是NaN
pd.notnull(df)  # 不是NaN

# 处理方式1：删除NaN所在的行列
dropna(axis=0,how='any',inplace=False)
# 填充数据
t.fillna(t.mean())
t.fillna(t.median())
t.fillna(0)
```

处理为0的数据：t[t==0]=np.nan

当然并不是每次为0的数据都需要处理

计算平均值等情况，nan是不参与计算的，但是0会

#### pandas常用统计方法

```python
# 评分的平均分
rating_mean = df["Rating"].mean()

# 导演的人数
temp_list = df["Actors"].str.split(",").tolist()
nums = set([i for j in temp_list for i in j])

# 电影市场的最大最小值
max_runtime = df["Runtime(Minutes)"].max()
max_runtime_index = df["Runtime(Minutes)"].argmax()
min_runtime = df["Runtime(Minutes)"].min()
min_runtime_index = df["Runtime(Minutes)"].argmin()
runtime_median = df["Runtime(Minutes)"].median()
```

#### 数据合并之join、merge

join：默认情况下他是把行索引相同的数据合并到一起

![image](http://note.youdao.com/yws/res/235402/E697F8CFF53D48638425634BBEED41C4)

merge:按照指定的列把数据按照一定的方式合并到一起

![image](http://note.youdao.com/yws/res/235404/067FFB4B69D7480A928B1DFBA9813D35)

默认的合并方式inner：并集

outer：交集，NaN补全

left：左边为准，NaN补全

right：右边为准，NaN补全

#### 分组和聚合

```python
grouped = df.groupby(by="columns_name")
```

grouped是一个DataFrameGroupBy对象，是可迭代的。
grouped中的每一个元素是一个元组
元组里面是（索引(分组的值)，分组之后的DataFrame）

如果我们需要对国家和省份进行分组统计，应该怎么操作呢？

```python
grouped = df.groupby(by=[df["Country"],df["State/Province"]])
```

很多时候我们只希望对获取分组之后的某一部分数据，或者说我们只希望对某几列数据进行分组，这个时候我们应该怎么办呢？

获取分组之后的某一部分数据：

```python
df.groupby(by=["Country","State/Province"])["Country"].count()
```

对某几列数据进行分组：

```python
df["Country"].groupby(by=[df["Country"],df["State/Province"]]).count()
```

观察结果，由于只选择了一列数据，所以结果是一个Series类型，如果我想返回一个DataFrame类型呢？

```python
t1 = df[["Country"]].groupby(by=[df["Country"],df["State/Province"]]).count()
t2 = df.groupby(by=["Country","State/Province"])[["Country"]].count()
```

以上的两条命令结果一样
和之前的结果的区别在于当前返回的是一个DataFrame类型

#### 索引和复合索引

简单的索引操作：

- 获取index：df.index
- 指定index ：df.index = ['x','y']
- 重新设置index : df.reindex(list("abcedf"))
- 指定某一列作为index ：df.set_index("Country",drop=False)
- 返回index的唯一值：df.set_index("Country").index.unique()

```python
a = pd.DataFrame({'a': range(7),'b': range(7, 0, -1),'c': ['one','one','one','two','two','two', 'two'],'d': list("hjklmno")})
a
>>>
	a	b	c	d
0	0	7	one	h
1	1	6	one	j
2	2	5	one	k
3	3	4	two	l
4	4	3	two	m
5	5	2	two	n
6	6	1	two	o

x = a.set_index(["c","d"])["a"]
x
>>>
c    d
one  h    0
     j    1
     k    2
two  l    3
     m    4
     n    5
     o    6
Name: a, dtype: int64
    
x["one", "h"]
>>>
0
```

x.swaplevel() 复合索引中交换索引

```python
x = x.swaplevel() 
x
>>>
d  c  
h  one    0
j  one    1
k  one    2
l  two    3
m  two    4
n  two    5
o  two    6
Name: a, dtype: int64
```

#### pandas中的时间序列

##### 时间序列的生成

```python
pd.date_range(start=None, end=None, periods=None, freq='D')
```

start和end以及freq配合能够生成start和end范围内以频率freq的一组时间索引
start和periods以及freq配合能够生成从start开始的频率为freq的periods个时间索引

![image](http://note.youdao.com/yws/res/235396/9F511157929D4547AA69BD50D623127B)

关于频率的更多缩写

![image](http://note.youdao.com/yws/res/235406/CA5CD7C0BD1B499EB91E020E2C1CECB5)

##### 在DataFrame中使用时间序列

```python
index=pd.date_range("20170101",periods=10)
df = pd.DataFrame(np.random.rand(10),index=index)
```

可以使用pandas提供的方法把时间字符串转化为时间序列

```python
df["timeStamp"] = pd.to_datetime(df["timeStamp"],format="")format参数大部分情况下可以不用写，但是对于pandas无法格式化的时间字符串，我们可以使用该参数，比如包含中文
```

##### pandas重采样

重采样：指的是将时间序列从一个频率转化为另一个频率进行处理的过程，将高频率数据转化为低频率数据为降采样，低频率转化为高频率为升采样。

pandas提供了一个resample的方法来帮助我们实现频率转化

![image](http://note.youdao.com/yws/res/235409/A712A2E8062C4880BEEBC0EF6E671EF6)

##### Periodlndex

之前所学习的DatetimeIndex可以理解为时间戳
那么现在我们要学习的PeriodIndex可以理解为时间段

```python
periods = pd.PeriodIndex(year=data["year"],month=data["month"],day=data["day"],hour=data["hour"],freq="H")
```
