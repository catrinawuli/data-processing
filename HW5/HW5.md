```python
import pandas as pd
import sqlite3 as sql
import re
```

##  SQL


```python
# create a connection with the database and create cursor that facilitates the interaction with the database
conn = sql.connect(r"C:\Users\catri\OneDrive\文档\LIWU\HW5\user_actions.db")
cur = conn.cursor()

# Query to retrieve the list of tables and their schema to know more about the database
tables = list(cur.execute("SELECT name FROM sqlite_master WHERE type='table';"))
#print(tables)

schema = list(cur.execute("SELECT name, sql FROM sqlite_master WHERE type='table';"))
#print(schema)
```


```python
# Query 1: Retrieve the usernames of all users who have performed the "signup" action.

query = "SELECT username FROM user_actions WHERE action = 'signup';"
user_name_signup = list(cur.execute(query))
#user_name_signup
```


```python
# Query 2: Find the total number of log entries for each user. Display the user_id, username, 
#and the count of log entries.
query = "SELECT user_id, username,COUNT(*) FROM user_actions GROUP BY user_id;" 
user_name_logEntries = list(cur.execute(query))
#user_name_logEntries
```


```python
# Query 3: Identify users who have both logged in (action = 'login') and signed up (action = 'signup') on the same day. 
# Display the user_id and username.
query = "SELECT user_id, username FROM user_actions WHERE action IN ('login', 'signup') GROUP BY user_id, username, DATE(timestamp) HAVING COUNT(DISTINCT action) = 2 ;" 
query3 = list(cur.execute(query))
#query3
```

## Regex


```python
# Task 1: Write a regular expression to extract all hashtags from a given comment. 
#For example, applying the regex to comment 1 should return ["#programming", "#tips"].

textfile = open("comments.txt", 'r')
matches_flat = []
matches_dict = {}
reg = re.compile(r'(#[\w]+)[\s|"]')
for line_num, line in enumerate(textfile):
    matches = reg.findall(line)
    matches_flat += matches
    matches_dict[line_num] = matches
    line_num += 1
#print(f">>>All hashtags: {matches_flat}")

```


```python
# Task 2: Create a regular expression to find comments that mention both "#programming" and "#python". 
# Apply the regex to comment 2 and check if it matches.

textfile.seek(0)
reg2 = re.compile(r'^(?=.*\#programming\b)(?=.*\#python\b).*$')

for line_num, line in enumerate(textfile):
    line_num += 1
    matches = reg2.findall(line)
    if matches:
        print(line_num, end= " ")

textfile.seek(0)
line2 = textfile.readlines()[1]
#print(">>>Comments contain both #programming and #python is", end = " ")
#print(f"\n>>> comment 2 {'' if reg2.findall(line2) else 'does not'} contains both #programming and #python")
```

    6 15 21 30 
