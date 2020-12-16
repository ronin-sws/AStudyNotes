[toc]

# 字符串

## 常用方法：

>字符串变量都用tmpstr表示

### 常用函数
```
len(tmpstr) :获取字符串的长度  

tmpstr.count(tmpstr2) :小字符串在大字符串中出现的次数

tmpstr[索引] :从字符串中取出单个字符
```


### 判断类型
```
tmpstr.isspace() ：如果 tmpstr 中只包含空格，则返回 True

tmpstr.isalpha()：如果tmpstr至少有一个字符并且所有字符都是字母返回 True

tmpstr.isalnum()：如果tmpstr至少有一个字符并且所有字符都是字母或数字则返回 True

tmpstr.isdecimal()： 如果 tmpstr 只包含数字则返回 True，全角数字

tmpstr.isdigit()：如果tmpstr只包含数字则返回True，全角数字、⑴、\u00b2

tmpstr.isnumeric()：如果tmpstr只包含数字则返回True，全角数字，汉字数字

tmpstr.istitle()：如果tmpstr是标题化的(每个单词的首字母大写)则返回 True

tmpstr.islower()：如果tmpstr中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True

tmpstr.isupper()：如果tmpstr中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True
```


### 查找和替换
```
tmpstr.startswith(str)：检查字符串是否是以 str 开头，是则返回 True

tmpstr.endswith(str)：检查字符串是否是以 str 结束，是则返回 True

tmpstr.find(str, start=0, end=len(tmpstr))：检查字符串是否是以 str 结束，是则返回 True

tmpstr.rfind(str, start=0, end=len(tmpstr))：类似于 find()，不过是从右边开始查找

tmpstr.index(str,start=0,end=len(tmpstr))：跟find()方法类似，不过如果 str 不在 string 会报错

tmpstr.rindex(str,start=0,end=len(tmpstr))：类似于index()，不过是从右边开始

tmpstr.replace(old_str, new_str, num=string.count(old))：把 string 中的 old_str 替换成 new_str，如果 num 指定，则替换不超过 num 次
```


	
### 大小写转换
```
tmpstr.capitalize()	把tmpstr的第一个字符大写

tmpstr.title()		把tmpstr的每个单词首字母大写

tmpstr.lower()		转换 tmpstr 中所有大写字符为小写

tmpstr.upper()		转换 tmpstr 中的小写字母为大写

tmpstr.swapcase()	翻转 tmpstr 中的大小写
```


### 文本对齐
```
tmpstr.ljust(width)		返回一个原字符串左对齐，并使用空格填充至长度 width 的新字符串
	
tmpstr.rjust(width)		返回一个原字符串右对齐，并使用空格填充至长度 width 的新字符串

tmpstr.center(width)		返回一个原字符串居中，并使用空格填充至长度 width 的新字符串
```

	
	
### 去除空白字符
```
tmpstr.lstrip()		截掉 tmpstr 左边（开始）的空白字符

tmpstr.rstrip()		截掉 tmpstr 右边（末尾）的空白字符

tmpstr.strip()		截掉 tmpstr 左右两边的空白字符
```


### 拆分和连接
```
tmpstr.partition(str)：把字符串 tmpstr 分成一个 3 元素的元组 (str前面, str, str后面)

tmpstr.rpartition(str)：类似于partition()方法，不过是从右边开始查找

tmpstr.split(str="", num)	以 str 为分隔符拆分 string，如果 num 有指定值，则仅分隔 num + 1 个子字符串，str 默认包含 '\r', '\t', '\n' 和空格

tmpstr.splitlines()		按照行('\r', '\n', '\r\n')分隔，返回一个包含各行作为元素的列表

tmpstr.join(seq)以 string 作为分隔符，将 seq 中所有的元素（的字符串表示）合并为一个新的字符串 【例如：",".join(["a","b","c"])  →“a,b,c” 】	
```


### 切片
```
name = "abcdefg"

print(name[2:5:1])  # cde
print(name[2:5])  # cde
print(name[:5])  # abcde
print(name[1:])  # bcdefg
print(name[:])  # abcdefg
print(name[::2])  # aceg
print(name[:-1])  # abcdef, 负1表示倒数第一个数据
print(name[-4:-1])  # def
print(name[::-1])  # gfedcba
```


	
### 字符串中的转义字符
	\\  :  反斜杠符号	
	
	\'  :  单引号
	
	\"  :  双引号
	
	\n  :  换行
	
	\t  ： 横向制表符【在控制台输出一个制表符，协助在输出文本时 垂直方向保持对齐】
	
	\r  ：回车

