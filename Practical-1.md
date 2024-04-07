## 🚀 **Practical 1 DDL operations on Relational Schema**

### > **Creating Database**
```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
  4 rows in set (0.01 sec)

mysql> create database prac1;
Query OK, 1 row affected (0.11 sec)

mysql> use prac1;
Database changed
```

### > **Creating tables**
```
mysql> CREATE TABLE `salesman` (`salesman_id` int, `name` text,`city` text,`comission` float,PRIMARY KEY (`salesman_id`));
Query OK, 0 rows affected (0.25 sec)

mysql> desc salesman;
+-------------+-------+------+-----+---------+-------+
| Field       | Type  | Null | Key | Default | Extra |
+-------------+-------+------+-----+---------+-------+
| salesman_id | int   | NO   | PRI | NULL    |       |
| name        | text  | YES  |     | NULL    |       |
| city        | text  | YES  |     | NULL    |       |
| comission   | float | YES  |     | NULL    |       |
+-------------+-------+------+-----+---------+-------+
4 rows in set (0.03 sec)
```

### > **Importing data into table using csv**

**Note: keep the file in path as follwos C:/ProgramData/MySQL/MySQL Server 8.3/Uploads/**

```
mysql> load data infile "C:/ProgramData/MySQL/MySQL Server 8.3/Uploads/salesman.csv"
    -> into table salesman
    -> fields terminated by ',' enclosed by '"'
    -> lines terminated by '\n'
    -> ignore 1 rows;
Query OK, 6 rows affected (0.05 sec)
Records: 6  Deleted: 0  Skipped: 0  Warnings: 0
```
**Similarly Create Table for ***Customers & Orders*****

### > **Show tables data**

```
mysql> show tables;
+-----------------+
| Tables_in_prac1 |
+-----------------+
| customer        |
| orders          |
| salesman        |
+-----------------+
3 rows in set (0.00 sec)

mysql> select * from customer;
+-------------+----------------+------------+-------+-------------+
| customer_id | cust_name      | city       | grade | salesman_id |
+-------------+----------------+------------+-------+-------------+
|        3001 | Brad Guzan     | London     |  NULL |        5005 |
|        3002 | Nick Rimando   | New York   |   100 |        5001 |
|        3003 | Jozy Altidor   | Moscow     |   200 |        5007 |
|        3004 | Fabian Johnson | Paris      |   300 |        5006 |
|        3005 | Graham Zusi    | California |   200 |        5002 |
|        3007 | Brad Davis     | New York   |   200 |        5001 |
|        3008 | Julian Green   | London     |   300 |        5002 |
|        3009 | Geoff Cameron  | Berlin     |   100 |        5003 |
+-------------+----------------+------------+-------+-------------+
8 rows in set (0.00 sec)

mysql> select * from salesman;
+-------------+------------+----------+-----------+
| salesman_id | name       | city     | comission |
+-------------+------------+----------+-----------+
|        5001 | James Hoog | New York |      0.15 |
|        5002 | Nail Knite | Paris    |      0.13 |
|        5003 | Lauson Hen | San Jose |      0.12 |
|        5005 | Pit Alex   | London   |      0.11 |
|        5006 | Mc Lyon    | Paris    |      0.14 |
|        5007 | Paul Adam  | Rome     |      0.13 |
+-------------+------------+----------+-----------+
6 rows in set (0.00 sec)

mysql> select * from orders;
+--------+-----------+------------+-------------+-------------+
| ord_no | purch_amt | ord_date   | customer_id | salesman_id |
+--------+-----------+------------+-------------+-------------+
|  70001 |       151 | 2012-10-05 |        3005 |        5002 |
|  70002 |        65 | 2012-10-05 |        3002 |        5001 |
|  70003 |      2480 | 2012-10-10 |        3009 |        5003 |
|  70004 |       111 | 2012-08-17 |        3009 |        5003 |
|  70005 |      2401 | 2012-07-27 |        3007 |        5001 |
|  70007 |       949 | 2012-09-10 |        3005 |        5002 |
|  70008 |      5760 | 2012-09-10 |        3002 |        5001 |
|  70009 |       271 | 2012-09-10 |        3001 |        5005 |
|  70010 |      1983 | 2012-10-10 |        3004 |        5006 |
|  70011 |        75 | 2012-08-17 |        3003 |        5007 |
|  70012 |       250 | 2012-06-27 |        3008 |        5002 |
|  70013 |      3046 | 2012-04-25 |        3002 |        5001 |
+--------+-----------+------------+-------------+-------------+
12 rows in set (0.00 sec)
```

> ### **Q.1) Display name and commission for all the salesmen.**
```
 mysql> select name, comission from salesman;
  +------------+-----------+
  | name       | comission |
  +------------+-----------+
  | James Hoog |      0.15 |
  | Nail Knite |      0.13 |
  | Lauson Hen |      0.12 |
  | Pit Alex   |      0.11 |
  | Mc Lyon    |      0.14 |
  | Paul Adam  |      0.13 |
  +------------+-----------+
  6 rows in set (0.00 sec)
```
> ### **Q.2) Retrieve salesman id of all salesmen from orders table without any repeats.**
```
mysql> select distinct(salesman_id) from orders;
+-------------+
| salesman_id |
+-------------+
|        5002 |
|        5001 |
|        5003 |
|        5005 |
|        5006 |
|        5007 |
+-------------+
6 rows in set (0.00 sec)
```
> ### **Q.3)Display names and city of salesman, who belongs to the city of Paris.**
```
mysql> select name, city from salesman where city = "paris";
+------------+-------+
| name       | city  |
+------------+-------+
| Nail Knite | Paris |
| Mc Lyon    | Paris |
+------------+-------+
2 rows in set (0.00 sec)
```
> ### **Q.4)Display all the information for those customers with a grade of 200.**
```
mysql> select * from customer where grade = 200;
+-------------+--------------+------------+-------+-------------+
| customer_id | cust_name    | city       | grade | salesman_id |
+-------------+--------------+------------+-------+-------------+
|        3003 | Jozy Altidor | Moscow     |   200 |        5007 |
|        3005 | Graham Zusi  | California |   200 |        5002 |
|        3007 | Brad Davis   | New York   |   200 |        5001 |
+-------------+--------------+------------+-------+-------------+
3 rows in set (0.00 sec)
```
> ### **Q.5)Display the order number, order date and the purchase amount for order(s) which will be delivered by the salesman with ID 5001**
```
mysql> select ord_no, purch_amt from orders where salesman_id=5001;
+--------+-----------+
| ord_no | purch_amt |
+--------+-----------+
|  70002 |        65 |
|  70005 |      2401 |
|  70008 |      5760 |
|  70013 |      3046 |
+--------+-----------+
4 rows in set (0.00 sec)
```
> ### **Q.12)Display all the customers, who are either belongs to the city New York or not had a grade above 100**
```
mysql> select * from customer where city='New York' or not grade >100;
+-------------+---------------+----------+-------+-------------+
| customer_id | cust_name     | city     | grade | salesman_id |
+-------------+---------------+----------+-------+-------------+
|        3002 | Nick Rimando  | New York |   100 |        5001 |
|        3007 | Brad Davis    | New York |   200 |        5001 |
|        3009 | Geoff Cameron | Berlin   |   100 |        5003 |
+-------------+---------------+----------+-------+-------------+
3 rows in set (0.34 sec)
```
> ### **Q.13)Find those salesmen with all information who gets the commission within a range of 0.12 and 0.14.**
```
mysql> select salesman_id,name,city,comission from salesman where comission between 0.12 and 0.14;
+-------------+------------+-------+-----------+
| salesman_id | name       | city  | comission |
+-------------+------------+-------+-----------+
|        5002 | Nail Knite | Paris |      0.13 |
|        5007 | Paul Adam  | Rome  |      0.13 |
+-------------+------------+-------+-----------+
2 rows in set (0.04 sec)
```
> ### **Q.14)Find all those customers with all information whose names are ending with the letter 'n'.**
```
mysql> select * from customer where cust_name like '%n';
+-------------+----------------+--------+-------+-------------+
| customer_id | cust_name      | city   | grade | salesman_id |
+-------------+----------------+--------+-------+-------------+
|        3001 | Brad Guzan     | London |  NULL |        5005 |
|        3004 | Fabian Johnson | Paris  |   300 |        5006 |
|        3008 | Julian Green   | London |   300 |        5002 |
|        3009 | Geoff Cameron  | Berlin |   100 |        5003 |
+-------------+----------------+--------+-------+-------------+
4 rows in set (0.00 sec)
```
> ### **Q.15)Find those salesmen with all information whose name containing the 1st character is 'N' and the 3rd character is 'l' and rests may be any character.**
```
mysql> select * from salesman where name like 'n__l%';
+-------------+------------+-------+-----------+
| salesman_id | name       | city  | comission |
+-------------+------------+-------+-----------+
|        5002 | Nail Knite | Paris |      0.13 |
+-------------+------------+-------+-----------+
1 row in set (0.00 sec)
```
> ### **Q.16)Find that customer with all information who does not get any grade except NULL.**
```
mysql> select * from customer where grade is null;
+-------------+------------+--------+-------+-------------+
| customer_id | cust_name  | city   | grade | salesman_id |
+-------------+------------+--------+-------+-------------+
|        3001 | Brad Guzan | London |  NULL |        5005 |
+-------------+------------+--------+-------+-------------+
1 row in set (0.00 sec)
```
> ### **Q.17)Find the total purchase amount of all orders. **
```
mysql> select sum(purch_amt) as Total_Purchase_Amount from orders;
+-----------------------+
| Total_Purchase_Amount |
+-----------------------+
|                 17542 |
+-----------------------+
1 row in set (0.01 sec)
```
> ### **Q.18)Find the number of salesman currently listing for all of their customers.**
```
mysql> select count(distinct salesman_id) as Count_of_Salesman from orders;
+-------------------+
| Count_of_Salesman |
+-------------------+
|                 6 |
+-------------------+
1 row in set (0.00 sec)
```
> ### **Q.19)Find the highest grade for each of the cities of the customers.**
```
mysql> select city , max(grade) from customer group by city;
+------------+------------+
| city       | max(grade) |
+------------+------------+
| London     |        300 |
| New York   |        200 |
| Moscow     |        200 |
| Paris      |        300 |
| California |        200 |
| Berlin     |        100 |
+------------+------------+
6 rows in set (0.00 sec)
```
> ### **Q.20)Find the highest purchase amount ordered by each customer with their ID and highest purchase amount.**
```
mysql> select customer_id,max(purch_amt) from orders group by customer_id order by 1;
+-------------+----------------+
| customer_id | max(purch_amt) |
+-------------+----------------+
|        3001 |            271 |
|        3002 |           5760 |
|        3003 |             75 |
|        3004 |           1983 |
|        3005 |            949 |
|        3007 |           2401 |
|        3008 |            250 |
|        3009 |           2480 |
+-------------+----------------+
8 rows in set (0.00 sec)
```
> ### **Q.21)Find the highest purchase amount ordered by each customer on a particular date with their ID, order date and highest purchase amount.**
```
mysql> select customer_id, ord_date, purch_amt from orders where purch_amt in (select max(purch_amt) from orders group by ord_date);
+-------------+------------+-----------+
| customer_id | ord_date   | purch_amt |
+-------------+------------+-----------+
|        3005 | 2012-10-05 |       151 |
|        3009 | 2012-10-10 |      2480 |
|        3009 | 2012-08-17 |       111 |
|        3007 | 2012-07-27 |      2401 |
|        3002 | 2012-09-10 |      5760 |
|        3008 | 2012-06-27 |       250 |
|        3002 | 2012-04-25 |      3046 |
+-------------+------------+-----------+
7 rows in set (0.01 sec)
```
> ### **Q.22) Find the highest purchase amount on a date '2012-08-17' for each salesman with their ID.**
```
mysql> select salesman_id, purch_amt from orders where ord_date = '2012-08-17' order by purch_amt DESC LIMIT 1;
+-------------+-----------+
| salesman_id | purch_amt |
+-------------+-----------+
|        5003 |       111 |
+-------------+-----------+
1 row in set (0.00 sec)
```
> ### **Q.23)Find the highest purchase amount with their customer ID and order date, for only those customers who have the highest purchase amount in a day is more than 2000.**
```
mysql> select customer_id, ord_date, max(purch_amt) from orders group by customer_id, ord_date having max(purch_amt) > 2000;
+-------------+------------+----------------+
| customer_id | ord_date   | MAX(purch_amt) |
+-------------+------------+----------------+
|        3009 | 2012-10-10 |           2480 |
|        3007 | 2012-07-27 |           2401 |
|        3002 | 2012-09-10 |           5760 |
|        3002 | 2012-04-25 |           3046 |
+-------------+------------+----------------+
4 rows in set (0.00 sec)
```
> ### **Q.24)Write a SQL statement that counts all orders for a date August 17th, 2012.**
```
mysql> select count(*) as "Sales on 17th August" from orders where ord_date='2012-08-17';
+----------------------+
| Sales on 17th August |
+----------------------+
|                    2 |
+----------------------+
1 row in set (0.00 sec)
```

