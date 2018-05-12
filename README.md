
# Unit 4 Assignment | Option 1 - Heroes of Pymoli | Josh Muchmore


```python
import pandas as pd
json_path = "purchase_data.json"
heroes_raw = pd.read_json(json_path)
heroes_raw.head(10)
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
    <tr>
      <th>5</th>
      <td>20</td>
      <td>Male</td>
      <td>10</td>
      <td>Sleepwalker</td>
      <td>1.73</td>
      <td>Tanimnya91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20</td>
      <td>Male</td>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>4.57</td>
      <td>Undjaskla97</td>
    </tr>
    <tr>
      <th>7</th>
      <td>29</td>
      <td>Female</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>3.32</td>
      <td>Iathenudil29</td>
    </tr>
    <tr>
      <th>8</th>
      <td>25</td>
      <td>Male</td>
      <td>118</td>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>2.77</td>
      <td>Sondenasta63</td>
    </tr>
    <tr>
      <th>9</th>
      <td>31</td>
      <td>Male</td>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>4.53</td>
      <td>Hilaerin92</td>
    </tr>
  </tbody>
</table>
</div>



# Player Count


```python
num_players = heroes_raw["SN"].nunique()
players = pd.DataFrame({"Number of Players": [heroes_raw["SN"].nunique()]})
players
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
      <th>Number of Players</th>
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



# Purchasing Analysis (Total)


```python
items = heroes_raw["Item Name"].nunique()
average_price = heroes_raw["Price"].mean()
purchases = len(heroes_raw)
revenue = heroes_raw["Price"].sum()

purchasing_analysis = pd.DataFrame({
                                    "Number of Unique Items": [items],
                                    "Average Purchase Price": [average_price],
                                    "Total Number of Purchases":[purchases],
                                    "Total Revenue":[revenue]
                                    })
purchasing_analysis["Average Purchase Price"] = purchasing_analysis["Average Purchase Price"].astype('float64')
purchasing_analysis["Average Purchase Price"] = purchasing_analysis["Average Purchase Price"].map("${:.2f}".format)

purchasing_analysis["Total Revenue"] = purchasing_analysis["Total Revenue"].astype('float64')
purchasing_analysis["Total Revenue"] = purchasing_analysis["Total Revenue"].map("${:.2f}".format)

purchasing_analysis
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
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>



# Gender Demographics



```python
heroes_dropped = heroes_raw.drop_duplicates(subset = "SN")

gender_heroes = heroes_dropped.groupby(["Gender"]).count()

count_female = gender_heroes["Age"]["Female"]
count_male = gender_heroes["Age"]["Male"]
count_other = gender_heroes["Age"]["Other / Non-Disclosed"]

percent_female = (count_female/num_players)*100
percent_male = (count_male/num_players)*100
percent_other = (count_other/num_players)*100

gender_purchase = pd.DataFrame({
                                    "Gender": ["Female" ,"Male", "Other / Non-Disclosed"],
                                    "Count": [count_female, count_male, count_other],
                                    "Percentage of Players": [percent_female, percent_male, percent_other]
                                    })

gender_purchase = gender_purchase.set_index("Gender")
gender_purchase["Percentage of Players"] = gender_purchase["Percentage of Players"].map("%{:.2f}".format)
gender_purchase_final = gender_purchase.sort_values("Percentage of Players", ascending=False)

gender_purchase_final
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
      <th>Count</th>
      <th>Percentage of Players</th>
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
      <td>465</td>
      <td>%81.15</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>%17.45</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>%1.40</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Gender)


```python
gender_purchases = heroes_raw["Gender"].value_counts()

female_purchases = gender_purchases["Female"]
male_purchases = gender_purchases["Male"]
other_purchases = gender_purchases["Other / Non-Disclosed"]

females = heroes_raw.loc[heroes_raw["Gender"] == "Female", :]
males = heroes_raw.loc[heroes_raw["Gender"] == "Male", :]
others = heroes_raw.loc[heroes_raw["Gender"] == "Other / Non-Disclosed", :]

female_avg_price = females["Price"].mean()
male_avg_price = males["Price"].mean()
other_avg_price = others["Price"].mean()

female_total_value = females["Price"].sum()
male_total_value = males["Price"].sum()
others_total_value = others["Price"].sum()

female_normalized = female_total_value / count_female
male_normalized = male_total_value / count_male
others_normalized = others_total_value / count_other

gender_purchases_final = pd.DataFrame({
                                    "Purchase Count": [female_purchases, male_purchases, other_purchases],
                                    "Average Purchase Price": [female_avg_price, male_avg_price, other_avg_price],
                                    "Total Purchase Value": [female_total_value, male_total_value, others_total_value],
                                    "Normalized Totals": [female_normalized, male_normalized, others_normalized],
                                    "Gender":["Female", "Male", "Other / Non-Disclosed"]
                                    })

gender_purchases_final = gender_purchases_final.set_index("Gender")
gender_purchases_final["Average Purchase Price"] = gender_purchases_final["Average Purchase Price"].map("${:.2f}".format)
gender_purchases_final["Total Purchase Value"] = gender_purchases_final["Total Purchase Value"].map("${:.2f}".format)
gender_purchases_final["Normalized Totals"] = gender_purchases_final["Normalized Totals"].map("${:.2f}".format)
gender_purchases_final
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
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>$2.82</td>
      <td>$3.83</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$4.02</td>
      <td>633</td>
      <td>$1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$4.47</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>



# Age Demographics


```python
bins = [0, 9, 14, 19, 24, 29, 34, 39, 44, 49]
groups = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-45", "45-50"]

heroes_raw["Age Group"] = pd.cut(heroes_raw["Age"], bins, labels=groups)

heroes_age = heroes_raw.groupby("Age Group")

heroes_age_dropped = heroes_raw.drop_duplicates(subset="SN") 
heroes_age_counts = heroes_age_dropped.groupby("Age Group").count()


age_count = heroes_age["Age"].count()
avg_age_price = heroes_age["Price"].mean()
age_total_value = heroes_age["Price"].sum()
age_normalized = age_total_value / heroes_age_counts["Age"]



age_demographics = pd.DataFrame({
                                    "Purchase Count": age_count,
                                    "Average Purchase Price": avg_age_price,
                                    "Total Purchase Value": age_total_value,
                                    "Normalized Totals": age_normalized
                                 })

age_demographics["Average Purchase Price"] = age_demographics["Average Purchase Price"].map("${:.2f}".format)
age_demographics["Total Purchase Value"] = age_demographics["Total Purchase Value"].map("${:.2f}".format)
age_demographics["Normalized Totals"] = age_demographics["Normalized Totals"].map("${:.2f}".format)

age_demographics
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
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>$2.98</td>
      <td>$4.39</td>
      <td>28</td>
      <td>$83.46</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>$2.77</td>
      <td>$4.22</td>
      <td>35</td>
      <td>$96.95</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>$2.91</td>
      <td>$3.86</td>
      <td>133</td>
      <td>$386.42</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>$2.91</td>
      <td>$3.78</td>
      <td>336</td>
      <td>$978.77</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>$2.96</td>
      <td>$4.26</td>
      <td>125</td>
      <td>$370.33</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>$3.08</td>
      <td>$4.20</td>
      <td>64</td>
      <td>$197.25</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>$2.84</td>
      <td>$4.42</td>
      <td>42</td>
      <td>$119.40</td>
    </tr>
    <tr>
      <th>40-45</th>
      <td>$3.19</td>
      <td>$5.10</td>
      <td>16</td>
      <td>$51.03</td>
    </tr>
    <tr>
      <th>45-50</th>
      <td>$2.72</td>
      <td>$2.72</td>
      <td>1</td>
      <td>$2.72</td>
    </tr>
  </tbody>
</table>
</div>



# Top Spenders


```python
heroes_sn = heroes_raw.groupby("SN")
top_spenders = heroes_sn.sum().sort_values("Price", ascending=False).head()

spenders_sn = top_spenders.index.values

spenders_filtered = heroes_raw[heroes_raw['SN'] == spenders_sn[0]]
spenders_filtered = spenders_filtered.append(heroes_raw[heroes_raw['SN'] == spenders_sn[1]])
spenders_filtered = spenders_filtered.append(heroes_raw[heroes_raw['SN'] == spenders_sn[2]])
spenders_filtered = spenders_filtered.append(heroes_raw[heroes_raw['SN'] == spenders_sn[3]])
spenders_filtered = spenders_filtered.append(heroes_raw[heroes_raw['SN'] == spenders_sn[4]])

spenders_purchase_count = spenders_filtered.groupby("SN")["Item ID"].count()
spenders_purchase_avg = spenders_filtered.groupby("SN")["Price"].mean()
spenders_purchase_sum = spenders_filtered.groupby("SN")["Price"].sum()

spenders_final = pd.DataFrame({
                                    "Purchase Count": spenders_purchase_count,
                                    "Average Purchase Price": spenders_purchase_avg,
                                    "Total Purchase Value": spenders_purchase_sum,
                                   })


spenders_final["Average Purchase Price"] = spenders_final["Average Purchase Price"].map("${:.2f}".format)
spenders_final["Total Purchase Value"] = spenders_final["Total Purchase Value"].map("${:.2f}".format)

spenders_display = spenders_final.sort_values("Total Purchase Value", ascending=False)
spenders_display
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
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>$3.41</td>
      <td>5</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>$3.39</td>
      <td>4</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>$3.18</td>
      <td>4</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>$4.24</td>
      <td>3</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>$3.86</td>
      <td>3</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>



# Most Popular Items


```python
heroes_items = heroes_raw.groupby("Item ID")
top_items = heroes_items.count().sort_values("Item Name", ascending=False).head()

top_items_names = top_items.index.values

top_items_filtered = heroes_raw[heroes_raw['Item ID'] == top_items_names[0]]
top_items_filtered = top_items_filtered.append(heroes_raw[heroes_raw['Item ID'] == top_items_names[1]])
top_items_filtered = top_items_filtered.append(heroes_raw[heroes_raw['Item ID'] == top_items_names[2]])
top_items_filtered = top_items_filtered.append(heroes_raw[heroes_raw['Item ID'] == top_items_names[3]])
top_items_filtered = top_items_filtered.append(heroes_raw[heroes_raw['Item ID'] == top_items_names[4]])

top_items_filtered = top_items_filtered.sort_values("Item ID", ascending=True)

top_items_purchase_count = top_items_filtered.groupby("Item ID")["Item Name"].count()
top_items_purchase_avg = top_items_filtered.groupby("Item ID")["Price"].mean()
top_items_purchase_sum = top_items_filtered.groupby("Item ID")["Price"].sum()



top_items_item_names = top_items_filtered["Item Name"].unique()
top_items_item_names

top_items_final = pd.DataFrame({
                                    "Purchase Count": top_items_purchase_count,
                                    "Item Price": top_items_purchase_avg,
                                    "Total Purchase Value": top_items_purchase_sum,
                                    "Item Name": top_items_item_names
                                   })

top_items_final["Item Price"] = top_items_final["Item Price"].map("${:.2f}".format)
top_items_final["Total Purchase Value"] = top_items_final["Total Purchase Value"].map("${:.2f}".format)

top_items_display = top_items_final.sort_values("Purchase Count", ascending=False)
top_items_display
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
      <th>Item Price</th>
      <th>Purchase Count</th>
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
      <th>39</th>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Arcane Gem</td>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Serenity</td>
      <td>$1.49</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Trickster</td>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Woeful Adamantite Claymore</td>
      <td>$1.24</td>
      <td>9</td>
      <td>$11.16</td>
    </tr>
  </tbody>
</table>
</div>



# Most Profitable Items


```python
heroes_profit_items = heroes_raw.groupby("Item ID")
profitable_items = heroes_profit_items.sum().sort_values("Price", ascending=False).head()

profitable_items_id = profitable_items.index.values

top_items_profit_filtered = heroes_raw[heroes_raw['Item ID'] == profitable_items_id[0]]
top_items_profit_filtered = top_items_profit_filtered.append(heroes_raw[heroes_raw['Item ID'] == profitable_items_id[1]])
top_items_profit_filtered = top_items_profit_filtered.append(heroes_raw[heroes_raw['Item ID'] == profitable_items_id[2]])
top_items_profit_filtered = top_items_profit_filtered.append(heroes_raw[heroes_raw['Item ID'] == profitable_items_id[3]])
top_items_profit_filtered = top_items_profit_filtered.append(heroes_raw[heroes_raw['Item ID'] == profitable_items_id[4]])

top_items_profit_filtered = top_items_profit_filtered.sort_values("Item ID", ascending=True)

top_items_profit_purchase_count = top_items_profit_filtered.groupby("Item ID")["Item Name"].count()
top_items_profit_purchase_avg = top_items_profit_filtered.groupby("Item ID")["Price"].mean()
top_items_profit_purchase_sum = top_items_profit_filtered.groupby("Item ID")["Price"].sum()

top_items_profit_names = top_items_profit_filtered["Item Name"].unique()

top_items_profit_final = pd.DataFrame({
                                    "Purchase Count": top_items_profit_purchase_count,
                                    "Item Price": top_items_profit_purchase_avg,
                                    "Total Purchase Value": top_items_profit_purchase_sum,
                                    "Item Name": top_items_profit_names
                                   })

top_items_profit_final["Item Price"] = top_items_profit_final["Item Price"].map("${:.2f}".format)
top_items_profit_final["Total Purchase Value"] = top_items_profit_final["Total Purchase Value"].map("${:.2f}".format)

top_items_profit_display = top_items_profit_final.sort_values("Total Purchase Value", ascending=False)
top_items_profit_display
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
      <th>Item Price</th>
      <th>Purchase Count</th>
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
      <th>34</th>
      <td>Retribution Axe</td>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Spectral Diamond Doomblade</td>
      <td>$4.25</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Orenmir</td>
      <td>$4.95</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Singed Scalpel</td>
      <td>$4.87</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Splitter, Foe Of Subtlety</td>
      <td>$3.61</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


