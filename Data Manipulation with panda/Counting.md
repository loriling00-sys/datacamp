# Counting Categorical Data in pandas

## 1. Lesson Goal
In this lesson, you learn how to summarize categorical data by counting values. 这一节的核心是学习如何统计分类数据。例如统计每个狗品种出现了多少次。

After this lesson, you should understand:
- how to avoid double counting
- how to use `drop_duplicates()`
- how to count categories with `value_counts()`
- how to calculate proportions with `normalize=True`
---
## 2. Why Counting Matters
Numeric columns can be summarized with statistics such as mean, median, min, and max. 数值型数据可以求平均值、最大值、最小值。  
Categorical columns need a different approach. We often summarize them by counting how many times each category appears.分类数据通常要统计每一类出现了多少次。  
Example question:

```text
How many dogs of each breed visited the vet?
```
---
## 3. The Double Counting Problem
Suppose we have a DataFrame of vet visits.  
Some dogs may have visited the vet more than once. 同一只狗可能去了很多次兽医诊所。 
| name | breed |
|---|---|
| Max | Chow Chow |
| Stella | Chihuahua |
| Max | Chow Chow |
| Stella | Chihuahua |
| Max | Labrador |

If we directly count the `breed` column, the same dog may be counted multiple times. 如果直接统计品种，就可能重复计算同一只狗。

---
## 4. Removing Duplicates with `drop_duplicates()`
### 4.1 Drop duplicates based on one column
```python
unique_dogs = vet_visits.drop_duplicates(subset="name")
```
This keeps only the first row for each unique dog name.
However, this can cause a problem.  
If two different dogs have the same name, one of them may be removed. 只根据 `name` 去重有风险，因为不同的狗可能重名。  
Example:
```text
Max the Chow Chow
Max the Labrador
```
If we only use `name`, pandas may keep only one Max.

---
## 5. Dropping Duplicate Pairs
To avoid removing different dogs with the same name, we can use more than one column.
```python
unique_dogs = vet_visits.drop_duplicates(subset=["name", "breed"])
```
This means pandas checks duplicate pairs of `name` and `breed`. `subset=["name", "breed"]` 的意思是同时看名字和品种。  
A row is considered duplicate only when both values are repeated. 只有名字和品种都相同，才认为是重复数据。  
Example:
| name | breed | Result |
|---|---|---|
| Max | Chow Chow | kept |
| Max | Chow Chow | duplicate |
| Max | Labrador | kept |

---
## 6. Counting Categories with `value_counts()`
After removing duplicates, we can count how many dogs belong to each breed.
```python
unique_dogs["breed"].value_counts()
```
Example output:
```text
Labrador     3
Poodle       2
Chow Chow    1
Chihuahua    1
Name: breed, dtype: int64
```
This tells us how many unique dogs there are in each breed. `value_counts()` 用来统计每个分类值出现的次数。

---
## 7. Sorting Counts
By default, `value_counts()` sorts the result from the largest count to the smallest count. `sort=True` 会让数量最多的类别排在最上面。  
这是默认行为。  
You can also make it explicit:
```python
unique_dogs["breed"].value_counts(sort=True)
```
If you do not want sorting:
```python
unique_dogs["breed"].value_counts(sort=False)
```
---
## 8. Calculating Proportions
Sometimes, counts are not enough. We may want to know the percentage of each category.  
Use `normalize=True`.`normalize=True` 会把数量转换成比例。  
```python
unique_dogs["breed"].value_counts(normalize=True)
```
Example output:
```text
Labrador     0.25
Poodle       0.20
Chow Chow    0.15
Chihuahua    0.10
Name: breed, dtype: float64
```
This means Labradors make up 25% of the dogs. 例如 `0.25` 表示 25%。  

---
## 9. Full Workflow
A common workflow is:
```python
# Remove duplicate dogs based on name and breed
unique_dogs = vet_visits.drop_duplicates(subset=["name", "breed"])

# Count dogs by breed
breed_counts = unique_dogs["breed"].value_counts()

# Calculate breed proportions
breed_props = unique_dogs["breed"].value_counts(normalize=True)
```
完整思路是先去重，再计数，再根据需要计算比例。

---

## 10. Key pandas Methods
| Method | Purpose | Example |
|---|---|---|
| `drop_duplicates()` | Remove repeated rows | `df.drop_duplicates(subset=["name", "breed"])` |
| `subset` | Choose columns used to detect duplicates | `subset="name"` |
| `value_counts()` | Count values in a column | `df["breed"].value_counts()` |
| `sort=True` | Sort counts from high to low | `value_counts(sort=True)` |
| `normalize=True` | Return proportions instead of counts | `value_counts(normalize=True)` |

---
## 11. Common Mistakes
### Mistake 1: Counting before removing duplicates
```python
vet_visits["breed"].value_counts()
```
This may count vet visits, not unique dogs.  
Better:
```python
unique_dogs = vet_visits.drop_duplicates(subset=["name", "breed"])
unique_dogs["breed"].value_counts()
```
直接统计可能得到的是就诊次数，而不是狗的数量。

### Mistake 2: Using only one column for duplicates
```python
vet_visits.drop_duplicates(subset="name")
```
This may remove different dogs with the same name.  
Better:
```python
vet_visits.drop_duplicates(subset=["name", "breed"])
``` 
如果只用名字去重，重名的狗可能会被误删。

---

## 12. What You Should Remember
The central idea of this lesson is:
```text
Remove duplicates first, then count categories.
```
In pandas:
```python
unique_dogs = vet_visits.drop_duplicates(subset=["name", "breed"])
unique_dogs["breed"].value_counts()
```
For proportions:
```python
unique_dogs["breed"].value_counts(normalize=True)
```
本节课最重要的逻辑是：  
先去重，再统计分类数量。  
如果要看比例，加上 `normalize=True`。
