### 列表推导式
```python
list1 = [i for i in range(0, 10, 2)]

list2 = [i for i in range(10) if i % 2 == 0]

list3 = [(i, j) for i in range(1, 3) for j in range(3)]

print(list1, list2, list3)
```

### 字典推导式
```python
dict1 = {i: i**2 for i in range(1, 5)}

list1 = ['name', 'age', 'gender', 'id']
list2 = ['Tom', 20, 'man']
dict2 = {list1[i]: list2[i] for i in range(len(list2))}
# 备注：如果两个列表数据个数不同，len统计数据多的列表数据个数会报错；len统计数据少的列表数据个数不会报错

counts = {'MBP': 268, 'HP': 125, 'DELL': 201, 'Lenovo': 199, 'acer': 99}
dict3 = {key: value for key, value in counts.items() if value >= 200}

print(dict1, dict2, dict3)
```

### 集合推导式
```python
tmplist = [1, 1, 2]
set1 = {i ** 2 for i in tmplist}
print(set1)
```