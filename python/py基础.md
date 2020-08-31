# 一、常用内置函数

* 内置函数可以直接使用

|      | 名称                                   | 返回值                                                       |
| ---- | :------------------------------------- | ------------------------------------------------------------ |
|      | chr(num)                               | 对应的ASCII字符                                              |
|      | cmp(x,y)                               | 数字，用于比较两个对象如果 x < y返回 -1, 如果 x == y 返回 0, 如果 x > y 返回 1 |
|      | dir(modelName)                         | 列表，模块内定义的所有函数                                   |
|      | dict()                                 | 字典，dict(a='a', b='b', t='t') dict([('one', 1), ('two', 2), ('three', 3)]) |
|      | enumerate(sequence, [start=0])         | 返回一个enumerate()对象 list(enumerate(['a','b'])) 返回[(0,'a'),(1,'b')] |
|      | filter(func,iterable)                  | 迭代器，过滤一个序列的数据                                   |
| ++   | len(iterable)                          | 数字，返回对象（字符、列表、元组等）长度或项目个数           |
|      | map(func,list)                         | 迭代器，对一个列表进行映射                                   |
|      | max(iterable) min(iterable)            | 数字                                                         |
|      | open(path)                             | 文件对象                                                     |
|      | ord(char)                              | 数字，字符的ASCII码                                          |
|      | pow(x,y)                               | 数字                                                         |
| ++   | range(start, stop[, step])             | 迭代器，不包括最后一位                                       |
|      | reversed(iterable)                     | 迭代器，tuple, string, list 或 range翻转                     |
| ++   | list(iterable)                         | 列表                                                         |
|      | round(num,n)                           | 数字，将数字四舍五入保留n位小数                              |
| ++   | sorted(iterable,cmp,key,reverse=False) | 列表                                                         |
| ++   | sum(iterable,add=0)                    | 数字，求和后再加上add                                        |
|      | type(object)                           | 返回对象类型 如：<type 'list'>                               |
|      | zip(iter1,iter2,...)                   | zip可迭代对象，list(zip([1,2],[2,3,4])) -> [(1,2),(2,3)]     |

# 二、注释

```
单行注释：以#开头
多行注释：用三个单引号（'''）或者三个双引号（"""）将注释括起来
```

# 三、运算符

## 3.1 算术运算符

![image-20200831105556864](../../../AppData/Roaming/Typora/typora-user-images/image-20200831105556864.png)



## 3.2 比较运算符

![image-20200831105643967](../../../AppData/Roaming/Typora/typora-user-images/image-20200831105643967.png)



## 3.3 赋值运算符

![image-20200831105714469](../../../AppData/Roaming/Typora/typora-user-images/image-20200831105714469.png)

## 3.4 位运算符

![image-20200831105750187](../../../AppData/Roaming/Typora/typora-user-images/image-20200831105750187.png)



## 3.5 逻辑运算符

![image-20200831105837667](../../../AppData/Roaming/Typora/typora-user-images/image-20200831105837667.png)



## 3.6 成员运算符++

![image-20200831105918984](../../../AppData/Roaming/Typora/typora-user-images/image-20200831105918984.png)



## 3.7 身份运算符

![image-20200831110012337](../../../AppData/Roaming/Typora/typora-user-images/image-20200831110012337.png)

# 四、基本数据类型

```
Python中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建.

Python 3中有六个标准的数据类型：
Numbers（数字）（支持int、float、bool、complex）
String（字符串）
List（列表）
Tuple（元组）
Sets（集合）
Dictionaries（字典）
```

![image-20200831110602631](../../../AppData/Roaming/Typora/typora-user-images/image-20200831110602631.png)

# 五、字符串

```python
# 单行字符串
a = 'hello'

# 多行字符串
a = ''' xxxxxx
xxxxxxxx'''
```

## 5.1 字符串方法

```python
['capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

![image-20200831111810785](../../../AppData/Roaming/Typora/typora-user-images/image-20200831111810785.png)

|      | 名称                        | 返回值                                                       |
| ---- | --------------------------- | ------------------------------------------------------------ |
|      | s.count(char,start,end)     | 数组，统计字符串s中char出现的次数                            |
|      | s.endswith(chars,start,end) | 布尔，检查指定范围内的字符串是否以chars结尾                  |
|      | s.find(chars,start,end)     | 布尔                                                         |
|      | s.format()                  | 字符串，格式化字符串                                         |
|      | s.index(char,beg,end)       | 数字，找到char的索引，没找到会报错                           |
|      | s.join(iterable)            | 字符串，以字符串s作为分隔符拼接可迭代对象(可迭代对象中不能有数字) |
|      | s.ljust(width)              | 字符串，返回一个原字符串左对齐,并使用空格填充至长度 width 的新字符串 |
|      | s.lstrip()                  | 字符串，去掉左边空格                                         |
|      | s.replace(str1,str2,nun)    | 字符串，把 string 中的 str1 替换成 str2,如果 num 指定，则替换不超过 num 次. |
| ++   | s.split(str)                | 列表，以字符str分割s                                         |



## 5.2 u r标志符

```python
# 1.u标志符表示字符串中可以输入Unicode字符，比如中文
a = u'我是xxx'
a = u'Hello\u0020World !'

# 2.r标志符会去掉反斜杠的转义机制
a = r'iam\nsd'

# 3.f标志符表示字符串中指出python表达式
import time
t0 = time.time()
time.sleep(1)
name = 'processing'
a = f'{name} done in {time.time() - t0:.2f} s'
```



## 5.3 字符串运算符

![image-20200831114030930](../../../AppData/Roaming/Typora/typora-user-images/image-20200831114030930.png)

# 六、列表

## 6.1 列表运算符

![image-20200831131031192](../../../AppData/Roaming/Typora/typora-user-images/image-20200831131031192.png)

```python
# 列表索引
l = [0,1,2,3,4]

l[-1] #4
l[1:] #1 2 3 4
l[0::2] #0 2 4 取偶数索引位置的元素
```



## 6.2 列表方法

```python
['append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```



|      | 方法名                                    | 返回值                            |
| ---- | ----------------------------------------- | --------------------------------- |
| ++   | list.append(ele)                          | None，在原来的列表后面追加        |
|      | list.clear()                              | None，清空列表                    |
|      | list.copy()                               | 列表，深复制第一层                |
|      | list.count(ele)                           | 数字，统计元素的出现的次数        |
| ++   | list.extend(iterable)                     | None，在原来列表中批量追加元素    |
|      | list.index(ele)                           | 索引，没找到报错                  |
|      | list.insert(index,ele)                    | None，在指定位置插入元素          |
| ++   | list.pop(index=-1) 等价于 del list[index] | None，删除指定位置的元素          |
|      | list.remove(ele)                          | None，在列表中删除第一次出现的ele |
|      | list.reverse()                            | None，翻转列表                    |
|      | list.sort(key,reverse=False)              | None，列表排序                    |



```python
list.sort(key,reverse=False) #指定元素进行排序 reverse = True 降序， reverse = False 升序（默认）
# 获取列表的第二个元素
def takeSecond(elem):
    return elem[1]
# 列表
random = [(2, 2), (3, 4), (4, 1), (1, 3)]
# 指定第二个元素排序
random.sort(key=takeSecond)
# 输出类别
print ('排序列表：', random)
# 结果[(4, 1), (2, 2), (1, 3), (3, 4)]
```

## 6.3 列表推导式

```python
>>> vec = [2, 4, 6]
>>> [3*x for x in vec]
[6, 12, 18]

>>> [x for x in vec if x%3 == 0] 
[6]


>>> [[x, x**2] for x in vec]
[[2, 4], [4, 16], [6, 36]]

# 对序列里每一个元素逐个调用某方法
>>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
>>> [weapon.strip() for weapon in freshfruit]
['banana', 'loganberry', 'passion fruit']

>>> vec1 = [2, 4, 6]
>>> vec2 = [4, 3, -9]
>>> [x*y for x in vec1 for y in vec2]
[8, 6, -18, 16, 12, -36, 24, 18, -54]
```

# 七、元组

```python
# 元组是有序，且不能修改，只能只读的序列(不能增删改)

tup = (1,2)
tup = (1,) #一个元素必须加逗号
```

## 7.1 元组运算符

```
1 索引运算符[]
2 合并 +运算符
```

## 7.2 元组方法

|      | 方法名         | 返回值             |
| ---- | -------------- | ------------------ |
|      | tup.count(ele) | 数字，统计元素次数 |
|      | tup.index(ele) | 索引值, 没找到报错 |

# 八、集合

```python
# 集合是无序和无索引的序列

s = {1,2}
```

## 8.1 集合方法

```python
['add', 'clear', 'copy', 'difference', 'difference_update', 'discard', 'intersection', 'intersection_update', 'isdisjoint', 'issubset', 'issuperset', 'pop', 'remove', 'symmetric_difference', 'symmetric_difference_update', 'union', 'update']
```

|      | 方法名                    | 返回值                                                      |
| ---- | ------------------------- | ----------------------------------------------------------- |
| ++   | s.add(ele)                | None，添加元素                                              |
|      | s.clear()                 | None，清空集合                                              |
|      | s.copy()                  | 集合，深复制第一层                                          |
|      | s.difference(s1)          | 集合，差集s - s1                                            |
| ++   | s.discard(ele)            | None，删除集合中的一个元素                                  |
| ++   | s.intersection(s1,s2,...) | 集合，多个集合的交集                                        |
|      | s.isdisjoint(s1)          | 布尔，是否没有相同的元素，如果没有返回 True，否则返回 False |
|      | s.issubset(s1)            | 布尔，s是否是s1的子集                                       |
|      | s.issuperset(s1)          | 布尔，s是否是s1的超集                                       |
|      | s.pop()                   | 删除的元素，随机删除一个元素                                |
|      | s.remove(ele)             | None，移除指定的元素，没有找到报错                          |
| ++   | s.union(s1)               | 集合，s和s1的并集                                           |

# 九、字典

```python
# 字典是一个无序、可变和有索引的集合
dic = {
    'name':'xwd',
    'sex':'male'
}
```

## 9.1 字典方法

```python
['clear', 'copy', 'fromkeys', 'get', 'items', 'keys', 'pop', 'popitem', 'setdefault', 'update', 'values']
```

|      | 方法名                       | 返回值                                       |
| ---- | ---------------------------- | -------------------------------------------- |
|      | dic.clear()                  | None，清空字典                               |
|      | dic.copy()                   | 字典，深复制第一层                           |
|      | dic.from(keyList,defaultVal) | 字典，创建拥有默认值的字典                   |
| ++   | dic.get(key) 等价于 dic[key] | 返回指定键的值，没找到报错                   |
|      | dic.items()                  | dict_items可迭代对象                         |
|      | dic.keys()                   | dict_keys可迭代对象                          |
|      | dic.pop(key)                 | 删除key的值，删除指定key，没有找到报错       |
|      | dic.popitem()                | 返回并删除字典中的最后一对键和值             |
|      | dic.setdefault(key,val)      | 返回指定key的值，如果不存在就添加并设值为val |
|      | dic.values()                 | dict_values可迭代对象                        |



# 十、函数

```python
# 默认参数
def my_function(country = "China"):
  print("I am from " + country)

# 参数数目未知
def my_function(*kids): #会把所有的参数变为一个列表kids
  print("The youngest child is " + kids[2])


# lambda函数 lambda arguments : expression
x = lambda a : a + 10
print(x(5)) # 15
```



# 十一、正则

```python
import re

# 注意这些函数的第一个参数都是正则表达式字符串


# findAll()方法 返回列表,没找到就是空列表
str = "China is a great country"
x = re.findall("USA", str)
print(x)

# search()方法 返回Match对象
str = "China is a great country"
x = re.search("\s", str)
print("The first white-space character is located in position:", x.start())

# split()方法 返回列表
str = "China is a great country"
x = re.split("\s", str) #将字符串str以空格分隔为列表
print(x)

# sub()方法 替换字符串中指定的模板
str = "China is a great country"
x = re.sub("\s", "9", str)
print(x)
```

## 11.1 Match对象方法

```python
['end', 'endpos', 'expand', 'group', 'groupdict', 'groups', 'lastgroup', 'lastindex', 'pos', 're', 'regs', 'span', 'start', 'string']
```

# 十二、conda所有命令

```powershell
conda list # 查看所有包
conda update xxx   #更新xxx文件包
conda uninstall xxx   #卸载xxx文件包
conda install --channel [https://xxx] [package_name] #通过指定的通道下载包

conda create --name [your_env_name] python=3.5 #创建虚拟环境同时指定python的版本

conda activate xxxx               #开启xxxx环境
conda deactivate                  #关闭环境
conda env list                    #显示所有的虚拟环境

conda config --show channels      #显示当前的下载通道
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ #添加一个下载通道
```

