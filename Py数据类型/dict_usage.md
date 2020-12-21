[toc]

# 字典


## 常用方法

> tmpdict = {'name': 'Tom', 'age': 20, 'gender': '男'}

### 取值：
```
tmpdict[key]	可以从字典中取值，key不存在会报错

tmpdict.get(key)	可以从字典中取值，key不存在不会报错


```



### 删除 ：
```
tmpdict.pop（key）	删除指定键值对，key不存在会报错

tmpdict.popitem	随机删除一个键值对

tmpdict.clear()	清空字典

del tmpdict[name]  删除字典或删除字典中指定键值对。

```


### 转换字典为列表：
```
tmpdict.keys()	所有key列表

tmpdict.values()	所有value列表

tmpdict.items()	所有(key,value)元组列表	

```


### 修改、新增、合并：
```
tmpdict[key]=value	key存在则修改，不存在则新建

tmpdict.setdefault(key,value) 如果key存在，不会修改数据，如果key不存在，新建键值对

tmpdict.updata（tmpdict2）	将字典2的数据合并到字典
```

### 统计
```
len(tmpdict)  统计字典长度
```
