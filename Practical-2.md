## **🚀 Practical 2 Subquery-join operations on Relational Schema**

## Q.1. USING (practical 1) 

> ### **Q.1) Count the customers with grades above New York’s average.**
```
mysql> select count(*) as Average_Count from customer where grade > (select avg(grade) from customer where city = "New York");
+---------------+
| Average_Count |
+---------------+
|             5 |
+---------------+
1 row in set (0.00 sec)
```

> ### **Q.2) Find the name and numbers of all salesmen who had more than one customer.**
```
mysql> select salesman_id,name from salesman a where 1<(select count(*) from
    -> customer where salesman_id=a.salesman_id);
+-------------+------------+
| salesman_id | name       |
+-------------+------------+
| 5001        | James Hoog |
| 5002        | Nail Knite |
+-------------+------------+
2 rows in set (0.01 sec)

```
> ### **Q.3) Demonstrate the DELETE operation by removing salesman with id 1000. All his orders must also be deleted**
```
mysql> ALTER TABLE orders
    -> ADD CONSTRAINT fk_salesman_id
    -> FOREIGN KEY (salesman_id)
    -> REFERENCES salesman(salesman_id)
    -> ON DELETE CASCADE;
Query OK, 12 rows affected (0.22 sec)
Records: 12  Duplicates: 0  Warnings: 0

mysql> DELETE FROM salesman where salesman_id = 5001;
Query OK, 1 row affected (0.01 sec)
```


## Q.2. Execute the following Queries on it: 

> ### 1)List the titles of all movies directed by ‘Hitchcock’.
```
select mov_title from movies where dir_id =
(select dir_id from director where dir_name = 'HITCHCOCK') 
```                 

> ### 2)Find the movie names where one or more actors acted in two or more movies.
```
select m.mov_title
    from movies m
    natural join movie_cast mc
     where act_id in (select act_id
     from movie_cast
     group by act_id
     having count(act_id)>1)
     group by mov_title
     having count(*) > 1;
```

> ### 3)List all actors who acted in a movie before 2000 and also in a movie after 2020 (use JOIN operation).
```
select distinct act_name
from (actor join movie_cast using(act_id)) join movies using(mov_id)
where mov_year not between 2000 and 2015;
```

> ### 4)Find the title of movies and number of stars for each movie that has at least one rating and find the highest number of stars that movie received. Sort the result by movie title
```
select mov_title , max(rev_stars)
from movies natural join rating
group by mov_title
order by mov_title;
```

> ### 5) Update rating of all movies directed by ‘Steven Spielberg’ to 5.
```
update rating set rev_stars = 5
where mov_id in ( select mov_id 
from director natural join movies
where dir_name = 'STEVEN SPIELBERG'
 );
```

## Q.3. Execute the following Queries on the given database

- *The Database has the following tables as below:* 
```javascript
mysql> show tables;
+-----------------+
| Tables_in_prac2 |
+-----------------+
| advising        |
| courses         |
| grades          |
| instructors     |
| students        |
+-----------------+
5 rows in set (0.00 sec)

mysql> desc students;
+-------+--------------+------+-----+---------+-------+
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| stno  | int          | NO   | PRI | NULL    |       |
| name  | varchar(50)  | YES  |     | NULL    |       |
| addr  | varchar(255) | YES  |     | NULL    |       |
| city  | varchar(50)  | YES  |     | NULL    |       |
| state | varchar(2)   | YES  |     | NULL    |       |
| zip   | varchar(10)  | YES  |     | NULL    |       |
+-------+--------------+------+-----+---------+-------+
6 rows in set (0.03 sec)

mysql> desc grades;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| stno  | int         | YES  | MUL | NULL    |       |
| empno | int         | YES  | MUL | NULL    |       |
| cno   | varchar(50) | YES  | MUL | NULL    |       |
| sem   | varchar(10) | YES  |     | NULL    |       |
| year  | int         | YES  |     | NULL    |       |
| grade | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
6 rows in set (0.00 sec)

mysql> desc courses;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| cno   | varchar(50) | NO   | PRI | NULL    |       |
| cname | varchar(50) | YES  |     | NULL    |       |
| cr    | int         | YES  |     | NULL    |       |
| cap   | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)

mysql> desc instructors;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| empno  | int         | NO   | PRI | NULL    |       |
| name   | varchar(50) | YES  |     | NULL    |       |
| ranks  | varchar(20) | YES  |     | NULL    |       |
| roomno | varchar(10) | YES  |     | NULL    |       |
| telno  | varchar(15) | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

mysql> desc advising;
+-------+------+------+-----+---------+-------+
| Field | Type | Null | Key | Default | Extra |
+-------+------+------+-----+---------+-------+
| stno  | int  | NO   | PRI | NULL    |       |
| empno | int  | NO   | PRI | NULL    |       |
+-------+------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

> ### 1) Find the names of students who took some four-credit courses. 
```javascript
mysql> SELECT DISTINCT s.name
    -> FROM students s
    -> JOIN grades g ON s.stno = g.stno
    -> JOIN courses c ON g.cno = c.cno
    -> WHERE c.cr = 4;
+------------------+
| name             |
+------------------+
| edwards p. david |
| Mixon Leatha     |
| Pierce Richard   |
| Rawlings Jerry   |
| Prior Lorraine   |
| Lewis Jerry      |
+------------------+
6 rows in set (0.01 sec)
```

> ### 2) Find the names of students who took every four-credit course. 
```javascript
mysql> SELECT s.name FROM students s
    -> WHERE NOT EXISTS (
    ->     SELECT c.cno FROM courses c
    ->     WHERE c.cr = 4
    ->          AND NOT EXISTS (
    ->         SELECT * FROM grades g
    ->         WHERE g.stno = s.stno
    ->         AND g.cno = c.cno
    ->     )
    -> );
+------------------+
| name             |
+------------------+
| edwards p. david |
| Mixon Leatha     |
| Pierce Richard   |
| Prior Lorraine   |
+------------------+
4 rows in set (0.00 sec)
```

> ### 4) Find the names of students who took cs210 and cs310. 
```javascript
mysql> SELECT DISTINCT s.name
    -> FROM students s
    -> JOIN grades g ON s.stno = g.stno
    -> WHERE g.cno IN ('cs210', 'cs310');
+------------------+
| name             |
+------------------+
| edwards p. david |
| Mixon Leatha     |
| Pierce Richard   |
| Prior Lorraine   |
| Lewis Jerry      |
+------------------+
5 rows in set (0.00 sec)
```

> ### 5) Find the names of all students whose advisor is not a full professor.
```javascript
mysql> SELECT DISTINCT s.name
    -> FROM students s
    -> JOIN advising a ON s.stno = a.stno
    -> JOIN instructors i ON a.empno = i.empno
    -> WHERE i.ranks != 'Full Professor';
+------------------+
| name             |
+------------------+
| edwards p. david |
| Grogan A. Mary   |
| Mixon Leatha     |
| McLane Sandy     |
| Rawlings Jerry   |
| Novak Roland     |
| Pierce Richard   |
| Prior Lorraine   |
| Lewis Jerry      |
+------------------+
9 rows in set (0.00 sec)
```

> ### 7) Find course numbers for courses that enroll exactly two students;  
```javascript
mysql> SELECT g.cno
    -> FROM grades g
    -> GROUP BY g.cno
    -> HAVING COUNT(g.stno) = 2;
+-------+
| cno   |
+-------+
| cs310 |
| cs410 |
+-------+
2 rows in set (0.00 sec)
```

> ### 8) Find the names of all students for whom no other student lives in the same city.  
```javascript
mysql> SELECT DISTINCT s.name
    -> FROM students s
    -> WHERE NOT EXISTS (
    ->     SELECT *
    ->     FROM students s2
    ->     WHERE s2.city = s.city AND s2.stno != s.stno
    -> );
+------------------+
| name             |
+------------------+
| edwards p. david |
| Grogan A. Mary   |
| Novak Roland     |
| Lewis Jerry      |
+------------------+
4 rows in set (0.01 sec)
```

> ### 10) Find the telephone numbers of instructors who teach a course taken by any student who lives in Boston. 
```javascript
mysql> SELECT DISTINCT i.telno
    -> FROM instructors i
    -> JOIN grades g ON i.empno = g.empno
    -> JOIN students s ON g.stno = s.stno
    -> WHERE s.city = 'Boston';
+-------+
| telno |
+-------+
| 9101  |
| 7122  |
| 5110  |
| 7024  |
+-------+
4 rows in set (0.00 sec)
```

> ### 12) Find the names of students who took only one course. 
```javascript
mysql> SELECT s.name
    -> FROM students s
    -> JOIN (
    ->     SELECT stno, COUNT(*) AS course_count
    ->     FROM grades
    ->     GROUP BY stno
    -> ) AS student_courses ON s.stno = student_courses.stno
    -> WHERE student_courses.course_count = 1;
+----------------+
| name           |
+----------------+
| Grogan A. Mary |
| Novak Roland   |
+----------------+
2 rows in set (0.00 sec)
```

> ### 13) Find the names of instructors who teach no course. 
```javascript
mysql> SELECT i.name
    -> FROM instructors i
    -> LEFT JOIN grades g ON i.empno = g.empno
    -> WHERE g.empno IS NULL;
+---------------+
| name          |
+---------------+
| Davis William |
+---------------+
1 row in set (0.00 sec)
```
> ### 14) Find the names of the instructors who taught only one course during the spring semester of 2001. 
```javascript
mysql> SELECT i.name
    -> FROM instructors i
    -> JOIN (
    ->     SELECT empno, COUNT(*) AS course_count
    ->     FROM grades
    ->     WHERE sem = 'spring' AND year = 2001
    ->     GROUP BY empno
    -> ) AS instructor_courses ON i.empno = instructor_courses.empno
    -> WHERE instructor_courses.course_count = 1;
Empty set (0.01 sec)
```
