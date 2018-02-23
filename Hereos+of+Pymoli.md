

```python
import pandas as pd
import numpy as np
```


```python
purchase_data_df = pd.read_json("purchase_data.json")
purchase_data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
number_of_players = purchase_data_df.Age.count()
```


```python
total_players = {"Total Players": [number_of_players]}
total_players_df = pd.DataFrame(data=total_players) 
total_players_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>780</td>
    </tr>
  </tbody>
</table>
</div>




```python
unique_items = purchase_data_df["Item Name"].nunique()
```


```python
average_purchase_price = purchase_data_df.Price.mean()
```


```python
# number_of_purchases = purchase_data_df["Price"].count()
```


```python
total_revenue = purchase_data_df['Price'].sum()
```


```python
number_of_males = purchase_data_df.Gender.value_counts().Male
number_of_females = purchase_data_df.Gender.value_counts().Female
number_of_other_gender = purchase_data_df.Gender.value_counts()['Other / Non-Disclosed']
```


```python
percent_males = number_of_males / number_of_players 
percent_females = number_of_females / number_of_players
percent_other = number_of_other_gender / number_of_players
```


```python
gender_df = purchase_data_df.groupby('Gender')
gender_df.mean()
gender_df.sum()

# gender_df.drop('Age', columns=1)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Item ID</th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>3068</td>
      <td>11983</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>14360</td>
      <td>57965</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>301</td>
      <td>1261</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
bins = np.arange(0,100,4)
bins
```




    array([ 0,  4,  8, 12, 16, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 60, 64,
           68, 72, 76, 80, 84, 88, 92, 96])


