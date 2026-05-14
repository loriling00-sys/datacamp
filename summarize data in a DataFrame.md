# Summary Statistics in pandas

This lesson teaches how to **summarize data in a DataFrame**.

中文：这一节的核心是用 pandas 快速总结数据，不需要逐行查看原始数据。

---

## 1. What are summary statistics?

**Summary statistics** are numbers that describe a dataset.

For example:

```python
dogs["weight_kg"].mean()
```

This gives the average dog weight.

中文：这行代码计算狗的平均体重。

Common summary statistics:

| Method | Meaning | 中文 |
|---|---|---|
| `.mean()` | average value | 平均值 |
| `.median()` | middle value | 中位数 |
| `.mode()` | most common value | 众数 |
| `.min()` | smallest value | 最小值 |
| `.max()` | largest value | 最大值 |
| `.var()` | variance | 方差 |
| `.std()` | standard deviation | 标准差 |
| `.sum()` | total value | 总和 |
| `.quantile()` | percentile value | 分位数 |

Example:

```python
dogs["height_cm"].max()
```

Meaning:

```text
Find the tallest dog.
```

中文：找出最高的狗。

---

## 2. Summary statistics for dates

You can also summarize date columns.

Example:

```python
dogs["date_of_birth"].min()
```

This finds the earliest birth date.

中文：`min()` 用在日期上，表示最早的日期。

```python
dogs["date_of_birth"].max()
```

This finds the latest birth date.

中文：`max()` 用在日期上，表示最晚的日期。

---

## 3. The `.agg()` method

`.agg()` means **aggregate**.

It lets you apply a function to one or more columns.

Example:

```python
def pct30(column):
    return column.quantile(0.3)

dogs["weight_kg"].agg(pct30)
```

Meaning:

```text
Calculate the 30th percentile of dog weights.
```

中文：计算狗体重的第 30 百分位数。

### Important idea

`.agg()` is useful when you want to use your own summary function.

Basic built-in summary method:

```python
dogs["weight_kg"].mean()
```

Custom summary method:

```python
dogs["weight_kg"].agg(pct30)
```

---

## 4. `.agg()` on multiple columns

You can use `.agg()` on several columns.

```python
dogs[["weight_kg", "height_cm"]].agg(pct30)
```

Meaning:

```text
Calculate the 30th percentile for both weight and height.
```

中文：同时计算体重和身高的第 30 百分位数。

---

## 5. `.agg()` with multiple functions

You can also apply several functions at once.

```python
def pct30(column):
    return column.quantile(0.3)

def pct40(column):
    return column.quantile(0.4)

dogs["weight_kg"].agg([pct30, pct40])
```

Result idea:

| Function | Result |
|---|---|
| `pct30` | 30th percentile |
| `pct40` | 40th percentile |

中文：一个列可以同时计算多个统计结果。

---

## 6. Cumulative statistics

Cumulative statistics return a value for every row.

### `.cumsum()`

```python
dogs["weight_kg"].cumsum()
```

Example idea:

| Dog | Weight | Cumulative sum |
|---|---:|---:|
| A | 10 | 10 |
| B | 20 | 30 |
| C | 15 | 45 |

Meaning:

```text
Each row shows the running total up to that row.
```

中文：每一行显示到目前为止的累计总和。

Other cumulative methods:

| Method | Meaning | 中文 |
|---|---|---|
| `.cumsum()` | cumulative sum | 累计和 |
| `.cummax()` | cumulative maximum | 累计最大值 |
| `.cummin()` | cumulative minimum | 累计最小值 |
| `.cumprod()` | cumulative product | 累计乘积 |

Example:

```python
dogs["weight_kg"].cummax()
```

Meaning:

```text
For each row, show the highest weight seen so far.
```

中文：每一行显示到当前行为止出现过的最大体重。

---

## 7. Basic pandas pattern

Most summary statistics follow this structure:

```python
df["column"].method()
```

Example:

```python
sales["weekly_sales"].mean()
```

Meaning:

```text
Calculate the average weekly sales.
```

中文：计算每周销售额的平均值。

---

## 8. Single result vs full column result

### Single summary result

These usually return one number:

```python
df["weekly_sales"].mean()
df["weekly_sales"].min()
df["weekly_sales"].max()
df["weekly_sales"].sum()
```

中文：这些方法通常返回一个数。

### Cumulative result

These return a full column:

```python
df["weekly_sales"].cumsum()
df["weekly_sales"].cummax()
df["weekly_sales"].cummin()
```

中文：累计方法会返回一整列结果。

---

## 9. Walmart dataset examples

The Walmart dataset has columns like:

| Column | Meaning |
|---|---|
| `store` | store ID |
| `type` | store type |
| `department` | department ID |
| `date` | week date |
| `weekly_sales` | weekly sales in dollars |
| `is_holiday` | whether it was a holiday week |
| `temperature_c` | average temperature |
| `fuel_price_usd_per_l` | fuel price |
| `unemployment` | unemployment rate |

Example questions you can answer:

```python
sales["weekly_sales"].mean()
```

Question:

```text
What is the average weekly sales?
```

中文：平均每周销售额是多少？

```python
sales["weekly_sales"].max()
```

Question:

```text
What is the highest weekly sales value?
```

中文：最高周销售额是多少？

```python
sales["date"].min()
```

Question:

```text
What is the earliest date in the dataset?
```

中文：数据集中最早的日期是哪一天？

```python
sales["date"].max()
```

Question:

```text
What is the latest date in the dataset?
```

中文：数据集中最晚的日期是哪一天？

---

## Key sentence to remember

**Summary statistics help you understand a dataset quickly with one or several meaningful numbers.**

中文：汇总统计可以帮助你用少量数字快速理解数据集。

重点掌握这 3 类代码：

```python
df["column"].mean()
```

```python
df["column"].agg(function)
```

```python
df["column"].cumsum()
```
