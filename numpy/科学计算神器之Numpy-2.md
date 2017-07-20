
# 科学计算神器之Numpy-2

### 4. Array对象的存取
#### 4.1 Array的Indexing，Slicing，Iterating
该部分集中在Numpy的array数据的存取，类似与pyhton的list的索引、切片等


```python
import numpy as np
a = np.arange(8)
a, a[0], a[5]
```




    (array([0, 1, 2, 3, 4, 5, 6, 7]), 0, 5)




```python
a[1:5]
```




    array([1, 2, 3, 4])




```python
a[:-1]
```




    array([0, 1, 2, 3, 4, 5, 6])




```python
a[::-1]
```




    array([7, 6, 5, 4, 3, 2, 1, 0])




```python
a[5:1:-2]
```




    array([5, 3])




```python
a[::3] = -100
a
```




    array([-100,    1,    2, -100,    4,    5, -100,    7])




```python
# iterat， Array对象也支持迭代协议
for i in a:
    print(i * (1/3.))
```

    -33.3333333333
    0.333333333333
    0.666666666667
    -33.3333333333
    1.33333333333
    1.66666666667
    -33.3333333333
    2.33333333333


#### 4.2 数据共享
与Python的list不同的是：通过下标、切片获取的新Array是和原Array共用一个数据空间，相当于引用.


```python
b = a[3:7]
b
```




    array([-100,    4,    5, -100])




```python
b[0] = 1
a, b   # a[3] <= b[0], a[3]=1
```




    (array([-100,    1,    2,    1,    4,    5, -100,    7]),
     array([   1,    4,    5, -100]))



#### 4.3 使用整数序列
Numpy除了可以使用类似Python的list方法获取元素外，还提供了整数序列和布尔数组获取元素. 它们均不与原Array共享数据空间.
使用整数序列时，将使用整数序列中的每个元素作为下标，整数序列可以使列表或数组.


```python
# 获取a中下标为 0,1,1,0的元素，组成一个新Array，下标可以是负数
a[[0, 1, 1, 0]], a[np.array([0, 1, 1, -1])]
```




    (array([-100,    1,    1, -100]), array([-100,    1,    1,    7]))




```python
a[[0, 1]] = 10, 11 # 修改元素的值
```


```python
a
```




    array([  10,   11,    2,    1,    4,    5, -100,    7])




```python
# a, b 不共享数据空间，修改b元素值并没有修改a的元素值
b = a[[0, 1]]
b[0] = 10000
a, b
```




    (array([  10,   11,    2,    1,    4,    5, -100,    7]),
     array([10000,    11]))



#### 4.4 使用布尔数组
使用布尔数组作为下标获取新Array时，将布尔数组对应的下标为True的元素构成新Array. 注意两点：1. 不共享数据空间， 2. 只能使用布尔数组，不能使用布尔列表


```python
# 注意以下两种的不同， list*2 相当于2个list相加，元素数为原来2倍，
# array*2则为array的乘法计算，为数据乘法计算，原始数相同，按整数数组取值
a[np.array([True, False, True, False]*2)]
```




    array([  10,    2,    4, -100])




```python
a[np.array([True, False, True, False])*2]
```




    array([ 2, 10,  2, 10])




```python
# array数值计算，元素数相同，True实际数值为1,False为0
np.array([True, False, True, False])*2
```




    array([2, 0, 2, 0])




```python
# 布尔数组修改元素
a[np.array([True, False]*4)] = -1
a
```




    array([-1, 11, -1,  1, -1,  5, -1,  7])



布尔数组一般通过布尔运算获得的，如：


```python
a>2
```




    array([False,  True, False, False, False,  True, False,  True], dtype=bool)




```python
a, a[a>2]
```




    (array([-1, 11, -1,  1, -1,  5, -1,  7]), array([11,  5,  7]))



这里仅集中在一维数组，下篇将涉及到多维数组的操作. 不过，对数组的存取方法以及共享情况都是一致的，特别是类似Python的list的切片等操作是共享数据空间（与list操作不同），整数序列和布尔数组的操作是不共享的.
