
```python
import os
import sys


root_path = os.path.abspath(os.path.join(os.path.dirname(__file__), '..'))
PROJECT_ROOT = os.path.realpath(os.path.dirname(__file__))

# __file__是用来获得模块所在的路径的
# os.path.dirname() 返回目录路径
# os.path 模块主要用于获取文件的属性
# os.path.abspath(path) 返回文件绝对路径
# os.path.abspath()返回绝对路径，但不处理符号链接
# os.path.realpath() 先处理路径中的符号链接，再返回绝对路径
# os.path.join(path1[1,path2[2,……]]) 把目录和文件名合成一个路径
# os.pardir 会返回父级目录

sys.path.insert(0, os.path.abspath(os.path.join(root_path, '')))
sys.path.insert(0, os.path.abspath(os.path.join(root_path, 'webapp')))
sys.path.insert(0, os.path.join(PROJECT_ROOT, os.pardir))

# sys.path 动态的改变python搜索路径
# sys.path.append("引用模块的地址")
# sys.path.insert(0, "引用模块的地址")

```
