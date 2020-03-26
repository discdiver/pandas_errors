# Common Pandas Errors
## By Jeff Hale

Each error is explained, and example is shown, and then the correct code is shown, if applicable. 

If you have other common errors you think would be helpful for others, please leave them in the comments and ping me on Twitter @discdiver. 

See my [Memorable Python](https://memorablepython.com) and [Memorable Pandas](https://memorablepandas.com) books to learn Python 🐍 and pandas 🐼!


```python
import pandas as pd
import numpy as np
```

### Make a DataFrame


```python
df = pd.DataFrame(dict(col1=['a', 'b', 'c']), index=['first', 'second', 'third'])
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>first</th>
      <td>a</td>
    </tr>
    <tr>
      <th>second</th>
      <td>b</td>
    </tr>
    <tr>
      <th>third</th>
      <td>c</td>
    </tr>
  </tbody>
</table>
</div>



#### Using the pandas library by the name `pandas` when you created an alias for it.


```python
pandas.__version__
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-2-068244f7ed4b> in <module>
    ----> 1 pandas.__version__
    

    NameError: name 'pandas' is not defined



```python
pd.__version__
```




    '1.0.1'



#### Calling `sort_values()` on a DataFrame without an column name to sort by.


```python
df.sort_values()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-23-9baf0daa5fc6> in <module>
    ----> 1 df.sort_values()
    

    TypeError: sort_values() missing 1 required positional argument: 'by'



```python
df.sort_values('col1')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>first</th>
      <td>a</td>
    </tr>
    <tr>
      <th>second</th>
      <td>b</td>
    </tr>
    <tr>
      <th>third</th>
      <td>c</td>
    </tr>
  </tbody>
</table>
</div>



#### Calling a Series method on a DataFrame when there is no DataFrame method of the same name.


```python
df.value_counts()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-25-986e25863b45> in <module>
    ----> 1 df.value_counts()
    

    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/generic.py in __getattr__(self, name)
       5272             if self._info_axis._can_hold_identifiers_and_holds_name(name):
       5273                 return self[name]
    -> 5274             return object.__getattribute__(self, name)
       5275 
       5276     def __setattr__(self, name: str, value) -> None:


    AttributeError: 'DataFrame' object has no attribute 'value_counts'



```python
df['col1'].value_counts()
```




    b    1
    c    1
    a    1
    Name: col1, dtype: int64



#### Using `.iloc[]` to try to index by row name.


```python
df.iloc['a']
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-27-37589c22a003> in <module>
    ----> 1 df.iloc['a']
    

    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/indexing.py in __getitem__(self, key)
       1765 
       1766             maybe_callable = com.apply_if_callable(key, self.obj)
    -> 1767             return self._getitem_axis(maybe_callable, axis=axis)
       1768 
       1769     def _is_scalar_access(self, key: Tuple):


    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/indexing.py in _getitem_axis(self, key, axis)
       2132             key = item_from_zerodim(key)
       2133             if not is_integer(key):
    -> 2134                 raise TypeError("Cannot index by location index with a non-integer key")
       2135 
       2136             # validate the location


    TypeError: Cannot index by location index with a non-integer key



```python
df.loc['second']
```




    col1    b
    Name: second, dtype: object



#### Subsetting by an row index number that doesn't exist.


```python
df.iloc[3]
```


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-19-dd7c84f36f80> in <module>
    ----> 1 df.iloc[3]
    

    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/indexing.py in __getitem__(self, key)
       1765 
       1766             maybe_callable = com.apply_if_callable(key, self.obj)
    -> 1767             return self._getitem_axis(maybe_callable, axis=axis)
       1768 
       1769     def _is_scalar_access(self, key: Tuple):


    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/indexing.py in _getitem_axis(self, key, axis)
       2135 
       2136             # validate the location
    -> 2137             self._validate_integer(key, axis)
       2138 
       2139             return self._get_loc(key, axis=axis)


    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/indexing.py in _validate_integer(self, key, axis)
       2060         len_axis = len(self.obj._get_axis(axis))
       2061         if key >= len_axis or key < -len_axis:
    -> 2062             raise IndexError("single positional indexer is out-of-bounds")
       2063 
       2064     def _getitem_tuple(self, tup: Tuple):


    IndexError: single positional indexer is out-of-bounds



```python
df.iloc[2]
```




    col1    c
    Name: 2, dtype: object



#### Subsetting a column with a column name that doesn't exist.


```python
df['col2']
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/indexes/base.py in get_loc(self, key, method, tolerance)
       2645             try:
    -> 2646                 return self._engine.get_loc(key)
       2647             except KeyError:


    pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()


    pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()


    pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()


    pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()


    KeyError: 'col2'

    
    During handling of the above exception, another exception occurred:


    KeyError                                  Traceback (most recent call last)

    <ipython-input-15-fbc8de4a449f> in <module>
    ----> 1 df['col2']
    

    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/frame.py in __getitem__(self, key)
       2798             if self.columns.nlevels > 1:
       2799                 return self._getitem_multilevel(key)
    -> 2800             indexer = self.columns.get_loc(key)
       2801             if is_integer(indexer):
       2802                 indexer = [indexer]


    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/indexes/base.py in get_loc(self, key, method, tolerance)
       2646                 return self._engine.get_loc(key)
       2647             except KeyError:
    -> 2648                 return self._engine.get_loc(self._maybe_cast_indexer(key))
       2649         indexer = self.get_indexer([key], method=method, tolerance=tolerance)
       2650         if indexer.ndim > 1 or indexer.size > 1:


    pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()


    pandas/_libs/index.pyx in pandas._libs.index.IndexEngine.get_loc()


    pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()


    pandas/_libs/hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()


    KeyError: 'col2'



```python
df['col1']
```




    0    a
    1    b
    2    c
    Name: col1, dtype: object



#### Leaving off the closing parentheses when calling a method - a vanilla Python issue.


```python
df = pd.DataFrame(dict(col1=['a', 'b', 'c']), index=['first', 'second', 'third']
```


      File "<ipython-input-30-c6c359318121>", line 1
        df = pd.DataFrame(dict(col1=['a', 'b', 'c']), index=['first', 'second', 'third']
                                                                                        ^
    SyntaxError: unexpected EOF while parsing




```python
df = pd.DataFrame(dict(col1=['a', 'b', 'c']), index=['first', 'second', 'third'])
```

#### Not creating a dictionary correctly - a vanilla Python issue.


```python
df = pd.DataFrame(dict(col1='a', 'b', 'c'), index=['first', 'second', 'third'])
```


      File "<ipython-input-2-303fbad1a183>", line 1
        df = pd.DataFrame(dict(col1='a', 'b', 'c'), index=['first', 'second', 'third'])
                                        ^
    SyntaxError: positional argument follows keyword argument




```python
df = pd.DataFrame(dict(col1=['a', 'b', 'c']), index=['first', 'second', 'third'])
```

#### `shape` is an attribute, not a method. 


```python
df.shape()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-36-0e566b70f572> in <module>
    ----> 1 df.shape()
    

    TypeError: 'tuple' object is not callable



```python
df.shape
```




    (3, 1)



#### Calling a method that exists on a DataFrame, not a Series.


```python
df['col1'].info()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-38-741823e1b22c> in <module>
    ----> 1 df['col1'].info()
    

    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/generic.py in __getattr__(self, name)
       5272             if self._info_axis._can_hold_identifiers_and_holds_name(name):
       5273                 return self[name]
    -> 5274             return object.__getattribute__(self, name)
       5275 
       5276     def __setattr__(self, name: str, value) -> None:


    AttributeError: 'Series' object has no attribute 'info'



```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 3 entries, first to third
    Data columns (total 1 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   col1    3 non-null      object
    dtypes: object(1)
    memory usage: 48.0+ bytes


#### Reminder of how the DataFrame looks:


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>first</th>
      <td>a</td>
    </tr>
    <tr>
      <th>second</th>
      <td>b</td>
    </tr>
    <tr>
      <th>third</th>
      <td>c</td>
    </tr>
  </tbody>
</table>
</div>



#### Forgetting to pass `axis='columns'` when trying to drop a column. 


```python
df.drop('col1')
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-41-1827a6319199> in <module>
    ----> 1 df.drop('col1')
    

    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/frame.py in drop(self, labels, axis, index, columns, level, inplace, errors)
       3995             level=level,
       3996             inplace=inplace,
    -> 3997             errors=errors,
       3998         )
       3999 


    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/generic.py in drop(self, labels, axis, index, columns, level, inplace, errors)
       3934         for axis, labels in axes.items():
       3935             if labels is not None:
    -> 3936                 obj = obj._drop_axis(labels, axis, level=level, errors=errors)
       3937 
       3938         if inplace:


    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/generic.py in _drop_axis(self, labels, axis, level, errors)
       3968                 new_axis = axis.drop(labels, level=level, errors=errors)
       3969             else:
    -> 3970                 new_axis = axis.drop(labels, errors=errors)
       3971             result = self.reindex(**{axis_name: new_axis})
       3972 


    ~/miniconda3/envs/main/lib/python3.7/site-packages/pandas/core/indexes/base.py in drop(self, labels, errors)
       5016         if mask.any():
       5017             if errors != "ignore":
    -> 5018                 raise KeyError(f"{labels[mask]} not found in axis")
       5019             indexer = indexer[~mask]
       5020         return self.delete(indexer)


    KeyError: "['col1'] not found in axis"


#### Forgetting to reassign the resulting DataFrame to make a change permanent.


```python
df.drop('col1', axis='columns')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>first</th>
      <td>a</td>
    </tr>
    <tr>
      <th>second</th>
      <td>b</td>
    </tr>
    <tr>
      <th>third</th>
      <td>c</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = df.drop('col1', axis='columns')
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>first</th>
    </tr>
    <tr>
      <th>second</th>
    </tr>
    <tr>
      <th>third</th>
    </tr>
  </tbody>
</table>
</div>



#### Hope you found this helpful! 😀

If you have other common errors you think would be helpful for others, please leave them in the comments and ping me on Twitter @discdiver. 

See my [Memorable Python](https://memorablepython.com) and [Memorable Pandas](https://memorablepandas.com) books to learn Python 🐍 and pandas 🐼!
