# Homework 2

## Apartment Prices

In our apartment price analysis, I started by creating a new column that represents the price per square meter (ppsqm). This was done by dividing the 'soldPrice' by the 'livingArea.' Notably, I inserted this new column as the first one in the table.

Next, I sorted the table based on the 'ppsqm' column and displayed the top five entries with the highest prices, as requested.

For the apartments located in Ekhagen, I used the 'mean' function to calculate the average ppsqm. I find the 'mean' function particularly intriguing because it simplifies the process of obtaining summary statistics from the data. Additionally, exploring different functions and methods in data analysis can lead to more efficient and effective ways of extracting insights from the dataset."


```python
import pandas as pd
import matplotlib.pyplot as plt 

# download the csv file to local folder, then read it
ApartmentPrice = pd.read_csv("Booli_sold.csv")

# create a new column derived from existing columns and insert it in the first column.
ApartmentPrice.insert(0, 'ppsqm', round(ApartmentPrice["soldPrice"]/ApartmentPrice["livingArea"],2)) 

# create a new DataFrame containing only the top five records. 
topFive = ApartmentPrice.sort_values("ppsqm", ascending = False).head()

# use the 'mean' function to calculate the average ppsqm.
mean =  round(ApartmentPrice["ppsqm"].mean(),2)
print("The avereage price per square meter in Ekhagen is ",mean)
```

    The avereage price per square meter in Ekhagen is  58759.38
    

# The Swedish Election of 2018


```python
# read csv file directly from the webside.
url = "https://raw.githubusercontent.com/mt4007-ht23/data/main/2018_R_per_kommun.csv"
swedishElection2018= pd.read_csv(url, sep=";", decimal=',')

# task 1: First, group the data by 'LÄNSNAMN,' then sum up the column 'RÖSTER GILTIGA' and select the data for 'Stockholms län' 
votesGrouped = swedishElection2018.groupby("LÄNSNAMN")
votesGroupedSum = votesGrouped["RÖSTER GILTIGA"].sum()
votesInStockholm_sum = votesGroupedSum.loc['Stockholms län']
print("The number of legitimate votes for all municipalities in Stockholm is" , votesInStockholm_sum )
```

    The number of legitimate votes for all municipalities in Stockholm is 1426237
    


```python
# task 2: The highest voting percentage is observed in Munkfors municipality.
VALDELTAGANDE = swedishElection2018.loc[:,["KOMMUNNAMN","VALDELTAGANDE"]]
swedishElection2018.sort_values("S", ascending = False).head(1)
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
      <th>LÄNSKOD</th>
      <th>KOMMUNKOD</th>
      <th>LÄNSNAMN</th>
      <th>KOMMUNNAMN</th>
      <th>M</th>
      <th>C</th>
      <th>L</th>
      <th>KD</th>
      <th>S</th>
      <th>V</th>
      <th>...</th>
      <th>TRP</th>
      <th>VL-S</th>
      <th>ÖVR</th>
      <th>OGEJ</th>
      <th>BLANK</th>
      <th>OG</th>
      <th>RÖSTER GILTIGA</th>
      <th>RÖSTANDE</th>
      <th>RÖSTBERÄTTIGADE</th>
      <th>VALDELTAGANDE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>188</th>
      <td>17</td>
      <td>62</td>
      <td>Värmlands län</td>
      <td>Munkfors</td>
      <td>8.33</td>
      <td>8.12</td>
      <td>2.91</td>
      <td>3.93</td>
      <td>52.18</td>
      <td>6.11</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.02</td>
      <td>NaN</td>
      <td>2340</td>
      <td>2364</td>
      <td>2777</td>
      <td>85.13</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 45 columns</p>
</div>




```python
# task 3: sort the values at a descending order, then select the top 3 records. 
VALDELTAGANDE.sort_values("VALDELTAGANDE", ascending = False).head(3)
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
      <th>KOMMUNNAMN</th>
      <th>VALDELTAGANDE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>103</th>
      <td>Lomma</td>
      <td>93.86</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Habo</td>
      <td>93.35</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Vellinge</td>
      <td>93.13</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```
