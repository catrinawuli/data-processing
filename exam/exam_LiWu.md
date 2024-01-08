```python
# pure functions:
""".The function is deterministic because calling it with the same arguments will always produce the same result.
   .The function has no side effects. It doesn't modify any external state or rely on external variables."""

# Immutability
"""Immutability in Python refers to the concept of creating objects whose state cannot be modified after they are created."""
"""All pandas data structures are value-mutable (the values they contain can be altered) but not always size-mutable. The length of a Series cannot be changed, but, for example, columns can be inserted into a DataFrame."""

#higher-order functions
""" higher-order functions are functions that can take other functions as arguments or return functions as results. """

def square(x):
    return x ** 2

numbers = [1, 2, 3, 4]
squared_numbers = list(map(square, numbers))
# Output: [1, 4, 9, 16]
squared_numbers

#· apply：应用在DataFrame的行或列中，也可以应用到单独一个Series的每个元素中，这个运算，map也可以实现。
#· map：应用在单独一个Series的每个元素中
#· applymap：应用在DataFrame的每个元素中

```




    [1, 4, 9, 16]



# Theoretical Part

## 1. Functional Programming (5p)

### The code:

URL = "https://raw.githubusercontent.com/mt4007-ht23/data/main/cell_phones_total.csv"  
data = pd.read_csv(URL)  
unique_values_per_column = data.apply(lambda x: x.unique())  


### Answer:  

About a functional language, I can provide an example from my homework 4 where I used functional programming to find the unique values in each column of a pandas DataFrame as the code above.   
According to the official documentation, series.unique() will return unique values of a Series object in the order of appearance. This function is deterministic because calling it with the same arguments will always produce the same result. Moreover, this function has no side effects; it doesn't modify any external state or rely on external variables. Therefore, series.unique() is a pure function.  
Regarding immutability, Objects passed to the function dataframe.apply() are Series objects whose index is the DataFrame’s index (axis=0) by default which means apply function to each column. And the length of a Series cannot be changed after it is created, so the Series object x here is immutable.  
In the end, we can see that I applied a pure function to a size-immutable object, making the code easy to understand, reason about, and modify if needed.  

## 2. SQL (5p)

### SQL query:  

SELECT StudentID, SUM(Grade * Credits)  AS  the_average_grade  
FROM StudentsGrades  
GROUP BY StudentID;  

### Explaining the significance of grouping data in SQL :  
**Aggregation and Conditional Aggregation:**  
Grouping allows the application of aggregate functions (such as SUM, AVG) on subsets of data instead of the entire dataset. This enables the calculation of summary statistics and metrics for specific groups. Conditional aggregation can also be applied. For instance, in my query, I calculate the weighted sum of Grade and Credits for each student.

**Enhancing Readability:**  
Grouping enhances the readability of query results by organizing data into manageable chunks. This makes it easier to interpret and understand information. As demonstrated in my example above, queries become more straightforward, requiring fewer lines to accomplish complex calculations.

**Performance Optimization:**  
Grouping contributes to performance optimization by enabling the database engine to process and aggregate data more efficiently. It reduces the amount of data that needs to be processed for certain operations. In my example, the sum of the products of Grade and Credits is aggregated separately for each student, minimizing the data processing requirements.

## 3. RegEx (5p)
回头再来做这个，这个肯定很耗时间。

# Practical Part

## 4. Monkeypox (10p)

**List the top 5 countries with highest number of total confirmed cases (ConfCases). Generate the following table. (2p)**  
Here we lised the ...


```python
import pandas as pd

URL = "https://raw.githubusercontent.com/mt4007-ht23/data/main/exam_data/monkeypox.csv"
monkeypox = pd.read_csv(URL)

# for the better understanding of data, we display the first 5 rows of the dataset.
#print(monkeypox.head().to_markdown())

totalConfCases = monkeypox.groupby("CountryExp", as_index=False)["ConfCases"].sum()
#print(totalConfCases.head().to_markdown())
#totalConfCases.info()

print(totalConfCases.sort_values(by = "ConfCases", ascending=False).head().reset_index(drop=True).to_markdown())
```

    |    | CountryExp   |   ConfCases |
    |---:|:-------------|------------:|
    |  0 | Spain        |        4942 |
    |  1 | Germany      |        2887 |
    |  2 | France       |        2423 |
    |  3 | Netherlands  |         959 |
    |  4 | Portugal     |         710 |
    

## 5. Data Storage (10p)

## 6. Algorithmic Trading (10p)


```python
# problem 1
# 做题前思考， unique是个pure function 满足上面的定义。 
# df is not always size-mutable由上面的官方的文档的描述，那么 Functions that operate on immutable data are generally easier to understand, reason about, and modify.
# Adding two spaces at the end of a line creates a line break.

# problem 2
# a weighted sum of Grade and Credits is sum of Grade*Credits（ the grade received in each course is multiplied by the corresponding credit hours for that course）
# When you use the expression quantity * price, SQL will perform the multiplication operation for each row in the table. The result will be a new column (or result set, depending on the context) where each value represents the product of the corresponding quantity and price for that row.
# SUM(quantity * price) adds up all these products for each group (grouped by product_id), giving you the total revenue for each product.
```


```python

```


```python

```


```python

```
