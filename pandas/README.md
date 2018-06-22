

```python
import pandas as pd
```


```python
json_path = "purchase_data.json"
players_df = pd.read_json(json_path)
players_df.head()
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
#total number of players
Number_of_Players = players_df["SN"].nunique()
num_players = pd.DataFrame({"Total Players":[Number_of_Players]})
num_players
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
#number of uniques items, average price, number of purchases, total rev
count = players_df["Item ID"].nunique()

avg = players_df["Price"].mean()

purchase = players_df["Item ID"].count()

revenue = players_df["Price"].sum()

purch_analysis = pd.DataFrame({"Number of Unique Items":[count],
                               "Average Price":[avg],
                               "Number of Purchases":[purchase],
                               "Total Revenue":[revenue]})
purch_analysis["Average Price"]= purch_analysis["Average Price"].map("${:,.2f}".format)
purch_analysis["Total Revenue"] = purch_analysis["Total Revenue"].map("${:,.2f}".format)
purch_analysis
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#group by gender, columns: Percentages of Players, Total Count
gender_total = players_df["Gender"].count()

#male_total = (players_df.loc[(players_df["Gender"]=="Male"),"Gender"]).mean()

grouped_gender = players_df.groupby(['Gender'])

gender_data = pd.DataFrame({"Percentage of Players":
    ((grouped_gender['Gender'].count()/gender_total)*100).round(2),
                            "Total Count":grouped_gender["Gender"].count()})

gender_data["Percentage of Players"]= gender_data["Percentage of Players"].map("{0:,.2f}%".format)
gender_data
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
      <th>Female</th>
      <td>17.44%</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.41%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#group by gender, columns: Purchase Count, Avg Purchase Price,Total Purchase Value, Normalized Totals 

count = players_df["Item ID"].nunique()
avg = players_df["Price"].mean()
purchase = players_df["Item ID"].count()
revenue = players_df["Price"].sum()
normalized_total = revenue/gender_total

grouped_gender = players_df.groupby(['Gender'])

gender_data = pd.DataFrame({"Purchase Count":[purchase],
                            "Average Purchase Price":[avg],
                            "Total Purchase Value":[revenue],
                            "Normalized Total":[normalized_total]})


gender_data["Average Purchase Price"]= gender_data["Average Purchase Price"].map("${:,.2f}".format)
gender_data["Total Purchase Value"]= gender_data["Total Purchase Value"].map("${:,.2f}".format)
gender_data["Normalized Total"]= gender_data["Normalized Total"].map("${:,.2f}".format)
gender_data
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
      <th>Normalized Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>780</td>
      <td>$2.93</td>
      <td>$2,286.33</td>
      <td>$2.93</td>
    </tr>
  </tbody>
</table>
</div>




```python
#age demographics
purchase = players_df["Item ID"].count()



bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_labels = ["10-14","15-19","20-24","25-29","30-34","35-39","40+","<10"]
print(players_df["Age"].max())
print(players_df["Age"].min())
pd.cut(players_df["Age"], bins,labels=group_labels)

players_group["Age"]=pd.cut(players_df["Age"],bins, labels=group_labels)
#players_group = players_df.groupby("Age")
players = pd.DataFrame({"Purchase Count":[purchase],
                               "Average Purchase Price": [avg],
                               "Total Purchase Value":[revenue],
                               "Normalized Total":[normalized_total]})
print(players_group)


#demo_data = pd.DataFrame
```

    45
    7
    


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-22-a394686d4d78> in <module>()
         10 pd.cut(players_df["Age"], bins,labels=group_labels)
         11 
    ---> 12 players_group["Age"]=pd.cut(players_df["Age"],bins, labels=group_labels)
         13 #players_group = players_df.groupby("Age")
         14 players = pd.DataFrame({"Purchase Count":[purchase],
    

    NameError: name 'players_group' is not defined

