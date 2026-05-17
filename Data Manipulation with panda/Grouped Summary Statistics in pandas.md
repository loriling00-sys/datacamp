# Grouped Summary Statistics in pandas

> **Main language: English**  
> 中文作为辅助说明，帮助你理解 pandas 的 `groupby()` 思路。

---

## 1. What this lesson is about

So far, you have learned how to calculate summary statistics for an entire DataFrame column.

For example:

```python
dogs["weight_kg"].mean()
```

This gives the average weight of **all dogs**.

But very often, we do not only want one overall number.  
We want to compare different groups.

For example:

- What is the average weight of each dog color?
- Are female dogs taller than male dogs on average?
- Which breed has the highest average weight?

> 中文理解：  
> 之前是对整列求平均、最大值、最小值。  
> 这一节的重点是：按类别分组以后，再分别计算统计值。

---

## 2. Why grouped summaries are useful

Imagine you have a dog dataset like this:

| name | color | breed | weight_kg |
|---|---|---|---|
| Bella | Brown | Labrador | 24 |
| Max | Black | Poodle | 8 |
| Lucy | Brown | Beagle | 12 |
| Charlie | White | Poodle | 7 |

If you only calculate:

```python
dogs["weight_kg"].mean()
```

You get one overall average.

But this does not tell you whether brown dogs are heavier than black dogs.

To compare groups, you could manually subset the DataFrame:

```python
dogs[dogs["color"] == "Brown"]["weight_kg"].mean()
dogs[dogs["color"] == "Black"]["weight_kg"].mean()
dogs[dogs["color"] == "White"]["weight_kg"].mean()
```

This works, but it is repetitive and easy to make mistakes.

A better way is to use `groupby()`.

> 中文理解：  
> 手动筛选每个颜色再求平均可以做到，但代码重复。  
> `groupby()` 可以一次完成所有分组统计。

---

## 3. Core syntax: `groupby()`

The basic pattern is:

```python
df.groupby("group_column")["value_column"].summary_function()
```

Example:

```python
dogs.groupby("color")["weight_kg"].mean()
```

Meaning:

1. Group the rows by `color`
2. Select the `weight_kg` column
3. Calculate the mean weight for each color

Output example:

```text
color
Black     8.0
Brown    18.0
White     7.0
Name: weight_kg, dtype: float64
```

> 中文理解：  
> `groupby("color")` 表示按颜色分组。  
> `["weight_kg"]` 表示只看体重这一列。  
> `.mean()` 表示每一组分别求平均值。

---

## 4. A useful mental model

Think of this code:

```python
dogs.groupby("color")["weight_kg"].mean()
```

As three steps:

```text
Split → Apply → Combine
```

### Step 1: Split

pandas splits the DataFrame into groups based on `color`.

```text
Brown dogs
Black dogs
White dogs
```

### Step 2: Apply

pandas applies the summary function to each group.

```text
mean weight of Brown dogs
mean weight of Black dogs
mean weight of White dogs
```

### Step 3: Combine

pandas combines the results into one output.

```text
color
Black     ...
Brown     ...
White     ...
```

> 中文理解：  
> `groupby()` 的核心逻辑是：先分组，再对每组计算，最后合并结果。

---

## 5. Multiple grouped summaries with `agg()`

You can calculate more than one statistic at the same time using `agg()`.

Example:

```python
dogs.groupby("color")["weight_kg"].agg(["min", "max", "sum"])
```

This gives:

| color | min | max | sum |
|---|---:|---:|---:|
| Black | 8 | 8 | 8 |
| Brown | 12 | 24 | 36 |
| White | 7 | 7 | 7 |

Meaning:

- `min`: smallest weight in each color group
- `max`: largest weight in each color group
- `sum`: total weight in each color group

> 中文理解：  
> `.agg()` 可以一次放多个统计函数。  
> 这样可以同时看每组的最小值、最大值、总和等。

---

## 6. Common summary functions

You can use many summary functions after `groupby()`.

| Function | Meaning | 中文 |
|---|---|---|
| `.mean()` | average value | 平均值 |
| `.median()` | middle value | 中位数 |
| `.min()` | smallest value | 最小值 |
| `.max()` | largest value | 最大值 |
| `.sum()` | total value | 总和 |
| `.count()` | number of non-missing values | 非缺失值数量 |
| `.nunique()` | number of unique values | 唯一值数量 |

Example:

```python
dogs.groupby("breed")["weight_kg"].median()
```

Meaning:

Calculate the median weight for each breed.

> 中文理解：  
> 分组后可以接很多统计函数。  
> 关键是先想清楚：你要按哪一列分组，要统计哪一列，要用什么统计方法。

---

## 7. Grouping by multiple variables

You can group by more than one column.

Example:

```python
dogs.groupby(["color", "breed"])["weight_kg"].mean()
```

Meaning:

1. First group by `color`
2. Then group by `breed` within each color
3. Calculate the mean weight for each color and breed combination

Output example:

```text
color  breed
Black  Poodle       8.0
Brown  Beagle      12.0
Brown  Labrador    24.0
White  Poodle       7.0
Name: weight_kg, dtype: float64
```

> 中文理解：  
> `groupby(["color", "breed"])` 表示先按颜色分组，再在每个颜色里按品种分组。  
> 最后得到的是每一种颜色和品种组合的平均体重。

---

## 8. Aggregating multiple columns

You can also calculate statistics for more than one value column.

Example:

```python
dogs.groupby("color")[["weight_kg", "height_cm"]].mean()
```

Meaning:

For each dog color, calculate:

- mean weight
- mean height

Output example:

| color | weight_kg | height_cm |
|---|---:|---:|
| Black | 8.0 | 35.0 |
| Brown | 18.0 | 50.0 |
| White | 7.0 | 30.0 |

> 中文理解：  
> 单中括号选择一列，结果通常是 Series。  
> 双中括号选择多列，结果通常是 DataFrame。

---

## 9. Many groups, many summaries

You can combine multiple grouping columns and multiple summary columns.

Example:

```python
dogs.groupby(["color", "breed"])[["weight_kg", "height_cm"]].agg(["mean", "min", "max"])
```

Meaning:

For each `color` and `breed` group, calculate:

- mean, min, and max weight
- mean, min, and max height

This is useful when you want a detailed comparison between categories.

> 中文理解：  
> 这是更完整的分组统计：  
> 多个分组条件，加多个数值列，再加多个统计函数。

---

## 10. How to read this kind of code

When you see code like this:

```python
dogs.groupby(["color", "breed"])["weight_kg"].mean()
```

Read it from left to right:

```text
dogs
→ group rows by color and breed
→ select the weight_kg column
→ calculate the mean for each group
```

中文可以理解为：

```text
在 dogs 这个表里
→ 按 color 和 breed 分组
→ 只看 weight_kg 这一列
→ 每组分别求平均值
```

---

## 11. Common mistakes

### Mistake 1: Forgetting square brackets around multiple group columns

Incorrect:

```python
dogs.groupby("color", "breed")["weight_kg"].mean()
```

Correct:

```python
dogs.groupby(["color", "breed"])["weight_kg"].mean()
```

When grouping by multiple columns, put the column names inside a list.

---

### Mistake 2: Selecting the wrong column

Example:

```python
dogs.groupby("color")["breed"].mean()
```

This does not make sense because `breed` is categorical text data.

Better:

```python
dogs.groupby("color")["weight_kg"].mean()
```

Use numerical columns for numerical summaries like mean, min, max, and sum.

---

### Mistake 3: Confusing `count()` and `nunique()`

```python
dogs.groupby("color")["name"].count()
```

This counts how many dog names appear in each color group.

```python
dogs.groupby("color")["name"].nunique()
```

This counts how many unique dog names appear in each color group.

> 中文理解：  
> `count()` 是数量统计。  
> `nunique()` 是去重后的数量统计。  
> 如果同一只狗出现多次，`nunique()` 更适合用来数有多少只不同的狗。

---

## 12. The most important patterns to remember

### Pattern 1: One group, one summary

```python
df.groupby("group_col")["value_col"].mean()
```

Example:

```python
dogs.groupby("color")["weight_kg"].mean()
```

---

### Pattern 2: One group, multiple summaries

```python
df.groupby("group_col")["value_col"].agg(["min", "max", "sum"])
```

Example:

```python
dogs.groupby("color")["weight_kg"].agg(["min", "max", "sum"])
```

---

### Pattern 3: Multiple groups, one summary

```python
df.groupby(["group_col_1", "group_col_2"])["value_col"].mean()
```

Example:

```python
dogs.groupby(["color", "breed"])["weight_kg"].mean()
```

---

### Pattern 4: Multiple groups, multiple value columns

```python
df.groupby(["group_col_1", "group_col_2"])[["value_col_1", "value_col_2"]].mean()
```

Example:

```python
dogs.groupby(["color", "breed"])[["weight_kg", "height_cm"]].mean()
```

---

## 13. Final takeaway

Grouped summary statistics help you compare categories inside your dataset.

The key question is always:

```text
For each group, what summary value do I want?
```

In pandas, this becomes:

```python
df.groupby("group_column")["value_column"].summary_function()
```

Once you understand this structure, grouped summaries become much easier to read and write.

> 中文总结：  
> 这一节的核心是 `groupby()`。  
> 它可以让你按照某个类别分组，然后对每个组分别计算平均值、最大值、最小值、总和等统计量。  
> 以后看到 `groupby()`，先判断三件事：按谁分组，统计哪一列，用什么统计函数。
