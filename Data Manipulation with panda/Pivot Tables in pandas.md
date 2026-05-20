# Pivot Tables in pandas
## 1. Core idea
A pivot table is another way to calculate grouped summary statistics.  
pivot table 可以理解为一种“分组汇总表”。它和 `groupby()` 做的事情很接近，都是按照某些类别对数据进行统计。  
In pandas, pivot tables are created with:
```python
df.pivot_table()
```
The most important parameters are:
| Parameter | Meaning | 中文解释 |
|---|---|---|
| `values` | the column to summarize | 要统计的数值列 |
| `index` | the column used as row groups | 按哪个变量分组，显示在行上 |
| `columns` | the column used as column groups | 按第二个变量分组，显示在列上 |
| `aggfunc` | the summary statistic | 使用哪种统计方法 |
| `fill_value` | value used to replace missing values | 用什么值填充缺失值 |
| `margins` | add row and column totals or summaries | 添加总计或整体汇总 |

---
## 2. From `groupby()` to `pivot_table()`
Previously, you may have used `groupby()` like this:
```python
dogs.groupby("color")["weight_kg"].mean()
```
This means:
1. Group dogs by `color`.
2. Select the `weight_kg` column.
3. Calculate the mean weight for each color.
   
The same result can be created with `pivot_table()`:
```python
dogs.pivot_table(
    values="weight_kg",
    index="color"
)
```
Because `pivot_table()` uses the mean by default, this gives the average weight for each dog color.  
`values="weight_kg"` 表示你要统计的是狗的体重。  
`index="color"` 表示按照颜色分组。  
默认统计方法是平均值 `mean`。

---
## 3. Using a different statistic with `aggfunc`
By default, `pivot_table()` calculates the mean.
If you want another statistic, use `aggfunc`.

Example: calculate the median weight for each color.
```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    aggfunc="median"
)
```
`aggfunc` 表示 aggregation function，也就是“汇总函数”。  
常见的统计函数包括：
| Function | Meaning | 中文 |
|---|---|---|
| `"mean"` | average | 平均值 |
| `"median"` | middle value | 中位数 |
| `"sum"` | total | 总和 |
| `"min"` | smallest value | 最小值 |
| `"max"` | largest value | 最大值 |
| `"count"` | number of rows or values | 数量 |

---
## 4. Multiple statistics
You can calculate more than one statistic at the same time by passing a list to `aggfunc`.
```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    aggfunc=["mean", "median"]
)
```
This produces both the mean and median weight for each color.  
如果你同时想看平均值和中位数，就把多个统计函数放进列表里。

---
## 5. Pivoting on two variables
A pivot table can group by two variables.
Example:
```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    columns="breed"
)
```
This means:
1. `color` becomes the row groups.
2. `breed` becomes the column groups.
3. Each cell shows the mean `weight_kg` for that color and breed combination.
4.  
这就像 Excel 里的交叉表。  
行是颜色，列是品种，表格中的每个数字是某种颜色和某个品种组合下的平均体重。

Example structure:
| color | Chihuahua | Labrador | Poodle |
|---|---:|---:|---:|
| black | NaN | 29 | 20 |
| brown | 3 | 24 | NaN |
| gray | NaN | NaN | 18 |

`NaN` means missing value.    
`NaN` 表示这个组合在数据中不存在。  
例如，没有 black Chihuahua，就会显示 `NaN`。

---
## 6. Filling missing values
To replace `NaN` values, use `fill_value`.

```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    columns="breed",
    fill_value=0
)
```
This replaces missing values with `0`.  
`fill_value=0` 表示把缺失值填成 0。  
这可以让表格更整齐，但要注意：0 不一定代表真实数值，它只是用来替代缺失值。

---
## 7. Adding margins
Use `margins=True` to add summary rows and columns.  
```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    columns="breed",
    margins=True
)
```
This adds:
1. A final row showing the summary for each column group.
2. A final column showing the summary for each row group.
3. A bottom-right value showing the summary for the whole dataset.
   
`margins=True` 会添加一个整体汇总。  
默认情况下，因为统计方法是 `mean`，所以这些汇总也是平均值。

For example:
| color | Chihuahua | Labrador | Poodle | All |
|---|---:|---:|---:|---:|
| black | NaN | 29 | 20 | 24.5 |
| brown | 3 | 24 | NaN | 13.5 |
| All | 3 | 26.5 | 20 | 19 |

Meaning:
| Position | Meaning | 中文 |
|---|---|---|
| last row | summary for each breed | 每个品种的总体汇总 |
| last column | summary for each color | 每种颜色的总体汇总 |
| bottom-right cell | summary for the whole dataset | 整个数据集的总体汇总 |

Important point:  
When `margins=True`, the summary is calculated from the actual data. Missing values are ignored.   
即使用了 `fill_value=0`，`margins=True` 的汇总通常仍然基于原始数据计算，缺失值不会被当作真实的 0 参与平均值计算。

---
## 8. `groupby()` vs `pivot_table()`
Both can calculate grouped summary statistics.  
| Task | `groupby()` | `pivot_table()` |
|---|---|---|
| Group by one variable | good | good |
| Group by two variables | possible | very clear |
| Spreadsheet-like layout | less direct | strong |
| Fill missing combinations | extra step needed | use `fill_value` |
| Add row and column summaries | extra step needed | use `margins=True` |

Example with `groupby()`:
```python
dogs.groupby(["color", "breed"])["weight_kg"].mean()
```
Example with `pivot_table()`:
```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    columns="breed"
)
```
`groupby()` 更像“按组计算”。  
`pivot_table()` 更像“把分组结果整理成表格”。  
当你想看二维分类汇总时，`pivot_table()` 通常更直观。

---
## 9. Mental model
When writing a pivot table, think in this order:
### Step 1: What number do I want to summarize?
This is `values`.
```python
values="weight_kg"
```
### Step 2: What should be shown as rows?
This is `index`.
```python
index="color"
```
### Step 3: Do I also want columns?
This is `columns`.
```python
columns="breed"
```
### Step 4: What statistic do I want?
This is `aggfunc`.
```python
aggfunc="mean"
```
### Step 5: How should missing combinations look?
This is `fill_value`.
```python
fill_value=0
```
### Step 6: Do I need overall summaries?
This is `margins`.
```python
margins=True
```
---

## 10. Most important code patterns
### Mean by one group
```python
dogs.pivot_table(
    values="weight_kg",
    index="color"
)
```
### Median by one group
```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    aggfunc="median"
)
```
### Multiple statistics

```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    aggfunc=["mean", "median"]
)
```
### Mean by two groups

```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    columns="breed"
)
```
### Fill missing values
```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    columns="breed",
    fill_value=0
)
```

### Add margins

```python
dogs.pivot_table(
    values="weight_kg",
    index="color",
    columns="breed",
    margins=True
)
```

---

## 11. Key takeaway

A pivot table helps you summarize data by one or two categorical variables and display the result in a table format.

中文总结：  
这节课的核心是学会用 `pivot_table()` 做分组统计。  
最常用的写法是：

```python
df.pivot_table(
    values="numeric_column",
    index="row_group",
    columns="column_group",
    aggfunc="mean",
    fill_value=0,
    margins=True
)
```

You do not need to memorize every parameter at once.  
Focus on these three first:

1. `values`: what to calculate
2. `index`: how to group rows
3. `aggfunc`: which statistic to use

Then add `columns`, `fill_value`, and `margins` when needed.
