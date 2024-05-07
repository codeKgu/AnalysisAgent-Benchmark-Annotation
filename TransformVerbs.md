## Transform verbs

### Derive

Derive a new column value based on the provided expressions.

```python
import pandas as pd
import numpy as np

# create a dataframe
df = pd.DataFrame({{
    'x': [1, 2, 3, 4, 5],
    'y': [5, 4, 3, 2, 1]
}})

# derive a new column 'sumXY' by adding 'x' and 'y'
df['sumXY'] = df['x'] + df['y']
```

|     | input_cols | output_cols |
| --: | :--------- | :---------- |
|   0 | ['x', 'y'] | sumXY       |

### Filter

Filter a table to a subset of rows based on the input criteria.
Filter should not have any output column

```python
# filter the dataframe to include only rows where 'x' is greater than 2
df = df[df['x'] > 2]
```

|     | input_cols | output_cols |
| --: | :--------- | :---------- |
|   0 | []         |             |

### Slice

Extract rows with indices from start to end (end not included).

```python
# slice the dataframe to include rows 2 to 4
df = df.iloc[2:4]
```

|     | input_cols | output_cols |
| --: | :--------- | :---------- |
|   0 | ['']       |             |

### Groupby

Group table rows based on a set of column values. Groupby should return a pandas groupby object in which we can perform subsequent derive or aggregate (rollup) operations. The groupby verb should not have any output column.

```python
# group the dataframe by 'x'
grouped = df.groupby('x')
```

|     | input_cols | output_cols |
| --: | :--------- | :---------- |
|   0 | ['x']      |             |

### De-duplicate

De-duplicate table rows by removing repeated row values.
This verb should not have any output column. If you are dropping rows based on a subset of columns, please specify that in the _input columns_.

```python
# remove duplicate rows in the dataframe
df = df.drop_duplicates()
```

|     | input_cols | output_cols |
| --: | :--------- | :---------- |
|   0 | []         |             |

### Impute

Impute missing values or rows.
This verb should not have any output column. If you are imputing values based on a subset of columns, please specify that in the _input columns_.

```python
# replace NaN values in column 'x' with the mean of 'x'
df['x'] = df['x'].fillna(df['x'].mean())
```

|     | input_cols | output_cols |
| --: | :--------- | :---------- |
|   0 | ['x']      |             |

### Rollup

Rollup a table to produce an aggregate summary. This is used in conjunction with groupby when we aggregate a group. Rollup should have an output column that represents the aggregated value.

_Prev code block_:

```python
df_grp = df.groupby('x')
```

_Rollup code block_:

```python
df = df_grp.agg(mean_y=('y', 'mean')).reset_index()
```

|     | input_cols | output_cols |
| --: | :--------- | :---------- |
|   0 | ['y']      | mean_y      |

## Example of multiple input to output column mappings

Sometimes, there can be multiple input and output column mappings for a single transform verb. To make it easier to specify, in such cases, you add multiple the input and output column mappings.

_Prev code block_:

```python
data = {'category': ['A', 'A', 'B', 'B', 'B', 'C', 'C'],
        'x': [1, 2, 3, 4, 5, 6, 7],
        'y': [10, 20, 30, 40, 50, 60, 70]}

df = pd.DataFrame(data)
df_grp = df.groupby('category')
```

_Current code block_:

```python
def calculate_z(group):
    # Example calculation: multiplying 'x' and 'y' within each group
    group['z'] = group['x'] * group['y']
    group['diff'] = group['x'] - group['y']
    return group

df = df_grp.apply(calculate_z)
```

Since we are not aggregating any columns after the groupby, the verb is `derive`.
| | input_cols | output_cols |
|---:|:-------------|:--------------|
| 0 | ['x', 'y'] | z |
| 1 | ['x', 'y'] | diff |
