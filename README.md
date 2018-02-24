

```python
import pandas as pd
import numpy as np
```


```python
purchase_data_df = pd.read_json("purchase_data.json")
purchase_data_df = purchase_data_df.rename(columns={'Item ID': 'Item_ID', 'Item Name': 'Item_Name'})
```


```python
#Player Count
number_of_players = purchase_data_df.SN.nunique()
```


```python
total_players = {"Total Players": [number_of_players]}
total_players_df = pd.DataFrame(total_players)
total_players_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Total)
```


```python
unique_items = purchase_data_df["Item_Name"].nunique()
```


```python
average_purchase_price = purchase_data_df.Price.mean()
```


```python
number_of_purchases = purchase_data_df.Age.count()
```


```python
total_revenue = purchase_data_df['Price'].sum()
```


```python
purchasing_analysis = {
    "Number of Unique Items": [unique_items], 
    "Average Purchase Price": [average_purchase_price], 
    "Total Number of Purchases": [number_of_purchases],
    "Total Revenue": [total_revenue]
        }
purchasing_analysis_df = pd.DataFrame(purchasing_analysis)


purchasing_analysis_df['Average Purchase Price'] = purchasing_analysis_df['Average Purchase Price'].map('${0:,.2f}'.format)
purchasing_analysis_df['Total Revenue'] = purchasing_analysis_df['Total Revenue'].map('${0:,.2f}'.format)
purchasing_analysis_df
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
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>179</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
```


```python
number_of_males = purchase_data_df.groupby(["Gender", "SN"]).SN.nunique().Male.sum()
number_of_females = purchase_data_df.groupby(["Gender", "SN"]).SN.nunique().Female.sum()
number_of_other_gender = purchase_data_df.groupby(["Gender", "SN"]).SN.nunique()['Other / Non-Disclosed'].sum()
```


```python
percent_males = number_of_males / number_of_players 
percent_females = number_of_females / number_of_players
percent_other = number_of_other_gender / number_of_players
```


```python
gender_demographics = {
    "Gender": ["Male", "Female", "Other"],
    "Percentage of Players": [percent_males, percent_females, percent_other],
    "Total Count": [number_of_males, number_of_females, number_of_other_gender],
}
gender_demographics_df = pd.DataFrame(gender_demographics).set_index("Gender")

gender_demographics_df['Percentage of Players'] = gender_demographics_df['Percentage of Players'].map('{:.2%}'.format)
gender_demographics_df
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
```


```python
gender_df = purchase_data_df.groupby('Gender')
```


```python
male_purchase_count = gender_df.count().Age.Male
female_purchase_count = gender_df.count().Age.Female
other_purchase_count = gender_df.count().Age["Other / Non-Disclosed"]
```


```python
f_avg_purchase_price = gender_df.mean().Price.Female
m_avg_purchase_price = gender_df.mean().Price.Male
o_avg_purchase_price = gender_df.mean().Price["Other / Non-Disclosed"]
```


```python
f_total_purchase_value = gender_df.sum().Price.Female
m_total_purchase_value = gender_df.sum().Price.Male
o_total_purchase_value = gender_df.sum().Price["Other / Non-Disclosed"]
```


```python
purchasing_analysis = {
    "Gender": ["Female", "Male", "Other / Non-Disclosed"],
    "Purchase Count": [female_purchase_count, male_purchase_count, other_purchase_count],
    "Average Purchase Price": [f_avg_purchase_price, m_avg_purchase_price, o_avg_purchase_price],
    "Total Purchase Value": [f_total_purchase_value, m_total_purchase_value, o_total_purchase_value], 
    #"Normalized Totals": [(purchase_data_df[purchase_data_df.Gender == "Male"].groupby("SN").Age.count().mean() - f_average_purchase_price) / purchase_data_df.Price.std()]
    # normalized totals (  -  ) / purchase_data_df.Price.std() )
}
purchasing_analysis_df = pd.DataFrame(purchasing_analysis).set_index("Gender")

purchasing_analysis_df['Average Purchase Price'] = purchasing_analysis_df['Average Purchase Price'].map('${0:,.2f}'.format)
purchasing_analysis_df['Total Purchase Value'] = purchasing_analysis_df['Total Purchase Value'].map('${0:,.2f}'.format)
purchasing_analysis_df
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
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
      <td>$2.82</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>633</td>
      <td>$1,867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics
```


```python
np.arange(10,100,4)
age_bins = [0, 10, 14, 18, 22, 26, 30, 34, 38, 42, 46, 100]
```


```python
purchase_data_df['Age_Group'] = pd.cut(purchase_data_df['Age'], age_bins)
```


```python
age_groups = purchase_data_df.groupby('Age_Group')
```


```python
age_bins_list = age_groups.count().Age.index.tolist()
by_age_purchase_count = age_groups.count().Age.tolist()
by_age_average_purchase_price = age_groups.mean().Price.tolist()
by_age_total_purchase_value = age_groups.sum().Price.tolist()
```


```python
age_demographics = [('Age Groups', age_bins_list),
                    ('Purchase Count', by_age_purchase_count),
                    ('Average Purchase Price', by_age_average_purchase_price),
                    ('Total Purchase Value', by_age_total_purchase_value),
                   # ('Normalized Totals', np.arange(0,11, 1))
                          ]
age_demographics_df = pd.DataFrame.from_items(age_demographics).set_index('Age Groups')

age_demographics_df['Average Purchase Price'] = age_demographics_df['Average Purchase Price'].map('${0:,.2f}'.format)
age_demographics_df['Total Purchase Value'] = age_demographics_df['Total Purchase Value'].map('${0:,.2f}'.format)
age_demographics_df
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Groups</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>(0, 10]</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
    </tr>
    <tr>
      <th>(10, 14]</th>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
    </tr>
    <tr>
      <th>(14, 18]</th>
      <td>111</td>
      <td>$2.88</td>
      <td>$319.32</td>
    </tr>
    <tr>
      <th>(18, 22]</th>
      <td>231</td>
      <td>$2.93</td>
      <td>$676.20</td>
    </tr>
    <tr>
      <th>(22, 26]</th>
      <td>207</td>
      <td>$2.94</td>
      <td>$608.02</td>
    </tr>
    <tr>
      <th>(26, 30]</th>
      <td>63</td>
      <td>$2.98</td>
      <td>$187.99</td>
    </tr>
    <tr>
      <th>(30, 34]</th>
      <td>46</td>
      <td>$3.07</td>
      <td>$141.24</td>
    </tr>
    <tr>
      <th>(34, 38]</th>
      <td>37</td>
      <td>$2.81</td>
      <td>$104.06</td>
    </tr>
    <tr>
      <th>(38, 42]</th>
      <td>20</td>
      <td>$3.13</td>
      <td>$62.56</td>
    </tr>
    <tr>
      <th>(42, 46]</th>
      <td>2</td>
      <td>$3.26</td>
      <td>$6.53</td>
    </tr>
    <tr>
      <th>(46, 100]</th>
      <td>0</td>
      <td>$nan</td>
      <td>$0.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders
```


```python
sn_groups = purchase_data_df.groupby('SN')
```


```python
top_5_total_purchase_value = sn_groups.sum().Price.nlargest(n=5)
top_5_total_purchase_value
```




    SN
    Undirrala66    17.06
    Saedue76       13.56
    Mindimnya67    12.74
    Haellysu29     12.73
    Eoda93         11.58
    Name: Price, dtype: float64




```python
top_5_total_purchase_value_list = top_5_total_purchase_value.index.tolist()
top_5_total_purchase_value_list
```




    ['Undirrala66', 'Saedue76', 'Mindimnya67', 'Haellysu29', 'Eoda93']




```python
top_5_total_purchase_value_df = purchase_data_df[purchase_data_df.SN.isin(top_5_total_purchase_value_list)].groupby("SN")
```


```python
top_5_purchase_count = top_5_total_purchase_value_df.Price.count()
top_5_avg_price = top_5_total_purchase_value_df.Price.mean()
top_5_purchase_value = top_5_total_purchase_value_df.Price.sum()
top_5_purchase_value
```




    SN
    Eoda93         11.58
    Haellysu29     12.73
    Mindimnya67    12.74
    Saedue76       13.56
    Undirrala66    17.06
    Name: Price, dtype: float64




```python
# Top 5 Spenders Table

top_spenders_table = [('SN', top_5_total_purchase_value_list),
                    ('Purchase Count', top_5_purchase_count),
                    ('Average Purchase Price', top_5_avg_price),
                    ('Total Purchase Value', top_5_purchase_value),]
top_spenders_df = pd.DataFrame.from_items(top_spenders_table).sort_values("Total Purchase Value", ascending=False).set_index('SN')
top_spenders_df

top_spenders_df['Average Purchase Price'] = top_spenders_df['Average Purchase Price'].map('${0:,.2f}'.format)
top_spenders_df['Total Purchase Value'] = top_spenders_df['Total Purchase Value'].map('${0:,.2f}'.format)
top_spenders_df
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Eoda93</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Undirrala66</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items
```


```python
item_name_groups = purchase_data_df.groupby("Item_Name")
item_name_list = item_name_groups.Item_Name.count().nlargest(n=5).index.tolist()
item_name_list
```




    ['Final Critic',
     'Arcane Gem',
     'Betrayal, Whisper of Grieving Widows',
     'Stormcaller',
     'Retribution Axe']




```python
# Data frame with only the most popular items
popular_items_df = purchase_data_df[purchase_data_df.Item_Name.isin(item_name_list)]
```


```python
#purchase count
pop_items_purchase_count_list = popular_items_df.Item_Name.value_counts()
pop_items_purchase_count_list.tolist()
```




    [14, 11, 11, 10, 9]




```python
#Item IDs
item_id_list = []

for item_name in item_name_list:
    item_something = purchase_data_df['Item_ID'].loc[purchase_data_df['Item_Name'] == item_name]
    temp_id_list = item_something.tolist()
    item_id_list.append(temp_id_list[0])
item_id_list

```




    [92, 84, 39, 30, 34]




```python
#Item Price
item_price_list = []
for item_name in item_name_list:
    item_something = purchase_data_df['Price'].loc[purchase_data_df['Item_Name'] == item_name]
    temp_id_list = item_something.tolist()
    item_price_list.append(temp_id_list[0])
item_price_list


```




    [1.3599999999999999, 2.23, 2.35, 4.15, 4.14]




```python
# total purchase value
item_purchase_value_list = []
for item_name in item_name_list:
    item_purchase_value_list.append(popular_items_df.groupby("Item_Name").Price.sum()[item_name])
item_purchase_value_list
```




    [38.599999999999994, 24.53, 25.850000000000005, 34.65, 37.26]




```python
# Most Popular Items Table

top_popular_items_table = [('Item ID', item_id_list),
                    ('Item Name', item_name_list),
                    ('Purchase Count', pop_items_purchase_count_list.tolist()),
                    ('Item Price', item_price_list),
                    ('Total Purchase Value', item_purchase_value_list)]
top_5_items_df = pd.DataFrame.from_items(top_popular_items_table).sort_values("Purchase Count", ascending=False).set_index("Item ID")

top_5_items_df['Item Price'] = top_5_items_df['Item Price'].map('${0:,.2f}'.format)
top_5_items_df['Total Purchase Value'] = top_5_items_df['Total Purchase Value'].map('${0:,.2f}'.format)
top_5_items_df
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>92</th>
      <td>Final Critic</td>
      <td>14</td>
      <td>$1.36</td>
      <td>$38.60</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Stormcaller</td>
      <td>10</td>
      <td>$4.15</td>
      <td>$34.65</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
```


```python
# 5 most profitable items
most_profitable_items_list = purchase_data_df.groupby("Item_Name").sum().Price.nlargest(n=5).index.tolist()
most_profitable_items_list

```




    ['Final Critic',
     'Retribution Axe',
     'Stormcaller',
     'Spectral Diamond Doomblade',
     'Orenmir']




```python
# Item ID
prof_item_id_list = []

for item_name in most_profitable_items_list:
    item_something = purchase_data_df['Item_ID'].loc[purchase_data_df['Item_Name'] == item_name]
    temp_id_list = item_something.tolist()
    prof_item_id_list.append(temp_id_list[0])
prof_item_id_list
```




    [92, 34, 30, 115, 32]




```python
# Create Profitable Items Data Frame
profitable_items_df = purchase_data_df[purchase_data_df.Item_Name.isin(most_profitable_items_list)]

```


```python
#purchase count
prof_items_purchase_count_list = []
for item_name in most_profitable_items_list:
    prof_items_purchase_count_list.append(profitable_items_df.Item_Name.value_counts()[item_name])
prof_items_purchase_count_list
```




    [14, 9, 10, 7, 6]




```python
#item price
prof_item_price_list = []

for item_name in most_profitable_items_list:
    item_something = profitable_items_df['Price'].loc[profitable_items_df['Item_Name'] == item_name]
    temp_price_list = item_something.tolist()
    prof_item_price_list.append(temp_price_list[0])
prof_item_price_list
    
```




    [1.3599999999999999, 4.14, 4.15, 4.25, 4.95]




```python
#total purchase value

prof_item_purchase_value_list = []
for item_name in most_profitable_items_list:
    prof_item_purchase_value_list.append(profitable_items_df.groupby("Item_Name").Price.sum()[item_name])
prof_item_purchase_value_list



```




    [38.599999999999994, 37.26, 34.65, 29.75, 29.7]




```python
# Most Profitable Items Table and Data Frame
top_popular_items_table = [('Item ID', prof_item_id_list),
                    ('Item Name', most_profitable_items_list),
                    ('Purchase Count', pop_items_purchase_count_list.tolist()),
                    ('Item Price', prof_item_price_list),
                    ('Total Purchase Value', prof_item_purchase_value_list)]
top_5_prof_items_df = pd.DataFrame.from_items(top_popular_items_table).sort_values("Total Purchase Value", ascending=False).set_index("Item ID")


#top_5_prof_items_df["Item Price"] = 
top_5_prof_items_df['Item Price'] = top_5_prof_items_df['Item Price'].map('${0:,.2f}'.format)
top_5_prof_items_df['Total Purchase Value'] = top_5_prof_items_df['Total Purchase Value'].map('${0:,.2f}'.format)
top_5_prof_items_df
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>92</th>
      <td>Final Critic</td>
      <td>14</td>
      <td>$1.36</td>
      <td>$38.60</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Retribution Axe</td>
      <td>11</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Stormcaller</td>
      <td>11</td>
      <td>$4.15</td>
      <td>$34.65</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Spectral Diamond Doomblade</td>
      <td>10</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Orenmir</td>
      <td>9</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Observable Trends
# Males make up the vast majority of purchasers at over 80%.
# The biggest spending is between the ages of 18 and 26. 
# Even the most popular item only has a purchase count of 14 out of 573 players which means that people are interested in a variety of items.  

```
