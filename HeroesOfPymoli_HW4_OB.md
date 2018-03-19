

```python
# Dependencies
import csv
import pandas as pd
import numpy as np


# You must include a written description of three observable trends based on the data.
# Few observations can be made out of the dataset given:

# 1/ Fist and foremost, this game is address for a young public given the bin breakdown by age. Indeed most of the purchases are targeting a teen public (15/19 years old)
# Within the teen large bracket (10/19 years old), it is worth noting that the (15/19 years old) generates almost 3 times more revenue than the (10/14 years old) given the fact that the former bracket is likely to have more money.

# 2/ The frequency distribution analysis shows that less and less players are likely to buy items as they grow older.

# 3/ Males make more than 80% of the players and as well 80% of the revenue.

# 4/ The most profitable items are for the given set roughly twice more expensive than the average ones.
```


```python
# The path to the json file
file = "generated_data/purchase_data.json"
file_df = pd.read_json(file)
file_df.head()
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
file_df.dtypes
```




    Age            int64
    Gender        object
    Item ID        int64
    Item Name     object
    Price        float64
    SN            object
    dtype: object




```python
# Check if missing data
file_df.count()
```




    Age          780
    Gender       780
    Item ID      780
    Item Name    780
    Price        780
    SN           780
    dtype: int64




```python
# Display an overview of the database
file_df.describe()
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
      <th>Age</th>
      <th>Item ID</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>780.000000</td>
      <td>780.000000</td>
      <td>780.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>22.729487</td>
      <td>91.293590</td>
      <td>2.931192</td>
    </tr>
    <tr>
      <th>std</th>
      <td>6.930604</td>
      <td>52.707537</td>
      <td>1.115780</td>
    </tr>
    <tr>
      <th>min</th>
      <td>7.000000</td>
      <td>0.000000</td>
      <td>1.030000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>19.000000</td>
      <td>44.000000</td>
      <td>1.960000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>22.000000</td>
      <td>91.000000</td>
      <td>2.880000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>25.000000</td>
      <td>135.000000</td>
      <td>3.910000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>45.000000</td>
      <td>183.000000</td>
      <td>4.950000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Player Count
players_count = file_df.loc[:,["Gender","SN","Age"]]
players_count = players_count.drop_duplicates()
players_count_df = players_count.count()[0]

pd.DataFrame({"Total Players":[players_count_df]})

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
# Display the unique items from the list of items
items_count=len(file_df["Item ID"].unique())
items_count
```




    183




```python
# Display an the average purchase price from the list of items
avg_purchase_price = file_df["Price"].mean()
avg_purchase_price
```




    2.931192307692303




```python
# Display the total number of purchase 
total_purchase_count = file_df["Price"].count()
total_purchase_count
```




    780




```python
# Display the total revenue
total_purchase_value = file_df["Price"].sum()
total_purchase_value
```




    2286.33




```python
# Purchasing Analysis (total)
purchases_summary_table = pd.DataFrame({"Number of Unique Items":items_count,
                                 "Average Price": [avg_purchase_price],
                                 "Number of Purchases":[total_purchase_count],
                                 "Total revenue":[total_purchase_value]}, 
                                       columns = ["Number of Unique Items",
                                                  "Average Price",
                                                  "Number of Purchases",
                                                 "Total revenue"])

purchases_summary_table.style.format({"Average Price":"${:.2f}","Total revenue": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_9d77ac4a_2b27_11e8_8da0_14abc5858625" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Number of Unique Items</th> 
        <th class="col_heading level0 col1" >Average Price</th> 
        <th class="col_heading level0 col2" >Number of Purchases</th> 
        <th class="col_heading level0 col3" >Total revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9d77ac4a_2b27_11e8_8da0_14abc5858625level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_9d77ac4a_2b27_11e8_8da0_14abc5858625row0_col0" class="data row0 col0" >183</td> 
        <td id="T_9d77ac4a_2b27_11e8_8da0_14abc5858625row0_col1" class="data row0 col1" >$2.93</td> 
        <td id="T_9d77ac4a_2b27_11e8_8da0_14abc5858625row0_col2" class="data row0 col2" >780</td> 
        <td id="T_9d77ac4a_2b27_11e8_8da0_14abc5858625row0_col3" class="data row0 col3" >$2286.33</td> 
    </tr></tbody> 
</table> 




```python
# Gender demographics
gender_breakdown = players_count["Gender"].value_counts()
gender_breakdown_perc = (gender_breakdown/players_count_df)*100


gender_summary_table = pd.DataFrame({"Total Count": gender_breakdown,"Percentage of Players":gender_breakdown_perc}, columns = ["Percentage of Players","Total Count"])
gender_summary_table


gender_summary_table.style.format({"Percentage of Players":"{:.2f}%","Total Count":"{:.0f}"})

```




<style  type="text/css" >
</style>  
<table id="T_9d9e787e_2b27_11e8_8b22_14abc5858625" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Count</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9d9e787e_2b27_11e8_8b22_14abc5858625level0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_9d9e787e_2b27_11e8_8b22_14abc5858625row0_col0" class="data row0 col0" >81.15%</td> 
        <td id="T_9d9e787e_2b27_11e8_8b22_14abc5858625row0_col1" class="data row0 col1" >465</td> 
    </tr>    <tr> 
        <th id="T_9d9e787e_2b27_11e8_8b22_14abc5858625level0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_9d9e787e_2b27_11e8_8b22_14abc5858625row1_col0" class="data row1 col0" >17.45%</td> 
        <td id="T_9d9e787e_2b27_11e8_8b22_14abc5858625row1_col1" class="data row1 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_9d9e787e_2b27_11e8_8b22_14abc5858625level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_9d9e787e_2b27_11e8_8b22_14abc5858625row2_col0" class="data row2 col0" >1.40%</td> 
        <td id="T_9d9e787e_2b27_11e8_8b22_14abc5858625row2_col1" class="data row2 col1" >8</td> 
    </tr></tbody> 
</table> 




```python
# Purchasing Analysis (gender)
gender_purchase_count = file_df.groupby(["Gender"]).count()["Price"].rename("Purchase Count")
gender_purchase_count 
gender_purchase_value = file_df.groupby(["Gender"]).sum()["Price"].rename("Total Purchase Value")
gender_purchase_value 
gender_avg_purchase = file_df.groupby(["Gender"]).mean()["Price"].rename("Average Purchase Price")
gender_avg_purchase
normalized_total = gender_purchase_value / gender_breakdown
normalized_total


gender_breakdown_summary_table = pd.DataFrame({"Purchase Count": gender_purchase_count,
                                               "Total Purchase Value":gender_purchase_value,
                                               "Average Purchase Price":gender_avg_purchase,
                                               "Normalized Totals":normalized_total
                                              }, columns = ["Purchase Count",
                                                            "Average Purchase Price",
                                                            "Total Purchase Value",
                                                           "Normalized Totals"])

gender_breakdown_summary_table


gender_breakdown_summary_table.style.format({"Total Purchase Value":"${:.2f}","Average Purchase Price":"${:.2f}", "Normalized Totals":"${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row0_col0" class="data row0 col0" >136</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row0_col1" class="data row0 col1" >$2.82</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row0_col2" class="data row0 col2" >$382.91</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row0_col3" class="data row0 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row1_col0" class="data row1 col0" >633</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row1_col1" class="data row1 col1" >$2.95</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row1_col2" class="data row1 col2" >$1867.68</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row1_col3" class="data row1 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row2_col0" class="data row2 col0" >11</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row2_col1" class="data row2 col1" >$3.25</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row2_col2" class="data row2 col2" >$35.74</td> 
        <td id="T_9dcc2a50_2b27_11e8_8a56_14abc5858625row2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 




```python
# Create the bins in which Data will be held
age_bins_values = [0,9,14,19,24,29,34,39,44,100]
# Create the names for the different bins
age_bins_names = ["<10","10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45+"]
# Cut age and place the scores into bins
file_df["Age Bins"] = pd.cut(file_df["Age"],age_bins_values, labels=age_bins_names)

file_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age Bins</th>
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
      <td>35-39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>30-34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>20-24</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a GroupBy object based upon "Age Bins"
player_bin_group = file_df.groupby("Age Bins")

# # Find how many rows fall into each bin
# print(player_bin_group["Age Bins"].count())

# Calculate the number and % by age bins
# player_count_bin = players_count["Age Bins"].value_counts()
# player_count_bin

player_bin_group = file_df.groupby("Age Bins")

# player_perc_bin
player_perc_bin = ((player_count_bin/players_count_df)*100).round(2)

# Format the dataframe
player_bin_summary_table = pd.DataFrame({"Age":["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45-49"],
                                         "Total Count": player_count_bin,
                                        "Percentage of Players":player_perc_bin}, 
                                        columns = ["Age", "Percentage of Players","Total Count"])

# Order the results by Age bin
player_bin_summary_table

# Print table
player_bin_summary_table.set_index("Age")


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
      <th>Age</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>45.20</td>
      <td>259</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>15.18</td>
      <td>87</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>8.20</td>
      <td>47</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>4.71</td>
      <td>27</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>4.01</td>
      <td>23</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>3.32</td>
      <td>19</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>1.75</td>
      <td>10</td>
    </tr>
    <tr>
      <th>45-49</th>
      <td>0.17</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Bin Purchase by Age
file_df["Age Bins"] = pd.cut(file_df["Age"],age_bins_values, labels=age_bins_names)

# Calculate Purchase Count, Total Sum and Avg Price
age_total_purchase = file_df.groupby(["Age Bins"]).sum()["Price"].rename("Total Purchase Value")
age_avg_purchase = file_df.groupby(["Age Bins"]).mean()["Price"].rename("Average Purchase Price")
age_counts_purchase = file_df.groupby(["Age Bins"]).count()["Price"].rename("Purchase Count")

# Calculate Normalized totals
normalized_total = age_total_purchase / player_bin_summary_table["Total Count"]

# Format the dataframe
player_data_summary_table = pd.DataFrame({"Age": ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45-49"],
                                          "Purchase Count": age_counts_purchase,
                                          "Average Purchase Price":age_avg_purchase,
                                          "Total Purchase Value":age_total_purchase,
                                          "Normalized Totals":normalized_total
                                         }, columns = ["Age","Purchase Count",
                                                       "Average Purchase Price",
                                                       "Total Purchase Value",
                                                      "Normalized Totals"])
# Index Age
player_age_summary_final = player_data_summary_table.set_index("Age")

# Format the results
player_age_summary_final.style.format({"Average Purchase Price":"${:.2f}","Total Purchase Value":"${:.2f}", "Normalized Totals":"${:.2f}"})

```




<style  type="text/css" >
</style>  
<table id="T_9e4aead4_2b27_11e8_863c_14abc5858625" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row0_col0" class="data row0 col0" >35</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row0_col1" class="data row0 col1" >$2.77</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row0_col2" class="data row0 col2" >$96.95</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row0_col3" class="data row0 col3" >$4.22</td> 
    </tr>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row1_col0" class="data row1 col0" >133</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row1_col1" class="data row1 col1" >$2.91</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row1_col2" class="data row1 col2" >$386.42</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row1_col3" class="data row1 col3" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row2_col0" class="data row2 col0" >336</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row2_col1" class="data row2 col1" >$2.91</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row2_col2" class="data row2 col2" >$978.77</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row2_col3" class="data row2 col3" >$3.78</td> 
    </tr>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row3_col0" class="data row3 col0" >125</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row3_col1" class="data row3 col1" >$2.96</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row3_col2" class="data row3 col2" >$370.33</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row3_col3" class="data row3 col3" >$4.26</td> 
    </tr>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row4_col0" class="data row4 col0" >64</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row4_col1" class="data row4 col1" >$3.08</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row4_col2" class="data row4 col2" >$197.25</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row4_col3" class="data row4 col3" >$4.20</td> 
    </tr>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row5_col0" class="data row5 col0" >42</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row5_col1" class="data row5 col1" >$2.84</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row5_col2" class="data row5 col2" >$119.40</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row5_col3" class="data row5 col3" >$4.42</td> 
    </tr>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row6_col0" class="data row6 col0" >16</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row6_col1" class="data row6 col1" >$3.19</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row6_col2" class="data row6 col2" >$51.03</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row6_col3" class="data row6 col3" >$5.10</td> 
    </tr>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row7" class="row_heading level0 row7" >40-44</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row7_col0" class="data row7 col0" >1</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row7_col1" class="data row7 col1" >$2.72</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row7_col2" class="data row7 col2" >$2.72</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row7_col3" class="data row7 col3" >$2.72</td> 
    </tr>    <tr> 
        <th id="T_9e4aead4_2b27_11e8_863c_14abc5858625level0_row8" class="row_heading level0 row8" >45-49</th> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row8_col0" class="data row8 col0" >28</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row8_col1" class="data row8 col1" >$2.98</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row8_col2" class="data row8 col2" >$83.46</td> 
        <td id="T_9e4aead4_2b27_11e8_863c_14abc5858625row8_col3" class="data row8 col3" >$4.39</td> 
    </tr></tbody> 
</table> 




```python
# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):

sn_purchase_value=file_df.groupby(["SN"])['Price'].sum().rename("Total Purchase Value")
sn_purchase_count=file_df.groupby(["SN"])['Price'].count().rename("Purchase Count")
sn_purchase_avg=file_df.groupby(["SN"])['Price'].mean().rename("Average Purchase Price")

# Convert to DataFrame
spenders_data = pd.DataFrame({"Total Purchase Value":sn_purchase_value,"Purchase Count":sn_purchase_count,"Average Purchase Price":sn_purchase_avg},columns = ["Purchase Count","Average Purchase Price","Total Purchase Value"])

spenders_data
                             
spenders_data_summary = spenders_data.sort_values("Total Purchase Value", ascending=False).head()
                             
# Format the results
spenders_data_summary.style.format({"Total Purchase Value":"${:.2f}","Average Purchase Price":"${:.2f}"})

```




<style  type="text/css" >
</style>  
<table id="T_9e7798ba_2b27_11e8_b31b_14abc5858625" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9e7798ba_2b27_11e8_b31b_14abc5858625level0_row0" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row0_col0" class="data row0 col0" >5</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row0_col1" class="data row0 col1" >$3.41</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row0_col2" class="data row0 col2" >$17.06</td> 
    </tr>    <tr> 
        <th id="T_9e7798ba_2b27_11e8_b31b_14abc5858625level0_row1" class="row_heading level0 row1" >Saedue76</th> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row1_col0" class="data row1 col0" >4</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row1_col1" class="data row1 col1" >$3.39</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row1_col2" class="data row1 col2" >$13.56</td> 
    </tr>    <tr> 
        <th id="T_9e7798ba_2b27_11e8_b31b_14abc5858625level0_row2" class="row_heading level0 row2" >Mindimnya67</th> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row2_col0" class="data row2 col0" >4</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row2_col1" class="data row2 col1" >$3.18</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row2_col2" class="data row2 col2" >$12.74</td> 
    </tr>    <tr> 
        <th id="T_9e7798ba_2b27_11e8_b31b_14abc5858625level0_row3" class="row_heading level0 row3" >Haellysu29</th> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row3_col0" class="data row3 col0" >3</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row3_col1" class="data row3 col1" >$4.24</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row3_col2" class="data row3 col2" >$12.73</td> 
    </tr>    <tr> 
        <th id="T_9e7798ba_2b27_11e8_b31b_14abc5858625level0_row4" class="row_heading level0 row4" >Eoda93</th> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row4_col0" class="data row4 col0" >3</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row4_col1" class="data row4 col1" >$3.86</td> 
        <td id="T_9e7798ba_2b27_11e8_b31b_14abc5858625row4_col2" class="data row4 col2" >$11.58</td> 
    </tr></tbody> 
</table> 




```python
#Identify the 5 most popular items by purchase count, then list (in a table):
item_data = file_df.loc[:,["Item ID", "Item Name", "Price"]]

pop_item_purchase = item_data.groupby(["Item ID", "Item Name"]).sum()["Price"].rename("Total Purchase Value")
pop_item_count = item_data.groupby(["Item ID", "Item Name"]).count()["Price"].rename("Purchase Count")
pop_item_avg = item_data.groupby(["Item ID", "Item Name"]).mean()["Price"].rename("Item Price")

# Convert to DataFrame
pop_item_data = pd.DataFrame({"Total Purchase Value":pop_item_purchase,
                              "Purchase Count":pop_item_count,
                              "Item Price":pop_item_avg},
                             columns = ["Purchase Count","Item Price","Total Purchase Value"])

pop_item_data.head()

pop_item_data_summary = pop_item_data.sort_values("Purchase Count", ascending=False).head()

# Format the results
pop_item_data_summary.style.format({"Total Purchase Value":"${:.2f}","Item Price":"${:.2f}"})

```




<style  type="text/css" >
</style>  
<table id="T_9ea6e722_2b27_11e8_8b55_14abc5858625" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level0_row0" class="row_heading level0 row0" >39</th> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level1_row0" class="row_heading level1 row0" >Betrayal, Whisper of Grieving Widows</th> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row0_col0" class="data row0 col0" >11</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row0_col1" class="data row0 col1" >$2.35</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row0_col2" class="data row0 col2" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level0_row1" class="row_heading level0 row1" >84</th> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level1_row1" class="row_heading level1 row1" >Arcane Gem</th> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row1_col0" class="data row1 col0" >11</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row1_col1" class="data row1 col1" >$2.23</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row1_col2" class="data row1 col2" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level0_row2" class="row_heading level0 row2" >31</th> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level1_row2" class="row_heading level1 row2" >Trickster</th> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row2_col0" class="data row2 col0" >9</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row2_col1" class="data row2 col1" >$2.07</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row2_col2" class="data row2 col2" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level0_row3" class="row_heading level0 row3" >175</th> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level1_row3" class="row_heading level1 row3" >Woeful Adamantite Claymore</th> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row3_col0" class="data row3 col0" >9</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row3_col1" class="data row3 col1" >$1.24</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row3_col2" class="data row3 col2" >$11.16</td> 
    </tr>    <tr> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level0_row4" class="row_heading level0 row4" >13</th> 
        <th id="T_9ea6e722_2b27_11e8_8b55_14abc5858625level1_row4" class="row_heading level1 row4" >Serenity</th> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row4_col0" class="data row4 col0" >9</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row4_col1" class="data row4 col1" >$1.49</td> 
        <td id="T_9ea6e722_2b27_11e8_8b55_14abc5858625row4_col2" class="data row4 col2" >$13.41</td> 
    </tr></tbody> 
</table> 




```python
#Identify the 5 most profitable items by total purchase value, then list (in a table):
pop_item_data_pv_summary = pop_item_data.sort_values("Total Purchase Value", ascending=False).head()

# Format the results
pop_item_data_pv_summary.style.format({"Total Purchase Value":"${:.2f}","Item Price":"${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_9ed110b8_2b27_11e8_9352_14abc5858625" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level0_row0" class="row_heading level0 row0" >34</th> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level1_row0" class="row_heading level1 row0" >Retribution Axe</th> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row0_col0" class="data row0 col0" >9</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row0_col1" class="data row0 col1" >$4.14</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row0_col2" class="data row0 col2" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level0_row1" class="row_heading level0 row1" >115</th> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level1_row1" class="row_heading level1 row1" >Spectral Diamond Doomblade</th> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row1_col0" class="data row1 col0" >7</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row1_col1" class="data row1 col1" >$4.25</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row1_col2" class="data row1 col2" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level0_row2" class="row_heading level0 row2" >32</th> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level1_row2" class="row_heading level1 row2" >Orenmir</th> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row2_col0" class="data row2 col0" >6</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row2_col1" class="data row2 col1" >$4.95</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row2_col2" class="data row2 col2" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level0_row3" class="row_heading level0 row3" >103</th> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level1_row3" class="row_heading level1 row3" >Singed Scalpel</th> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row3_col0" class="data row3 col0" >6</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row3_col1" class="data row3 col1" >$4.87</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row3_col2" class="data row3 col2" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level0_row4" class="row_heading level0 row4" >107</th> 
        <th id="T_9ed110b8_2b27_11e8_9352_14abc5858625level1_row4" class="row_heading level1 row4" >Splitter, Foe Of Subtlety</th> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row4_col0" class="data row4 col0" >8</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row4_col1" class="data row4 col1" >$3.61</td> 
        <td id="T_9ed110b8_2b27_11e8_9352_14abc5858625row4_col2" class="data row4 col2" >$28.88</td> 
    </tr></tbody> 
</table> 


