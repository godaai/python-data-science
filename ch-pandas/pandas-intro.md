# pandas 入门

pandas 库是一个流行的 Python 数据处理和分析库，它提供了用于处理和操作数据的强大工具和数据结构。Pandas 的核心数据结构包括 Series（序列）和 DataFrame（数据框），它们使数据的读取、清理、转换和分析变得更加容易。

### 导入 Pandas

```{.python .input}
!pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple
```

导入 pandas：

```{.python .input}
import pandas as pd
```

### Series

在 pandas 中，`Series` 是一种一维的带标签的数组状数据结构。

我们首先以 4 个数创建一个 `Series`，并命名为 `my series`。

```{.python .input}
s = pd.Series([1, 2, 3, 4], name = 'my series')
```

`Series` 是一个数组状数据结构，数组最重要的结构是索引（index）。index 主要用于标记第几个位置存储什么数据。`pd.Series()` 中不指定 index参数时，默认从0开始，逐一自增，形如： 0，1，... 

- Series 支持计算操作。

  ```{.python .input}
  s * 100
  ```

- Series 支持描述性统计。

  获得所有统计信息。
  ```{.python .input}
  s.describe()
  ```

  分别计算平均值，中位数和标准差。
  ```{.python .input}
  s.mean()
  ```

  ```{.python .input}
  s.median()
  ```

  ```{.python .input}
  s.std()
  ```

- Series 的索引很灵活。

  ```{.python .input}
  s.index = ['number1','number2','number3','number4']
  ```

  这时，`Series` 就像一个 Python 中的字典 `dict`，可以使用像 `dict` 一样的语法来访问 `Series` 中的元素，其中 `index` 相当于 `dict` 的键 `key`。例如，使用 `[]` 操作符访问 `number1` 对应的值。

  ```{.python .input}
  s['number1']
  ```
  
  又例如，使用 `in` 表达式判断某个索引是否在 Series 中。

  ```{.python .input}
  'number1' in s
  ```

### DataFrame

`DataFrame` 可以简单理解为一个 Excel 表，有很多列和很多行。
`DataFrame` 的列（column）表示一个字段；`DataFrame` 的行（row）表示一条数据。`DataFrame` 常被用来分析像 Excel 这样的、有行和列的表格类数据。Excel 也正在兼容 `DataFrame`，使得用户在 Excel 中进行 pandas 数据处理与分析。

#### 创建 DataFrame
`DataFrame` 可以来自列表、字典、文件等。

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
data = {'Column1': [1, 2], 'Column2': [3, 4]}
df = pd.DataFrame(data)
```

- 基于文件创建

对于不同类型的文件，使用不同的函数，比如 `read_csv` 读取 csv 类型的数据。`df = pd.read_csv('csv 文件的绝对路径')` 用来读取一个 csv 文件，`df =  pd.read_excel('excel 文件的绝对路径')` 用来读取一个 excel 文件

> 注：pd.read_csv 默认分隔符为逗号，pd.read_table 默认分隔符为换行符。它们还支持许多其他参数，可以使用 `help()` 函数查看。
> 例：help(pd.read_csv)

### 案例：PWT

[PWT](https://www.rug.nl/ggdc/productivity/pwt/) 是一个经济学数据库，用于比较国家和地区之间的宏观经济数据，该数据集包含了各种宏观经济指标，如国内生产总值（GDP）、人均收入、劳动力和资本等因素，以及价格水平、汇率等信息。我们先下载，并使用 pandas 简单探索该数据集。

```{.python .input}
import urllib.request
import zipfile
import os

folder_path = os.path.join(os.getcwd(), "./data/pwt")
download_url = "https://www.rug.nl/ggdc/docs/pwt70_06032011version.zip"
file_name = download_url.split("/")[-1]
if not os.path.exists(folder_path):
    # 创建文件夹
    os.makedirs(folder_path)
    print(f"文件夹不存在，已创建。")

    zip_file_path = os.path.join(folder_path, file_name)
    
    urllib.request.urlretrieve(download_url, zip_file_path)
    with zipfile.ZipFile(zip_file_path, 'r') as zip_ref:
        zip_ref.extractall(folder_path)
    print("数据已下载并解压缩。")
else:
    print(f"文件夹已存在，无需操作。")
```

#### 查看数据

使用 `read_csv()` 读取数据。

```{.python .input}
import pandas as pd
df = pd.read_csv(os.path.join(folder_path, "pwt70_w_country_names.csv"))
```

`head()` 函数可以指定查看前 n 行。

```{.python .input}
n = 5
df.head(n)
```

`tail()` 函数指定查看后 n 行。

```{.python .input}
df.tail(n)
```

`info()` 函数可以查看数据基本信息，包括字段类型和非空值计数。

```{.python .input}
df.info()
```

#### 数据切片

实际中，我们常常想要选取感兴趣的数据子集。

- 切片选择行
  
  从第 2 行到第 5 行（不包括第 5 行）：
    
  ```{.python .input}
  df[2:5]
  ```

- 列名索引选择列

  要选择列，我们可以传递一个列表，其中包含以字符串表示的所需列的名称。
  
  ```{.python .input}
  df[['country', 'tcgdp']]
  ```

  如果只选取一列，df['country'] 等价于 `df.country`。

- `iloc` 方法选择，形式应为 `.iloc[rows, columns]`

  选择第 2 行到第 5 行（不包括第 5 行），第 0 列到第 4 列（不包括第 4 列）。
  ```{.python .input}
  df.iloc[2:5, 0:4]
  ```

- `loc`方法选择，可以使用整数和标签混合的方法

  选择第 2 行到第 5 行（不包括第 5 行），`country` 和 `tcgdp` 列。
  ```{.python .input}
  df.loc[df.index[2:5], ['country', 'tcgdp']]
  ```

- 条件选择
  > 用`[]`操作符。
  
  例：选取 POP 大于 20000 的行。

  `df.POP >= 20000`返回一系列布尔值，则`df[]`返回条件判断为 True 的行。
  
  ```{.python .input}
  df[df.POP >= 20000]
  ```
  > 等价于`.query()`方法，且这种方法在处理大规模数据时更快。
  ```{.python .input}
  df.query("POP >= 20000")
  ```

    该方法允许不同列之间做算术运算。
  
    例：选择 cc 列和 cg 列的和大于 80 并且 POP 小于 20000 的行。
    ```{.python .input}
    df[(df.cc + df.cg >= 80) & (df.POP <= 20000)]
    ```
  
    同样有`.query()`的等价方法。
    ```{.python .input}
    df.query("cc + cg >= 80 & POP <= 20000")
    ```

  > `.loc[]`和`[]`操作符有相同的用法。
  ```{.python .input}
  df.loc[df.cc == max(df.cc)] #可以选择cc列最大值的那一行
  ```

  > `.loc[ , ]`第一个参数接受条件，第二个参数接受我们想要返回的列列表。
  ```{.python .input}
  df.loc[(df.cc + df.cg >= 80) & (df.POP <= 20000), ['country', 'year', 'POP']]
  ```

#### Apply 方法

`.apply()`方法也是一个 pandas 中常用的函数。它可以对每行/列（也可以对基于切片选择特定的列或行）应用一个函数并返回一个`Series`。该函数可以是一些内置函数，如max函数、lambda函数或自定义函数。

- max 函数
    
  例：返回`year`, `POP`, `XRAT`, `tcgdp`, `cc`, `cg`列的最大值（当该列有空值时，无法计算最大值，则返回空值）。
    ```{.python .input}
    df[['year', 'POP', 'XRAT', 'tcgdp', 'cc', 'cg']].apply(max)
    ```

- lambda 函数，形式为 `lambda arguments: expression`
  
  `arguments`是参数列表，可以指定一个或多个参数，就像定义普通函数一样；`expression`是一个表达式，定义了lambda函数的计算逻辑，通常包括参数，用于返回计算结果。

  lambda函数通常用于一次性的、简单的操作。

  例：对每一行返回自身：
    ```{.python .input}
    df.apply(lambda row: row, axis=1)
    ```
  axis = 1 设置函数对每一行操作；axis = 0 设置函数对每一列操作；默认axis = 0。
  
  例：和`.loc[]`一起使用，进行更高级的数据切片。
  
     `.apply()`返回对每一行做条件判断的一系列布尔值，以`[]`操作选择部分列。
    ```{.python .input}
    complexCondition = df.apply(
    lambda row: row.POP > 40000 if row.country in ['Argentina', 'India', 'South Africa'] else row.POP < 20000, 
    axis=1), ['country', 'year', 'POP', 'XRAT', 'tcgdp']
    complexCondition
    ```
   
    ```{.python .input}
    df.loc[complexCondition]
    ```
    则返回符合条件的行和列。

#### 数据框更改

更改`DataFrame`的部分值（行、列）在数据清洗过程中十分重要。

- `.where()`方法保留行，并用其他值替代其余行。

  例：找出 POP 大于 20000 的行，其余部分用 `False` 替代（可以自行选择任意值，比如 0 、1 等等）。
  
    ```{.python .input}
    df.where(df.POP >= 20000, False)
    ```
  
- `.loc[]`方法指定想修改的列，并赋值。
  
  例：找出 cg 列的最大值，赋值为 1 。
  
    ```{.python .input}
    df.loc[df.cg == max(df.cg), 'cg'] = 1
    df
    ```

- `.apply()`函数，根据自定义函数修改行和列。
   
  例：将 POP 小于 10000 的修改为 1， 将 XRAT 缩小十倍。
  
    ```{.python .input}
    def update_row(row):
      # modify POP
      row.POP = 1 if row.POP<= 10000 else row.POP

      # modify XRAT
      row.XRAT = row.XRAT / 10
      return row

    df.apply(update_row, axis=1)
    ``` 

- `.applymap()`函数，修改数据框中的所有单独元素。
   
  例：将 DataFrame 中的数值型元素保留两位小数。
  
    ```{.python .input}
    df.applymap(lambda x : round(x,2) if type(x)!=str else x)
    ```
    
- `del`方法删除指定列。
  
  例：删除`ppp`列。
  
    ```{.python .input}
    del df['ppp']
    df
    ```

- `df['NewColumn'] = values`增加新列。

  例：增加一列 `GDP percap` 显示人均GDP。
    ```{.python .input}
    df['GDP percap'] = df['tcgdp'] / df['POP']
    df
    ```
 

  
#### 缺失值处理
数据缺失情况在现实问题中非常普遍。

- `.isna()`或`df.isnull()`方法查看缺失值。
  函数返回 DataFrame 每个元素的布尔值，如果为空，为 True，非空则为 False。

  例：通过`.sum()`查看每一列的空缺值计数。
    ```{.python .input}
    df.isnull().sum()
    ```

  
- `.dropna()` 函数删除有缺失值的行或列。

  具体形式：`df.dropna(axis=0, how='any', inplace=False)`
  
   axis：指定要删除的轴。axis=0 表示删除行（默认），axis=1 表示删除列；how：指定删除的条件。how='any' 表示删除包含任何缺失值的行（默认），how='all' 表示只删除所有值都是缺失值的行；inplace：指定是否在原始 DataFrame 上进行修改，默认为 False，表示不修改原始 DataFrame，而是返回一个新的 DataFrame。

    例：删除包含任何缺失值的行。
    ```{.python .input}
    df.dropna()
    ```
  
- `.fillna()`函数填补数据框中的缺失值。
  
  例：将 DataFrame 中的缺失值用0填充。
  
    ```{.python .input}
    df = df.fillna(0)
    df
    ```
#### 数据排序

- `.sort_values()`方法，按照指定列排序。

  形如`.sort_values(by='column1', ascending = True/False)`，ascending 参数设置为 True 升序，False 为降序。
  
  例：按照 POP 列升序排列。
  ```{.python .input}
  df = df.sort_values(by='POP', ascending=True)
  df
  ```

- `.sort_index()`方法，按照索引 `index` 排序。
  
  ```{.python .input}
  df.sort_index()
  ```

  

  
  















