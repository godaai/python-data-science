# Pandas

Pandas库是一个流行的Python数据处理和分析库，它提供了用于处理和操作数据的强大工具和数据结构。Pandas的核心数据结构包括Series（序列）和DataFrame（数据框），它们使数据的读取、清理、转换和分析变得更加容易。

## 导入Pandas

```{.python .input}
!pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple
```

- 导入pandas
```{.python .input}
import pandas as pd
```

# Pandas

Pandas库是一个流行的Python数据处理和分析库，它提供了用于处理和操作数据的强大工具和数据结构。Pandas的核心数据结构包括Series（序列）和DataFrame（数据框），它们使数据的读取、清理、转换和分析变得更加容易。

## 导入Pandas

- 安装pandas
```{.python .input}
!pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple
```
- 导入pandas
```
import pandas as pd
```


## Series
在 pandas 中，Series 是一种一维的带标签的数组状数据结构。

我们首先以4个数创建一个Series，并命名为 `my series`。

```{.python .input}
s = pd.Series([1,2,3,4], name = 'my series')
```
`pd.Series()`中不指定index（索引）参数时，默认为0，1，... 开始的index，也就是该数组的标签。

- Series 支持计算操作。

  ```{.python .input}
  s * 100
  ```
- Series 支持描述性统计。
  
  ```{.python .input}
  s.describe()
  ```

- Series 的索引很灵活。
  
  ```{.python .input}
  s.index = ['number1','number2','number3','number4']
  ```
  这时，Series就像一个`python`中的`dict（字典）`元素，可以使用一样的语法，其中`index`相当于`dict`的`key（键）`。
  
  >例：使用`[]`操作符访问 index 对应的值。

    ```{.python .input}
    s['number1']
    ```
  >例：使用 `<index>in<series>` 表达式判断 index 是否在Series中.
      ```{.python .input}
      'number1' in s
      ```
  
## DataFrame

`DataFrame`本质上是`Series`的集合，包含多列，每个变量对应一列，类似于高度优化的Excel表格。因此，它是一个强大的工具，用于表示和分析自然组织成行和列的数据，通常为单个行和单个列提供描述性索引。

### 创建DataFrame
`DataFrame`可以来自列表、字典、文件等。

- 基于列表创建
```{.python .input}
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 22]
cities = ['New York', 'San Francisco', 'Los Angeles']
data = {'Name': names, 'Age': ages, 'City': cities}
df = pd.DataFrame(data)
```

- 基于字典创建
```{.python .input}
data = {'Column1': [value1, value2, ...], 'Column2': [value1, value2, ...]}
df = pd.DataFrame(data)
```

- 基于文件创建（对于不同类型的文件，使用不同的函数）
```{.python .input}
df =  pd.read_csv('csv文件的绝对路径')
df =  pd.read_excel('excel文件的绝对路径')
df =  pd.read_table('txt文件的绝对路径', delimiter='\t') #delimiter参数指定数据分隔符，可以为换行符`\t`，也可以为`,`等
```
>注：pd.read_csv默认分隔符为逗号，pd.read_table默认分隔符为换行符。它们还支持许多其他参数，可以使用`help()`函数查看。  
  >例：help(pd.read_csv)

### 查看数据

```{.python .input}
df.head(n)
df.tail(n)
```
`.head`函数可以指定查看前n行；`.tail`函数指定查看后n行。

```{.python .input}
df.info()
```
`.info`函数可以查看数据基本信息，包括字段类型和非空值计数。

### 数据切片

实际中，我们常常想要选取感兴趣的数据子集。

以 https://www.rug.nl/ggdc/productivity/pwt/pwt-releases/pwt-7.0 中的部分数据为例，读取为df。

|   | country       | country isocode | year | POP         | XRAT      | tcgdp        | cc        | cg        |
|---|---------------|-----------------|------|-------------|-----------|--------------|-----------|-----------|
| 0 |     Argentina |             ARG | 2000 |   37335.653 |  0.999500 | 2.950722e+05 | 75.716805 |  5.578804 |
| 1 |     Australia |             AUS | 2000 |   19053.186 |  1.724830 | 5.418047e+05 | 67.759026 |  6.720098 |
| 2 |         India |             IND | 2000 | 1006300.297 | 44.941600 | 1.728144e+06 | 64.575551 | 14.072206 |
| 3 |        Israel |             ISR | 2000 |    6114.570 |  4.077330 | 1.292539e+05 | 64.436451 | 10.266688 |
| 4 |        Malawi |             MWI | 2000 |   11801.505 | 59.543808 | 5.026222e+03 | 74.707624 | 11.658954 |
| 5 |  South Africa |             ZAF | 2000 |   45064.098 |  6.939830 | 2.272424e+05 | 72.718710 |  5.726546 |
| 6 | United States |             USA | 2000 |  282171.957 |  1.000000 | 9.898700e+06 | 72.347054 |  6.032454 |
| 7 |       Uruguay |             URY | 2000 |    3219.793 | 12.099592 | 2.525596e+04 | 78.978740 |  5.108068 |

- 切片选择行
  ```{.python .input}
  df[2:5]
  ```
  |   | country | country isocode | year | POP         | XRAT      | tcgdp        | cc        | cg        |
  |---|---------|-----------------|------|-------------|-----------|--------------|-----------|-----------|
  | 2 |   India |             IND | 2000 | 1006300.297 | 44.941600 | 1.728144e+06 | 64.575551 | 14.072206 |
  | 3 |  Israel |             ISR | 2000 |    6114.570 |  4.077330 | 1.292539e+05 | 64.436451 | 10.266688 |
  | 4 |  Malawi |             MWI | 2000 |   11801.505 | 59.543808 | 5.026222e+03 | 74.707624 | 11.658954 |
   > 注：index为5的行取不到。
   
- 列名索引选择列
  
    > 要选择列，我们可以传递一个列表，其中包含以字符串表示的所需列的名称。
  
    ```{.python .input}
    df[['country', 'tcgdp']]
    ``` 
  
     > 如果只选取一列，df['country']等价于df.country。
       
  |   | country       | tcgdp        |
  |---|---------------|--------------|
  | 0 |     Argentina | 2.950722e+05 |
  | 1 |     Australia | 5.418047e+05 |
  | 2 |         India | 1.728144e+06 |
  | 3 |        Israel | 1.292539e+05 |
  | 4 |        Malawi | 5.026222e+03 |
  | 5 |  South Africa | 2.272424e+05 |
  | 6 | United States | 9.898700e+06 |
  | 7 |       Uruguay | 2.525596e+04 |
     
- `iloc`方法选择，形式应为`.iloc[rows, columns]`

  ```{.python .input}
  df.iloc[2:5, 0:4]
  ```
  
