# 多表操作
:label:`dataframe-merge&concat`

Pandas 提供了关于多表之间的合并和连接操作函数，分别是 merge() 和 concat() 函数。需要注意的是，对于多表之间的纵向合并，必须保证多表的列数和数据类型一致；对于多表之间的水平扩展，必须保证多表要有共同的匹配字段。

```{.python .input}
# Hide outputs
# Hide code
import urllib.request
import zipfile
import os
import pandas as pd
%本数据集来自于 Kaggle 的“European Soccer Database”数据集。数据集囊括2008年至2016年赛季的11个国家顶级联赛中超过25000场的比赛，包括上千支球队的数据。
df1 = pd.read_csv('Team_Attributes.csv') 
df1
df2 = pd.read_csv('Team.csv')
df2
```

### merge 操作
Pandas的merge操作是用于合并两个或多个DataFrame对象的功能，类似于SQL中的JOIN操作。这个函数允许你根据一个或多个键（key）来合并数据，这些键可以是列名或者索引。merge操作的目的是将不同的数据集合并成一个新的数据集，以便进行更复杂的数据分析和处理。

该函数最大的缺点是每次只能操作两张数据表的连接，如果有 n 张表需要连接，则必须经过 n-1 次的 merge 函数使用。

函数形式为：`pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, sort=False,suffixes=('_x','_y'))`

- left: 指定需要连接的主表；

- right: 指定需要连接的辅表；

- how: 指定连接方式，默认为 inner 内连接，还有其他选项，如左连接 left，右连接 right 和外连接 outer；
  
  内连接，也称为交集连接。只保留两个DataFrame中连接键相匹配的行，丢弃不匹配的行。结果包含连接键在两个DataFrame中都存在的数据。

  外连接，也称为并集连接。保留两个DataFrame中的所有行，对于不匹配的行，用NaN或者指定的填充值填充。结果包含了所有数据，其中一些数据可能在一个DataFrame中存在，而在另一个DataFrame中不存在。

  左连接。以左侧DataFrame为基准，保留左侧DataFrame中的所有行，并添加右侧DataFrame中与之匹配的行。对于不匹配的行，用NaN或者指定的填充值填充。

  右连接。以右侧DataFrame为基准，保留右侧DataFrame中的所有行，并添加左侧DataFrame中与之匹配的行。对于不匹配的行，用NaN或者指定的填充值填充。


- on: 指定连接两张表的共同字段；

- left_on: 指定主表中需要连接的共同字段；

- right_on: 指定辅表中需要连接的共同字段；

- left_index: 布尔类型参数，是否将主表中的行索引用作表连接的共同字段，默认为 False；

- right_index: 布尔类型参数，是否将辅表中的行索引用作表连接的共同字段，默认为 False；

- sort: 布尔类型参数，是否对连接后的数据按照共同字段排序，默认为 False;

- suffixes: 如果数据连接的结果中存在重叠的变量名，则使用各自的前缀进行区分。

例如，对 df1 和 df2 按照左连接的方式合并，共同字段是 `team_api_id`，其他为默认。则可以得到每个队伍的具体名称。因为是基于 df1 的左连接，因此 merge 操作后的行数和 df1 的行数相同。
```{.python .input}
pd.merge(left = df1, right = df2, how = 'left', left_on = 'team_api_id', right_on = 'team_api_id')
```

尝试右连接，即基于 df2。由于数据集本身 df2 囊括了比赛的所有队伍以及对应的国家，因此，对于 df1 队伍表现中没有包括的队伍，会用 NaN 代替没有的属性，操作后的行数增加。
```{.python .input}
pd.merge(left = df1, right = df2, how = 'right', left_on = 'team_api_id', right_on = 'team_api_id')
```

内连接（类似于求交集）：
```{.python .input}
pd.merge(left = df1, right = df2, how = 'inner', left_on = 'team_api_id', right_on = 'team_api_id')
```

外连接（类似于求并集）：
```{.python .input}
pd.merge(left = df1, right = df2, how = 'outer', left_on = 'team_api_id', right_on = 'team_api_id')
```

### concat 操作
concat 是 Pandas 库中用于合并（连接）多个 DataFrame 或 Series 对象的函数。它允许你在不同的轴（行或列）上将多个数据结构进行堆叠、连接，以创建一个新的数据结构。concat 并不执行数据库样式的合并操作，而是简单地将数据结构堆叠在一起。

函数形式为：`pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False, keys=None)`

- objs: 指定需要合并的对象；
     
- axis: 指定合并的轴，默认为 0 ，表示合并多个数据的行；如果为 1 ，表示合并多个数据的列；
     
- join: 指定合并的方式，默认为 outer，表示合并所有数据，如果改为 inner，表示合并公共部分的数据；
    
- join_axes: 合并数据后，指定保留的数据轴；
    
- ignore_index: 布尔类型的参数，表示是否忽略原数据集的索引，默认为 False，如果设置为 True，就表示忽略原索引并生成新索引；
    
- keys: 为合并后的数据添加新索引，用于区分各个数据部分。

例如，默认按照行合并，则是将 df1 和 df2 纵向堆叠起来，可以看到操作后的行数就是两个 DataFrame 的行数和，第一列由0，1，.... 299 是两个 DataFrame 的 index堆叠，其实是 0，1，...1457，0，1....299的过程。
```{.python .input}
pd.concat([df1,df2])
```

设置 join 参数为 inner，则显示公共部分。
```{.python .input}
pd.concat([df1,df2], join = 'inner')
```



  
