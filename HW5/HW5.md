# Homework 5


```python
import sqlite3
```


```python
con = sqlite3.connect("user_actions.db")
```


```python
cur = con.cursor()
```

Let's see what we what we have to work with


```python
tables = list(cur.execute("SELECT * FROM sqlite_master WHERE type = 'table';"))
tables
```




    [('table',
      'user_actions',
      'user_actions',
      2,
      'CREATE TABLE user_actions (\n    user_id INTEGER,\n    username TEXT,\n    email TEXT,\n    action TEXT,\n    timestamp TEXT\n)'),
     ('table',
      'new_table',
      'new_table',
      118,
      'CREATE TABLE new_table(username TEXT,"action" TEXT)'),
     ('table',
      'signups',
      'signups',
      157,
      'CREATE TABLE signups(username TEXT,"action" TEXT,times TEXT)')]



We want to find the usernames of user who performed the sign-up operation


```python
list(cur.execute("SELECT username , action FROM user_actions WHERE action = 'signup'"))
```




    [('user34', 'signup'),
     ('user28', 'signup'),
     ('user1', 'signup'),
     ('user24', 'signup'),
     ('user15', 'signup'),
     ('user20', 'signup'),
     ('user18', 'signup'),
     ('user25', 'signup'),
     ('user3', 'signup'),
     ('user9', 'signup'),
     ('user27', 'signup'),
     ('user16', 'signup'),
     ('user17', 'signup'),
     ('user4', 'signup'),
     ('user8', 'signup'),
     ('user13', 'signup'),
     ('user19', 'signup'),
     ('user31', 'signup'),
     ('user10', 'signup'),
     ('user23', 'signup'),
     ('user11', 'signup'),
     ('user33', 'signup'),
     ('user12', 'signup'),
     ('user29', 'signup'),
     ('user21', 'signup'),
     ('user6', 'signup'),
     ('user14', 'signup'),
     ('user30', 'signup'),
     ('user7', 'signup'),
     ('user26', 'signup'),
     ('user22', 'signup'),
     ('user5', 'signup'),
     ('user35', 'signup'),
     ('user2', 'signup'),
     ('user32', 'signup')]



We now want to find the log count for each user along with username and user id


```python
res = cur.execute("SELECT COUNT(user_id), user_id, username FROM user_actions GROUP BY user_id;")
```


```python
res.fetchall()
```




    [(104, 1, 'user1'),
     (149, 2, 'user2'),
     (108, 3, 'user3'),
     (436, 4, 'user4'),
     (192, 5, 'user5'),
     (457, 6, 'user6'),
     (362, 7, 'user7'),
     (329, 8, 'user8'),
     (118, 9, 'user9'),
     (170, 10, 'user10'),
     (328, 11, 'user11'),
     (209, 12, 'user12'),
     (470, 13, 'user13'),
     (320, 14, 'user14'),
     (35, 15, 'user15'),
     (379, 16, 'user16'),
     (367, 17, 'user17'),
     (122, 18, 'user18'),
     (58, 19, 'user19'),
     (362, 20, 'user20'),
     (323, 21, 'user21'),
     (275, 22, 'user22'),
     (309, 23, 'user23'),
     (162, 24, 'user24'),
     (136, 25, 'user25'),
     (217, 26, 'user26'),
     (211, 27, 'user27'),
     (91, 28, 'user28'),
     (49, 29, 'user29'),
     (165, 30, 'user30'),
     (389, 31, 'user31'),
     (32, 32, 'user32'),
     (32, 33, 'user33'),
     (180, 34, 'user34'),
     (394, 35, 'user35')]



We now want to identify users who have both logged in and signed up on the same day. I think we can create two tables first table contains all signins and the second only signups then inner join them to single table on the timestamp varible. To join the tables we need to extract the date from the timestamp data with the DATE() function.


```python
cur.execute("CREATE TABLE signups_date_logs AS SELECT username, action, DATE(timestamp) AS date_of_action FROM user_actions WHERE action = 'signup';")
```




    <sqlite3.Cursor at 0x270e5b2e570>




```python
cur.execute("CREATE TABLE login_date_logs AS SELECT username, action, DATE(timestamp) AS date_of_action FROM user_actions WHERE action = 'login';")
```




    []



We can look our tables and see that it looks good


```python
list(cur.execute("SELECT * FROM login_date_logs"))[:5]
```




    [('user27', 'login', '2015-04-17'),
     ('user27', 'login', '2015-04-21'),
     ('user27', 'login', '2015-05-31'),
     ('user27', 'login', '2015-06-25'),
     ('user27', 'login', '2015-07-15')]




```python
list(cur.execute("SELECT * FROM signups_date_logs"))[:5]
```




    [('user34', 'signup', '2015-02-04'),
     ('user28', 'signup', '2015-03-09'),
     ('user1', 'signup', '2015-11-06'),
     ('user24', 'signup', '2015-12-11'),
     ('user15', 'signup', '2016-04-03')]



Now we can join these two tables by date. I Use the distinct function to get rid of possible duplicates the are created when (I assume) a login multiple times the same day they signed up.

We now the list of user who signed up and logged in on the same day.


```python
list(cur.execute("SELECT DISTINCT signups_date_logs.username, signups_date_logs.date_of_action FROM signups_date_logs INNER JOIN login_date_logs ON login_date_logs.date_of_action=signups_date_logs.date_of_action;"))
```




    [('user15', '2016-04-03'),
     ('user25', '2016-06-16'),
     ('user3', '2016-06-28'),
     ('user16', '2017-07-02'),
     ('user4', '2018-01-25'),
     ('user8', '2018-04-15'),
     ('user13', '2019-02-09'),
     ('user19', '2019-04-21'),
     ('user31', '2019-06-14'),
     ('user10', '2019-07-08'),
     ('user23', '2019-09-30'),
     ('user11', '2019-10-27'),
     ('user33', '2020-04-27'),
     ('user12', '2020-05-07'),
     ('user29', '2020-10-03'),
     ('user21', '2021-02-23'),
     ('user6', '2021-06-13'),
     ('user14', '2021-08-12'),
     ('user30', '2021-11-15'),
     ('user7', '2021-12-19'),
     ('user26', '2021-12-31'),
     ('user22', '2022-07-27'),
     ('user5', '2022-08-19'),
     ('user35', '2022-08-26'),
     ('user2', '2022-11-05'),
     ('user32', '2022-11-28')]




```python
import re

comments = open("comments.txt", 'r')
commentstext = comments.read()
comments.close()
matches = re.findall("(#+[a-zA-Z0-9]{1,})", commentstext)#match words starting with "#"
matches
```




    ['#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#data',
     '#programming',
     '#coding',
     '#data',
     '#analysis',
     '#programming',
     '#python',
     '#tips',
     '#tech',
     '#innovation',
     '#coding',
     '#python',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#data',
     '#programming',
     '#coding',
     '#data',
     '#insights',
     '#programming',
     '#python',
     '#tips',
     '#tech',
     '#research',
     '#coding',
     '#python',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#innovation',
     '#programming',
     '#python',
     '#tips',
     '#tech',
     '#data',
     '#coding',
     '#python',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#python',
     '#tips',
     '#coding',
     '#innovation',
     '#tech',
     '#data',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#data',
     '#analysis',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#data',
     '#analysis',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python',
     '#tech',
     '#innovation',
     '#programming',
     '#coding',
     '#data',
     '#analytics',
     '#programming',
     '#tips',
     '#coding',
     '#python']




```python
comments_mention_python = re.findall(("[^.]* #python [^.]*\."), commentstext)#find all comments contain #python
comments_mention_python = "\n".join(comments_mention_python)
re.findall(("[^.]* #programming [^.]*\."),comments_mention_python)#find all comments contain #programming from comments containing #python
```




    [' #programming #python #tips"\n7.',
     '\n #programming #python #tips"\n16.',
     '\n "Love the content! #programming #python #tips"\n22.',
     '\n #programming #python #tips"\n31.']




```python
!jupyter nbconvert --to markdown HW5.ipynb
```

    [NbConvertApp] Converting notebook HW5.ipynb to markdown
    [NbConvertApp] Writing 5686 bytes to HW5.md
    


```python

```
