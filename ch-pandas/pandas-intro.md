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

  ```{.python .input}
  s.describe()
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

```{.python .input}
df.iloc[2:5, 0:4]
```

