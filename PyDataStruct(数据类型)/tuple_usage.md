[toc]
# 元组tuple

## 常用方法

> 元组数据不支持修改，只支持查找


```
tmptuple = ('aa', 'bb', 'cc', 'bb')

tmptuple[0]  # 取下标

tmptuple.index('aa')  # 0 查找某个数据，如果数据存在返回对应的下标，否则报错

tmptuple..count('bb')  # 2 统计某个数据在当前元组出现的次数。
 
len(tmptuple)  # 统计元组中数据的个数


```
- 不能直接修改元组内的数据，报错
- 如果元组里有里列表，可以修改列表里的数据