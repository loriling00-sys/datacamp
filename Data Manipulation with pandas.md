### `user_id.nunique()`
> Counting each user only once.
### `nunique()`
> means **number of unique values.**
```python
user_count = orders_df['user_id'].nunique()
user_count
```
### Total count
```python
trips_count = trips_df['trip_id'].count()
trips_count
```
### Unique rider count
```python
riders_count = trips_df['rider_id'].nunique()
riders_count
```
## Data Aggregation Fundamentals
A data summary provides insights that would otherwise be difficult to recognize by examining raw data.  
Data aggregation is the data manipulation operation used to summarize data by applying operations to specific columns to produce single values.  
> **Data aggregation solves this by applying an operation to an entire column to produce one meaningful number:**

| Summary type | Example value |
|---|---:|
| Total revenue | \$127,056.74 |
| Smallest order | \$60.01 |
| Average order | \$140.39 |
| Number of unique users | 157 |
## mean vs median
- The **mean** is ***the sum of all values*** divided by the number of values.
- The **median** is ***the middle value*** when data is sorted.
```python
orders_df = pd.read_csv('orders.csv')
```
## Working with DataFrames
- Create a new DataFrame by loading data into Python from CSV files
```python
amounts = orders_df['amount']
```
- To extract a column from a DataFrame
```python
amounts = orders_df['amount']
```
## Summarizing Data in Python
```python
amounts= orders_df['amount']
total_revenue = amounts.sum()
```
> The key summary methods
- **sum():** Calculates the total of all values
- **mean():** Calculates the average value
- **median():** Identifies the middle value when data is sorted
- **min() and max():** Finds the minimum or maximum valuemax()
- **count():** Counts the number of rows or values
- **nunique():** Counts the number of unique values
