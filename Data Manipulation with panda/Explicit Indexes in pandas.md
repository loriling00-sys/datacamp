# Explicit Indexes in pandas—— pandas 中的显式索引
## 1. Big Picture

In pandas, a DataFrame has three main parts: 一个 DataFrame 不只是表格里的数据。它还包含列名和行索引。
```text
DataFrame
├── data values
├── column index
└── row index
```
In Chapter 1, you learned that:
```python
dogs.columns
dogs.index
```
- `.columns` shows the column labels. 是列名。
- `.index` shows the row labels.  是行索引

By default, the row index is usually simple numbers:
```text
0, 1, 2, 3, ...
```
But you can also use one or more columns as the row index.

---

## 2. What Is an Explicit Index?
An explicit index means using meaningful values as the row index.  
For example, instead of using row numbers:
```text
0, 1, 2, 3
```
you can use dog names:
```text
Bella, Stella, Max, Lucy
```
Example:
```python
dogs_ind = dogs.set_index("name")
```
This moves the `name` column into the index.   
`set_index("name")` 会把 `name` 这一列变成行索引。  
原来 `name` 是普通列，现在它变成了每一行的“标签”。

---

## 3. Setting a Column as the Index
Basic syntax:
```python
df.set_index("column_name")
```
Example:
```python
dogs_ind = dogs.set_index("name")
```
Before:
```text
   name     breed        color
0  Bella    Labrador     Brown
1  Stella   Chihuahua    Tan
```
After:
```text
          breed        color
name
Bella     Labrador     Brown
Stella    Chihuahua    Tan
```
Key idea:
```python
set_index()
```
does not change the original DataFrame unless you save the result or use `inplace=True`. `set_index()` 会返回一个新的 DataFrame。通常要赋值给一个新变量。  
Recommended style:
```python
dogs_ind = dogs.set_index("name")
```

---

## 4. Resetting an Index
To move the index back into the DataFrame as a normal column.     
use: `reset_index()` 可以把索引重新变回普通列。
```python
dogs_ind.reset_index()
```
Example:
```python
dogs_reset = dogs_ind.reset_index()
```
This is useful when you want to return to a normal tabular structure.

---

## 5. Dropping an Index
Sometimes you do not want to keep the index values.`drop=True` 表示不要把原来的 index 放回表格，直接丢掉。
Use:
```python
dogs_ind.reset_index(drop=True)
```
This removes the current index completely and creates a new default numeric index.  
Example:
```python
dogs_ind.reset_index(drop=True)
```
Result:
```text
0, 1, 2, 3, ...
```
Be careful: this can delete useful information if the index contains important data.

---

## 6. Why Use Indexes?
Indexes can make subsetting cleaner.  
Without setting `name` as the index:
```python
dogs[dogs["name"].isin(["Bella", "Stella"])]
```
With `name` as the index:
如果某一列经常被用来查找行，可以考虑把它设为 index。  
这样可以用 `.loc[]` 直接通过 index value 找数据。
```python
dogs_ind.loc[["Bella", "Stella"]]
```
The second version is shorter and easier to read.

---

## 7. `.loc[]` with Index Values
`.loc[]` is used to select rows by index labels.  
`.loc[]` 是按照“标签”找数据。  
如果 index 是名字，就用名字找。  
如果 index 是品种，就用品种找。  
Example:
```python
dogs_ind.loc["Bella"]
```
This returns the row where the index is `"Bella"`.  
To select multiple rows:
```python
dogs_ind.loc[["Bella", "Stella"]]
```
Important syntax:
```python
df.loc[index_value]
df.loc[[index_value_1, index_value_2]]
```

---
## 8. Index Values Do Not Need to Be Unique
Index values can be duplicated.  
index 不一定必须唯一。 
如果多个行有同一个 index value，`.loc[]` 会返回所有匹配的行。  
Example:
```python
dogs_breed = dogs.set_index("breed")
```
There may be several dogs with the same breed, such as `"Labrador"`.  
Then:
```python
dogs_breed.loc["Labrador"]
```
returns all rows where the index is `"Labrador"`.

---

## 9. Multi-Level Indexes
A multi-level index uses more than one column as the index.  
multi-level index 就是多层索引。  
例如第一层是 `breed`，第二层是 `color`。  
Example:
```python
dogs_multi = dogs.set_index(["breed", "color"])
```
This creates a hierarchical index.  
English terms:  
- multi-level index
- hierarchical index  
These two terms mean the same thing.

Example structure:
```text
breed       color
Labrador    Brown
Labrador    Black
Chihuahua   Tan
```
Here:

- `breed` is the outer level. 外层索引：`breed`  
- `color` is the inner level. 内层索引：`color`  

---

## 10. Subsetting the Outer Level

If you want to select rows using only the outer index level, pass a list to `.loc[]`.  
如果只按照第一层索引筛选，直接传入外层 index 的值即可。  
Example:
```python
dogs_multi.loc[["Labrador", "Chihuahua"]]
```
This returns all rows where the outer index is either `"Labrador"` or `"Chihuahua"`.

---

## 11. Subsetting Inner Levels with Tuples

To select specific combinations of index levels, use a list of tuples.
多层索引中，如果要同时指定外层和内层，就用 tuple。  
Example:

```python
dogs_multi.loc[[("Labrador", "Brown"), ("Chihuahua", "Tan")]]
```

Each tuple represents one full index path.

```python
("Labrador", "Brown")
```

means:

```text
breed = Labrador
color = Brown
```

One tuple = one combination.

Example:

```python
("Labrador", "Brown")
```
```text
品种是 Labrador，并且颜色是 Brown
```

A black Labrador would not be selected because the color condition does not match.

---

## 12. Sorting by Index

You already know:

```python
df.sort_values("column_name")
```

This sorts by column values.  
`sort_values()` 按普通列排序。 

To sort by index values, use:
`sort_index()` 按索引排序。  
```python
df.sort_index()
```

Example:

```python
dogs_multi.sort_index()
```

By default, pandas sorts all index levels from outer to inner in ascending order.

---

## 13. Controlling `sort_index()`

You can control which index level to sort by.

Example:

```python
dogs_multi.sort_index(level="breed")
```

You can also control ascending or descending order.
您还可以控制升序或降序排列。  
Example:

```python
dogs_multi.sort_index(level=["breed", "color"], ascending=[True, False])
```
`level` 控制按哪一层 index 排序。 
`ascending` 控制升序或降序。  
Meaning:

```text
breed: ascending
color: descending
```

---

## 14. The Controversy Around Indexes

Indexes are useful, but they can also make code harder to understand.

Advantages:

- Cleaner subsetting with `.loc[]`
- 使用 `.loc[]` 可以更清晰地进行子集划分  
- Useful for time series
- 适用于时间序列数据
- Useful for hierarchical data
- 适用于层级数据
- Can make some operations more convenient
- 可以简化某些操作

Disadvantages:

- Index values are still data.
- 索引值仍然是数据。
- Data stored in the index is less visible.
- 存储在索引中的数据不太容易被看到。
- Index syntax is different from normal column syntax.
- 索引语法与普通列语法不同。
- Code can become harder to debug.
- 代码调试难度会增加。
- It can violate the idea of tidy data.
- 这可能会违背数据整洁的原则。
 
index 很方便，但不要滥用。  
如果数据放在 index 里面，它就不再像普通列那样直观。

---

## 15. Tidy Data Idea

Tidy data means:

1. Each row is one observation. 每一行是一个观察对象。
2. Each column is one variable. 每一列是一个变量。
3. Each cell is one value. 每一个单元格是一个值。

Indexes can violate the second rule because an index value is also a variable, but it is not stored as a normal column.  
索引可能会违反第二条规则，因为索引值也是一个变量，但它并不像普通列那样存储。  
Example:
```text
name
Bella
Stella
```
If `name` is an index, it is no longer a normal column.

That can make the dataset less tidy.

---

## 16. When Should You Use Indexes?

Use indexes when:

- You repeatedly select rows by the same column.
- 您反复按同一列选择行。
- You work with time series data.
- 您处理的是时间序列数据。
- You need hierarchical grouping.
- 您需要进行层次分组。
- You are reading code or datasets that already use indexes.
- 您正在阅读的代码或数据集已经使用了索引。

Avoid indexes when:

- You want a clean and simple table.
-你想要一个简洁明了的表格。
- You are still exploring the data.
- 你还在探索数据。
- You need to frequently manipulate the index column as normal data.
- 你需要像操作普通数据一样频繁地操作索引列。
- The index makes your code harder to read.
- 索引会使你的代码更难阅读。

中文理解：  
初学阶段可以先把数据保留为普通列。  
当你真的需要更简洁的 `.loc[]` 筛选时，再使用 index。

---

## 17. Key Code Summary

### Check columns and index

```python
df.columns
df.index
```

### Set one column as index

```python
df_ind = df.set_index("column_name")
```

### Reset index

```python
df_reset = df_ind.reset_index()
```

### Drop index 删除索引

```python
df_dropped = df_ind.reset_index(drop=True)
```

### Select one index value

```python
df_ind.loc["value"]
```

### Select multiple index values

```python
df_ind.loc[["value1", "value2"]]
```

### Set multi-level index

```python
df_multi = df.set_index(["column1", "column2"])
```

### Select outer level

```python
df_multi.loc[["outer_value1", "outer_value2"]]
```

### Select specific multi-level combinations

```python
df_multi.loc[[("outer1", "inner1"), ("outer2", "inner2")]]
```

### Sort by index

```python
df_multi.sort_index()
```

### Sort by specific index levels

```python
df_multi.sort_index(level=["column1", "column2"], ascending=[True, False])
```

---

## 18. Common Mistakes

### Mistake 1: Forgetting to save the result

Wrong:

```python
dogs.set_index("name")
dogs.loc["Bella"]
```

Correct:

```python
dogs_ind = dogs.set_index("name")
dogs_ind.loc["Bella"]
```

中文理解：  
`set_index()` 不会自动修改原来的 `dogs`，除非你保存结果。

---

### Mistake 2: Using `.loc[]` on a column value before setting it as index

Wrong:

```python
dogs.loc["Bella"]
```

This only works if `"Bella"` is an index value.

Correct:

```python
dogs_ind = dogs.set_index("name")
dogs_ind.loc["Bella"]
```

---

### Mistake 3: Confusing column filtering with index filtering

Column filtering:

```python
dogs[dogs["name"] == "Bella"]
```

Index filtering:

```python
dogs_ind.loc["Bella"]
```

中文理解：  
普通列筛选用布尔条件。  
索引筛选用 `.loc[]`。

---

### Mistake 4: Forgetting tuple syntax in multi-level indexes

Wrong:

```python
dogs_multi.loc[["Labrador", "Brown"]]
```

This does not mean `breed = Labrador` and `color = Brown`.

Correct:

```python
dogs_multi.loc[[("Labrador", "Brown")]]
```

中文理解：  
多层索引要指定完整组合时，用 tuple。

---

## 19. Mental Model

Think of an index as a row label.

If the row label is a number:

```python
df.loc[0]
```

If the row label is a name:

```python
df.loc["Bella"]
```

If the row label has two levels:

```python
df.loc[[("Labrador", "Brown")]]
```

中文理解：  
index 就是“行的名字”。  
`.loc[]` 就是通过“行的名字”找数据。

---

## 20. Final Takeaway

The most important idea in this lesson:

```text
Indexes let you select rows by labels instead of writing longer filtering conditions.
```

中文理解：  
index 的核心作用是让你用标签直接找行。

But remember:

```text
Indexes can simplify subsetting, but they can also make data less tidy and code harder to understand.
```

建议：

At the beginner stage, learn how indexes work, but do not overuse them.  
Use normal columns first. Use indexes when they clearly make the code cleaner.
