```python
from wordcloud import WordCloud, STOPWORDS
import matplotlib.pyplot as plt
import requests
from bs4 import BeautifulSoup
import pandas as pd
import numpy as np
```

## REST API
The goal of this part of the assignment is to fetch data from the Nobel Prize REST API. First, we retrieve the data in JSON format. Upon examination, we observe that the 'motivation' is enclosed in square brackets, with 'laureates' serving as the title name in the JSON format. We use for loops to extract all the motivations from the JSON list and visualize the frequencies using a word cloud.


```python
# Import the requests package to get the JSON format. 
url = 'https://api.nobelprize.org/2.1/nobelPrizes'
resp = requests.get(url=url)
data = resp.json()
```


```python
# extract all the prize motivations 
motivations = []
for item in data['nobelPrizes']:
    motivations += [laureate['motivation']['en'] for laureate in item['laureates']]
```


```python
#  visualise using a word cloud.
words = ''
for motivation in motivations:
    words += ' ' + motivation.lower() + ' '
wordcloud = WordCloud(width=800, height=800,
                     background_color='white',
                     stopwords=set(STOPWORDS),
                     min_font_size=10).generate(words)

plt.figure(figsize = (8, 8), facecolor = None)
plt.imshow(wordcloud)
plt.axis("off")
plt.tight_layout(pad = 0)
 
plt.show()
```


    
![png](output_4_0.png)
    


## Web Scraping

In this part, we need to scrape data from the first three pages of a specified webpage. Initially, we obtain the links containing all the information we need, such as UPC, title, price, and rating. Next, we retrieve the data from these links and create a dataframe using the gathered information.


```python
# use requests and BeautifulSoup package. 
# get all the links for the books in the first three pages.
base_url = 'https://books.toscrape.com/catalogue/'
links = []
for index in range(1, 4):
    resp = requests.get(base_url+'page-' + str(index) + '.html')
    soup = BeautifulSoup(resp.content, 'html.parser')
    product_links = soup.select('h3 a')
    links += [base_url + link['href'] for link in product_links]
```


```python
#find the needed information in the links and make a dataframe for them. 
results = []
for link in links:
    resp = requests.get(link)
    soup = BeautifulSoup(resp.content, 'html.parser')
    title = soup.find('h1').text.strip()
    price = soup.find('p', {'class': 'price_color'}).text.strip()
    rating = soup.find('p', {'class': 'star-rating'})['class'][1]
    upc_table = soup.find('table', {'class': 'table'})

    if upc_table:
        upc_row = upc_table.select('tr')[0]

        upc_value = upc_row.find('td').text.strip() if upc_row else ''
    results.append([upc_value, title, price, rating])
```


```python
# print the head of the dataframe
df = pd.DataFrame(np.array(results), columns=['upc', 'title', 'price', 'rating'])
print(df.head().to_markdown(index = False))
```

    | upc              | title                                 | price   | rating   |
    |:-----------------|:--------------------------------------|:--------|:---------|
    | a897fe39b1053632 | A Light in the Attic                  | £51.77  | Three    |
    | 90fa61229261140a | Tipping the Velvet                    | £53.74  | One      |
    | 6957f44c3847a760 | Soumission                            | £50.10  | One      |
    | e00eb4fd7b871a48 | Sharp Objects                         | £47.82  | Four     |
    | 4165285e1663650f | Sapiens: A Brief History of Humankind | £54.23  | Five     |
    
