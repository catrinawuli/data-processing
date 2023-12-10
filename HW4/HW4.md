```python
import pandas as pd
import matplotlib.pyplot as plt 
import seaborn as sns
```

# Cleaning data

Before cleaning the data, it is essential to check the unique values in each column because our interest lies in the number of cell phones per capita across different years and countries. Examining the code below reveals that, from the year 1960 to 1979, the number of cell phones is either null or zero. Therefore, we can safely delete these columns directly, resulting in fewer columns to process.

Next, we address the remaining zero and null values in the data. We convert them to numeric values and use the forward-fill method to handle the null values.

With these steps, we obtain the cleaned data for the first table.


```python
# read the number of cellphones data
URL = "https://raw.githubusercontent.com/mt4007-ht23/data/main/cell_phones_total.csv"
data = pd.read_csv(URL)

#check the unique values of each column
unique_values_per_column = data.apply(lambda x: x.unique())
print(unique_values_per_column.head(20).to_markdown())
```

    |       | 0                                                                        |
    |:------|:-------------------------------------------------------------------------|
    | iso-3 | ['ABW' 'AFG' 'AGO' 'ALB' 'AND' 'ARE' 'ARG' 'ARM' 'ASM' 'ATG' 'AUS' 'AUT' |
    |       |  'AZE' 'BDI' 'BEL' 'BEN' 'BFA' 'BGD' 'BGR' 'BHR' 'BHS' 'BIH' 'BLR' 'BLZ' |
    |       |  'BMU' 'BOL' 'BRA' 'BRB' 'BRN' 'BTN' 'BWA' 'CAF' 'CAN' 'CHE' 'CHL' 'CHN' |
    |       |  'CIV' 'CMR' 'COD' 'COG' 'COL' 'COM' 'CPV' 'CRI' 'CUB' 'CUW' 'CYM' 'CYP' |
    |       |  'CZE' 'DEU' 'DJI' 'DMA' 'DNK' 'DOM' 'DZA' 'ECU' 'EGY' 'ERI' 'ESP' 'EST' |
    |       |  'ETH' 'FIN' 'FJI' 'FRA' 'FRO' 'FSM' 'GAB' 'GBR' 'GEO' 'GHA' 'GIB' 'GIN' |
    |       |  'GMB' 'GNB' 'GNQ' 'GRC' 'GRD' 'GRL' 'GTM' 'GUM' 'GUY' 'HKG' 'HND' 'HRV' |
    |       |  'HTI' 'HUN' 'IDN' 'IND' 'IRL' 'IRN' 'IRQ' 'ISL' 'ISR' 'ITA' 'JAM' 'JOR' |
    |       |  'JPN' 'KAZ' 'KEN' 'KGZ' 'KHM' 'KIR' 'KNA' 'KOR' 'KWT' 'LAO' 'LBN' 'LBR' |
    |       |  'LBY' 'LCA' 'LIE' 'LKA' 'LSO' 'LTU' 'LUX' 'LVA' 'MAC' 'MAR' 'MCO' 'MDA' |
    |       |  'MDG' 'MDV' 'MEX' 'MHL' 'MKD' 'MLI' 'MLT' 'MMR' 'MNE' 'MNG' 'MNP' 'MOZ' |
    |       |  'MRT' 'MUS' 'MWI' 'MYS' 'NAM' 'NCL' 'NER' 'NGA' 'NIC' 'NLD' 'NOR' 'NPL' |
    |       |  'NRU' 'NZL' 'OMN' 'PAK' 'PAN' 'PER' 'PHL' 'PLW' 'PNG' 'POL' 'PRI' 'PRK' |
    |       |  'PRT' 'PRY' 'PSE' 'PYF' 'QAT' 'ROU' 'RUS' 'RWA' 'SAU' 'SDN' 'SEN' 'SGP' |
    |       |  'SLB' 'SLE' 'SLV' 'SMR' 'SOM' 'SRB' 'SSD' 'STP' 'SUR' 'SVK' 'SVN' 'SWE' |
    |       |  'SWZ' 'SXM' 'SYC' 'SYR' 'TCA' 'TCD' 'TGO' 'THA' 'TJK' 'TKM' 'TLS' 'TON' |
    |       |  'TTO' 'TUN' 'TUR' 'TUV' 'TZA' 'UGA' 'UKR' 'URY' 'USA' 'UZB' 'VCT' 'VEN' |
    |       |  'VGB' 'VIR' 'VNM' 'VUT' 'WSM' 'XKX' 'YEM' 'ZAF' 'ZMB' 'ZWE']            |
    | 1960  | [ 0. nan]                                                                |
    | 1965  | [ 0. nan]                                                                |
    | 1966  | [nan]                                                                    |
    | 1967  | [nan]                                                                    |
    | 1968  | [nan]                                                                    |
    | 1969  | [nan]                                                                    |
    | 1970  | [ 0. nan]                                                                |
    | 1971  | [nan]                                                                    |
    | 1972  | [nan]                                                                    |
    | 1973  | [nan]                                                                    |
    | 1974  | [nan]                                                                    |
    | 1975  | [ 0. nan]                                                                |
    | 1976  | [ 0. nan]                                                                |
    | 1977  | [ 0. nan]                                                                |
    | 1978  | [ 0. nan]                                                                |
    | 1979  | [ 0. nan]                                                                |
    | 1980  | ['0' nan '23.5k']                                                        |
    | 1981  | ['0' nan '28.3k' '13.3k' '1670' '20.4k']                                 |
    | 1982  | ['0' '2330' nan '7200' '33.9k' '19.8k' '11.1k' '27.2k']                  |
    


```python
#delete the column 1960-1979 which have only null or zero values.
data.drop(data.iloc[:, 1:17].columns,inplace = True, axis=1)
data = data.fillna(0) 
```


```python
# convert the values to numeric
def convert_to_numeric(value):
    multiplier = 1
    value = str(value)
    if 'k' in value:
        multiplier = 1000
    elif 'M' in value:
        multiplier = 1000000
    elif 'B' in value:
        multiplier = 1000000000

    return pd.to_numeric(value[:-1]) * multiplier

data[data.columns[1:]] = data[data.columns[1:]].applymap(convert_to_numeric)
```


```python
# we use the forward fill to fill the null values.
data[data.columns[1:]] = data[data.columns[1:]].ffill(axis = 1)
data = data.fillna(0)
sorted_data = data.sort_values(by="2015", ascending=False)

print_table = pd.concat([sorted_data.iloc[:, :1], sorted_data.iloc[:, -5:]], axis=1)
print(print_table.head().to_markdown(index = False))
```

    | iso-3   |     2015 |     2016 |     2017 |     2018 |     2019 |
    |:--------|---------:|---------:|---------:|---------:|---------:|
    | CHN     | 1.29e+09 | 1.36e+09 | 1.47e+09 | 1.65e+09 | 1.73e+09 |
    | IND     | 1e+09    | 1.13e+09 | 1.17e+09 | 1.18e+09 | 1.15e+09 |
    | USA     | 3.82e+08 | 3.96e+08 | 4e+08    | 4.22e+08 | 4.22e+08 |
    | IDN     | 3.39e+08 | 3.86e+08 | 4.35e+08 | 3.19e+08 | 3.45e+08 |
    | BRA     | 2.58e+08 | 2.44e+08 | 2.18e+08 | 2.07e+08 | 2.07e+08 |
    

# Combining Data
We read the second table containing population data for each country. We retain the corresponding columns from the first table and remove any irrelevant columns.

Then we merge the two tables and create a new cloumn number of capital which is derived from the existing columns 
population and n_cellphones.


```python
# read the population table 
URL2 = "https://raw.githubusercontent.com/mt4007-ht23/data/main/pop_data.csv"
pop_data = pd.read_csv(URL2)
```


```python
#retain the corresponding columns from the first table and remove any irrelevant columns.
pop_data.drop(pop_data.iloc[:, 2:22].columns,inplace = True, axis=1)
pop_data.drop("Unnamed: 0",inplace = True , axis=1)
pop_data.drop("2020",inplace = True ,axis=1)
```


```python
# transform two tables as required
pop_data_melted = pd.melt(pop_data, id_vars='iso-3', var_name='year', value_name='population')
data_melted = pd.melt(data, id_vars='iso-3', var_name='year', value_name='n_cellphones')

# merge the two tables by primary key which is ['iso-3', 'year']
combined_data = pd.merge(data_melted, pop_data_melted, on=['iso-3', 'year'])

#create a new column "num_per_cap"
combined_data.loc[:, "num_per_cap"] = round(combined_data["n_cellphones"] / combined_data["population"], 5)

print_table_pop = combined_data.iloc[:, 0:4]
print(print_table_pop.head().to_markdown(index = False))
```

    | iso-3   |   year |   n_cellphones |      population |
    |:--------|-------:|---------------:|----------------:|
    | ABW     |   1980 |              0 | 62267           |
    | AFG     |   1980 |              0 |     1.24866e+07 |
    | AGO     |   1980 |              0 |     8.33005e+06 |
    | ALB     |   1980 |              0 |     2.672e+06   |
    | AND     |   1980 |              0 | 35611           |
    

We read the third dataset, from which we can extract country names through mapping. Then, we pivot the tables and display the countries with the highest number of cell phones per capita in 2019 in a table.

Following that, we create a plot illustrating the growth rates of the countries 'Macao,' 'Hong Kong,' 'United Arab Emirates,' 'Antigua and Barbuda,' and 'Seychelles' over the last 10 years.


```python
#read the third table country
url3 = "https://raw.githubusercontent.com/mt4007-ht23/data/main/country_data.csv"
country_data = pd.read_csv(url3)
```


```python
#we map the country name to the merged table above. 
dictionary_country_data = dict(zip(country_data['alpha-3'], country_data['name']))
combined_data["country"] = combined_data["iso-3"].map(dictionary_country_data)

# Then we pivot the table which are combined with the informations from the three tables. 
pivoted = combined_data.pivot(index = "year", columns = "country", values = "num_per_cap")

#we pivot the tables and display the countries with the highest number of cell phones per capita in 2019 in a table.
sorted_countries_by_rows = pivoted.sort_index()
sorted_countries = sorted_countries_by_rows.sort_values(by = "2019",ascending=False , axis=1)
sorted_countries_topFive = sorted_countries.iloc[:, 0:5].tail()
print(sorted_countries_topFive.tail().to_markdown())
```

    |   year |   Macao |   Hong Kong |   United Arab Emirates |   Antigua and Barbuda |   Seychelles |
    |-------:|--------:|------------:|-----------------------:|----------------------:|-------------:|
    |   2015 | 3.08823 |     2.2904  |                2.00742 |               1.95684 |      1.58426 |
    |   2016 | 3.14351 |     2.39893 |                2.21252 |               1.98754 |      1.60546 |
    |   2017 | 3.2101  |     2.48877 |                2.18343 |               2.01934 |      1.74243 |
    |   2018 | 3.34874 |     2.67021 |                2.19908 |               2.00816 |      1.8499  |
    |   2019 | 3.28485 |     2.86365 |                2.12774 |               1.99746 |      1.9872  |
    


```python
# Keep only rows with specified country names and years 
combined_data['year'] = pd.to_numeric(combined_data['year'])
filtered_data = combined_data[combined_data['year'] >= 2009]
selected_countries = ['Macao', 'Hong Kong', 'United Arab Emirates','Antigua and Barbuda', 'Seychelles']
filtered_data = filtered_data[filtered_data['country'].isin(selected_countries)]

#create a plot that illustrates the growth rate of the countries i
sns.lineplot(x="year", y="num_per_cap", hue="country", data=filtered_data)
plt.legend(loc="upper left")
plt.show()
```


    
![png](output_13_0.png)
    

