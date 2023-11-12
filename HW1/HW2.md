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

topFive = ApartmentPrice.sort_values("ppsqm", ascending = False).head()
print(topFive.to_markdown(index=False))
```

    |    ppsqm |   listPrice |   rent |   livingArea |   rooms | published           |   constructionYear | objectType   |   booliId | soldDate   |   soldPrice | soldPriceSource   | url                                 |   floor |   additionalArea |   apartmentNumber |   plotArea | location.address.streetAddress   |   location.position.latitude |   location.position.longitude |   location.position.isApproximate | location.region.municipalityName   | location.region.countyName   |   location.distance.ocean | source.name   |   source.id | source.type   | source.url                   |
    |---------:|------------:|-------:|-------------:|--------:|:--------------------|-------------------:|:-------------|----------:|:-----------|------------:|:------------------|:------------------------------------|--------:|-----------------:|------------------:|-----------:|:---------------------------------|-----------------------------:|------------------------------:|----------------------------------:|:-----------------------------------|:-----------------------------|--------------------------:|:--------------|------------:|:--------------|:-----------------------------|
    | 104167   |     1595000 |   1464 |         24   |       1 | 2016-01-07 14:51:06 |               1935 | Lägenhet     |   2018259 | 2016-01-21 |     2500000 | bid               | https://www.booli.se/annons/2018259 |       3 |              nan |              1305 |        nan | Ekhagsvägen 8                    |                      59.3718 |                       18.0553 |                               nan | Stockholm                          | Stockholms län               |                       161 | Notar         |        1566 | Broker        | http://www.notar.se/         |
    | 102083   |     1890000 |   1464 |         24   |       1 | 2016-06-11 08:55:51 |               1935 | Lägenhet     |   2125576 | 2016-06-21 |     2450000 | bid               | https://www.booli.se/annons/2125576 |       3 |              nan |              1305 |        nan | Ekhagsvägen 8                    |                      59.3718 |                       18.0553 |                               nan | Stockholm                          | Stockholms län               |                       161 | HusmanHagberg |        1610 | Broker        | http://www.husmanhagberg.se/ |
    | 100000   |     1795000 |   1374 |         23   |       1 | 2016-04-15 19:10:49 |               1935 | Lägenhet     |   2078171 | 2016-04-28 |     2300000 | bid               | https://www.booli.se/annons/2078171 |       1 |              nan |              1105 |        nan | Ekhagsvägen 10                   |                      59.372  |                       18.0554 |                                 1 | Stockholm                          | Stockholms län               |                       135 | Notar         |        1566 | Broker        | http://www.notar.se/         |
    |  88400   |     1750000 |   1144 |         25   |       1 | 2017-05-18 16:20:57 |               1936 | Lägenhet     |   2330886 | 2017-06-01 |     2210000 | bid               | https://www.booli.se/annons/2330886 |       0 |                0 |               nan |        nan | Torphagsvägen 4                  |                      59.3715 |                       18.0539 |                               nan | Stockholm                          | Stockholms län               |                       218 | HusmanHagberg |        1610 | Broker        | http://www.husmanhagberg.se/ |
    |  85501.9 |     1895000 |   1522 |         26.9 |       1 | 2016-04-01 22:56:37 |               1968 | Lägenhet     |   2067384 | 2016-04-19 |     2300000 | bid               | https://www.booli.se/annons/2067384 |       2 |              nan |              1203 |        nan | Docentbacken 1B                  |                      59.3706 |                       18.0585 |                                 1 | Stockholm                          | Stockholms län               |                       298 | Notar         |        1566 | Broker        | http://www.notar.se/         |
    


```python
# use the 'mean' function to calculate the average ppsqm.
mean =  round(ApartmentPrice["ppsqm"].mean(),2)
print("The avereage price per square meter in Ekhagen is ",mean)
# create a new DataFrame containing only the top five records. 
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
NO_1Percentage = swedishElection2018.sort_values("S", ascending = False).head(1)
print(NO_1Percentage.to_markdown(index=False))
```

    |   LÄNSKOD |   KOMMUNKOD | LÄNSNAMN      | KOMMUNNAMN   |    M |    C |    L |   KD |     S |    V |   MP |    SD |   FI |   AfS |   BASIP |   CSIS |   DD |   DjuP |   EAP |   ENH |   FHS |   Gup |   INI |   KLP |   KrVP |   LPo |   MED |   NMR |   NORRP |   NYREF |   PP |   RNP |   S-FRP |   SKP |   SKÅ |   TRP |   VL-S |   ÖVR |   OGEJ |   BLANK |   OG |   RÖSTER GILTIGA |   RÖSTANDE |   RÖSTBERÄTTIGADE |   VALDELTAGANDE |
    |----------:|------------:|:--------------|:-------------|-----:|-----:|-----:|-----:|------:|-----:|-----:|------:|-----:|------:|--------:|-------:|-----:|-------:|------:|------:|------:|------:|------:|------:|-------:|------:|------:|------:|--------:|--------:|-----:|------:|--------:|------:|------:|------:|-------:|------:|-------:|--------:|-----:|-----------------:|-----------:|------------------:|----------------:|
    |        17 |          62 | Värmlands län | Munkfors     | 8.33 | 8.12 | 2.91 | 3.93 | 52.18 | 6.11 | 1.37 | 15.43 | 0.64 |  0.51 |    0.04 |    nan |  nan |    nan |   nan |  0.04 |   nan |   nan |   nan |  0.04 |    nan |  0.09 |  0.04 |  0.09 |     nan |     nan | 0.09 |   nan |     nan |   nan |  0.04 |   nan |    nan |   nan |    nan |    1.02 |  nan |             2340 |       2364 |              2777 |           85.13 |
    


```python
# task 3: sort the values at a descending order, then select the top 3 records. 
topThreeVALDELTAGANDE = VALDELTAGANDE.sort_values("VALDELTAGANDE", ascending = False).head(3)
print(topThreeVALDELTAGANDE.to_markdown(index=False))
```

    | KOMMUNNAMN   |   VALDELTAGANDE |
    |:-------------|----------------:|
    | Lomma        |           93.86 |
    | Habo         |           93.35 |
    | Vellinge     |           93.13 |
    
