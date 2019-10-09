# Common Pandas Operations

![Cimberly_panda_from_Pixabay.jpg](https://serve-md.charlesdlandau.net/img/panda.jpg)
_Image by [Cimberley](https://pixabay.com/photos/panda-panda-bear-sleep-rest-relax-1236875/) on Pixabay_

## Why?

Why am I writing the billionth article on Pandas data munging? Aren't the other articles enough? Maybe! If you already read a few of those and you feel like you know a lot about Pandas maybe your time would be better spent moving on to some of these materials:

* [Python for Data Analysis by Pandas BDFL Wes McKinney](https://github.com/wesm/pydata-book)
* [Enhancing Performance by the Pandas dev team](https://pandas.pydata.org/pandas-docs/stable/user_guide/enhancingperf.html)
* [Introduction to Dask for parallel computing in Python](https://docs.dask.org/en/latest/)
* [The Python Data Science Handbook by Jake VanderPlas](https://jakevdp.github.io/PythonDataScienceHandbook/)

## Getting started

_Quick note: throughout this article I'm going to refer to features and observations. When you start working with multidimensional problems it's an important distinctions, but in this article a feature is the same as a spreadsheet column, and an observation is the same as a spreadsheet row._


```python
import numpy as np
import pandas as pd
```

We're going to work with some random data, which you can construct directly. Pandas `DataFrames` are constructed from Pandas `Series` objects, which in turn use `numpy` objects and operations for many of their behaviors.


```python
# Create a DataFrame with some randomized columns
df = pd.DataFrame({
    "str_feature": [np.random.choice(["dog", "cat", "snake"]) for _ in range(10000)],
    "int_feature": np.arange(10000)
})
```

The `.head` method can grab the top `n` rows, five by default.


```python
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>dog</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>cat</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>cat</td>
      <td>3</td>
    </tr>
    <tr>
      <td>4</td>
      <td>dog</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>dog</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>cat</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>cat</td>
      <td>3</td>
    </tr>
    <tr>
      <td>4</td>
      <td>dog</td>
      <td>4</td>
    </tr>
    <tr>
      <td>5</td>
      <td>snake</td>
      <td>5</td>
    </tr>
    <tr>
      <td>6</td>
      <td>snake</td>
      <td>6</td>
    </tr>
    <tr>
      <td>7</td>
      <td>dog</td>
      <td>7</td>
    </tr>
    <tr>
      <td>8</td>
      <td>cat</td>
      <td>8</td>
    </tr>
    <tr>
      <td>9</td>
      <td>dog</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>



Of course in some circumstances we don't want to take the first 5 or 10. We can take a pseudo-random sample of the data using `.sample`. Note that `.sample` defaults to 1 observation.


```python
df.sample(5)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2476</td>
      <td>cat</td>
      <td>2476</td>
    </tr>
    <tr>
      <td>2793</td>
      <td>dog</td>
      <td>2793</td>
    </tr>
    <tr>
      <td>3120</td>
      <td>snake</td>
      <td>3120</td>
    </tr>
    <tr>
      <td>9338</td>
      <td>snake</td>
      <td>9338</td>
    </tr>
    <tr>
      <td>3593</td>
      <td>dog</td>
      <td>3593</td>
    </tr>
  </tbody>
</table>
</div>



We can get the underlying types by accessing the `.dtypes` attribute.


```python
df.dtypes
```




    str_feature    object
    int_feature     int32
    dtype: object



It should be stressed that `object` features are less memory efficient than `int` features. A discussion of the basic `dtypes` is included in the [official getting started guide](https://pandas.pydata.org/pandas-docs/stable/getting_started/basics.html#basics-dtypes). 

Descriptive statistics can be reported using `.describe` -- note that only numerical features will be summarized.


```python
df.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>10000.00000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>4999.50000</td>
    </tr>
    <tr>
      <td>std</td>
      <td>2886.89568</td>
    </tr>
    <tr>
      <td>min</td>
      <td>0.00000</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>2499.75000</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>4999.50000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>7499.25000</td>
    </tr>
    <tr>
      <td>max</td>
      <td>9999.00000</td>
    </tr>
  </tbody>
</table>
</div>



The descriptive statistics in this report have their own methods, for example here's `std`:


```python
df.std()
```




    int_feature    2886.89568
    dtype: float64



`pandas-profiling` is a new-ish package that creates much more detailed reports from a `DataFrame`. You can check it out [here](https://github.com/pandas-profiling/pandas-profiling).

`.size` returns the number of observations multiplied by the number of features.


```python
df.size
```




    20000



`.info` returns metadata about the `DataFrame`


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10000 entries, 0 to 9999
    Data columns (total 2 columns):
    str_feature    10000 non-null object
    int_feature    10000 non-null int32
    dtypes: int32(1), object(1)
    memory usage: 78.2+ KB
    

## Selection and Assignment

We tend to access data from the `DataFrame` using the following patterns:

1. `df[condition]`
2. `df.iloc` & `df.loc`
3. `df.at` & `df.iat`


```python
# First pattern
df[df['str_feature'] == 'snake'].head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
    </tr>
    <tr>
      <td>5</td>
      <td>snake</td>
      <td>5</td>
    </tr>
    <tr>
      <td>6</td>
      <td>snake</td>
      <td>6</td>
    </tr>
    <tr>
      <td>14</td>
      <td>snake</td>
      <td>14</td>
    </tr>
    <tr>
      <td>18</td>
      <td>snake</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Second pattern
df.loc[:, "int_feature"]
```




    0          0
    1          1
    2          2
    3          3
    4          4
            ... 
    9995    9995
    9996    9996
    9997    9997
    9998    9998
    9999    9999
    Name: int_feature, Length: 10000, dtype: int32



Note that here the lone `:` means to access everything on that axis, the same way `my_list[:]` tells a plain Python `list` to access everything.

`.iloc` behaves similarly but works on axes only.


```python
df.iloc[0:5]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>dog</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>cat</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>cat</td>
      <td>3</td>
    </tr>
    <tr>
      <td>4</td>
      <td>dog</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[[0,5]]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
    </tr>
    <tr>
      <td>5</td>
      <td>snake</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



`.at` and `.iat` are used similarly to `.loc` and `.iloc` but they can't retrieve more than one observation.

`df.at[0:5, 'str_feature']` will give a `ValueError` because of `0:5`.

`df.at[0]` will give a `TypeError` because there is no `col` specified.


```python
df.at[0, 'str_feature']
```




    'snake'



There is a [discussion](https://stackoverflow.com/a/37216587/10553976) on SO about how you can get some performance gains by accepting the limitations of `.at`.

There's one more method I want to cover for accessing data from our `DataFrame`:


```python
df.int_feature.head()
```




    0    0
    1    1
    2    2
    3    3
    4    4
    Name: int_feature, dtype: int32



This works fine until:

1. Your feature is named `my super feature` and you try to access `df.my super feature`
2. You forget that the reverse operation is illegal.

That's right, you cannot do this:


```python
df.new_feature = df[df['int_feature'] == 1]
```

    c:\users\cdl\documents\personalproject\common_pandas_blog\.venv\lib\site-packages\ipykernel_launcher.py:1: UserWarning: Pandas doesn't allow columns to be created via a new attribute name - see https://pandas.pydata.org/pandas-docs/stable/indexing.html#attribute-access
      """Entry point for launching an IPython kernel.
    

Trying to will trigger a warning, at least in in newer versions of `Pandas`. We can confirm that the feature was not added to the `DataFrame`:


```python
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>dog</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>cat</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>cat</td>
      <td>3</td>
    </tr>
    <tr>
      <td>4</td>
      <td>dog</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



Instead, let's add a new feature like so:


```python
df['new_feature'] = df.loc[:, 'str_feature'] == 'snake'
```

Now we can see that the new feature was added properly:


```python
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>new_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
      <td>True</td>
    </tr>
    <tr>
      <td>1</td>
      <td>dog</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <td>2</td>
      <td>cat</td>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <td>3</td>
      <td>cat</td>
      <td>3</td>
      <td>False</td>
    </tr>
    <tr>
      <td>4</td>
      <td>dog</td>
      <td>4</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



## Manipulation

Let's perform some common tasks to manipulate our data. Namely:

1. Filter values
2. Handle missing or invalid values
3. SQL-like join two `DataFrame`s
4. SQL-like groupby


### 1. Filtering values

To filter values, we can simply overwrite our `df` variable like so:


```python
# We're cat people now.
df = df[df.loc[:, 'str_feature'] == 'cat']
```


```python
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>new_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2</td>
      <td>cat</td>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <td>3</td>
      <td>cat</td>
      <td>3</td>
      <td>False</td>
    </tr>
    <tr>
      <td>8</td>
      <td>cat</td>
      <td>8</td>
      <td>False</td>
    </tr>
    <tr>
      <td>10</td>
      <td>cat</td>
      <td>10</td>
      <td>False</td>
    </tr>
    <tr>
      <td>11</td>
      <td>cat</td>
      <td>11</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



Since we've just filtered our `DataFrame` to use only use cat rows, our snakes-only `new_feature` will be full of `False` values and `str_feature` will be all `cat`s. Let's filter our columns.


```python
df.drop(["new_feature", "str_feature"], axis=1, inplace=True)
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>int_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <td>10</td>
      <td>10</td>
    </tr>
    <tr>
      <td>11</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



As discussed [here](https://stackoverflow.com/a/18145399/10553976), the `axis` argument tells `pandas` to drop on the basis of the `features` axis, and the `inplace` argument makes this equivalent to `df = df.drop(...)`.

As a final note, don't make the mistake of assuming that any of these methods will automatically free up memory. Instead, use tools like Python `del` and `pandas` I/O features like `low_memory` or `memory_map`.

### 2. Handle missing or invalid values

Another common data munging task is to handle cases where the data is invalid in some way. Let's make a new, messy `DataFrame`:


```python
# Create a DataFrame with some randomized columns
df = pd.DataFrame({
    "str_feature": [np.random.choice(["dog", "cat", "snake"]) for _ in range(10000)],
    "int_feature": np.arange(10000),
    "messy_feature": [np.random.choice([1, None, np.nan, False, True, -1]) for _ in range(10000)]
})
```

This simplest thing we could do is clean our data by filtering all observations with `np.nan`, using `.isna`:


```python
# Note the rarely used ~ negation operator below
clean_df = df[~df['messy_feature'].isna()]
clean_df.sample(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>messy_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>4134</td>
      <td>cat</td>
      <td>4134</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>2127</td>
      <td>snake</td>
      <td>2127</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>7014</td>
      <td>dog</td>
      <td>7014</td>
      <td>False</td>
    </tr>
    <tr>
      <td>922</td>
      <td>dog</td>
      <td>922</td>
      <td>False</td>
    </tr>
    <tr>
      <td>5451</td>
      <td>dog</td>
      <td>5451</td>
      <td>True</td>
    </tr>
    <tr>
      <td>5059</td>
      <td>cat</td>
      <td>5059</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>1289</td>
      <td>snake</td>
      <td>1289</td>
      <td>False</td>
    </tr>
    <tr>
      <td>5016</td>
      <td>dog</td>
      <td>5016</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>9765</td>
      <td>snake</td>
      <td>9765</td>
      <td>True</td>
    </tr>
    <tr>
      <td>7094</td>
      <td>cat</td>
      <td>7094</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



As you can see, this method will clear just the `NaN` values. That's great, but what if we only want values that are truthy in Python?

`df[df['messy_feature']]` won't work, because it's a `ValueError` to filter using a column with `NaN` values. Instead, we have to filter the `NaN`, then check for truthiness. Just like with a plain Python conditional `and` statement, the conditions are checked left to right.


```python
clean_df = df[~df['messy_feature'].isna() & df['messy_feature']]
clean_df.sample(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>messy_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1049</td>
      <td>snake</td>
      <td>1049</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4769</td>
      <td>snake</td>
      <td>4769</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>4924</td>
      <td>cat</td>
      <td>4924</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>6907</td>
      <td>snake</td>
      <td>6907</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>5914</td>
      <td>dog</td>
      <td>5914</td>
      <td>True</td>
    </tr>
    <tr>
      <td>6291</td>
      <td>cat</td>
      <td>6291</td>
      <td>1</td>
    </tr>
    <tr>
      <td>5872</td>
      <td>dog</td>
      <td>5872</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1794</td>
      <td>cat</td>
      <td>1794</td>
      <td>1</td>
    </tr>
    <tr>
      <td>8987</td>
      <td>cat</td>
      <td>8987</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3848</td>
      <td>dog</td>
      <td>3848</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>



Imputing values is easy too:


```python
clean_df = df.copy()
clean_df['messy_feature'].fillna(clean_df['str_feature'], inplace=True)
clean_df.sample(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>messy_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>8063</td>
      <td>snake</td>
      <td>8063</td>
      <td>snake</td>
    </tr>
    <tr>
      <td>3901</td>
      <td>cat</td>
      <td>3901</td>
      <td>cat</td>
    </tr>
    <tr>
      <td>3702</td>
      <td>dog</td>
      <td>3702</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>906</td>
      <td>dog</td>
      <td>906</td>
      <td>1</td>
    </tr>
    <tr>
      <td>8039</td>
      <td>dog</td>
      <td>8039</td>
      <td>dog</td>
    </tr>
    <tr>
      <td>2433</td>
      <td>dog</td>
      <td>2433</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>4996</td>
      <td>snake</td>
      <td>4996</td>
      <td>False</td>
    </tr>
    <tr>
      <td>3015</td>
      <td>snake</td>
      <td>3015</td>
      <td>snake</td>
    </tr>
    <tr>
      <td>8307</td>
      <td>cat</td>
      <td>8307</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1904</td>
      <td>cat</td>
      <td>1904</td>
      <td>cat</td>
    </tr>
  </tbody>
</table>
</div>



And we can go beyond simple `.fillna`:


```python
clean_df = df.copy()
clean_df['messy_feature'] = clean_df.apply(
    lambda row: row['messy_feature'] if row['messy_feature'] == -1 else row['str_feature'],
    axis=1
)
clean_df.sample(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>messy_feature</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>9217</td>
      <td>snake</td>
      <td>9217</td>
      <td>snake</td>
    </tr>
    <tr>
      <td>5018</td>
      <td>snake</td>
      <td>5018</td>
      <td>snake</td>
    </tr>
    <tr>
      <td>6383</td>
      <td>dog</td>
      <td>6383</td>
      <td>dog</td>
    </tr>
    <tr>
      <td>1297</td>
      <td>cat</td>
      <td>1297</td>
      <td>-1</td>
    </tr>
    <tr>
      <td>9701</td>
      <td>dog</td>
      <td>9701</td>
      <td>dog</td>
    </tr>
    <tr>
      <td>5192</td>
      <td>cat</td>
      <td>5192</td>
      <td>cat</td>
    </tr>
    <tr>
      <td>4018</td>
      <td>dog</td>
      <td>4018</td>
      <td>dog</td>
    </tr>
    <tr>
      <td>7619</td>
      <td>snake</td>
      <td>7619</td>
      <td>snake</td>
    </tr>
    <tr>
      <td>9890</td>
      <td>dog</td>
      <td>9890</td>
      <td>dog</td>
    </tr>
    <tr>
      <td>5102</td>
      <td>snake</td>
      <td>5102</td>
      <td>snake</td>
    </tr>
  </tbody>
</table>
</div>



Just be careful with `.apply` as it has the potential to significantly degrade performance. See: [enhancing performance](https://pandas.pydata.org/pandas-docs/stable/user_guide/enhancingperf.html) in the Pandas docs.


### 3. SQL-like joins

I would definitely recommend that you learn about SQL joins and then just map that knowledge to Pandas joins. The arguments like `on`, `how`, `left`, and `right` all make sense if you just keep in mind how the joins would work in SQL.


```python
merged_df = df.copy().join(df.copy(), how='left', lsuffix="_l", rsuffix="_r")
merged_df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature_l</th>
      <th>int_feature_l</th>
      <th>messy_feature_l</th>
      <th>str_feature_r</th>
      <th>int_feature_r</th>
      <th>messy_feature_r</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
      <td>False</td>
      <td>snake</td>
      <td>0</td>
      <td>False</td>
    </tr>
    <tr>
      <td>1</td>
      <td>cat</td>
      <td>1</td>
      <td>NaN</td>
      <td>cat</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2</td>
      <td>snake</td>
      <td>2</td>
      <td>False</td>
      <td>snake</td>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <td>3</td>
      <td>dog</td>
      <td>3</td>
      <td>False</td>
      <td>dog</td>
      <td>3</td>
      <td>False</td>
    </tr>
    <tr>
      <td>4</td>
      <td>snake</td>
      <td>4</td>
      <td>None</td>
      <td>snake</td>
      <td>4</td>
      <td>None</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>9995</td>
      <td>snake</td>
      <td>9995</td>
      <td>False</td>
      <td>snake</td>
      <td>9995</td>
      <td>False</td>
    </tr>
    <tr>
      <td>9996</td>
      <td>dog</td>
      <td>9996</td>
      <td>True</td>
      <td>dog</td>
      <td>9996</td>
      <td>True</td>
    </tr>
    <tr>
      <td>9997</td>
      <td>snake</td>
      <td>9997</td>
      <td>NaN</td>
      <td>snake</td>
      <td>9997</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>9998</td>
      <td>dog</td>
      <td>9998</td>
      <td>NaN</td>
      <td>dog</td>
      <td>9998</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>9999</td>
      <td>dog</td>
      <td>9999</td>
      <td>1</td>
      <td>dog</td>
      <td>9999</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>10000 rows × 6 columns</p>
</div>



### 4. SQL-like groupby

Like the SQL JOIN advice, I strongly recommend learning about SQL GROUP BY and letting that be the basis of your Pandas `.groupby` knowledge. Here's a simple GROUP BY SUM in Pandas


```python
df.groupby('str_feature').sum()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>int_feature</th>
    </tr>
    <tr>
      <th>str_feature</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>cat</td>
      <td>16476822</td>
    </tr>
    <tr>
      <td>dog</td>
      <td>16729175</td>
    </tr>
    <tr>
      <td>snake</td>
      <td>16789003</td>
    </tr>
  </tbody>
</table>
</div>



Note that `.groupby` must be called by a method that aggregates the grouping. Let's see what happens if we neglect that part...


```python
df.groupby('str_feature')
```




    <pandas.core.groupby.generic.DataFrameGroupBy object at 0x0A6F91D0>



`.groupby` is particularly powerful when combined with the `.apply` method...but my earlier warning about the performance of `.apply` still holds.


```python
# Simple sum/size calculation
df.groupby(['str_feature']).apply(lambda x: x.sum()/x.size)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>int_feature</th>
      <th>messy_feature</th>
    </tr>
    <tr>
      <th>str_feature</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>cat</td>
      <td>2485.191855</td>
      <td>0.088688</td>
    </tr>
    <tr>
      <td>dog</td>
      <td>2509.627213</td>
      <td>0.086709</td>
    </tr>
    <tr>
      <td>snake</td>
      <td>2504.326223</td>
      <td>0.089499</td>
    </tr>
  </tbody>
</table>
</div>



## Moving Windows

Often we want to calculate rolling values (like rolling sums):


```python
df['rolling'] = df.rolling(3).sum()
df.head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>messy_feature</th>
      <th>rolling</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>snake</td>
      <td>0</td>
      <td>False</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>1</td>
      <td>cat</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2</td>
      <td>snake</td>
      <td>2</td>
      <td>False</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>dog</td>
      <td>3</td>
      <td>False</td>
      <td>6.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>snake</td>
      <td>4</td>
      <td>None</td>
      <td>9.0</td>
    </tr>
    <tr>
      <td>5</td>
      <td>snake</td>
      <td>5</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <td>6</td>
      <td>snake</td>
      <td>6</td>
      <td>None</td>
      <td>15.0</td>
    </tr>
    <tr>
      <td>7</td>
      <td>cat</td>
      <td>7</td>
      <td>-1</td>
      <td>18.0</td>
    </tr>
    <tr>
      <td>8</td>
      <td>cat</td>
      <td>8</td>
      <td>NaN</td>
      <td>21.0</td>
    </tr>
    <tr>
      <td>9</td>
      <td>snake</td>
      <td>9</td>
      <td>NaN</td>
      <td>24.0</td>
    </tr>
  </tbody>
</table>
</div>



## Indexing by Time

A related task is to use datetime, which we accomplish using `pd.to_datetime`. There are helper methods like `.astype` for casting to datetime types, as well as `.set_index` if we want to use a `datetime` column as our index.


```python
import time
df['Datetime'] = pd.to_datetime([time.time() - (86400*x) for x in range(df.shape[0])], unit='s')

# Round to nearest day
# see https://stackoverflow.com/a/13788301/10553976
df['Datetime'] = np.round(df['Datetime'].astype(np.int64), -9).astype('datetime64[ns]')

# Set as index
df = df.set_index('Datetime')
```

## I/O

One of the most attractive features of Pandas is the robust I/O capabilities. The whole list is [here](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html) but let's take a look at two important ones, CSV and SQL.

### CSV

Writing a CSV in Pandas couldn't be easier:


```python
df.to_csv('./data.csv')
```

Reading it back in is just as simple:


```python
df = pd.read_csv('./data.csv')
```

But wait: let's look at our `df` now...


```python
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Datetime</th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>messy_feature</th>
      <th>rolling</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2019-10-08 21:51:50</td>
      <td>snake</td>
      <td>0</td>
      <td>False</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2019-10-07 21:51:50</td>
      <td>cat</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2019-10-06 21:51:50</td>
      <td>snake</td>
      <td>2</td>
      <td>False</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2019-10-05 21:51:50</td>
      <td>dog</td>
      <td>3</td>
      <td>False</td>
      <td>6.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2019-10-04 21:51:50</td>
      <td>snake</td>
      <td>4</td>
      <td>NaN</td>
      <td>9.0</td>
    </tr>
  </tbody>
</table>
</div>



We can't infer that any particular column is the index, so `.read_csv` created a `DataFrame` with a simple index instead of using the `DatetimeIndex` we set before writing to a file. Of course we could always use `.set_index` after the fact, but in resource constrained scenarios we want control of how large the created `DataFrame` will be. Searching for a solution, we come to the `.read_csv` [documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html) and the power of this tool immediately becomes apparent:

* Read CSVs from URL (!!!!!), path, or file-like
* Specify the exact window in a CSV file to read from with kwargs like `header`, `index_col`, `usecols`, `nrows`, `skiprows`, and `skipfooter`
* Manage `NaN` with kwargs like `na_filter` and `na_values`
* `sep` for handling `tsv` and other delimiter situations
* Kwargs to help save memory like `low_memory`, `memory_map`, and `dtype`

Armed with tons of new knowledge about how to read csv files with Pandas, we can confidently read our `data.csv` file with the `DatetimeIndex` specified.


```python
df = pd.read_csv('./data.csv', index_col='Datetime')
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>messy_feature</th>
      <th>rolling</th>
    </tr>
    <tr>
      <th>Datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019-10-08 21:51:50</td>
      <td>snake</td>
      <td>0</td>
      <td>False</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2019-10-07 21:51:50</td>
      <td>cat</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2019-10-06 21:51:50</td>
      <td>snake</td>
      <td>2</td>
      <td>False</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>2019-10-05 21:51:50</td>
      <td>dog</td>
      <td>3</td>
      <td>False</td>
      <td>6.0</td>
    </tr>
    <tr>
      <td>2019-10-04 21:51:50</td>
      <td>snake</td>
      <td>4</td>
      <td>NaN</td>
      <td>9.0</td>
    </tr>
  </tbody>
</table>
</div>



Just the way it originally was! Let's clean that file up:


```python
import os
os.remove('./data.csv')
```

_Note: if you aren't using Pandas features for data manipulation, consider using the stdlib `csv` library for I/O instead of Pandas. Pandas objects create some overhead in memory, and Pandas itself is a nontrivial dependency._

### SQL

If you want to read and write data that lives in a SQL database, Pandas uses the `pd.read_sql` and `.to_sql` methods to interface with databases. In this example we'll use the stdlib `sqlite3` dbAPI, but Pandas can integrate with any kind of DB that is supported by [sqlalchemy](https://www.sqlalchemy.org/). Also note that we're going to use a `try...finally` block to enforce `.close` always being called.


```python
import sqlite3

# DB operations
try:
    # Connect to db
    conn = sqlite3.connect('data.db')
    # Write dataframe to table 'df'
    df.to_sql('df', conn, if_exists='replace')
    
    # Read a SELECT * query of the same table, specifying index column
    sqldf = pd.read_sql('SELECT * FROM df', conn, index_col='Datetime')

# Ensure the connection closes
finally:
    conn.close()

# Cleanup file
os.remove('data.db')
```


```python
sqldf.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>str_feature</th>
      <th>int_feature</th>
      <th>messy_feature</th>
      <th>rolling</th>
    </tr>
    <tr>
      <th>Datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019-10-08 21:51:50</td>
      <td>snake</td>
      <td>0</td>
      <td>False</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2019-10-07 21:51:50</td>
      <td>cat</td>
      <td>1</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2019-10-06 21:51:50</td>
      <td>snake</td>
      <td>2</td>
      <td>False</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>2019-10-05 21:51:50</td>
      <td>dog</td>
      <td>3</td>
      <td>False</td>
      <td>6.0</td>
    </tr>
    <tr>
      <td>2019-10-04 21:51:50</td>
      <td>snake</td>
      <td>4</td>
      <td>None</td>
      <td>9.0</td>
    </tr>
  </tbody>
</table>
</div>



Once again our data has made a round trip from memory to disk and back again. Well done Pandas and well done to you, dear reader!

## Wrapping Up

In this article you've learned how Pandas implements some basic data munging tasks, specifically:

1. Extracting metadata and descriptive statistics
2. Filtering, imputing, joining and grouping data
3. Windowed functions (like rolling sum)
4. Using Datetime data and indexing by time
5. Reading and writing data to files and databases

Happy wrangling!
