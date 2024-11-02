---
title: numpy
date: 2023-02-09 10:33:02
description: numpy数组学习
tags:
- 数据分析
categories:
- python
- python数据分析
music:
 server: netease   # netease, tencent, kugou, xiami, baidu
 type: playlist        # song, playlist, album, search, artist
 id: 8109052701      # song id / playlist id / album id / search keyword
author: 林宇轩
---

**通过数组进行运算的**

## Array创建

 **数组数据类型必须统一，且必须是有序的集合**

1. `Code`np.array  （**元素类型不一致会强转） **
2. np.routines函数

```python
# 用1来填充生成高维数组，shape决定行数列数，dtype则指数据类型
np.ones(shape,dtype = None, order = 'C')
np.ones(shape(3,2),dtype = np.int) # 三行两列
np.ones(shape(3,5,2),dtype = np.int) # 三行五列的二维数组
#  np.zeros  用0填充
#  np.full(shape,fill_value = 6,dtype = None,order = "C")  # 用6来填充  

# 对角线为一其他为0
np.eye(N,M = none ,k = 0,dtype = float)
np.eye(3,M = none ,k = 0,dtype = float) # 三维矩阵
np.eye(3,M = 2 ,k = 1,dtype = float) # 三行两列 向右偏一个 

# 生成等差数列
# 按个数取，endpoint表示是否取末端
np.linspace(start,stop,num = 10 ,endpoint = True,restep = False,dtype = None) 
np.linspace(0.10.num = 11 endpoint = True) 
# 按步长取 ,不取末端
np.arrange(start，stop,step ,dtype = None)

# 生成随机数
np.random.randint(low,high,size,dtype) # size即前面的shape

# 正态分布函数
np.random.randn(d0,d1,...dn)  # 标准正态分布 参数表示维数
np.random.normal(loc,scale,size) # 普通正态分布 loc 期望值，scale方差，size个数

# 生成0到1的随机数
np.random.random(size)
```

## Array属性

- ndim:维度
- shape：形状（各维度的形状长度）
- size : 长度大小，即元素个数
- dtype : 数据类型

## Array的基本操作

### 访问

- 直接访问  `Code`np[roe_dinex,col_index]
- 间接访问 `Code`np[roe_dinex][col_index]...
- 行访问 `Code`np[roe_dinex,:]
- 列访问 `Code`np[:,col_index]
- 多个元素访问（**索引切片都是左闭右开区间**) 
  1.元素连续`Code`np[1:3,2:5]  （1:3为第一维，2:5为第二维）
  2.元素不连续 **使用列表作为索引值 
  
  **3.使用bool列表访问,True对应的值会被返回 【**BOOL的长度必须列长匹配**  】

```python
array([[1, 2, 3, 9, 3],
       [2, 8, 4, 6, 8],
       [5, 6, 1, 8, 8],
       [7, 3, 3, 1, 5],
       [9, 3, 6, 1, 5]])
np[1:3,1:3] # 第一维取1和2 第二维取1和2
np[[1,2],1]  # 第一维取1和2，第二维取1
```

### 切片

**左闭右开**

```python
# 采用：：两个冒号来指定步长
np[::2] # 步长为二
np[::-1] # 倒序输出
```

### 变形

**参数是一个元组，且变前变后元素数量不能变**

```python
n1 = np.random.randint(1,10,size = (5,5))
n1.reshape((-1,1))  # 变成一列
n1.reshape((1,-1))  # 变成一行
```

### 级联

**两个级联的方向上数据数量要一致**

```python
np.concatenate(array_like,axis) # array_like列表类型 axis 默认为0 竖着级联 axis = 1 横着连
np.hstack()  # 横向
np.vstack()  # 竖向
```

### 切分

```python
np.split(array,indices_or_sections,axis) # indices_or_sections表示切成的份数
np.split(array,indices_or_sections = [1,4] ,axis) # 0:1,1:4,4:  b不可以整除用切片规范 
np.hsplit()  # 横向
np.vsplit()  # 竖向
```

### 副本

- 深复制 - **内存空间** `Code`copy()
- 浅复制 - **地址  **

## Array聚合运算

**默认把整个数组的数据进行运算**
**axis参数控制行列，行为1  列为 0**

```python
# 求和
n1.sum

# 空值
np.nan 
```

## Array矩阵运算


**numpy**默认的运算符方式，数组中**对应位置的数据**相互运算

```python
# 矩阵积
np.dot
```

### 广播机制（数据结构不同时）

**拓展补充数据，拓展维度**

## 排序

### 全排序

`Code` np.sort 
`Code` arry.sort

### 部分排序

 `Code`np.partition(a,k)
** k为正取最大k个，k为负取最小k个**

