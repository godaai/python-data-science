# Numpy库

## 1.概述

Numpy库主要作于科学计算，可用于对多维数组执行计算，库中提供了大量的库函数和操作，内置运算功能，包含实用的线性代数、傅里叶变换和随机数生成函数。Numpy库是scipy/pandas等库的基础。

Numpy库提供了两种基本的对象：ndarray（N-dimensional Array Object）和ufunc（Universal Function Object）。

ndarray是储存单一数据类型的多维数组，ufunc是能够对数组进行处理的函数。

## 2.常见用法

### 导入包


```python
import numpy as np
#导入numpy库，为简化代码简称为np
```

### 创建数组

#### np.array()函数

array 函数接受任何序列型参数，如列表、元组等。但是数组中有限的元素类型必须是一致的。


```python
#通过 array 函数可以创建一个指定类型的数组，只要在括号中传入一个参数，参数类型就是数组的类型
ar1 = np.array([1,2,3,4]) #列表list
ar2 = np.array((1,2,3,4)) #元组tuple
```

### 使用NumPy中函数创建数组

#### np.arange()函数

arange函数可用于特殊数组的创建，类似于range(),在给定间隔内放回均匀间隔的数值


```python
np.arange(10)
```


#### np.linspace()函数

linspace函数用于生成放回在间隔[start,stop]上计算num个均匀间隔的样本，也就是创建一个等差数列构成的一维数组。



常见参数：linspace(start,stop,num= ,endpoint=True/False)



```python
#start：起始值
#stop：终止值
#num：数列的个数
#endpoint：默认值为True，来决定是否包含终止值，如果为真则包含；否则不包括在内。
```


```python
a = np.linspace(1,5,5) #生成在1和5之间5个均匀间隔的数列，赋值为a
print(a) #显示该数列
```


#### np.concatenate()函数

将两个或者多个数组合并成一个新数组


```python
b = np.linspace(1,9,7)
print(b)
  
c= np.concatenate((a,b)) #将a,b两个数组合并成为c
c
```


#### np.ones()函数

np.ones(shape)根据shape生成一个全1数组，shape是元组类型


```python
np.ones((3,3,6)) #生成3个元素，每个元素下有3个维度，每个维度下有6个元素
#默认生成float类型，通过指定dtype改变数据类型
```


##### np.ones_like(a)

根据数组a的形状生成一个全1数组


```python
np.ones_like(a)
```


#### np.zeros()函数

np.zeros(shape)根据shape生成一个全0数组，shape是元组类型


```python
np.zeros((3,3,6)) #生成3个元素，每个元素下有3个维度，每个维度下有6个元素
#默认生成float类型，通过指定dtype改变数据类型
```

##### np.zeros_like(a)

根据数组a的形状生成一个全0数组


```python
np.zeros_like(a)
```


#### np.full()函数

np.full(shape,val)根据shape生成一个数组，每个元素值都是val


```python
np.full((3,3,6),6)#生成3个元素，每个元素下有3个维度，每个维度下有6个元素,元素值都为6
```


##### np.full_like(a,val)

根据数组a的形状生成一个数组,元素值全为val


```python
np.full_like(a,6)
```


#### np.eye()函数

np.eye(n)创建一个n*n单位矩阵，对角线为1，其余为0


```python
np.eye(5)
```


### 常用属性


```python
#生成数组ar，并查看相关属性
ar = np.array([[1,1,2,3,],
               [4,5,6,7],
               [8,9,10,11]])
ar
```


```python
ar.ndim #秩，轴的数量或数组维度的数量
```


```python
ar.shape #数组的尺度
```


```python
ar.size #元素的个数
```


```python
ar.dtype #元素类型
```


```python
ar.itemsize #每个元素的大小，以字节为单位
```


### 数组维度变换


```python
#使用上文生成的数组ar作为演示练习
ar #显示原数组ar
```


#### 不改变原数组

##### .reshape()函数

.reshape(shape)函数，不改变数组的元素，返回一个shape形状的数组


```python
ar.reshape((2,6)) #将数组变换为2*6形状的数组
```


```python
ar #显示原数组，结果显示未改变原数组
```



##### .flatten()函数

.flatten()函数，对数组进行降维，返回折叠后的一个一维数组


```python
ar.flatten() #将数组ar降维为一个一维数组
```


```python
ar #显示原数组，结果显示未改变原数组
```


#### 改变原数组

##### .resize()函数

.resize(shape)函数，返回一个shape形状的数组，功能与.reshape()函数一致，但是修改原数组。

（提示：当新数组可容纳数据少于原数据，按照原数据中行列顺讯排列shape个数据；如果多于，则新数组会按照原数组中的数据顺序进行填空）


```python
ar_new = ar.resize((2,6)) #将数组变换为2*6形状的数组，赋值为ar_new
ar #显示原数组，结果显示已经改变原数组
```


### 数组类型变换


```python
#使用上文生成的数组ar作为演示练习
ar #显示原数组ar
```



```python
ar.dtype #元素类型
```


#### .astype()函数

.astype(new_type)函数，对数组进行类型变化，基于对原始数据的拷贝创建一个新的数组。


```python
new_ar = ar.astype(np.float64) #将int类型元素修改为float类型
new_ar #显示新数组
```


```python
new_ar.dtype #查看新数组元素类型
```



### ndarray数组向列表转换

列表作为Python中原始的数据类型，虽然运算速度慢于numpy，但是在与原生Python语言相适应的程序中经常做此转换


```python
ar.tolist()
```


### 数组的索引及切片

从左往右索引时起始位置从0开始，从右往左索引时起始位置从-1开始。

#### 一维数组的情况


```python
#生成一维数组a
a = np.array([2,0,2,3,9,2,3,4])
a
```



##### 一维数组的索引


```python
#索引查找数组a中编号为4的元素，即查找第5个元素
#注：编程语言中编号从0开始
a[4]
```


##### 一维数组的切片


```python
#将数组a切片[起始编号：终止编号（不包含）：步长]
a[0:6:2]
```


#### 多维数组的情况


```python
#生成多维数组b
b = np.arange(36).reshape((3,4,3))
b
```




##### 多维数组的索引


```python
#查找b数组中，编号为0的元素中编号为3的维度下编号为2的元素
#索引b数组中第1个矩阵中第4行第3个元素
b[0,3,2]
```



```python
#查找b数组中，编号为1的元素中编号为2的维度下编号为1的元素
#索引b数组中第2个矩阵中第3行第2个元素
b[1,2,1]
```




```python
#查找b数组中，编号为-2的元素中编号为-2的维度下编号为-2的元素
#索引b数组中第2个矩阵中第3行第2个元素，与上述[1,2,1]索引元素一致
b[-2,-2,-2]
```


##### 多维数组的切片


```python
#选取一个维度进行切片
b[:,2,-3] #选择每个矩阵中从上往下第3行，从右往左第3个元素
```



```python
#对维度进行切片
#每个维度切片方法与一维数组相同
b[:,1:4:2,:] #维度起始值为1，终止值为4（不包含），步长为2
```



```python
#对每个维度切片
#使用步长跳跃切片
b[:,:,::2]
```



### 数组运算


```python
#生成数组x，y，z用作演示练习
x = np.array([[-5,-4,-3,-2,-1],
             [0,1,2,3,4],
             [5,6,7,8,9]])
x
```


```python
y = np.array([1,2,3,4])
y
```



```python
z = np.array([-1.2,1.4,1.5,1.7])
z
```



#### 一元函数：对一个数组执行元素级运算


```python
#计算数组各元素的绝对值
np.abs(x)
```



```python
#计算数组各元素的绝对值,元素类型为float
np.fabs(x)
```




```python
#计算数组各元素的平方根
np.sqrt(y)
```



```python
#计算数组各元素的平方
np.square(x)
```



```python
#计算数组各元素的自然对数
np.log(y)
```



```python
#计算数组各元素的10底对数
np.log10(y)
```



```python
#计算数组各元素的2底对数
np.log2(y)
```



```python
#计算数组各元素的ceiling值,即每个元素向上舍入
np.ceil(z)
```




```python
#计算数组各元素的floor值,即每个元素向下舍入
np.floor(z)
```



```python
#计算数组各元素四舍五入的值
np.rint(z)
```



```python
#将数组各元素的小数和整数部分以两个独立数组形式返回
np.modf(z)
```


除去以上常用的NumPy库中的一元函数，还有以下函数较为常用：

①计算数组各元素的普通型三角函数和双曲型三角函数：

np.cos(), np.cosh();np,sin(),np.sinh();np.tan(),np.tanh().

②计算数组各元素的指数值:

np.exp()


③计算数组各元素的符号值

np.sign()

#### 统计函数：对一个数组行、列及整体运算

axis=0/1，该参数不指定时默认求整体，axis=0对列计算，axis=1对行计算。（以求平均值为例，下同不再演示）


```python
# 求整体平均值
x.mean()

```


```python
# 对每列平均值
x.mean(axis=0)

```



```python
# 对每行求平均值
x.mean(axis=1)
```


```python
# 对整体求最小值
x.min()
```



```python
# 对整体求最大值
x.max()
```



```python
# 对整体求标准差
x.std()
```



```python
# 对整体求方差
x.var()
```




```python
# 对整体求和
x.sum()
```




#### 二元函数：对两个数组进行运算


```python
#两个数组各元素进行对应运算
y+z
```




```python
y-z
```





```python
y*z
```



```python
# 两个数组间进行元素级最大值计算
np.fmax(y,z)
```






```python
np.maximum(y,z)
```




```python
# 两个数组间进行元素级最小值计算
np.fmin(y,z)
```



```python
np.minimum(y,z)
```


```python
# 元素及模运算
np.mod(y,z)
```


```python
# 将数组z中各元素值的符号赋值给数组y对应元素
np.copysign(y,z)
```


除此之外，还可以对两个数组进行算数比较，产生布尔型数组


函数: <（小于）， >（大于）， >=（大于等于）， <=（小于等于）， ==（等于）， !=（不等于）


```python
# 举例，y与x进行元素级比较是否相等
y==z #结果显示不相等
```


### 随机数生成 np.random

#### .rand()函数：均匀分布

rand(d0,d1,..,dn)函数，根据d0-dn创建随机数数组，生成一个[0,1)之间的随机浮点数或N维浮点数组



```python
np.random.rand(2,2,3) # 随机生成2个，维度为2，维度下元素为3的浮点数组，服从[0,1)均匀分布
```




#### .randn()函数：正态分布

randn(d0,d1,..,dn)函数，根据d0-dn创建随机数数组，生成一个的随机浮点数或N维浮点数组，服从标准正态分布



```python
np.random.randn(2,2,3) # 随机生成2个，维度为2，维度下元素为3的浮点数组，服从标准正态分布
```




#### .randint()函数：制定随机生成范围

randint(low[,low,shape]))函数，根据shape创建随机整数或整数数组，范围为[low,high)


```python
np.random.randint(100,400,(2,3,4)) 
# 在[100,400)之前随机生成2个维度为3，维度下元素为4的随机整数数组
```




#### .seed()：随机种子

seed(s)函数，随机数种子，s是给定的种子值，用于保证每次随机生成的数组或数与上一次相同


```python
np.random.seed(10) #规定随机种子为10，则每次运行下方随机数生成代码生成的随机数相同
np.random.randint(100,400,(2,3,4)) 
# 在[100,400)之前随机生成2个维度为3，维度下元素为4的随机整数数组
```





```python
np.random.seed(10) #规定随机种子为10，则每次运行下方随机数生成代码生成的随机数相同
np.random.randint(100,400,(2,3,4)) 
# 在[100,400)之前随机生成2个维度为3，维度下元素为4的随机整数数组
```





```python
np.random.seed(2) #更换随机种子
np.random.randint(100,400,(2,3,4)) 
# 在[100,400)之前随机生成2个维度为3，维度下元素为4的随机整数数组
```





## 3.数据输入输出

### （1）保存数组：np.save(file,array)

保存一个数组到二进制的文件中，保存格式是.npy

### （2）读取数组：np.load(file)

读取npy文件到内存


```python
arr1 = np.array([1,2,3])
#保存数组
np.save('arr1.npy', arr)
#读取数组
np.load('arr1.npy')
```



### （3）保存文件：np.savetxt()

np.savetxt ( fname , array , fmt='%.18e ', delimiter=None )

参数： 
  
    fname：写入的文件、字符串或产生器，可以是.gz或bz.2的压缩文件
    array：存入文件的数组
    fmt：写入文件的格式，如%d, %2f,%18e
    delimiter：分割字符串，一般默认为空格


### （4）读取文件：np.loadtxt()

np.loadtxt ( fname , dtype = np.float, array ,delimiter=None , unpack = True/False )

参数： 
  
    fname：读入文件的文件来源，可以是文件、字符串或产生器，可以是.gz或bz.2的压缩文件
    dtype：数据类型
    delimiter：分割字符串，一般默认为空格
    unpack：如果为真，读入属性将分别写入不同变量



```python
arr2 = np.arange(50).reshape(5,10)
#保存CSV格式文件
np.savetxt('arr2.csv',arr2,fmt='%d',delimiter=',')
#读取CSV格式文件
np.loadtxt('arr2.csv',dtype=np.int64, delimiter=',', unpack=True)
```




