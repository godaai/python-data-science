# 分组汇总
:label:`dataframe-groupby`

分组汇总操作中，会涉及分组变量、度量变量和汇总统计量。Pandas 提供了 groupby 方法进行分组汇总。

```{.python .input}
# Hide outputs
# Hide code
import urllib.request
import zipfile
import os

import pandas as pd

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

df = pd.read_csv(os.path.join(folder_path, "pwt70_w_country_names.csv"))
```

### 分组变量

在进行分组汇总时，分组变量可以有多个。

例如，按照 country 和 year 顺序对 tcgdp 查询平均值，此时在 groupby 后接多个分组变量，以列表形式写出。
```{.python .input}
df.groupby(['country','year'])[['tcgdp']].mean()
```
结果中产生了多重索引，指代相应组的情况。

### 度量变量
在进行分组汇总时，汇总变量也可以有多个。

例如按照 year 汇总 tcgdp 和 POP，在 groupby 后直接使用`[]`筛选相应列，再接汇总统计量。

```{.python .input}
df.groupby(['year'])['tcgdp','POP'].mean()
```
  

### 汇总统计量
groupby 后可接的汇总统计量有：
- mean - 均值

- max - 最大值

- min - 最小值

- median - 中位数

- std - 标准差

- mad - 平均绝对偏差

- count - 计数

- skew - 偏度

- quantile - 指定分位数

这些统计量可以直接接 groupby 对象使用，此外，`agg`方法提供了一次汇总多个统计量的方法。

例如，汇总各个国家 country 人口 POP 的均值、最大值、最小值。

```{.python .input}
df.groupby(['country'])['POP'].agg(['mean','min','max'])
```

### 多重索引

在进行分组汇总操作时，产生的结果并不是常见的二维表数据框，而是具有多重索引的数据框。 Pandas 开发者设计这种类型的数据框是借鉴了 Excel 数据透视表的功能。

例如，按照 country 和 year 顺序对 tcgdp 和 POP 进行分组汇总，汇总统计量为最小值和最大值。
```{.python .input}
df.groupby(['country','year'])['tcgdp','POP'].agg(['min','max'])
```

此时数据框中有两个行索引和两个列索引。需要筛选列时，第一个`[]`筛选第一重列索引，第二个`[]`筛选第二重列索引。

例如，查询各个国家 country 各年 year 人口 POP 的最小值。
```{.python .input}
df_query = df.groupby(['country','year'])['tcgdp','POP'].agg(['min','max'])
df_query['POP']['min']
```
  
