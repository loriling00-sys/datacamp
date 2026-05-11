# Lesson goal
## 1.Sort rows 排序
> Sorting means changing the order of the rows.  
### Sort from smallest to largest
```python
dogs.sort_values("weight_kg")
```
### Sort from largest to smallest
```python
dogs.sort_values("weight_kg", ascending=False)
```
***Key point:***
```python
ascending=True
```
> means small to large
```python
ascending=Flase
```
> means large to small
## 2.Sorting by multiple columns
> Sometimes one column is not enough.
```python
dogs.sort_values(["weight_kg", "height_cm"])
```
> First sort by weight.
If some dogs have the same weight, then sort those dogs by height.
```python
dogs.sort_values(["weight_kg", "height_cm"], ascending=[True, False])
```
> Sort weight from small to large.
For dogs with the same weight, sort height from large to small.
## 3.Selecting one column
```python
dogs["name"]
```
> result_Series
## 4.Selecting multiple columns
```python
dogs[["name", "breed"]]
```
> From dogs, select the columns name and breed.
```python
cols = ["name", "breed"]
dogs[cols]
```
> This is useful when the column list is long.
## 5.Filtering rows by numbers
```python
dogs["height_cm"] > 50
```
> result:a Boolean Series.
0     True
1    False
2    False
3    False
4     True
Name: height_cm, dtype: bool
```python
dogs[dogs["height_cm"] > 50]
```
> Show only dogs whose height is greater than 50 cm.
## 6.Filtering rows by text
```python
dogs[dogs["breed"] == "Labrador"]
```
> Show only dogs whose breed is Labrador.
## 7.Filtering rows by text
```python
dogs[dogs["date_of_birth"] < "2015-01-01"]
```
> Show dogs born before January 1, 2015.
## 8.Filtering with multiple conditions
```python
dogs[(dogs["height_cm"] > 50) & (dogs["breed"] == "Labrador")]
```
> Show dogs that are taller than 50 cm and whose breed is Labrador.
## 9.Filtering with .isin()
```python
dogs[dogs["color"].isin(["Black", "Brown"])]
```
> Show dogs whose color is Black or Brown.  
Without .isin(), you would need to write:  
dogs[(dogs["color"] == "Black") | (dogs["color"] == "Brown")]













