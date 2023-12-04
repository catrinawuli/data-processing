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
print(print_table.head().to_markdown())
```

    |     | iso-3   |     2015 |     2016 |     2017 |     2018 |     2019 |
    |----:|:--------|---------:|---------:|---------:|---------:|---------:|
    |  35 | CHN     | 1.29e+09 | 1.36e+09 | 1.47e+09 | 1.65e+09 | 1.73e+09 |
    |  87 | IND     | 1e+09    | 1.13e+09 | 1.17e+09 | 1.18e+09 | 1.15e+09 |
    | 200 | USA     | 3.82e+08 | 3.96e+08 | 4e+08    | 4.22e+08 | 4.22e+08 |
    |  86 | IDN     | 3.39e+08 | 3.86e+08 | 4.35e+08 | 3.19e+08 | 3.45e+08 |
    |  26 | BRA     | 2.58e+08 | 2.44e+08 | 2.18e+08 | 2.07e+08 | 2.07e+08 |
    

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
print(print_table_pop.head().to_markdown())
```

    |    | iso-3   |   year |   n_cellphones |      population |
    |---:|:--------|-------:|---------------:|----------------:|
    |  0 | ABW     |   1980 |              0 | 62267           |
    |  1 | AFG     |   1980 |              0 |     1.24866e+07 |
    |  2 | AGO     |   1980 |              0 |     8.33005e+06 |
    |  3 | ALB     |   1980 |              0 |     2.672e+06   |
    |  4 | AND     |   1980 |              0 | 35611           |
    

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
#sorted_countries.tail()
print(sorted_countries.tail().to_markdown(index=False))
```

    |   Macao |   Hong Kong |   United Arab Emirates |   Antigua and Barbuda |   Seychelles |   Montenegro |   Thailand |   South Africa |   Lithuania |   Russian Federation |   Sint Maarten (Dutch part) |   Kuwait |   Maldives |   Saint Kitts and Nevis |   Costa Rica |   Botswana |   Singapore |   Turkmenistan |   Mauritius |   Philippines |   El Salvador |   Estonia |   Cayman Islands |   Georgia |   Côte d'Ivoire |   Japan |   Trinidad and Tobago |   Viet Nam |   Qatar |   Uruguay |   Sri Lanka |   Kazakhstan |    Oman |   Poland |   Mongolia |   Iran (Islamic Republic of) |   Malaysia |   Nepal |   Slovakia |   Suriname |   Moldova, Republic of |   Italy |   Luxembourg |   Kyrgyzstan |   Korea, Republic of |   Aruba |   Panama |   Palau |   Colombia |   Cambodia |   Chile |   Virgin Islands (British) |   Gabon |   Argentina |   Ghana |   Finland |   Germany |   Morocco |    Peru |   New Zealand |   United States of America |   Armenia |   Indonesia |   Bahrain |   Brunei Darussalam |   Switzerland |   Gambia |   Malta |   Guatemala |   Netherlands |   Liechtenstein |   Denmark |   Gibraltar |   Czechia |   Sweden |   Ukraine |   Belarus |   China |   Tunisia |   Serbia |   Austria |   Slovenia |   Israel |   United Kingdom of Great Britain and Northern Ireland |   Curaçao |   Spain |   Romania |   Bulgaria |   Portugal |   Greenland |   Saudi Arabia |   Namibia |   Myanmar |   Andorra |   Iceland |   Paraguay |   San Marino |    Fiji |   Senegal |   Timor-Leste |   Barbados |   Greece |   Algeria |   Australia |   Faroe Islands |   Bosnia and Herzegovina |   Croatia |   Latvia |   Azerbaijan |   Mauritania |   Jamaica |   Kenya |    Mali |   Norway |   Lesotho |   France |   Dominica |   Tajikistan |   Puerto Rico |   Hungary |   Bahamas |   Ireland |   Saint Lucia |   Cabo Verde |   Bermuda |   French Polynesia |   Cyprus |   Bangladesh |   Guinea |   Belgium |   Uzbekistan |   North Macedonia |   Saint Vincent and the Grenadines |   Brazil |   Burkina Faso |   Bolivia (Plurinational State of) |   Turkey |   Mexico |   Syrian Arab Republic |   New Caledonia |   Bhutan |   Grenada |   Zambia |   Palestine, State of |   Albania |   Canada |   Ecuador |   Libya |   Monaco |   Egypt |   Eswatini |   Congo |    Iraq |   Vanuatu |   Nicaragua |   Zimbabwe |   Nigeria |   Benin |   Sierra Leone |   India |   Nauru |   Dominican Republic |   Cameroon |   Guinea-Bissau |   Guyana |   Tanzania, United Republic of |   Sao Tome and Principe |   Sudan |    Togo |   Virgin Islands (U.S.) |   Rwanda |   Pakistan |   Lebanon |   Jordan |   Comoros |   Honduras |   Solomon Islands |   Belize |   Lao People's Democratic Republic |   Afghanistan |   Tonga |   Samoa |   Turks and Caicos Islands |    Guam |   Haiti |   Venezuela (Bolivarian Republic of) |   Uganda |   Burundi |    Cuba |   Liberia |   Yemen |   Somalia |    Chad |   Malawi |   Mozambique |   Angola |   Kiribati |   Papua New Guinea |   Congo, Democratic Republic of the |   Northern Mariana Islands |   Madagascar |   Equatorial Guinea |   Niger |   Djibouti |   Marshall Islands |   South Sudan |   Ethiopia |     nan |   Central African Republic |   Micronesia (Federated States of) |   Eritrea |   Korea (Democratic People's Republic of) |   Tuvalu |   American Samoa |
    |--------:|------------:|-----------------------:|----------------------:|-------------:|-------------:|-----------:|---------------:|------------:|---------------------:|----------------------------:|---------:|-----------:|------------------------:|-------------:|-----------:|------------:|---------------:|------------:|--------------:|--------------:|----------:|-----------------:|----------:|----------------:|--------:|----------------------:|-----------:|--------:|----------:|------------:|-------------:|--------:|---------:|-----------:|-----------------------------:|-----------:|--------:|-----------:|-----------:|-----------------------:|--------:|-------------:|-------------:|---------------------:|--------:|---------:|--------:|-----------:|-----------:|--------:|---------------------------:|--------:|------------:|--------:|----------:|----------:|----------:|--------:|--------------:|---------------------------:|----------:|------------:|----------:|--------------------:|--------------:|---------:|--------:|------------:|--------------:|----------------:|----------:|------------:|----------:|---------:|----------:|----------:|--------:|----------:|---------:|----------:|-----------:|---------:|-------------------------------------------------------:|----------:|--------:|----------:|-----------:|-----------:|------------:|---------------:|----------:|----------:|----------:|----------:|-----------:|-------------:|--------:|----------:|--------------:|-----------:|---------:|----------:|------------:|----------------:|-------------------------:|----------:|---------:|-------------:|-------------:|----------:|--------:|--------:|---------:|----------:|---------:|-----------:|-------------:|--------------:|----------:|----------:|----------:|--------------:|-------------:|----------:|-------------------:|---------:|-------------:|---------:|----------:|-------------:|------------------:|-----------------------------------:|---------:|---------------:|-----------------------------------:|---------:|---------:|-----------------------:|----------------:|---------:|----------:|---------:|----------------------:|----------:|---------:|----------:|--------:|---------:|--------:|-----------:|--------:|--------:|----------:|------------:|-----------:|----------:|--------:|---------------:|--------:|--------:|---------------------:|-----------:|----------------:|---------:|-------------------------------:|------------------------:|--------:|--------:|------------------------:|---------:|-----------:|----------:|---------:|----------:|-----------:|------------------:|---------:|-----------------------------------:|--------------:|--------:|--------:|---------------------------:|--------:|--------:|-------------------------------------:|---------:|----------:|--------:|----------:|--------:|----------:|--------:|---------:|-------------:|---------:|-----------:|-------------------:|------------------------------------:|---------------------------:|-------------:|--------------------:|--------:|-----------:|-------------------:|--------------:|-----------:|--------:|---------------------------:|-----------------------------------:|----------:|------------------------------------------:|---------:|-----------------:|
    | 3.08823 |     2.2904  |                2.00742 |               1.95684 |      1.58426 |      1.62338 |    1.46527 |        1.5749  |     1.43894 |              1.57533 |                     1.77205 |  1.95971 |    1.69888 |                 1.52752 |      1.54027 |    1.50965 |     1.4869  |        1.35959 |     1.39364 |       1.14528 |       1.49734 |   1.44442 |          1.5301  |   1.48982 |         1.07642 | 1.26631 |               1.45188 |    1.30164 | 1.54893 |   1.51933 |     1.12014 |      1.49919 | 1.58644 |  1.43472 |    1.0355  |                      0.90719 |    1.41943 | 0.996   |    1.23161 |    1.30327 |                1.30819 | 1.44408 |      1.41677 |      1.27247 |              1.15456 | 1.35243 |  1.45056 | 1.33191 |    1.21605 |    1.3556  | 1.29826 |                    1.94783 | 1.39018 |     1.43281 | 1.21229 |   1.35048 |   1.18012 |   1.24277 | 1.11358 |       1.21491 |                    1.191   |   1.20198 |     1.30842 |   1.85003 |             1.09862 |       1.35227 |  1.20721 | 1.17739 |     1.16268 |       1.22787 |         1.09758 |   1.24571 |     1.27306 |   1.18528 |  1.28582 |   1.34429 |   1.20494 | 0.93488 |   1.26322 |  1.29098 |   1.56201 |    1.13882 |  1.2649  |                                                1.21782 |   1.20268 | 1.10023 |   1.16575 |    1.2803  |    1.12955 |     1.08707 |        1.61222 |   1.1171  |   0.79636 |   0.99378 |   1.16077 |    1.19943 |      1.09026 | 1.05321 |   1.04485 |       1.14446 |    1.20468 |  1.16442 |   1.09248 |     1.08331 |         1.08981 |                  0.97607 |   1.05148 |  1.30972 |      1.10888 |      0.9224  |   1.12366 | 0.80467 | 1.25325 |  1.10049 |   1.01014 |  1.00228 |    1.10418 |      0.996   |       0.92133 |   1.00579 |   0.79196 |   1.04212 |       1.07047 |      1.10655 |   0.58249 |            0.91848 |  0.93491 |      0.83001 |  0.92895 |   1.13534 |      0.69651 |           1.00472 |                            1.06121 |  1.25738 |        0.76931 |                            0.91974 |  0.92409 |  0.89888 |                0.74459 |         0.96489 |  0.90949 |   1.00857 |  0.71392 |               0.82668 |   1.18027 |  0.83467 |   0.7965  | 1.57455 |  0.92492 | 0.96189 |    0.82985 | 1.03073 | 0.88988 |   0.63305 |     1.15264 |    0.90428 |   0.82067 | 0.85248 |        0.77378 | 0.75593 | 0.08404 |              0.84568 |    0.79087 |         0.69316 |  0.71918 |                        0.75557 |                 0.91983 | 0.73092 | 0.65032 |                 0.74551 |  0.75239 |    0.59724 |   0.72825 |  1.45351 |   0.58202 |    0.8661  |           0.6937  |  0.5891  |                            0.54955 |       0.58364 | 0.6926  | 0.59439 |                    0.68696 | 0.58341 | 0.69104 |                              0.95317 |  0.53899 |   0.46611 | 0.29454 |   0.79136 | 0.52601 |   0.4243  | 0.38684 |  0.38786 |      0.74879 |  0.49417 |    0.35131 |            0.41004 |                             0.48057 |                    0.39795 |      0.43057 |             0.3957  | 0.44515 |    0.31205 |            0.3137  |       0.25906 |    0.4128  | 0.31428 |                    0.2573  |                            0.20555 |   0.14222 |                                   0.12828 |  0.06068 |          0.00438 |
    | 3.14351 |     2.39893 |                2.21252 |               1.98754 |      1.60546 |      1.67121 |    1.69955 |        1.46042 |     1.46432 |              1.58651 |                     1.72133 |  1.89225 |    1.78755 |                 1.60291 |      1.68446 |    1.39856 |     1.50875 |        1.46032 |     1.43225 |       1.14422 |       1.54227 |   1.444   |          1.53723 |   1.48357 |         1.13572 | 1.31417 |               1.47686 |    1.29931 | 1.40646 |   1.49981 |     1.20417 |      1.43306 | 1.56205 |  1.39584 |    1.11237 |                      0.96631 |    1.3798  | 1.15214 |    1.2871  |    1.3879  |                1.35203 | 1.4185  |      1.31268 |      1.25175 |              1.19685 | 1.34447 |  1.27659 | 1.33026 |    1.23252 |    1.27363 | 1.28844 |                    1.33495 | 1.41884 |     1.46133 | 1.29592 |   1.31385 |   1.25078 |   1.18209 | 1.18525 |       1.29399 |                    1.22573 |   1.19686 |     1.47413 |   2.12108 |             1.22772 |       1.33758 |  1.22561 | 1.28471 |     1.1562  |       1.22722 |         1.17791 |   1.22032 |     1.26209 |   1.183   |  1.25969 |   1.25987 |   1.20388 | 0.97998 |   1.22372 |  1.28784 |   1.27051 |    1.15736 |  1.24035 |                                                1.20253 |   1.17121 | 1.10791 |   1.1623  |    1.25845 |    1.12344 |     1.08568 |        1.43343 |   1.1449  |   0.9751  |   1.04908 |   1.19843 |    1.19522 |      1.08175 | 1.13244 |   1.03041 |       1.21676 |    1.19146 |  1.15999 |   1.16512 |     1.09959 |         1.10101 |                  0.97673 |   1.05645 |  1.35236 |      1.04532 |      0.89094 |   1.16673 | 0.81429 | 1.08021 |  1.09466 |   1.0635  |  1.01313 |    1.1188  |      1.07732 |       0.95401 |   1.01386 |   0.90915 |   1.02622 |       1.00333 |      1.07809 |   0.98987 |            0.95387 |  0.94333 |      0.85115 |  0.93035 |   1.11195 |      0.7316  |           0.98915 |                            1.06641 |  1.17954 |        0.79894 |                            0.89674 |  0.92694 |  0.92166 |                0.70132 |         0.96218 |  0.93096 |   0.99195 |  0.71566 |               0.84954 |   1.17173 |  0.85296 |   0.84552 | 1.21932 |  0.89828 | 0.98012 |    0.87088 | 1.04496 | 0.86309 |   0.77326 |     1.21298 |    0.89257 |   0.81625 | 0.78951 |        0.83801 | 0.84414 | 0.08656 |              0.82735 |    0.78864 |         0.70317 |  0.77066 |                        0.73527 |                 0.86985 | 0.70599 | 0.71919 |                 0.74687 |  0.74764 |    0.63693 |   0.68386 |  0.98548 |   0.60839 |    0.82763 |           0.6639  |  0.60711 |                            0.57463 |       0.62362 | 0.75681 | 0.73464 |                    0.65628 | 0.58213 | 0.60669 |                              0.89781 |  0.58841 |   0.49159 | 0.35179 |   0.66297 | 0.56022 |   0.46527 | 0.38307 |  0.41251 |      0.54158 |  0.4459  |    0.43877 |            0.42476 |                             0.3549  |                    0.40092 |      0.3137  |             0.41174 | 0.35704 |    0.33716 |            0.32072 |       0.24399 |    0.48626 | 0.31616 |                    0.25488 |                            0.21287 |   0.15036 |                                   0.14218 |  0.07003 |          0.00446 |
    | 3.2101  |     2.48877 |                2.18343 |               2.01934 |      1.74243 |      1.67102 |    1.72078 |        1.56247 |     1.54151 |              1.57097 |                     1.69567 |  1.73095 |    1.905   |                 1.60929 |      1.77018 |    1.34897 |     1.49316 |        1.57161 |     1.45468 |       1.08677 |       1.51277 |   1.44225 |          1.52089 |   1.47532 |         1.27576 | 1.36251 |               1.37291 |    1.27615 | 1.47137 |   1.49027 |     1.31121 |      1.48023 | 1.52801 |  1.32983 |    1.25645 |                      1.02952 |    1.32288 | 1.28089 |    1.30901 |    1.35476 |                1.36107 | 1.38594 |      1.33146 |      1.36653 |              1.24022 | 1.33727 |  1.28904 | 1.3287  |    1.28641 |    1.17493 | 1.25214 |                    1.46374 | 1.24287 |     1.40539 | 1.21765 |   1.29988 |   1.3308  |   1.23564 | 1.2308  |       1.32957 |                    1.23031 |   1.22374 |     1.64462 |   1.61995 |             1.26663 |       1.31332 |  1.24728 | 1.29274 |     1.24321 |       1.19664 |         1.22463 |   1.23851 |     1.25759 |   1.1893  |  1.24283 |   1.24244 |   1.2052  | 1.05285 |   1.21069 |  1.22777 |   1.23898 |    1.1808  |  1.20505 |                                                1.19742 |   1.16747 | 1.12677 |   1.14352 |    1.20549 |    1.1456  |     1.10731 |        1.17568 |   1.13342 |   0.91607 |   1.08753 |   1.16773 |    1.17538 |      1.11581 | 1.12076 |   1.04237 |       1.25479 |    1.182   |  1.19948 |   1.11337 |     1.08562 |         1.12682 |                  0.99999 |   1.04739 |  1.26657 |      1.02496 |      0.97836 |   1.10028 | 0.87439 | 1.13923 |  1.08396 |   1.09646 |  1.03111 |    1.06814 |      1.10918 |       1.01946 |   1.01655 |   0.88717 |   1.01926 |       0.99908 |      1.08327 |   1.01764 |            0.97478 |  0.9764  |      0.93329 |  0.95582 |   1.00218 |      0.75026 |           0.96891 |                            1.09902 |  1.04554 |        0.90241 |                            0.96191 |  0.94774 |  0.92804 |                0.81124 |         0.96008 |  0.96678 |   0.93449 |  0.77465 |               0.89791 |   1.26329 |  0.86742 |   0.8804  | 0.94383 |  0.89083 | 1.01189 |    0.91194 | 0.9525  | 0.84298 |   0.78556 |     1.26224 |    0.95586 |   0.74937 | 0.75624 |        0.86356 | 0.86398 | 0.85602 |              0.82369 |    0.824   |         0.76071 |  0.84245 |                        0.7109  |                 0.83639 | 0.70305 | 0.77297 |                 0.7485  |  0.72116 |    0.67012 |   0.72022 |  0.94955 |   0.61576 |    0.8549  |           0.72246 |  0.63786 |                            0.53016 |       0.67053 | 1.02452 | 0.59722 |                    0.62996 | 0.58124 | 0.58084 |                              0.80161 |  0.62053 |   0.53068 | 0.40665 |   0.55456 | 0.51275 |   0.479   | 0.42225 |  0.43454 |      0.41653 |  0.44027 |    0.38301 |            0.44104 |                             0.42001 |                    0.40411 |      0.33359 |             0.39016 | 0.4039  |    0.35857 |            0.33908 |       0.26177 |    0.366   | 0.31379 |                    0.23615 |                            0.20918 |   0.2046  |                                   0.14932 |  0.07388 |          0.00455 |
    | 3.34874 |     2.67021 |                2.19908 |               2.00816 |      1.8499  |      1.81606 |    1.7574  |        1.61145 |     1.63838 |              1.58502 |                     1.68236 |  1.64459 |    1.75189 |                 1.6101  |      1.68626 |    1.3788  |     1.51986 |        1.54656 |     1.51709 |       1.24345 |       1.50247 |   1.45237 |          1.49035 |   1.46516 |         1.3258  | 1.41944 |               1.30922 |    1.48555 | 1.42768 |   1.50859 |     1.39825 |      1.42807 | 1.39965 |  1.34563 |    1.33376 |                      1.036   |    1.30867 | 1.37511 |    1.32923 |    1.2666  |                1.34456 | 1.37864 |      1.31425 |      1.3823  |              1.28719 | 1.33067 |  1.37327 | 1.32669 |    1.30893 |    1.21059 | 1.34749 |                    1.31861 | 1.33667 |     1.31702 | 1.32488 |   1.29634 |   1.30268 |   1.24417 | 1.3104  |       1.30596 |                    1.29116 |   1.26209 |     1.19446 |   1.40519 |             1.30332 |       1.26845 |  1.30066 | 1.27107 |     1.25406 |       1.22449 |         1.23884 |   1.24619 |     1.25582 |   1.19474 |  1.2383  |   1.20791 |   1.22897 | 1.17625 |   1.24025 |  1.20729 |   1.24427 |    1.191   |  1.20458 |                                                1.1962  |   1.16734 | 1.15818 |   1.16052 |    1.1943  |    1.15716 |     1.11561 |        1.17939 |   1.14728 |   1.16014 |   1.10114 |   1.16523 |    1.15468 |      1.12718 | 1.12079 |   1.06582 |       1.16496 |    1.17631 |  1.13669 |   1.12577 |     1.10548 |         1.11471 |                  1.01761 |   1.07392 |  1.07411 |      1.03624 |      1.07008 |   1.05269 | 0.99093 | 1.10363 |  1.07682 |   1.08279 |  1.04827 |    1.07027 |      1.08456 |       1.04279 |   1.02296 |   0.96789 |   1.0211  |       1.03998 |      1.06792 |   1.01693 |            1.01812 |  0.98455 |      0.98971 |  0.9558  |   0.99763 |      0.70397 |           0.94884 |                            1.00683 |  0.98493 |        0.94642 |                            0.98217 |  0.96728 |  0.96763 |                0.88448 |         0.95881 |  0.92377 |   0.95208 |  0.86903 |               0.95862 |   0.94544 |  0.89572 |   0.92856 | 0.92933 |  0.88309 | 0.90418 |    0.90484 | 0.91894 | 0.89922 |   0.84427 |     1.13204 |    0.85702 |   0.87203 | 0.79225 |        0.80903 | 0.86194 | 0.83864 |              0.83043 |    0.73774 |         0.76885 |  0.81857 |                        0.74883 |                 0.77125 | 0.71668 | 0.76305 |                 0.75046 |  0.77403 |    0.70086 |   0.74275 |  0.83462 |   0.64278 |    0.77506 |           0.73113 |  0.64387 |                            0.51513 |       0.59967 | 1.0271  | 0.59132 |                    0.60501 | 0.58099 | 0.58116 |                              0.69403 |  0.59014 |   0.54988 | 0.47404 |   0.54404 | 0.49691 |   0.4964  | 0.44732 |  0.38546 |      0.4792  |  0.42528 |    0.43595 |            0.4309  |                             0.41912 |                    0.40752 |      0.39856 |             0.39345 | 0.38889 |    0.37363 |            0.34791 |       0.35304 |    0.35634 | 0.31273 |                    0.25124 |                            0.20824 |   0.20172 |                                   0.14861 |  0.07363 |          0.00465 |
    | 3.28485 |     2.86365 |                2.12774 |               1.99746 |      1.9872  |      1.84879 |    1.82308 |        1.66991 |     1.66778 |              1.66198 |                     1.65353 |  1.65049 |    1.6412  |                 1.61175 |      1.60683 |    1.60019 |     1.59199 |        1.52312 |     1.51661 |       1.51294 |       1.50154 |   1.46959 |          1.46218 |   1.44617 |         1.43034 | 1.42143 |               1.42109 |    1.41997 | 1.39639 |   1.39423 |     1.38972 |      1.38816 | 1.38612 |  1.37757 |    1.36739 |                      1.36315 |    1.35959 | 1.35958 |    1.35677 |    1.35599 |                1.35499 | 1.34943 |      1.34838 |      1.33515 |              1.33102 | 1.32467 |  1.32309 | 1.32284 |    1.32105 |    1.32036 | 1.31831 |                    1.30676 | 1.30641 |     1.304   | 1.29749 |   1.29491 |   1.28771 |   1.28635 | 1.28561 |       1.28535 |                    1.28529 |   1.28341 |     1.27975 |   1.27159 |             1.27155 |       1.2711  |  1.2675  | 1.25977 |     1.25873 |       1.25686 |         1.25513 |   1.24518 |     1.2391  |   1.2369  |  1.23554 |   1.23462 |   1.23145 | 1.22892 |   1.22829 |  1.21666 |   1.20497 |    1.20189 |  1.19284 |                                                1.18947 |   1.18139 | 1.17323 |   1.17182 |    1.16546 |    1.15688 |     1.15429 |        1.15275 |   1.1526  |   1.15196 |   1.15138 |   1.14543 |    1.13935 |      1.13523 | 1.12144 |   1.1187  |       1.11681 |    1.11357 |  1.10991 |   1.10291 |     1.10102 |         1.09905 |                  1.09798 |   1.08234 |  1.08161 |      1.07738 |      1.0744  |   1.07329 | 1.07161 | 1.06965 |  1.06958 |   1.06933 |  1.06844 |    1.06121 |      1.0603  |       1.0552  |   1.05412 |   1.053   |   1.04371 |       1.03593 |      1.03288 |   1.01704 |            1.01095 |  1.00909 |      1.00292 |  1.00174 |   1.00096 |      0.99463 |           0.99196 |                            0.98166 |  0.97742 |        0.97367 |                            0.96796 |  0.96788 |  0.96734 |                0.96526 |         0.95856 |  0.94989 |   0.94521 |  0.93578 |               0.93484 |   0.92145 |  0.92018 |   0.91676 | 0.91641 |  0.91267 | 0.9023  |    0.89773 | 0.89755 | 0.87817 |   0.87055 |     0.86886 |    0.85968 |   0.85094 | 0.83805 |        0.83635 | 0.83146 | 0.82427 |              0.82247 |    0.82227 |         0.80692 |  0.805   |                        0.79669 |                 0.77354 | 0.76332 | 0.757   |                 0.7528  |  0.75263 |    0.73894 |   0.73332 |  0.72719 |   0.72694 |    0.71193 |           0.70816 |  0.65537 |                            0.60454 |       0.59837 | 0.5917  | 0.58517 |                    0.58264 | 0.58117 | 0.57345 |                              0.57297 |  0.57044 |   0.55917 | 0.53372 |   0.53357 | 0.485   |   0.47868 | 0.47498 |  0.47171 |      0.46557 |  0.45745 |    0.44027 |            0.42127 |                             0.41265 |                    0.41117 |      0.38862 |             0.38055 | 0.37452 |    0.36779 |            0.35772 |       0.35127 |    0.347   | 0.31416 |                    0.2937  |                            0.2074  |   0.19864 |                                   0.14793 |  0.07302 |          0.00475 |
    


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
    

