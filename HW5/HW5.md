```python
import pandas as pd
import sqlite3 as sql
import re
```

##  SQL

In this part, we need to connect Python with SQLite and perform different queries on the database, which is 'useractions.db'. 

Initially, we create a connection and inspect the tables and schema of the database to gain a better understanding before proceeding with any queries.

Upon inspection, we observe that the database contains only one table with six attributes.


```python
# create a connection with the database and create cursor that facilitates the interaction with the database
conn = sql.connect(r"C:\Users\catri\OneDrive\文档\LIWU\HW5\user_actions.db")
cur = conn.cursor()

# Query to retrieve the list of tables and their schema to know more about the database
tables = list(cur.execute("SELECT name FROM sqlite_master WHERE type='table';"))

schema = list(cur.execute("SELECT name, sql FROM sqlite_master WHERE type='table';"))
print(tables)

print(schema)
```

    [('user_actions',)]
    [('user_actions', 'CREATE TABLE user_actions (\n    user_id INTEGER,\n    username TEXT,\n    email TEXT,\n    action TEXT,\n    timestamp TEXT\n)')]
    

#### Query 1: Retrieve the usernames of all users who have performed the "signup" action.
In the printed results below, usernames are visible.


```python
query = "SELECT username FROM user_actions WHERE action = 'signup';"
user_name_signup = list(cur.execute(query))
user_name_signup
```




    [('user34',),
     ('user28',),
     ('user1',),
     ('user24',),
     ('user15',),
     ('user20',),
     ('user18',),
     ('user25',),
     ('user3',),
     ('user9',),
     ('user27',),
     ('user16',),
     ('user17',),
     ('user4',),
     ('user8',),
     ('user13',),
     ('user19',),
     ('user31',),
     ('user10',),
     ('user23',),
     ('user11',),
     ('user33',),
     ('user12',),
     ('user29',),
     ('user21',),
     ('user6',),
     ('user14',),
     ('user30',),
     ('user7',),
     ('user26',),
     ('user22',),
     ('user5',),
     ('user35',),
     ('user2',),
     ('user32',)]



Query 2: Find the total number of log entries for each user. Display the user_id, username, 
and the count of log entries.
We can determine the number of log entries for each user by grouping them based on their user_id and then counting the occurrences.


```python
query = "SELECT user_id, username,COUNT(*) FROM user_actions GROUP BY user_id;" 
user_name_logEntries = list(cur.execute(query))
user_name_logEntries
```




    [(1, 'user1', 104),
     (2, 'user2', 149),
     (3, 'user3', 108),
     (4, 'user4', 436),
     (5, 'user5', 192),
     (6, 'user6', 457),
     (7, 'user7', 362),
     (8, 'user8', 329),
     (9, 'user9', 118),
     (10, 'user10', 170),
     (11, 'user11', 328),
     (12, 'user12', 209),
     (13, 'user13', 470),
     (14, 'user14', 320),
     (15, 'user15', 35),
     (16, 'user16', 379),
     (17, 'user17', 367),
     (18, 'user18', 122),
     (19, 'user19', 58),
     (20, 'user20', 362),
     (21, 'user21', 323),
     (22, 'user22', 275),
     (23, 'user23', 309),
     (24, 'user24', 162),
     (25, 'user25', 136),
     (26, 'user26', 217),
     (27, 'user27', 211),
     (28, 'user28', 91),
     (29, 'user29', 49),
     (30, 'user30', 165),
     (31, 'user31', 389),
     (32, 'user32', 32),
     (33, 'user33', 32),
     (34, 'user34', 180),
     (35, 'user35', 394)]



#### Query 3: Identify users who have both logged in (action = 'login') and signed up (action = 'signup') on the same day. 
At the beginning, we filter the table to include only 'login' or 'signup' actions. We then group the data by user_id and date. By adding a condition that the count of distinct actions is 2, we can identify users who have both 'login' and 'signup' actions on the same day. For instance, 'user2' has both 'login' and 'signup' actions on 2022-11-05.


```python
# here using group by in two attributes and have two conditons one is made by "WHERE", the other is by "HAVING"
query = "SELECT * FROM user_actions WHERE action IN ('login', 'signup') GROUP BY user_id, DATE(timestamp) HAVING COUNT(DISTINCT action) = 2 ;" 
query3 = list(cur.execute(query))
query3
```




    [(2, 'user2', 'user2@email.com', 'signup', '2022-11-05 14:40:57'),
     (5, 'user5', 'user5@email.com', 'signup', '2022-08-19 13:13:23'),
     (7, 'user7', 'user7@email.com', 'signup', '2021-12-19 17:24:55'),
     (8, 'user8', 'user8@email.com', 'signup', '2018-04-15 11:49:50'),
     (12, 'user12', 'user12@email.com', 'signup', '2020-05-07 09:15:09'),
     (22, 'user22', 'user22@email.com', 'signup', '2022-07-27 11:49:56'),
     (30, 'user30', 'user30@email.com', 'signup', '2021-11-15 08:41:50'),
     (32, 'user32', 'user32@email.com', 'signup', '2022-11-28 11:50:55')]



## Regex

#### Task 1:  
For this task, our goal is to write a regular expression to extract all hashtags from a given comment. We will then proceed to extract and print all the hashtags found in the file.


```python
# the regular expression is (#[\w]+)[\s|"]. 
#  \w matches any word character
# \s|" ends with whitespace or " 
textfile = open("comments.txt", 'r')
matches_flat = []
matches_dict = {}
reg = re.compile(r'(#[\w]+)[\s|"]')
for line_num, line in enumerate(textfile):
    matches = reg.findall(line)
    matches_flat += matches
    matches_dict[line_num] = matches
    line_num += 1
print(f">>>All hashtags: {matches_flat}")

```

    >>>All hashtags: ['#programming', '#tips', '#coding', '#python', '#tech', '#data', '#programming', '#coding', '#data', '#analysis', '#programming', '#python', '#tips', '#tech', '#innovation', '#coding', '#python', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#data', '#programming', '#coding', '#data', '#insights', '#programming', '#python', '#tips', '#tech', '#research', '#coding', '#python', '#data', '#analytics', '#programming', '#tips', '#coding', '#innovation', '#programming', '#python', '#tips', '#tech', '#data', '#coding', '#python', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#python', '#tips', '#coding', '#innovation', '#tech', '#data', '#programming', '#tips', '#coding', '#python', '#data', '#analysis', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#data', '#analysis', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python', '#tech', '#innovation', '#programming', '#coding', '#data', '#analytics', '#programming', '#tips', '#coding', '#python']
    

#### Task 2: 
For this task, our goal is to write a regular expression that identifies comments mentioning both '#programming' and '#python'. We will then apply this regex to comment 2 to verify if it matches. Additionally, we'll check the lines that match the regex and print the results.


```python
# the regex : ^(?=.*\#programming\b)(?=.*\#python\b).*$
# Positive lookahead assertion for the presence of "#programming" with a word boundary.
# (?=.*\#python\b): Another positive lookahead assertion for the presence of "#python" with a word boundary.

textfile.seek(0)
reg2 = re.compile(r'^(?=.*\#programming\b)(?=.*\#python\b).*$')
print(">>>Comments contain both #programming and #python is", end = " ")

for line_num, line in enumerate(textfile):
    line_num += 1
    matches = reg2.findall(line)
    if matches:
        print(line_num, end= " ")

textfile.seek(0)
line2 = textfile.readlines()[1]

print(f"\n>>> comment 2 {'' if reg2.findall(line2) else 'does not'} contains both #programming and #python")
```

    >>>Comments contain both #programming and #python is 6 15 21 30 
    >>> comment 2 does not contains both #programming and #python
    
