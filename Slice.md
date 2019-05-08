## Python 中的Slice

在Python中对于具有序列结构的数据来说都可以使用切片操作，需注意的是序列对象某个索引位置返回的是一个元素，而切片操作返回是和被切片对象相同类型对


象的副本。


如下面的例子，虽然都是一个元素，但是对象类型是完全不同的：


```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
alist[0]
0
alist[0:1]
[0]
```


通常一个切片操作要提供三个参数 [start_index:  stop_index:  step] 


start_index是切片的起始位置


stop_index是切片的结束位置（不包括）


step可以不提供，默认值是1，步长值不能为0，不然会报错ValueError。



当 step 是正数时，以list[start_index]元素位置开始， step做为步长到list[stop_index]元素位置（不包括）为止，从左向右截取，


start_index和stop_index不论是正数还是负数索引还是混用都可以，但是要保证 list[stop_index]元素的【逻辑】位置


必须在list[start_index]元素的【逻辑】位置右边，否则取不出元素。


比如下面的几个例子都是合法的：


```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
alist[1:5]
[1, 2, 3, 4]
alist[1:-1]
[1, 2, 3, 4, 5, 6, 7, 8]
 alist[-8:6]
[2, 3, 4, 5]

```
当 step 是负数时，以list[start_index]元素位置开始， step做为步长到list[stop_index]元素位置（不包括）为止，从右向左截取，


start_index和stop_index不论是正数还是负数索引还是混用都可以，但是要保证 list[stop_index]元素的【逻辑】位置


必须在list[start_index]元素的【逻辑】位置左边，否则取不出元素。


比如下面的几个例子都是合法的：


```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
 alist[-1: -5: -1]
[9, 8, 7, 6]
alist[9: 5: -1]
[9, 8, 7, 6]
alist[-1:1:-1]
[9, 8, 7, 6, 5, 4, 3, 2]
alist[6:-8:-1]
[6, 5, 4, 3]
```


假设list的长度（元素个数）是length, start_index和stop_index在符合虚拟的逻辑位置关系时，


start_index和stop_index的绝对值是可以大于length的。比如下面两个例子：


```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
alist[-11:11]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
alist[11:-11:-1]
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```


另外start_index和stop_index都是可以省略的，比如这样的形式 alist[:], 被省略的默认由其对应左右边界起始元素开始截取。


看一下具体的实例：


```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
alist[:]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

```

## Python中切片操作的实现机制


（注：Python中前后双下划线名字的方法（函数）叫特殊方法，也有称魔术方法的，这是从ruby那里借用的。


通常特殊方法都是应当由解释器去调用的，对程序员的接口通常是看起来更简洁的方式，如常见的 len(list)


实质是解释器调用list.__len__()方法。）


实际上在Python中对list引用元素和形式优雅简洁的切片操作都是由解释器调用的list.__getitem__(x)特殊方法。

```python
help(list.__getitem__)
Help on method_descriptor:


__getitem__(...)
    x.__getitem__(y) <==> x[y]
```


其中x可以是个整数对象或切片对象。


alist[5] 和 alist.__getitem__(5) 是完全等效的。
```pyhotn
alist[5]
5
alist.__getitem__(5)
5
```


而切片操作是把切片对象作参数调用__getitem__()，
```python
help(slice)
Help on class slice in module builtins:


class slice(object)
 |  slice(stop)
 |  slice(start, stop[, step])
 |  
 |  Create a slice object.  This is used for extended slicing (e.g. a[0:10:2]).

```

见下面的例子。


```python
alist[1:7:2]
[1, 3, 5]
slice_obj = slice(1,7,2)
alist.__getitem__(slice_obj)
[1, 3, 5]
 
```


一些常用的切片操作

```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### 取前一部分
```python
alist[:5]
```
```
[0, 1, 2, 3, 4]
```


### 取后一部分
```python
alist[-5:]
```
```
[5, 6, 7, 8, 9]
```


### 取偶数位置元素
```python
alist[::2]
```
```
[0, 2, 4, 6, 8]
```


### 取奇数位置元素
```python
alist[1::2]
```
```
[1, 3, 5, 7, 9]
```


### 浅复制，等价于list.copy()更加面向对象的写法
```python
blist = alist[:]
blist
```
```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```


### 返回一个逆序列表
推荐reversed(list)的写法
```python
alist[::-1]
```
```
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```


### 在某个位置插入多个元素
```python
alist[3:3] = ['a','b','c']
alist
```
```
[0, 1, 2, 'a', 'b', 'c', 3, 4, 5, 6, 7, 8, 9]
```


### 在开始位置之前插入多个元素
```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
alist[:0] = ['a','b','c']
alist
```
```
['a', 'b', 'c', 0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

```

### 替换多个元素
```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
alist[0:3] = ['a','b','c']
alist
```
```
['a', 'b', 'c', 3, 4, 5, 6, 7, 8, 9]
```


### 删除切片
```python
alist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
del alist[3:6]
alist
```
```
[0, 1, 2, 6, 7, 8, 9]
```


参考链接：https://blog.csdn.net/xpresslink/article/details/77727507 
