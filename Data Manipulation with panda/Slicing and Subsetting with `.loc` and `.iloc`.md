## Learning Goal

In this lesson, you learn how to select a continuous range of rows or columns from lists and pandas DataFrames.

中文理解：这一节的核心是“按范围取数据”。范围可以来自 list 的位置，也可以来自 DataFrame 的 index label, column label, row number, or column number.

---

## 1. What Slicing Means

Slicing means selecting consecutive elements from an object.

For a Python list, slicing uses square brackets:

```python
breeds[start:stop]
```

Meaning:

```text
start is included
stop is excluded
```

中文理解：

```text
start 会被包含
stop 不会被包含
```

Example:

```python
breeds = ["Beagle", "Boxer", "Chow Chow", "Labrador", "Poodle", "Chihuahua"]

breeds[2:5]
```

Result:

```python
["Chow Chow", "Labrador", "Poodle"]
```

Explanation:

```text
Position 2 is Chow Chow
Position 5 is Chihuahua
Position 5 is excluded
So the result ends at Poodle
```

中文理解：Python list 的位置从 0 开始，所以 `2` 表示第三个元素。

---

## 2. Useful List Slicing Forms

### Start from the beginning

```python
breeds[:3]
```

This returns the first three elements.

中文理解：冒号前面为空，表示从开头开始。

### Select everything

```python
breeds[:]
```

This returns the whole list.

中文理解：冒号两边都为空，表示保留全部。

---

## 3. DataFrame Slicing Needs a Sorted Index DataFrame 切片需要排序索引

When slicing a DataFrame by index labels, it is best to sort the index first.

当按索引标签对 DataFrame 进行切片时，最好先对索引进行排序。

Example:

```python
dogs_ind = dogs.set_index(["breed", "color"]).sort_index()
```

This code does two things:

```python
dogs.set_index(["breed", "color"])
```

sets `breed` and `color` as a multi level index.

将 `breed` 和 `color` 设置为多级索引。

```python
.sort_index()
```

sorts the index so slicing works predictably.

对索引进行排序，使切片操作能够按预期工作。

中文理解：DataFrame 用 index label 做范围切片时，先排序可以让 pandas 按顺序找到起点和终点。

---

## 4. Slicing Rows with `.loc`

`.loc` selects data by labels.

Basic structure:

```python
df.loc[start_label:stop_label]
```

Example:

```python
dogs_ind.loc["Chow Chow":"Poodle"]
```

Important rule:

```text
For .loc slicing, the stop label is included.
```

中文理解：`.loc` 按 index 名字切片，结束位置会被包含。

This is different from list slicing, where the stop position is excluded.

---

## 5. Slicing a Multi Level Index

When the DataFrame has a multi level index, each row label can contain more than one part.

当 DataFrame 具有多级索引时，每一行标签可以包含多个部分。

Example index structure:

```text
breed, color
```

To slice across multiple index levels, use tuples. 

要跨越多个索引级别进行切片，请使用元组。

Example:

```python
dogs_ind.loc[("Labrador", "Brown"):("Schnauzer", "Grey")]
```

Here:

```python
("Labrador", "Brown")
```

is the first index value to include.

```python
("Schnauzer", "Grey")
```

is the last index value to include.

中文理解：如果 index 有多层，就要用 tuple 表达每一行的完整 index 位置。

---

## 6. Slicing Columns with `.loc`

A DataFrame has two dimensions:

DataFrame 具有两个维度：

```text
rows
columns
```

`.loc` can take two arguments: 两个参数：

```python
df.loc[row_slice, column_slice]
```

To keep all rows and slice columns:

```python
dogs.loc[:, "name":"height_cm"]
```

Meaning:

```text
: keeps all rows
"name":"height_cm" selects columns from name through height_cm
```

中文理解：`.loc` 里面第一个位置控制行，第二个位置控制列。单独的 `:` 表示这一维度全部保留。

---

## 7. Slicing Rows and Columns Together

You can slice rows and columns in one line.

Example:

```python
dogs_ind.loc["Chow Chow":"Poodle", "name":"height_cm"]
```

Meaning:

```text
Rows from Chow Chow through Poodle
Columns from name through height_cm
```

中文理解：第一个切片给 rows，第二个切片给 columns。

General pattern:

```python
df.loc[row_start:row_stop, col_start:col_stop]
```

---

## 8. Slicing by Dates

Date columns are often useful as an index.

First, set the date column as the index and sort it:

```python
dogs_birth = dogs.set_index("date_of_birth").sort_index()
```

Then slice by full dates:

```python
dogs_birth.loc["2014-08-25":"2016-09-16"]
```

中文理解：日期作为 index 后，可以像普通 label 一样用 `.loc` 切片。

---

## 9. Slicing by Partial Dates 日期切片

pandas can understand partial dates.

Example:

```python
dogs_birth.loc["2014":"2016"]
```

This means:

```text
from the start of 2014
to the end of 2016
```

So it includes all dates in:

```text
2014
2015
2016
```

中文理解：只写年份时，pandas 会自动理解为这个年份的完整范围。

---

## 10. Subsetting by Row and Column Number with `.iloc`

`.iloc` selects data by integer position. 按整数位置选择数据。

Basic structure:

```python
df.iloc[row_start:row_stop, col_start:col_stop]
```

Example:

```python
dogs.iloc[2:5, 1:4]
```

Meaning:

```text
Rows from position 2 up to position 5
Columns from position 1 up to position 4
```

For `.iloc`, the stop position is excluded.

中文理解：`.iloc` 的规则和 Python list 更接近，结束位置不包含。

---

## 11. `.loc` and `.iloc` Core Difference

### `.loc`

Use labels:

```python
df.loc[row_labels, column_labels]
```

Example:

```python
dogs.loc["Chow Chow":"Poodle", "name":"height_cm"]
```

Chinese support:

```text
.loc 看 index 名字和 column 名字
```

### `.iloc`

Use integer positions -- 使用整数位置：

```python
df.iloc[row_positions, column_positions]
```

Example:

```python
dogs.iloc[2:5, 1:4]
```

Chinese support:

```text
.iloc 看第几行和第几列
```

---

## 12. Final Mental Model

Use this model when reading or writing slicing code:

```text
.loc  means label based selection
.iloc means position based selection
```

For `.loc`:

```python
df.loc[row_label_start:row_label_stop, column_label_start:column_label_stop]
```

For `.iloc`:

```python
df.iloc[row_number_start:row_number_stop, column_number_start:column_number_stop]
```

中文总结：

```text
.loc 用名字
.iloc 用数字位置
DataFrame 切片时，先写行，再写列
日期也可以作为 index 来切片
MultiIndex 切片时，用 tuple 表达完整 index
```
