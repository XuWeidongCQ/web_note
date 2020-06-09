# #内置函数

* 这些可以直接使用

  ```python
  abs()				dict()				help()			min()					setattr()
  all()				dir()				hex()			next()					slice()
  any()				divmod()			id()			object()				sorted()
  ascii()				enumerate()			input()			 oct()					staticmethod()
  bin()				eval()				int()			open()					str()
  bool()				exec()				isinstance()	 ord()					sum()
  bytearray()			filter()			issubclass()	 pow()					 super()
  bytes()			    float()				iter()			print()					tuple()
  callable()	         format()			 len()			 property()				 type()
  chr()				frozenset()			list()			range()**				vars()
  classmethod()		getattr()			locals()		 repr()					**zip()
  compile()			globals()			map()			reversed()				__import__()
  complex()			hasattr()			max()			round()	 
  delattr()			hash()			    memoryview()	 set()
  ```

* 常用的函数

  ```python
  chr() #输入一个数字，返回对应的ASCII字符
  dir(model) #找到模块内定义的所有名称。以一个字符串列表的形式返回:
  dict() #创建一个字典 dict(a='a', b='b', t='t') dict([('one', 1), ('two', 2), ('three', 3)])
  enumerate(sequence, [start=0]) #返回一个enumerate()对象 list(enumerate(['a','b'])) 返回[(0,'a'),(1,'b')]
  filter(func,iterable) #过滤一个序列的数据 返回一个迭代器对象
  iter() #生成迭代器
  input() #从键盘读入一行文本
  len() #返回对象（字符、列表、元组等）长度或项目个数
  list() #将字符串或者元组变为列表
  map(func,list) #对一个列表进行映射
  max(list)
  min(list)
  open() #打开一个文件 返回一个文件对象
  ord() #返回一个字符的ASCII或Unicode数字
  pow(x,y) # x**y
  range(start, stop[, step]) #返回一个可迭代对象，不是列表啊 但是可以用list()转换为列表 不包括最后一位
  reversed(seq) # tuple, string, list 或 range翻转 返回列表
  round(x,n) #四舍五入
  sorted(seq,key,reverse=False) #对可迭代对象进行排序 返回一个新的列表
  sum(iterable,x) #对可迭代对象（列表，元组，集合）进行求和
  type(object) #返回对象类型 如：<type 'list'>
  zip(iter1,iter2,...) #返回一个zip对象，需要用list转化 list(zip([1,2],[2,3,4])) [(1,2),(2,3)]
  ```

  

# 一、基本数据类型

* Number -- int float bool complex
* String
* Tuple
* List
* Set
* Dictionary
* 前三个是不可变数据（简单数据），后三个是可变数据类型（引用类型）

## 1.1 列表

### 1.1.1 列表方法

```python
list = []

list.append(ele) #在列表末尾添加一个元素
list.count(ele) #统计某个元素在列表中出现的次数
list.extend(list1) #在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）
list.index(ele) #返回ele元素的索引 没找到会抛出异常
list.insert(index,ele) #在index位置插入ele
list.pop(index=-1) #移除一个元素（默认最后一个） 并返回这个元素
list.remove(ele) #移除列表中某个值的第一个匹配项 没有就报错
list.reverse() #翻转
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
list.clear() #清空列表
list.copy() #复制列表 浅拷贝（只能拷贝第一层）
```

### 1.1.2 列表推导式

```python
>>> vec = [2, 4, 6]
>>> [3*x for x in vec]
[6, 12, 18]

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





## 1.2 元组

### 1.2.1 元组方法

```python
# 元组中的元素不能修改 只能读取
# 但是可以拼接两个元组 删除整个元组

tup1 = (12, 34.56)
tup2 = ('abc', 'xyz')
# 以下修改元组元素操作是非法的。
# tup1[0] = 100
tup3 = tup1 + tup2
print (tup3)

# 删除后的元组不能访问了，否则出错
tup = ('Google', 'Runoob', 1997, 2000)
print (tup)
del tup
```

## 1.3 字典

### 1.3.1字典方法

```python
dic = {}

dic.clear() #删除字典中的所有问题
dic.copy() #浅复制 （只复制第一层，嵌套对象不能复制）
dic.fromkeys(keyList,vallist) #创建一个新字典，以序列keyList中元素做字典的键，vallist为字典所有键对应的初始值
dic.get(key) #返回指定键的值，如果值不在字典中返回default值
key in dic #如果键在字典dict里返回true，否则返回false
dic.items() #以列表返回可遍历的(键, 值) 元组数组 例子：dict_items([('Age', 7), ('Name', 'Runoob')]) 这个不是列表
dic.keys() #返回一个迭代器，可以使用 list() 来转换为列表
dic.values() #返回一个迭代器，可以使用 list() 来转换为列表
dic.pop(key) #删除字典给定键 key 所对应的值，返回值为被删除的值
dic.popitem() #返回并删除字典中的最后一对键和值
```

## 1.4 集合

### 1.4.1 集合方法

```python
s = new set()

s.add(ele) #添加元素
s.clear() #移除集合中的所有元素
s.copy() #拷贝一个集合
++ s.different(s1) #返回的集合元素包含在第一个集合中，但不包含在第二个集合(方法的参数)中
s.discard(ele) #该方法不同于 remove() 方法，因为 remove() 方法在移除一个不存在的元素时会发生错误，而 discard() 方法不会
++ s.intersection(s1,s2,...) #返回多个集合的交集
s.isdisjoint(s1) #判断两个集合是否包含相同的元素，如果没有返回 True，否则返回 False
s.issubset(s1) #判断集合的所有元素是否都包含在指定集合中，如果是则返回 True，否则返回 False s是否都在s1
s.issuperset(s1) #判断指定集合的所有元素是否都包含在原始的集合中，如果是则返回 True，否则返回 False s是否包含s1
s.pop() #随机移除元素 返回该元素
s.remove(ele) #移除指定的元素
++ s.symmetric_difference(s1) #返回两个集合中不重复的元素集合，即会移除两个集合中都存在的元素(返回两个集合的并集-交集)
++ s.union(s1) #返回两个集合的并集
s.update(obj) #添加一个数组（会被拆开） 字典 元组
```

## 1.5 字符串

### 1.5.1 运算符

```
+
*
[]
[:]
in
not in
%
```

### 1.5.2 字符串方法

```python
s = ''

s.capitalize() #将字符串的第一个字符转换为大写
s.center(width,fillchar) #返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格
s.count(str,beg=0,end=len(string)) #统计字符串里某个字符出现的次数。可选参数为在字符串搜索的开始与结束位置
s.encode('UTF-8') s.encode('GBK') #返回以字符串以特定的格式编码的字符串
s.decode('UTF-8') #将这个以UTF8编码的字符串解码
s.endswith(s1) #如果字符串含有指定的后缀返回 True，否则返回 False
s.find(s1,beg=0,end=12) #检测str是否包含在字符串中，如果指定范围 beg 和 end ，则检查是否包含在指定范围内，如果包含返回开始的索引值，否则返回-1
s.isalnum() #如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True,否则返回 False
s.isalpha() #如果字符串至少有一个字符并且所有字符都是字母则返回 True, 否则返回 False
s.isdigit() #如果字符串只包含数字则返回 True 否则返回 False.
s.islower() #如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False
s.isnumeric() #如果字符串中只包含数字字符，则返回 True，否则返回 False
s.isspace() #如果字符串中只包含空白，则返回 True，否则返回 False.
s.issuper() #所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False
s.split() #分割产生列表
s.lstrip()
s.rstrip()
s.strip()
s.startwith(s1,beg,end) #查字符串是否是以指定子字符串s1开头，是则返回 True，否则返回 False。如果beg和 end 指定值，则在指定范围内检查
```

# 二、语句

## 2.1 循环语句

```python
while 判断条件(condition)：
    执行语句(statements)……
    
while <expr>:
    <statement(s)>
else:
    <additional_statement(s)>   
 
for <variable> in <sequence>: #<sequence>是一个可迭代对象
    <statements>
else:
    <statements>
    

```

## 2.2 运算符

```python
+
-
*
/
%
**
//

```



# 三、模块

* 模块是一个包含所有你定义的**函数和变量**的文件，其后缀名是.py。模块可以被别的程序引入，以使用该模块中的函数等功能。这也是使用 python 标准库的方法。
* 相同名字的函数和变量完全可以分别存在不同的模块中，因此，我们自己在编写模块时，不必考虑名字会与其他模块冲突。但是也要注意，尽量不要与内置函数名字冲突
* 个模块只会被导入一次，不管你执行了多少次import。这样可以防止导入模块被一遍又一遍地执行。

## 3.1 模块的导入

```python
#一个support.py文件
def print_func( par ):
    print ("Hello : ", par)
    return

#用法
import support
support.print_func('xu')

#从模块中导入一个指定的部分到当前命名空间中
from modname import name1[, name2[, ... nameN]]
```

## 3.2 内置的模块

### OS

* **os** 模块提供了非常丰富的方法用来处理文件和目录

### sys

* 命令行参数

###  re

* 正则模块

### math

### urllib

### datetime

