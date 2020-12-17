[toc]

```
import os
```


### 【1】split()函数：指定分隔符对字符串进行切片

```
str.split(str="", num=string.count(str))


str -- 分隔符，默认为所有的空字符，包括空格、换行(\n)、制表符(\t)等。

num -- 分割次数。

返回值：返回分割后的字符串列表。
```
- Demo:

```
sentence="All good things come to those who wait."
 
#分隔符以空格
print("分隔符以空格: ",sentence.split(' '))
print()
#分隔符以空格 ，分割1次
print("分隔符以空格 ，分割1次: ",sentence.split(' ',1))
print()
#分隔符以空格 ，分割2次
print("分隔符以空格 ，分割2次: ",sentence.split(' ',2))
print()
#分隔符以空格 ，分割2次,并取序列为1的项
print("分隔符以空格 ，分割2次,并取序列为1的项: ",sentence.split(' ',2)[1])
```
- Demo运行结果

![image](https://note.youdao.com/yws/res/142430/C2C8C980F1C34570871A789A31DC3E75)

###  【2】os.path.join() :   将分离的部分合成一个整体

```
filename=os.path.join('/home/ubuntu/python_coding','split_func')

print filename

#输出为：/home/ubuntu/python_coding/split_func
```


### 【3】os.path.split() :  返回文件的路径和文件名

```
dirname,filename=os.path.split('/home/ubuntu/python_coding/split_func/split_function.py')

print dirname

print filename


#输出为：
/home/ubuntu/python_coding/split_func

split_function.py
```



### 【4】os.path.splitext() : 将文件名和扩展名分开

```
fname,fename=os.path.splitext('/home/ubuntu/python_coding/split_func/split_function.py')

print 'fname is:',fname

print 'fename is:',fename


#输出为：

fname is:/home/ubuntu/python_coding/split_func/split_function

fename is:.py
```
