[toc]
# 列表list

## 常用方法

> 列表都用tmplist表示

### 查找：

```
tmplist.index(数据, 开始位置下标, 结束位置下标) # 返回指定数据所在位置的下标，如果查找的数据不存在则报错。
```

### 增加 ： 
```
tmplist.insert(索引, 数据)	# 在指定位置插入数据  
    
tmplist.append(数据)	# 在末尾追加数据  
    
tmplist.extend(tmplist2)	# 将列表2 的数据追加到列表
```

   
      
### 修改：
```
tmplist[索引] = 数据	# 修改指定索引的数据

```

    
### 删除：
```
del tmplist[索引] 	# 删除指定索引的数据（从内存中删除）

tmplist.remove[数据]	# 删除第一个出现的指定数据

tmplist.pop	# 删除末尾数据

tmplist.pop(索引)	# 删除指定索引数据

tmplist.clear	# 清空列表 
```

### 统计：
```
len(tmplist) 	# 列表长度

tmplist.count(数据)	 # 数据在列表中出现的次数
```     

### 排序：
```
tmplist.sort()	# 升序

tmplist.sort(reverse=True) 	# 降序

tmplist.reverse()	#逆序、反转
```

### 复制
```
tmplist.copy()  
```

