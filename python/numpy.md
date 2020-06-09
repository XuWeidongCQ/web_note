# 一、ndarray对象

* 多维数组对象

  ```python
  #维度的个数就是嵌套中括号的对数
  [1,2] #这是一维
  [[1,2],[3,4]] #这是二维
  [
      [[1,2],[3,4]],
      [[5,6],[7,8]]
  ] #这是三维
  ```

* 每个元素必须是同一类型的

  ```python
  numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)
  # object 数组或者嵌套的数列
  # dtype 里面元素的类型 （常用 int8 int16 int32 int64 float16 unit16 complex16等）
  # order 创建数组的样式，C为行方向，F为列方向，A为任意方向（默认）
  # ndim 生成数组的最小维度
  ```

## 1.1 ndarray的属性

### nd.ndim  返回维度数目

### nd.shape 返回维度元组

### nd.size 返回元素总个数

### nd.dtype 返回元素的类型

## 1.2 切片

```python
import numpy as np

a = np.arange(10)  
b = a[2:7:2]   # 从索引 2 开始到索引 7 停止，间隔为 2
print(b)

a = np.array([[1,2,3],[3,4,5],[4,5,6]])  
print (a[:,1])   # 第2列元素
print (a[1,:])   # 第2行元素
print (a[:,1:])  # 第2列及剩下的所有元素
[2 4 5]

[3 4 5]

[[2 3]
 [4 5]
 [5 6]]
```

## 1.3 高级索引

```python
import numpy as np 
# 获取数组中(0,0)，(1,1)和(2,0)位置处的元素
x = np.array([[1,  2],  [3,  4],  [5,  6]]) 
y = x[[0,1,2],  [0,1,0]]  
print (y)


```

## 1.4 布尔索引

# 二、静态方法

* 这些方法是直接在numpy中定义的，不是ndarray对象上的

## 2.1 创建ndarray对象的方法

### np.empty()

```python
# 指定形状的数组（内部为随机值）
numpy.empty(shape, dtype = float, order = 'C')
```

### np.zeros()

```python
# 以0填充
numpy.zeros(shape, dtype = float, order = 'C')
```

### np.ones()

```python
numpy.ones(shape, dtype = None, order = 'C')
```

### np.asarray() np.array()

```python
# 从列表, 列表的元组, 元组, 元组的元组, 元组的列表，多维数组创建ndarray
numpy.asarray(a, dtype = None, order = None)
```

### np.fromiter()

```python
# 从可迭代对象中建立 ndarray 对象，返回一维数组
numpy.fromiter(iterable, dtype, count=-1)
```

### np.arrage()

```python
# 根据 start 与 stop 指定的范围以及 step 设定的步长，生成一个 ndarray(不包含step)
numpy.arange(start, stop, step, dtype)
```

## 







### 





