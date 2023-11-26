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
print(data.head().to_markdown(index=False))
```

    | iso-3   |   1980 |   1981 |   1982 |   1983 |   1984 |   1985 |   1986 |   1987 |   1988 |   1989 |   1990 |   1991 |   1992 |   1993 |   1994 |   1995 |   1996 |   1997 |   1998 |   1999 |   2000 |   2001 |   2002 |         2003 |          2004 |          2005 |          2006 |          2007 |          2008 |          2009 |          2010 |          2011 |          2012 |          2013 |          2014 |          2015 |          2016 |          2017 |          2018 |          2019 |
    |:--------|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|--------------:|
    | ABW     |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      2 |      2 |      2 |    172 |    300 |    340 |    538 |  12000 |  15000 |  53000 |  61800 |  70000       |  98400        | 103000        | 109000        | 114000        | 121000        | 128000        | 132000        | 132000        | 135000        | 139000        | 140000        | 141000        | 141000        | 141000        | 141000        | 141000        |
    | AFG     |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |  25000 | 200000       | 600000        |      1.2e+06  |      2.52e+06 |      4.67e+06 |      7.9e+06  |      1.05e+07 |      1.02e+07 |      1.38e+07 |      1.53e+07 |      1.68e+07 |      1.84e+07 |      1.97e+07 |      2.16e+07 |      2.39e+07 |      2.2e+07  |      2.26e+07 |
    | AGO     |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |    110 |    182 |    199 |    330 |    705 |    982 |  24000 |  25800 |  75000 | 140000 | 350000       | 740000        |      1.61e+06 |      3.05e+06 |      4.96e+06 |      6.77e+06 |      8.11e+06 |      9.4e+06  |      1.21e+07 |      1.28e+07 |      1.33e+07 |      1.41e+07 |      1.39e+07 |      1.3e+07  |      1.33e+07 |      1.33e+07 |      1.48e+07 |
    | ALB     |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |    230 |    330 |    560 |  11000 |  29800 | 393000 | 851000 |      1.1e+06 |      1.26e+06 |      1.53e+06 |      1.91e+06 |      2.32e+06 |      1.86e+06 |      2.46e+06 |      2.69e+06 |      3.1e+06  |      3.5e+06  |      3.69e+06 |      3.36e+06 |      3.4e+06  |      3.37e+06 |      3.63e+06 |      2.71e+06 |      2.63e+06 |
    | AND     |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |      0 |     77 |     78 |     78 |    283 |    549 |    862 |  14100 |  20600 |  23500 |  29400 |  32800 |  51900       |  58400        |  64600        |  69000        |  63500        |  64200        |  64500        |  65500        |  65000        |  63900        |  63900        |  66200        |  71300        |  76100        |  80300        |  82600        |  87900        |
    

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
```

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
```


```python
# Then we pivot the table which are combined with the informations from the three tables. 
pivoted = combined_data1.pivot(index = "year", columns = "country", values = "num_per_cap")
```


```python
#pivoted_sorted = pivoted.sort_values(by = "num_per_cap", axis=0, ascending=False, inplace=False, kind="quicksort", na_position="last")
sorted_countries = pivoted.loc["2019"].sort_values(ascending=False)
sorted_table = pd.DataFrame(sorted_countries).transpose()
print(sorted_table.head().to_markdown(index=False))
```

    |   Macao |   Hong Kong |   United Arab Emirates |   Antigua and Barbuda |   Seychelles |   Montenegro |   Thailand |   South Africa |   Lithuania |   Russian Federation |   Sint Maarten (Dutch part) |   Kuwait |   Maldives |   Saint Kitts and Nevis |   Costa Rica |   Botswana |   Singapore |   Turkmenistan |   Mauritius |   Philippines |   El Salvador |   Estonia |   Cayman Islands |   Georgia |   Côte d'Ivoire |   Japan |   Trinidad and Tobago |   Viet Nam |   Qatar |   Uruguay |   Sri Lanka |   Kazakhstan |    Oman |   Poland |   Mongolia |   Iran (Islamic Republic of) |   Malaysia |   Nepal |   Slovakia |   Suriname |   Moldova, Republic of |   Italy |   Luxembourg |   Kyrgyzstan |   Korea, Republic of |   Aruba |   Panama |   Palau |   Colombia |   Cambodia |   Chile |   Virgin Islands (British) |   Gabon |   Argentina |   Ghana |   Finland |   Germany |   Morocco |    Peru |   New Zealand |   United States of America |   Armenia |   Indonesia |   Bahrain |   Brunei Darussalam |   Switzerland |   Gambia |   Malta |   Guatemala |   Netherlands |   Liechtenstein |   Denmark |   Gibraltar |   Czechia |   Sweden |   Ukraine |   Belarus |   China |   Tunisia |   Serbia |   Austria |   Slovenia |   Israel |   United Kingdom of Great Britain and Northern Ireland |   Curaçao |   Spain |   Romania |   Bulgaria |   Portugal |   Greenland |   Saudi Arabia |   Namibia |   Myanmar |   Andorra |   Iceland |   Paraguay |   San Marino |    Fiji |   Senegal |   Timor-Leste |   Barbados |   Greece |   Algeria |   Australia |   Faroe Islands |   Bosnia and Herzegovina |   Croatia |   Latvia |   Azerbaijan |   Mauritania |   Jamaica |   Kenya |    Mali |   Norway |   Lesotho |   France |   Dominica |   Tajikistan |   Puerto Rico |   Hungary |   Bahamas |   Ireland |   Saint Lucia |   Cabo Verde |   Bermuda |   French Polynesia |   Cyprus |   Bangladesh |   Guinea |   Belgium |   Uzbekistan |   North Macedonia |   Saint Vincent and the Grenadines |   Brazil |   Burkina Faso |   Bolivia (Plurinational State of) |   Turkey |   Mexico |   Syrian Arab Republic |   New Caledonia |   Bhutan |   Grenada |   Zambia |   Palestine, State of |   Albania |   Canada |   Ecuador |   Libya |   Monaco |   Egypt |   Eswatini |   Congo |    Iraq |   Vanuatu |   Nicaragua |   Zimbabwe |   Nigeria |   Benin |   Sierra Leone |   India |   Nauru |   Dominican Republic |   Cameroon |   Guinea-Bissau |   Guyana |   Tanzania, United Republic of |   Sao Tome and Principe |   Sudan |   Togo |   Virgin Islands (U.S.) |   Rwanda |   Pakistan |   Lebanon |   Jordan |   Comoros |   Honduras |   Solomon Islands |   Belize |   Lao People's Democratic Republic |   Afghanistan |   Tonga |   Samoa |   Turks and Caicos Islands |    Guam |   Haiti |   Venezuela (Bolivarian Republic of) |   Uganda |   Burundi |    Cuba |   Liberia |   Yemen |   Somalia |    Chad |   Malawi |   Mozambique |   Angola |   Kiribati |   Papua New Guinea |   Congo, Democratic Republic of the |   Northern Mariana Islands |   Madagascar |   Equatorial Guinea |   Niger |   Djibouti |   Marshall Islands |   South Sudan |   Ethiopia |     nan |   Central African Republic |   Micronesia (Federated States of) |   Eritrea |   Korea (Democratic People's Republic of) |   Tuvalu |   American Samoa |
    |--------:|------------:|-----------------------:|----------------------:|-------------:|-------------:|-----------:|---------------:|------------:|---------------------:|----------------------------:|---------:|-----------:|------------------------:|-------------:|-----------:|------------:|---------------:|------------:|--------------:|--------------:|----------:|-----------------:|----------:|----------------:|--------:|----------------------:|-----------:|--------:|----------:|------------:|-------------:|--------:|---------:|-----------:|-----------------------------:|-----------:|--------:|-----------:|-----------:|-----------------------:|--------:|-------------:|-------------:|---------------------:|--------:|---------:|--------:|-----------:|-----------:|--------:|---------------------------:|--------:|------------:|--------:|----------:|----------:|----------:|--------:|--------------:|---------------------------:|----------:|------------:|----------:|--------------------:|--------------:|---------:|--------:|------------:|--------------:|----------------:|----------:|------------:|----------:|---------:|----------:|----------:|--------:|----------:|---------:|----------:|-----------:|---------:|-------------------------------------------------------:|----------:|--------:|----------:|-----------:|-----------:|------------:|---------------:|----------:|----------:|----------:|----------:|-----------:|-------------:|--------:|----------:|--------------:|-----------:|---------:|----------:|------------:|----------------:|-------------------------:|----------:|---------:|-------------:|-------------:|----------:|--------:|--------:|---------:|----------:|---------:|-----------:|-------------:|--------------:|----------:|----------:|----------:|--------------:|-------------:|----------:|-------------------:|---------:|-------------:|---------:|----------:|-------------:|------------------:|-----------------------------------:|---------:|---------------:|-----------------------------------:|---------:|---------:|-----------------------:|----------------:|---------:|----------:|---------:|----------------------:|----------:|---------:|----------:|--------:|---------:|--------:|-----------:|--------:|--------:|----------:|------------:|-----------:|----------:|--------:|---------------:|--------:|--------:|---------------------:|-----------:|----------------:|---------:|-------------------------------:|------------------------:|--------:|-------:|------------------------:|---------:|-----------:|----------:|---------:|----------:|-----------:|------------------:|---------:|-----------------------------------:|--------------:|--------:|--------:|---------------------------:|--------:|--------:|-------------------------------------:|---------:|----------:|--------:|----------:|--------:|----------:|--------:|---------:|-------------:|---------:|-----------:|-------------------:|------------------------------------:|---------------------------:|-------------:|--------------------:|--------:|-----------:|-------------------:|--------------:|-----------:|--------:|---------------------------:|-----------------------------------:|----------:|------------------------------------------:|---------:|-----------------:|
    | 3.28485 |     2.86365 |                2.12774 |               1.99746 |       1.9872 |      1.84879 |    1.82308 |        1.66991 |     1.66778 |              1.66198 |                     1.65353 |  1.65049 |     1.6412 |                 1.61175 |      1.60683 |    1.60019 |     1.59199 |        1.52312 |     1.51661 |       1.51294 |       1.50154 |   1.46959 |          1.46218 |   1.44617 |         1.43034 | 1.42143 |               1.42109 |    1.41997 | 1.39639 |   1.39423 |     1.38972 |      1.38816 | 1.38612 |  1.37757 |    1.36739 |                      1.36315 |    1.35959 | 1.35958 |    1.35677 |    1.35599 |                1.35499 | 1.34943 |      1.34838 |      1.33515 |              1.33102 | 1.32467 |  1.32309 | 1.32284 |    1.32105 |    1.32036 | 1.31831 |                    1.30676 | 1.30641 |       1.304 | 1.29749 |   1.29491 |   1.28771 |   1.28635 | 1.28561 |       1.28535 |                    1.28529 |   1.28341 |     1.27975 |   1.27159 |             1.27155 |        1.2711 |   1.2675 | 1.25977 |     1.25873 |       1.25686 |         1.25513 |   1.24518 |      1.2391 |    1.2369 |  1.23554 |   1.23462 |   1.23145 | 1.22892 |   1.22829 |  1.21666 |   1.20497 |    1.20189 |  1.19284 |                                                1.18947 |   1.18139 | 1.17323 |   1.17182 |    1.16546 |    1.15688 |     1.15429 |        1.15275 |    1.1526 |   1.15196 |   1.15138 |   1.14543 |    1.13935 |      1.13523 | 1.12144 |    1.1187 |       1.11681 |    1.11357 |  1.10991 |   1.10291 |     1.10102 |         1.09905 |                  1.09798 |   1.08234 |  1.08161 |      1.07738 |       1.0744 |   1.07329 | 1.07161 | 1.06965 |  1.06958 |   1.06933 |  1.06844 |    1.06121 |       1.0603 |        1.0552 |   1.05412 |     1.053 |   1.04371 |       1.03593 |      1.03288 |   1.01704 |            1.01095 |  1.00909 |      1.00292 |  1.00174 |   1.00096 |      0.99463 |           0.99196 |                            0.98166 |  0.97742 |        0.97367 |                            0.96796 |  0.96788 |  0.96734 |                0.96526 |         0.95856 |  0.94989 |   0.94521 |  0.93578 |               0.93484 |   0.92145 |  0.92018 |   0.91676 | 0.91641 |  0.91267 |  0.9023 |    0.89773 | 0.89755 | 0.87817 |   0.87055 |     0.86886 |    0.85968 |   0.85094 | 0.83805 |        0.83635 | 0.83146 | 0.82427 |              0.82247 |    0.82227 |         0.80692 |    0.805 |                        0.79669 |                 0.77354 | 0.76332 |  0.757 |                  0.7528 |  0.75263 |    0.73894 |   0.73332 |  0.72719 |   0.72694 |    0.71193 |           0.70816 |  0.65537 |                            0.60454 |       0.59837 |  0.5917 | 0.58517 |                    0.58264 | 0.58117 | 0.57345 |                              0.57297 |  0.57044 |   0.55917 | 0.53372 |   0.53357 |   0.485 |   0.47868 | 0.47498 |  0.47171 |      0.46557 |  0.45745 |    0.44027 |            0.42127 |                             0.41265 |                    0.41117 |      0.38862 |             0.38055 | 0.37452 |    0.36779 |            0.35772 |       0.35127 |      0.347 | 0.31416 |                     0.2937 |                             0.2074 |   0.19864 |                                   0.14793 |  0.07302 |          0.00475 |
    


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


    
![png](output_15_0.png)
    



```python

```
