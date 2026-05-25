## 1. What this lesson is about

This lesson is about working with pivot tables after they have already been created.

You learned earlier how to create a pivot table with `pivot_table()`. In this lesson, the focus shifts to two practical skills:

1. Subsetting pivot tables with `.loc[]` and slicing.
2. Calculating summary statistics across rows or columns with the `axis` argument.

中文说明：  
这一节不是单纯讲如何创建 pivot table，而是讲创建之后如何继续操作它，比如筛选某一部分数据，或者按行、按列计算平均值。

---

## 2. Why use a bigger dog dataset?

The lesson uses a larger dog dataset because pivot tables become more useful when there are enough rows to summarize.

A small dataset may not show clear patterns, but a larger dataset allows us to calculate meaningful group-level summaries, such as:

- average height by breed
- average weight by color
- average value grouped by both breed and color

中文说明：  
数据量更大时，pivot table 才更有意义。因为它的核心作用是“分组后汇总”，数据太少就不容易看出规律。

---

## 3. Creating a pivot table

A pivot table is created with `.pivot_table()`.

Basic structure:

```python
dogs.pivot_table(
    values="height_cm",
    index="breed",
    columns="color"
)
```

Meaning of each argument:

| Argument | Meaning | 中文说明 |
|---|---|---|
| `values` | The column whose values will be summarized | 要被计算的数值列 |
| `index` | The column used as row groups | 放在行方向的分组变量 |
| `columns` | The column used as column groups | 放在列方向的分组变量 |
| default aggregation | Mean | 默认计算平均值 |

Example idea:

```python
height_by_breed_vs_color = dogs.pivot_table(
    values="height_cm",
    index="breed",
    columns="color"
)
```

This creates a table where:

- each row is a dog breed
- each column is a dog color
- each cell is the mean height for that breed and color combination

中文说明：  
这个表的每一个格子，表示“某个品种 + 某种颜色”的狗的平均身高。

---

## 4. Pivot tables are still DataFrames

A pivot table in pandas is still a DataFrame.

That means you can use DataFrame tools on it, such as:

```python
.loc[]
```

```python
mean()
```

```python
sum()
```

```python
median()
```

The lesson highlights this idea:

> `.loc[]` plus slicing is a powerful combination for subsetting pivot tables.

中文说明：  
pivot table 看起来像一种特殊表格，但在 pandas 里它仍然是 DataFrame。所以之前学过的 `.loc[]`、slicing、summary statistics 都可以继续使用。

---

## 5. Using `.loc[]` and slicing on pivot tables

Because pivot tables usually have sorted indexes, `.loc[]` slicing works well.

Example:

```python
height_by_breed_vs_color.loc["Chow Chow":"Poodle"]
```

This selects rows from `"Chow Chow"` to `"Poodle"` based on the row index.

You can also subset rows and columns together:

```python
height_by_breed_vs_color.loc["Chow Chow":"Poodle", "Black":"Brown"]
```

General structure:

```python
df.loc[row_slice, column_slice]
```

中文说明：  
`.loc[]` 使用标签名进行筛选。对于 pivot table 来说，行标签通常是 `index` 参数指定的分组变量，列标签通常是 `columns` 参数指定的分组变量。

---

## 6. Understanding the `axis` argument

Many pandas summary methods have an `axis` argument.

For example:

```python
df.mean(axis="index")
```

```python
df.mean(axis="columns")
```

The `axis` argument controls the direction of calculation.

---

## 7. `axis="index"`: calculate down each column

The default value is:

```python
axis="index"
```

This means pandas calculates down the rows for each column.

Example:

```python
height_by_breed_vs_color.mean(axis="index")
```

This calculates the mean height for each color.

Why?

Because each column represents a color, and pandas looks down the rows within each color column.

Result idea:

```text
color
Black    mean height across breeds
Brown    mean height across breeds
Gray     mean height across breeds
White    mean height across breeds
```

中文说明：  
`axis="index"` 表示沿着行方向向下计算。结果通常是“每一列得到一个统计值”。在这个例子中，每一列是颜色，所以结果是每种颜色的平均身高。

A simple way to remember:

```text
axis="index" → calculate down rows → one result per column
```

---

## 8. `axis="columns"`: calculate across each row

To calculate across columns, use:

```python
axis="columns"
```

Example:

```python
height_by_breed_vs_color.mean(axis="columns")
```

This calculates the mean height for each breed.

Why?

Because each row represents a breed, and pandas looks across the color columns within that row.

Result idea:

```text
breed
Chihuahua      mean height across colors
Chow Chow      mean height across colors
Labrador       mean height across colors
Poodle         mean height across colors
```

中文说明：  
`axis="columns"` 表示沿着列方向横向计算。结果通常是“每一行得到一个统计值”。在这个例子中，每一行是品种，所以结果是每个品种的平均身高。

A simple way to remember:

```text
axis="columns" → calculate across columns → one result per row
```

---

## 9. Why pivot tables work well with `axis`

For many ordinary DataFrames, using `axis="columns"` may not be meaningful.

Reason:

An ordinary DataFrame often contains different kinds of data in different columns, such as:

- names
- dates
- categories
- numbers

Calculating a row-wise mean across mixed data types may not make sense.

Pivot tables are different because their value columns usually contain the same kind of numeric data.

For example, every column may contain height values.

中文说明：  
普通 DataFrame 的列可能是姓名、日期、类别、数字混在一起，所以横向求平均值通常没有实际意义。pivot table 的列通常来自同一个数值变量，比如都是身高，所以横向计算更合理。

---

## 10. Core code summary

Create a pivot table:

```python
height_by_breed_vs_color = dogs.pivot_table(
    values="height_cm",
    index="breed",
    columns="color"
)
```

Subset rows:

```python
height_by_breed_vs_color.loc["Chow Chow":"Poodle"]
```

Subset rows and columns:

```python
height_by_breed_vs_color.loc["Chow Chow":"Poodle", "Black":"Brown"]
```

Calculate the mean for each column:

```python
height_by_breed_vs_color.mean(axis="index")
```

Calculate the mean for each row:

```python
height_by_breed_vs_color.mean(axis="columns")
```

---

## 11. Knowledge chain

The logic of this lesson is:

```text
Raw dog dataset
→ create a pivot table with pivot_table()
→ rows are controlled by index
→ columns are controlled by columns
→ values are summarized by mean by default
→ use .loc[] and slicing to subset the pivot table
→ use axis="index" to summarize each column
→ use axis="columns" to summarize each row
```

中文说明：  
这节课的知识链路很清楚：先把原始数据变成 pivot table，然后把 pivot table 当作 DataFrame 继续操作。筛选时用 `.loc[]`，计算汇总统计时用 `axis` 控制计算方向。

---

## 12. Final takeaway

A pivot table is a summarized DataFrame.

Once it is created, you can treat it like a normal DataFrame:

- use `.loc[]` to select rows and columns
- use slicing to select continuous labels
- use `mean()` or other summary methods to calculate statistics
- use `axis="index"` for column-wise results
- use `axis="columns"` for row-wise results

中文总结：  
pivot table 的重点是“分组汇总后的表格”。创建完成后，它仍然可以用 DataFrame 的方法继续分析。掌握 `.loc[]` 和 `axis`，就能对 pivot table 做进一步筛选和计算。
