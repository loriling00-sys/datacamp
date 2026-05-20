## 1. What this lesson is about
So far, you have learned how to calculate summary statistics for an entire DataFrame column.  
For example:
```python
dogs["weight_kg"].mean()
```
> This gives the average weight of **all dogs**.  
> But very often, we do not only want one overall number.  
> We want to compare different groups.
> 之前是对整列求平均、最大值、最小值。  
> 这一节的重点是：按类别分组以后，再分别计算统计值。
For example:
- What is the average weight of each dog color?
- Are female dogs taller than male dogs on average?
- Which breed has the highest average weight?
  
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
> This works, but it is repetitive and easy to make mistakes. 手动筛选每个颜色再求平均可以做到，但代码重复。  
> A better way is to use `groupby()`. `groupby()` 可以一次完成所有分组统计。

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
> Meaning:
> 1. Group the rows by `color`, 表示按颜色分组。  
> 2. Select the `weight_kg` column '表示只看体重这一列。 
> 3. Calculate the mean weight for each color,`.mean()` 表示每一组分别求平均值。

Output example:
```text
color
Black     8.0
Brown    18.0
White     7.0
Name: weight_kg, dtype: float64
```
---
## 4. A useful mental model
Think of this code:
```python
dogs.groupby("color")["weight_kg"].mean()
```
As three steps:
```text
Split → Apply → Combine, `groupby()` 的核心逻辑是：先分组，再对每组计算，最后合并结果。
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

---
## 5. Multiple grouped summaries with `agg()`
You can calculate more than one statistic at the same time using `agg()`.可以一次放多个统计函数。这样可以同时看每组的最小值、最大值、总和等。    
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
> Meaning:
> Calculate the median weight for each breed.  
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
> `groupby(["color", "breed"])` 表示先按颜色分组，再在每个颜色里按品种分组。  
> 最后得到的是每一种颜色和品种组合的平均体重。

Output example:
```text
color  breed
Black  Poodle       8.0
Brown  Beagle      12.0
Brown  Labrador    24.0
White  Poodle       7.0
Name: weight_kg, dtype: float64
```
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
- > 单中括号选择一列，结果通常是 Series。  
> 双中括号选择多列，结果通常是 DataFrame。

Output example:
| color | weight_kg | height_cm |
|---|---:|---:|
| Black | 8.0 | 35.0 |
| Brown | 18.0 | 50.0 |
| White | 7.0 | 30.0 |

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
在 dogs 这个表里
→ 按 color 和 breed 分组
→ 只看 weight_kg 这一列
→ 每组分别求平均值
```
---
