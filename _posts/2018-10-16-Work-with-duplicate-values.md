
## Data Processing for beginners: How to deal with duplicates 

Let's create a dataframe that has multiple columns and rows has repeated values:

```python
import pandas as pd
df = pd.DataFrame({'a': [1,2,2,3,4,4,5],
                   'b': ['a','b','b','v','c','c','c']})
print(df)
```
       a  b
    0  1  a
    1  2  b
    2  2  b
    3  3  v
    4  4  c
    5  4  c
    6  5  c
We can see that both column a and b has rows that have duplicate values. This DF is very small so we can see the values but what if we have millions of rows. It is hard to know if any rows have duplicate values, so what we can do.

We can inspect each column using 'duplicated' method of pandas. duplicated method will return a series object where each row is marked with either False or True ( I will explain in a minute what are our options of marking a row duplicate)

Let's run duplicated on our dataframe's 'a' column with keep assigne to False


```python
df.duplicated('a',keep=False)
```

    0    False
    1     True
    2     True
    3    False
    4     True
    5     True
    6    False
    dtype: bool

Sometime we need to know how many duplicates we are dealing with.

The easiest way to deal with it to add a 'is_duplicate' column to the df and then sum the value over 'is_duplicated' column. This works because True = 1 and False = 0, so when we sum, it will print 4


```python
df['is_duplicate'] = df.duplicated('a', keep=False)
print(df)
```
       a  b  is_duplicate
    0  1  a         False
    1  2  b          True
    2  2  b          True
    3  3  v         False
    4  4  c          True
    5  4  c          True
    6  5  c         False
    
```python
df.is_duplicate.sum()
```
    4

Now 'keep' can take False, 'first' or 'last' argument. These argument tells pandas how to mark duplicates when they were encountered. 

'first' will mark the first encounter of duplicates with False but second onwards true(Mark duplicates as True except for the first occurrence.) . Like this:

```python
df['is_duplicate'] = df.duplicated('a', keep='first')
print(df)
```
       a  b  is_duplicate
    0  1  a         False
    1  2  b         False
    2  2  b          True
    3  3  v         False
    4  4  c         False
    5  4  c          True
    6  5  c         False
    

'last' will make all duplicates true except the last one (Mark duplicates as True except for the last occurrence.). Like this:

```python
#List all the duplicate items
df[df['is_duplicate']]
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
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>is_duplicate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>b</td>
      <td>True</td>
    </tr>
    <tr>
      <th>5</th>
      <td>4</td>
      <td>c</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>

```python
df['is_duplicate'] = df.duplicated('a', keep='last')
print(df)
```
       a  b  is_duplicate
    0  1  a         False
    1  2  b          True
    2  2  b         False
    3  3  v         False
    4  4  c          True
    5  4  c         False
    6  5  c         False
    
'False' will make all duplicates as true(Mark all duplicates as True.). We used False above so we can know how many elements are duplicates.

Now how to deal with duplicates. Most of the time we just want to drop the duplicate rows to clean the data

This can be done by using drop_duplicates method.

Here you need to be careful about choosing the 'keep' option. If you want to keep at least one row of the duplicate item, you should use keep='first':

```python
df.drop_duplicates('a', keep='first')

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
      <th>a</th>
      <th>b</th>
      <th>is_duplicate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>b</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>v</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>c</td>
      <td>True</td>
    </tr>
    <tr>
      <th>6</th>
      <td>5</td>
      <td>c</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



If you want to drop the duplicates all togheter, than use 'keep=false':


```python
df.drop_duplicates('a', keep=False)
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
      <th>a</th>
      <th>b</th>
      <th>is_duplicate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>v</td>
      <td>False</td>
    </tr>
    <tr>
      <th>6</th>
      <td>5</td>
      <td>c</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.drop_duplicates('a', keep='last')

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
      <th>a</th>
      <th>b</th>
      <th>is_duplicate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>b</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>v</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5</th>
      <td>4</td>
      <td>c</td>
      <td>True</td>
    </tr>
    <tr>
      <th>6</th>
      <td>5</td>
      <td>c</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



That's all for dealing with duplicates in pandas.
