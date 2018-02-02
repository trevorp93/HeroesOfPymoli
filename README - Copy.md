

```python
import pandas as pd
import json
import numpy
```


```python
file = "purchase_data.json"
```


```python
df = pd.read_json(file)
df.head()

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
# Player Count
Players = len(df["SN"].unique())
PlayerCount_table =pd.DataFrame({"Number of Players":Players},index =[0])
PlayerCount_table


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




```python
# Purchasing Analysis (Total)
Items = len(df['Item Name'].unique())
AvePurchasePrice = df['Price'].mean()
TotalNumber = df['Price'].count()
TotalRevenue = df['Price'].sum()
PA_table = pd.DataFrame({'Unique Items':Items, 'Average Price':AvePurchasePrice, 
                         'Number of Purchases':TotalNumber,"Total Revenue":TotalRevenue},index = [0])
PA_table = PA_table[['Unique Items','Average Price','Number of Purchases','Total Revenue']]
PA_table['Average Price']= PA_table['Average Price'].map("${:.2f}".format)
PA_table['Total Revenue']= PA_table['Total Revenue'].map("${:.2f}".format)
PA_table
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
      <th>Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics

Male = len(df['SN'].loc[df["Gender"] =='Male'].unique())
Female= len(df['SN'].loc[df["Gender"] =='Female'].unique())
Other = len(df['SN'].loc[df["Gender"] =='Other / Non-Disclosed'].unique())
MaleP = Male / Players * 100
FemaleP = Female / Players * 100
OtherP = Other / Players * 100

TC = pd.DataFrame({" ":['Male','Female','Other / Non-Disclosed'],"Percentage":[MaleP, FemaleP,OtherP],"Total Count":[Male,Female,Other]})
TC.set_index(' ',inplace=True)
TC.head()

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
      <th>Percentage</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.151832</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.452007</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.396161</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)

Mdf = df.loc[df['Gender']=='Male']
MTotalNumber = Mdf['SN'].count()
MAveragePurchasePrice = Mdf['Price'].mean()
MTotalRevenue = Mdf['Price'].sum()

Fdf = df.loc[df['Gender']=='Female']
FTotalNumber = Fdf['SN'].count()
FAveragePurchasePrice = Fdf['Price'].mean()
FTotalRevenue = Fdf['Price'].sum()

Odf = df.loc[df['Gender']=='Other / Non-Disclosed']
OTotalNumber = Odf['SN'].count()
OAveragePurchasePrice = Odf['Price'].mean()
OTotalRevenue = Odf['Price'].sum()

PAG_df = pd.DataFrame({"Gender":['Male','Female','Other/Non-Disclosed'],
                        "Purchase Count":[MTotalNumber,FTotalNumber,OTotalNumber],
                      "Total Purchase Volume":[MTotalRevenue,FTotalRevenue,OTotalRevenue],
                      "Average Purchase Price":[MAveragePurchasePrice,FAveragePurchasePrice,OAveragePurchasePrice]})
PAG_df.set_index('Gender',inplace=True)


Max = PAG_df['Total Purchase Volume'].max()
Min = PAG_df['Total Purchase Volume'].min()
PAG_df['Normalized Value'] = (PAG_df['Total Purchase Volume']-Min)/(Max-Min)
# (X- Min)/(Max-Min)
PAG_df["Average Purchase Price"] = PAG_df["Average Purchase Price"].map("${:.2f}".format)
PAG_df["Total Purchase Volume"] = PAG_df["Total Purchase Volume"].map("${:.2f}".format)
PAG_df["Normalized Value"] = PAG_df["Normalized Value"].map("${:.2f}".format)
PAG_df.head()
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Volume</th>
      <th>Normalized Value</th>
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
      <th>Male</th>
      <td>$2.95</td>
      <td>633</td>
      <td>$1867.68</td>
      <td>$1.00</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>$2.82</td>
      <td>136</td>
      <td>$382.91</td>
      <td>$0.19</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>$3.25</td>
      <td>11</td>
      <td>$35.74</td>
      <td>$0.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
bins = [0,10,15,20,25,30,35,40,300]
group_names = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
df["Age Group"]=pd.cut(df["Age"], bins, labels=group_names)
One = df.groupby('Age Group')['Age'].count()
Two =df.groupby('Age Group')['Price'].mean()
Three = df.groupby('Age Group')['Price'].sum()
Max = df['Price'].max()
Min = df['Price'].min()
Four = (df.groupby('Age Group')['Price'].mean()-Min)/(Max-Min)

df.groupby('Age Group')['Age'].count()

AD=pd.DataFrame({"Number of Transations":One,"Average Purchase": Two, "Total Value":Three,"Normalized Value":Four})
AD=AD[["Number of Transations",'Average Purchase','Total Value','Normalized Value']]
AD
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
      <th>Number of Transations</th>
      <th>Average Purchase</th>
      <th>Total Value</th>
      <th>Normalized Value</th>
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
      <td>32</td>
      <td>3.019375</td>
      <td>96.62</td>
      <td>0.507494</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>78</td>
      <td>2.873718</td>
      <td>224.15</td>
      <td>0.470336</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>184</td>
      <td>2.873587</td>
      <td>528.74</td>
      <td>0.470303</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>305</td>
      <td>2.959377</td>
      <td>902.61</td>
      <td>0.492188</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>76</td>
      <td>2.892368</td>
      <td>219.82</td>
      <td>0.475094</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>58</td>
      <td>3.073448</td>
      <td>178.26</td>
      <td>0.521288</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>44</td>
      <td>2.897500</td>
      <td>127.49</td>
      <td>0.476403</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
      <td>2.880000</td>
      <td>8.64</td>
      <td>0.471939</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Top Spenders**
df.head()

Sum =df.groupby('SN')['Price'].sum()
Average = df.groupby('SN')['Price'].mean()
Number = df.groupby('SN')['Price'].count()

TS_df = pd.DataFrame({"Amount of Transations":Number,"Average Transation":Average,"Total Amount":Sum})
TS_df.sort_values(by=['Total Amount'],ascending = False).head(5)
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
      <th>Amount of Transations</th>
      <th>Average Transation</th>
      <th>Total Amount</th>
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
      <td>5</td>
      <td>3.412000</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.390000</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.185000</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>4.243333</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>3.860000</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
Sum =df.groupby('Item Name')['Price'].sum()
Average = df.groupby('Item Name')['Price'].mean()
Number = df.groupby('Item Name')['Price'].count()
ITEM = df.groupby('Item Name')['Item ID'].mean()

POP_df = pd.DataFrame({"Amount of Transations":Number,"Average Transation":Average,"Total Amount":Sum,"Item ID":ITEM})
POP_df.sort_values(by=['Amount of Transations'],ascending = False).head(5)



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
      <th>Amount of Transations</th>
      <th>Average Transation</th>
      <th>Item ID</th>
      <th>Total Amount</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
      <td>2.757143</td>
      <td>95.857143</td>
      <td>38.60</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>2.230000</td>
      <td>84.000000</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>2.350000</td>
      <td>39.000000</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>3.465000</td>
      <td>105.000000</td>
      <td>34.65</td>
    </tr>
    <tr>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>1.240000</td>
      <td>175.000000</td>
      <td>11.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
Sum =df.groupby('Item Name')['Price'].sum()
Average = df.groupby('Item Name')['Price'].mean()
Number = df.groupby('Item Name')['Price'].count()
ITEM = df.groupby('Item Name')['Item ID'].mean()

POP_df = pd.DataFrame({"Amount of Transations":Number,"Average Transation":Average,"Total Amount":Sum,"Item ID":ITEM})
POP_df.sort_values(by=['Total Amount'],ascending = False).head(5)

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
      <th>Amount of Transations</th>
      <th>Average Transation</th>
      <th>Item ID</th>
      <th>Total Amount</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
      <td>2.757143</td>
      <td>95.857143</td>
      <td>38.60</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>4.140000</td>
      <td>34.000000</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>3.465000</td>
      <td>105.000000</td>
      <td>34.65</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>4.250000</td>
      <td>115.000000</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <td>6</td>
      <td>4.950000</td>
      <td>32.000000</td>
      <td>29.70</td>
    </tr>
  </tbody>
</table>
</div>




```python
#1. Final Critic is the most popular and profitable item.
#2. Our audience is largely male
#3. Undirrala66 is funding out next game.
```
