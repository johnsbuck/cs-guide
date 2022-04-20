---
tags:
  - pandas
  - python
  - data-processing
---

# Pandas
> [!INFO]- References
> [[References#^refbetaveroscheatsheet]]

`import pandas as pd`

```python
pd.read_csv(magic)
pd.read_table(magic) # tsv by default
```

1-D data is Series. 2-D data is DataFrame (`df` below). They are quite similar to ndarray. The most important philosophical difference is that the data in Series and DataFrame are tightly associated with their labels or indices, and operations generally preserve and operate between corresponding labels and indices (for example, slicing [1:4] will give a result that still has indices 1, 2, 3) (whereas in ndarrays, operations operate between corresponding fickle positions.)

## Basic Info
```python
df.info() # sizes etc
df.describe() # descriptive statistics: count, mean etc.
df['colname'].dtype # data type
s.values, df.values # ndarray
s.shape, df.shape # consistent with ndarray
series.index; df.index, df.columns
```

## Slicing
```python
df.loc['foo'] # index by label only
df.iloc[3] # index by numeric position only, 0-indexed

df.head(10)
df.tail(10)

df.drop([1, 2, 3])
df.drop(['colname'], axis=1)
```

## New Columns
```python
df['newcolname'] = df['colname'] + 1
# nondestructive:
df.assign({'newcolname': df['colname'] + 1})
df.assign(newcolname = df['colname'] + 1)
```

## Index Manipulation
```python
series.reindex(list_of_indices)
# always keeps association between each index and its data
# drops missing indexes; fills new indexes with NaN

series.sort_index()

series/df.rename(index: dict|(OldIndex → NewIndex)) # changes indexes
df.rename(columns: dict|(OldIndex → NewIndex)) # changes column labels
# or, where mapper : dict | (OldIndex → NewIndex)
df.rename(mapper, axis='index')
df.rename(mapper, axis='columns')

df.set_index('colname') # makes the data indexed by existing column 'colname'
df.set_index(['colname', 'colname2']) # ditto, but MultiIndex
df.reset_index() # demotes index to a normal column, index by 0, 1, etc.
```

If you just want to blow away the index-value association, I think `df.values` is the best way.

## Working with Conditions

```python
df1 == df2, df1 < df2 # etc. broadcast fine
(df + df).equals(df * 2) # ==, except NaN equals NaN
(df['colname'] < 10).count()
df[df['colname'] < 10]
df[df['colname'].isin([1, 3, 5, 7, 9])]
```

## NaN

```python
np.nan
df.dropna(how='any')
df.fillna(value=5)
df.isna(), df.notna() # because nan != nan
```

## Reductions
```python
series.empty, df.empty

series.mean(), series.sum(), series.std(), ...
series.nunique() # number of distinct values
series.value_counts() # value → frequency of value
pd.cut(series, num_bins), pd.qcut(series)
series.idxmin(), series.idxmax() # index of min and max; like argmax, the name numpy uses
series.agg(np.sum)
(series > 0).all(), .any(), .bool()
# above can also be done to DataFrames
# gives results are per column, for columns that make sense
```

## Maps/Lifting

`df.pipe(f) # f(df), but may be more readable/chainable style`

### Lift (ndarray → ndarray) to (DataFrame → DataFrame)
```python
df.transform(np.abs) # map on underlying ndarray
df.transform([np.abs, lambda x: x + 1])
```  

### Lift (Series → Series) to (DataFrame → DataFrame)
```python
df.apply(lambda x: x.max() - x.min(0)) # map columns
```

### Lift (ndarray → ndarray) to (Series → Series) (also works to lift element → element?)
```python
series.apply(some_function)
df['colname'].apply(some_function) # map each element of column
df['colname'] = df['colname'].apply(some_function)
```

### Lift (element → element) to (Series → Series)
```python
series.map(lambda x: len(str(x))) # elementwise map
```

### Lift (element → element) to (DataFrame → DataFrame)
```python
df.applymap(lambda x: len(str(x))) # elementwise map
```

### Cast
```python
df.astype('float32')
```
 
### Proxy-ish Map [string operations](https://pandas.pydata.org/pandas-docs/stable/user_guide/text.html)
```python
series.str.lower()
```

## Sort
```python
df.sort_values(by='colname')
df.sort_values(by='colname', ascending=False)
df['colname'].sort_values()
```

## More Stats
```python
df.cov()
series1.corr(series2, _[method='pearson'|'kendall'|'spearman']_)
series.rank() # minimum is 1, maximum is n; ties become average of tied ranks by default
```

## Group (split → apply → combine)
Aggregation: Lifts Group[S] → T reducer. New DataFrame, indexed by value in grouped-by column; value is result

```python
df.groupby('colname').mean() # mean per group
etc: .size() .sum() .std()
df.groupby('colname').describe()
df.groupby('colname').colname2.agg(['mean', 'min', 'max'])
df.groupby('colname').get_group('colvalue')
df.groupby(('colname1', 'colname2'))
```

Transformation: Lifts Group[S] → Group[T] mapper. New DataFrame, same index, each group's values mapped separately

```python
df.groupby('colname').transform(lambda x: x - x.mean())
```

Filtration: Keep only items in groups satisfying the condition.

```python
df.filter(lambda group: group['colname'].sum() > 300)
```
