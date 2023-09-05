# Pandas

Pandas库是一个流行的Python数据处理和分析库，它提供了用于处理和操作数据的强大工具和数据结构。Pandas的核心数据结构包括Series（序列）和DataFrame（数据框），它们使数据的读取、清理、转换和分析变得更加容易。

## 导入Pandas

- 安装pandas
```py
!pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple
```
- 导入pandas
```
import pandas as pd
```


## Series
在 pandas 中，Series 是一种一维的带标签的数组状数据结构。

我们首先以4个数创建一个Series，并命名为 `my series`。

```py
s = pd.Series([1,2,3,4], name = 'my series')
```
`pd.Series()`中不指定index（索引）参数时，默认为0，1，... 开始的index，也就是该数组的标签。

- Series 支持计算操作。

  ```py
  s * 100
  ```
- Series 支持描述性统计。
  
  ```py
  s.describe()
  ```

- Series 的索引很灵活。
  
  ```py
  s.index = ['number1','number2','number3','number4']
  ```
  这时，Series就像一个`python`中的`dict（字典）`元素，可以使用一样的语法，其中`index`相当于`dict`的`key（键）`。
  
  >例：使用`[]`操作符访问 index 对应的值。

    ```py
    s['number1']
    ```

  
  >例：使用 `<index>in<series>` 表达式判断 index 是否在Series中.
    ```py
    'number1' in s
    ```


## DataFrame

`DataFrame`本质上是`Series`的集合，包含多列，每个变量对应一列，类似于高度优化的Excel表格。因此，它是一个强大的工具，用于表示和分析自然组织成行和列的数据，通常为单个行和单个列提供描述性索引。






