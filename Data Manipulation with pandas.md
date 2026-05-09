# Introducing DataFrames 课程笔记

## 1. 本节课学习目标

学完这一节后，你需要能回答下面几个问题：

1. What is pandas?
2. What is rectangular data?
3. What is a DataFrame?
4. How do we quickly explore a new DataFrame?
5. What are the main components of a DataFrame?
6. What is the difference between a method and an attribute in pandas?

---

## 2. Key Vocabulary

| English term | 中文理解 | 记忆方式 |
|---|---|---|
| pandas | Python 数据处理包 | 用来整理、分析、查看表格数据 |
| DataFrame | 数据框 | pandas 中最核心的表格对象 |
| rectangular data | 矩形数据 / 表格数据 | 行和列组成的数据 |
| observation | 观察对象 / 一条记录 | DataFrame 中的一行 |
| variable | 变量 / 属性 | DataFrame 中的一列 |
| method | 方法 | 需要加括号，例如 `.head()` |
| attribute | 属性 | 不加括号，例如 `.shape` |
| index | 行标签 | 每一行的名字或编号 |
| columns | 列标签 | 每一列的名字 |
| missing values | 缺失值 | 数据中空缺的部分 |
| summary statistics | 汇总统计量 | mean, median, count 等 |

---

## 3. What is pandas?

pandas is a Python package for data manipulation.

中文理解：

pandas 是 Python 中最常用的数据处理工具之一。  
它主要用来处理表格形式的数据，比如 Excel 表、CSV 文件、数据库表格等。

pandas 可以做的事情包括：

1. 查看数据
2. 筛选数据
3. 修改数据
4. 汇总数据
5. 可视化数据
6. 读取 CSV、Excel 等文件

本节课重点是认识 pandas 中最核心的数据结构：DataFrame。

---

## 4. pandas is built on NumPy and Matplotlib

pandas is built on top of NumPy and Matplotlib.

中文理解：

pandas 的底层依赖两个重要工具：

| Package | 作用 |
|---|---|
| NumPy | 提供数组结构和数值计算能力 |
| Matplotlib | 提供绘图和数据可视化能力 |

简单理解：

pandas 用 NumPy 来存储和处理数据。  
pandas 用 Matplotlib 来画图。

---

## 5. Rectangular Data

Rectangular data is also called tabular data.

中文理解：

Rectangular data 指的是像表格一样的数据。

表格数据通常由行和列组成：

| dog name | breed | color | height_cm | weight_kg |
|---|---|---|---:|---:|
| Bella | Labrador | Brown | 56 | 25 |
| Max | Poodle | White | 43 | 10 |
| Lucy | Beagle | Black | 38 | 12 |

在这个例子中：

| 概念 | 对应内容 |
|---|---|
| observation | 每一只狗，也就是每一行 |
| variable | 狗的属性，比如 breed、color、height_cm |
| row | 一条记录 |
| column | 一个变量 |

一句话记忆：

A row is one observation.  
A column is one variable.

---

## 6. What is a DataFrame?

In pandas, rectangular data is represented as a DataFrame object.

中文理解：

DataFrame 就是 pandas 里的表格对象。  
你可以把它理解成 Python 里的 Excel 表。

DataFrame 的特点：

1. 有行
2. 有列
3. 每一列有列名
4. 每一行有行标签
5. 不同列可以有不同的数据类型
6. 同一列中的数据通常属于同一种类型

例如：

```python
import pandas as pd

dogs = pd.DataFrame({
    "name": ["Bella", "Max", "Lucy"],
    "breed": ["Labrador", "Poodle", "Beagle"],
    "height_cm": [56, 43, 38],
    "weight_kg": [25, 10, 12]
})

dogs
```

---

## 7. Exploring a DataFrame

当你拿到一个新数据集时，不应该立刻计算。  
第一步应该是快速查看数据长什么样。

常用方法和属性有：

| Code | Type | Purpose |
|---|---|---|
| `.head()` | method | 查看前几行 |
| `.info()` | method | 查看列名、数据类型、缺失值 |
| `.shape` | attribute | 查看行数和列数 |
| `.describe()` | method | 查看数值列的统计摘要 |
| `.to_numpy()` | method | 转成 NumPy 二维数组 |
| `.columns` | attribute | 查看列名 |
| `.index` | attribute | 查看行标签 |

---

## 8. `.head()`

`.head()` returns the first few rows of the DataFrame.

中文理解：

`.head()` 用来查看 DataFrame 的前几行。  
默认显示前 5 行。

```python
dogs.head()
```

如果想看前 3 行：

```python
dogs.head(3)
```

使用场景：

1. 快速检查数据内容
2. 查看列名是否正确
3. 判断数据格式是否正常
4. 确认数据是否成功导入

---

## 9. `.info()`

`.info()` displays information about the DataFrame.

中文理解：

`.info()` 用来查看 DataFrame 的整体信息，包括：

1. 行数
2. 列数
3. 每列的列名
4. 每列的数据类型
5. 每列有多少非缺失值

```python
dogs.info()
```

你需要重点看三件事：

| 信息 | 为什么重要 |
|---|---|
| column names | 后面选列时需要用 |
| non-null count | 判断是否有缺失值 |
| dtype | 判断数据类型是否合理 |

常见 dtype：

| dtype | 含义 |
|---|---|
| object | 文本或混合类型 |
| int64 | 整数 |
| float64 | 小数 |
| bool | 布尔值 True/False |
| datetime64 | 日期时间 |

---

## 10. `.shape`

`.shape` contains the number of rows and columns.

中文理解：

`.shape` 用来查看 DataFrame 的形状。

```python
dogs.shape
```

输出结果类似：

```python
(3, 4)
```

含义：

```python
(rows, columns)
```

也就是：

```python
(行数, 列数)
```

重要区别：

```python
dogs.shape
```

`.shape` 是 attribute，所以不加括号。

错误写法：

```python
dogs.shape()
```

---

## 11. `.describe()`

`.describe()` computes summary statistics for numerical columns.

中文理解：

`.describe()` 用来查看数值列的统计信息。

```python
dogs.describe()
```

常见输出包括：

| Statistic | 中文理解 |
|---|---|
| count | 非缺失值数量 |
| mean | 平均值 |
| std | 标准差 |
| min | 最小值 |
| 25% | 第一四分位数 |
| 50% | 中位数 |
| 75% | 第三四分位数 |
| max | 最大值 |

注意：

`.describe()` 默认主要分析数值列。  
如果某一列是文本，它通常不会出现在默认的统计结果中。

---

## 12. `.to_numpy()`

`.to_numpy()` returns the data values as a 2-dimensional NumPy array.

中文理解：

`.to_numpy()` 会把 DataFrame 中的数据值转换成 NumPy 二维数组。

```python
dogs.to_numpy()
```

输出类似：

```python
array([
    ['Bella', 'Labrador', 56, 25],
    ['Max', 'Poodle', 43, 10],
    ['Lucy', 'Beagle', 38, 12]
], dtype=object)
```

使用场景：

1. 想把 pandas 数据传给 NumPy 处理
2. 想做更底层的数值计算
3. 想理解 DataFrame 内部的数据结构

注意：

转换成 NumPy array 后，列名和行标签会丢失。

---

## 13. `.columns`

`.columns` contains the column labels.

中文理解：

`.columns` 用来查看所有列名。

```python
dogs.columns
```

输出可能是：

```python
Index(['name', 'breed', 'height_cm', 'weight_kg'], dtype='object')
```

这说明 DataFrame 的列名本身是一个 Index object。

常见用途：

```python
list(dogs.columns)
```

可以把列名转换成普通 Python list。

---

## 14. `.index`

`.index` contains the row labels.

中文理解：

`.index` 用来查看行标签。

```python
dogs.index
```

默认情况下，pandas 会自动生成从 0 开始的行标签：

```python
RangeIndex(start=0, stop=3, step=1)
```

含义：

| Part | Meaning |
|---|---|
| start=0 | 从 0 开始 |
| stop=3 | 到 3 之前结束 |
| step=1 | 每次增加 1 |

注意：

行标签存在 `.index` 中。  
pandas 中没有 `.rows` 这个常用属性。

错误理解：

```python
dogs.rows
```

这种写法通常不可用。

---

## 15. Method vs Attribute

这是本节非常重要的基础。

| Type | 是否加括号 | Example |
|---|---|---|
| method | 加括号 | `.head()` |
| method | 加括号 | `.info()` |
| method | 加括号 | `.describe()` |
| method | 加括号 | `.to_numpy()` |
| attribute | 不加括号 | `.shape` |
| attribute | 不加括号 | `.columns` |
| attribute | 不加括号 | `.index` |

记忆方式：

方法像“动作”，所以需要调用。  
属性像“信息”，所以直接读取。

Example:

```python
dogs.head()
dogs.info()
dogs.describe()
dogs.to_numpy()

dogs.shape
dogs.columns
dogs.index
```

---

## 16. pandas Philosophy

pandas often provides multiple ways to solve the same problem.

中文理解：

在 pandas 中，同一个问题经常有多种写法。  
这让 pandas 很强大，也会让初学者觉得难。

学习建议：

1. 先掌握最常见写法
2. 暂时不要追求所有写法
3. 每次只记一个最清晰的方法
4. 通过真实数据多练习

本课程会采用 streamlined approach，也就是只学习最重要、最常用的方式。

---

## 17. 本节核心代码总结

```python
import pandas as pd

dogs = pd.DataFrame({
    "name": ["Bella", "Max", "Lucy"],
    "breed": ["Labrador", "Poodle", "Beagle"],
    "height_cm": [56, 43, 38],
    "weight_kg": [25, 10, 12]
})

# View first rows
dogs.head()

# View DataFrame information
dogs.info()

# View number of rows and columns
dogs.shape

# Summary statistics for numeric columns
dogs.describe()

# Convert to NumPy array
dogs.to_numpy()

# View column labels
dogs.columns

# View row labels
dogs.index
```

---

## 18. 小练习

### Exercise 1

Given a DataFrame called `dogs`, how do you view the first five rows?

Answer:

```python
dogs.head()
```

### Exercise 2

How do you check the number of rows and columns?

Answer:

```python
dogs.shape
```

### Exercise 3

How do you check column names and data types?

Answer:

```python
dogs.info()
```

### Exercise 4

How do you get summary statistics for numerical columns?

Answer:

```python
dogs.describe()
```

### Exercise 5

Which one needs parentheses?

```python
dogs.shape
dogs.head()
```

Answer:

```python
dogs.head()
```

Because `.head()` is a method.  
`.shape` is an attribute.

---

## 19. 易错点总结

### Mistake 1: 给 attribute 加括号

错误：

```python
dogs.shape()
```

正确：

```python
dogs.shape
```

原因：

`.shape` 是属性，不是方法。

### Mistake 2: 忘记给 method 加括号

错误：

```python
dogs.head
```

正确：

```python
dogs.head()
```

原因：

`.head()` 是方法，需要调用。

### Mistake 3: 把 row labels 叫成 `.rows`

错误：

```python
dogs.rows
```

正确：

```python
dogs.index
```

原因：

pandas 用 `.index` 表示行标签。

### Mistake 4: 以为 `.describe()` 会分析所有列

`.describe()` 默认主要分析 numerical columns。  
文本列通常不会出现在默认结果中。

---

## 20. 最后复习框架

可以按照下面顺序复习：

1. pandas 是什么？
2. DataFrame 是什么？
3. row 和 column 分别代表什么？
4. `.head()` 查看什么？
5. `.info()` 查看什么？
6. `.shape` 输出的顺序是什么？
7. `.describe()` 统计哪些内容？
8. `.to_numpy()` 会丢失什么？
9. `.columns` 和 `.index` 分别是什么？
10. method 和 attribute 的区别是什么？

---

## 21. 一句话总结

pandas uses DataFrames to store rectangular data, and we can quickly explore a DataFrame using `.head()`, `.info()`, `.shape`, `.describe()`, `.to_numpy()`, `.columns`, and `.index`.

中文理解：

pandas 用 DataFrame 存储表格数据。  
拿到新数据后，先用这些方法和属性快速检查数据结构、数据类型、行列数量和基本统计信息。
