# Pandas

[Source: 10 minutes to pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html)

Pandas is a library used for data structures and data analysis

## import

```python
import numpy as np
import pandas as pd
```

## Create Object

### Series

```python
s = pd.Series([1, 3, 5, np.nan, 6, 8])
```

Creates default index values

```txt
0    1.0
1    3.0
2    5.0
3    NaN
4    6.0
5    8.0
dtype: float64
```

### DataFrame

- Primary pandas data structure

#### Example: Pass in dict

```python
df2 = pd.DataFrame(
     {
         "A": 1.0,
         "B": pd.Timestamp("20130102"),
         "C": pd.Series(1, index=list(range(4)), dtype="float32"),
         "D": np.array([3] * 4, dtype="int32"),
         "E": pd.Categorical(["test", "train", "test", "train"]),
         "F": "foo",
    }
)
```

```txt
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
2  1.0 2013-01-02  1.0  3   test  foo
3  1.0 2013-01-02  1.0  3  train  foo
```

## View Data

- `df.head(num)` - can optionally set number of rows
- `df.tail(num)` - can optionally set number of rows
- `df.index` - gets indexes
- `df.columns` - get column names
- `df.to_numpy()` - return numpy representation (expensive for mixed types)
- `df.describe()` - quick stats (count, mean, std, min, etc)
- `df.T` - transpose
- `df.sort_index(axis=1, ascending=False)` - sort by index
- `df.sort_values(by="B")` - sort by value(column name)

## Selection

- `df["A"]` - Selects column "A", same as df.A
- `df[0:3]` - Selects first 3 rows

### Selection by label

- `df.loc[dates[0]]` - Cross section with dates label
- `df.loc[:, ["A", "B"]]` - multi-axis by label
- `df.loc["20130102":"20130104", ["A", "B"]]` - label slicing, inclusive
- `df.at[dates[0], "A"]` - specific value

### Selection by position

- `df.iloc[3]` - 3rd row, all columns, transposed. Inclusive.
- `df.iloc[3:5, 0:2]` - 3rd-5th row, 0-2 column. Like NumPy/Python.
- `df.iloc[[1, 2, 4], [0, 2]]` - List explist row/col to include
- `df.iloc[1:3, :]` - Explicit rows
- `df.iloc[:, 1:3]` - Explicit col
- `df.iat[1, 1]` - Explicit row/col

### Boolean indexing

- `df[df["A"] > 0]` - Select rows matching column criteria
- `df[df > 0]` - Select all values meeting criteria
- `df[df["E"].isin(["two", "four"])]` - Finds rows meeting column criteria

### Setting

- `df["F"] = s1` - add a new column (where s1 is a Series)
- `df.at[dates[0], "A"]` - by label
- `df.iat[0, 1]` - by position
- `df.loc[:, "D"] = np.array([5] * len(df))` - with numpy array
- `df[df > 0] = -df` - turn all values greater than 0 negative

```python
s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range("20130102", periods=6))
```

```txt
                  A         B         C  D    F
2013-01-01  0.000000  0.000000 -1.509059 -5  NaN
2013-01-02 -1.212112 -0.173215 -0.119209 -5 -1.0
2013-01-03 -0.861849 -2.104569 -0.494929 -5 -2.0
2013-01-04 -0.721555 -0.706771 -1.039575 -5 -3.0
2013-01-05 -0.424972 -0.567020 -0.276232 -5 -4.0
2013-01-06 -0.673690 -0.113648 -1.478427 -5 -5.0
```

## Missing Data

- `df1.dropna(how="any")` - drop downs w/ NaN
- `df1.fillna(value=5)` - fill missing data

## Operations

- `df.mean()` - mean per column
- `df.mean(1)` - mean per row
- `df.apply(np.cumsum)` - apply function to all column
- `df.apply(lambda x: x.max() - x.min())` - apply function to get 1 val per column
- `s.value_counts()` - histogram for series
- `s.str.lower()` - lowercase for series (more string methods exist too)

## Merge

- `pd.concat(list of rows)` - Concat pandas objects
- `pd.merge(left, right, on="key")` - sql style join (left and right are df's)

## Grouping

- `df.groupby("A").sum()`

## Reshaping

- Stack
- Pivot tables

## Time series

- `ts.resample("5Min").sum()` - Sampling
- `ts_utc.tz_convert("US/Eastern")` - Time zone conversion
- `ts.to_period()` - Convert to diff period

## Categoricals

```python
df = pd.DataFrame(
     {"id": [1, 2, 3, 4, 5, 6], "raw_grade": ["a", "b", "b", "a", "a", "e"]}
)
```

- `df["grade"] = df["raw_grade"].astype("category")` - Convert raw to categoy
- `df["grade"].cat.categories = ["very good", "good", "very bad"]` rename
- `df.sort_values(by="grade")` - Sort
- `df.groupby("grade").size()` - Group by

## Plotting

```python
import matplotlib.pyplot as plt
plt.close("all")

ts = pd.Series(np.random.randn(1000), index=pd.date_range("1/1/2000", periods=1000))
ts = ts.cumsum()
ts.plot()
```

![plot example](https://pandas.pydata.org/pandas-docs/stable/_images/series_plot_basic.png)

- `plt.figure()`
- `df.plot()`
- `plt.legend(loc='best')`

![plot example](https://pandas.pydata.org/pandas-docs/stable/_images/frame_plot_basic.png)

## Getting data in/out

- `df.to_csv("foo.csv")` - write to csv
- `pd.read_csv("foo.csv")` - read from csv
- `df.to_excel("foo.xlsx", sheet_name="Sheet1")` - write to excel
- `pd.read_excel("foo.xlsx", "Sheet1", index_col=None, na_values=["NA"])` - read from excel
