# SQL-casestudy-1
Objective: Generate an output table identify employees from different departments who have the same highest salary. The goal is to list employees alongside their departments and salaries in the output table
Input Data:

+----+------+-------+--------+
| id | D_id | name  | salary |
+----+------+-------+--------+
|  1 |    1 | joe   |  70000 |
|  2 |    1 | jim   |  90000 |
|  3 |    2 | henry |  80000 |
|  4 |    2 | sam   |  60000 |
|  5 |    1 | max   |  90000 |
+----+------+-------+--------+

Department
+----+-----------+
| id | dept_name |
+----+-----------+
|  1 | IT        |
|  2 | SALES     |
+----+-----------+

Expected Output:
+-----------+-------+--------+
| department | name  | salary |
+-----------+-------+--------+
| IT        | jim   |  90000 |
| IT        | max   |  90000 |
| SALES     | henry |  80000 |
+-----------+-------+--------+

============================================================================
============================================================================
solution ==>

MariaDB [casestudy1]> create table emp(id int primary key auto_increment,D_id int, name varchar(50),salary float);

MariaDB [casestudy1]> desc emp;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int(11)     | NO   | PRI | NULL    | auto_increment |
| D_id   | int(11)     | YES  |     | NULL    |                |
| name   | varchar(50) | YES  |     | NULL    |                |
| salary | float       | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+

MariaDB [casestudy1]> insert into emp (D_id,name,salary)
-> values(1,'joe',70000),(1,'jim',90000),(2,'henry',80000),(2,'sam',60000),(1,'max',90000);
-----------------------------------------------------------------------------------------------
 create table dept(id int primary key auto_increment,dept_name varchar(50));
 desc dept;
+-----------+-------------+------+-----+---------+----------------+
| Field     | Type        | Null | Key | Default | Extra          |
+-----------+-------------+------+-----+---------+----------------+
| id        | int(11)     | NO   | PRI | NULL    | auto_increment |
| dept_name | varchar(50) | YES  |     | NULL    |                |
+-----------+-------------+------+-----+---------+----------------+

MariaDB [casestudy1]> insert into dept( dept_name)
    -> values('IT'),('SALES');
-------------------------------------------------------------------------------------------------
MariaDB [casestudy1]> show tables;
+----------------------+
| Tables_in_casestudy1 |
+----------------------+
| dept                 |
| emp                  |
+----------------------+
2 rows in set (0.001 sec)
-------------------------------------------------------------------------------------------------
MariaDB [casestudy1]> select * from dept;
+----+-----------+
| id | dept_name |
+----+-----------+
|  1 | IT        |
|  2 | SALES     |
+----+-----------+
2 rows in set (0.014 sec)
-------------------------------------------------------------------------------------------------
MariaDB [casestudy1]> select * from emp;
+----+------+-------+--------+
| id | D_id | name  | salary |
+----+------+-------+--------+
|  1 |    1 | joe   |  70000 |
|  2 |    1 | jim   |  90000 |
|  3 |    2 | henry |  80000 |
|  4 |    2 | sam   |  60000 |
|  5 |    1 | max   |  90000 |
+----+------+-------+--------+
5 rows in set (0.012 sec)
-------------------------------------------------------------------------------------------------
MariaDB [casestudy1]> select dept_name, name,salary from dept d
    -> join emp e on d.id=e.D_id;
+-----------+-------+--------+
| dept_name | name  | salary |
+-----------+-------+--------+
| IT        | joe   |  70000 |
| IT        | jim   |  90000 |
| SALES     | henry |  80000 |
| SALES     | sam   |  60000 |
| IT        | max   |  90000 |
+-----------+-------+--------+
5 rows in set (0.001 sec) 
-------------------------------------------------------------------------------------------------
MariaDB [casestudy1]> select dept_name, name,salary from dept d
    -> join emp e on d.id=e.D_id
    -> order by e.salary desc, name asc;
+-----------+-------+--------+
| dept_name | name  | salary |
+-----------+-------+--------+
| IT        | jim   |  90000 |
| IT        | max   |  90000 |
| SALES     | henry |  80000 |
| IT        | joe   |  70000 |
| SALES     | sam   |  60000 |
+-----------+-------+--------+
5 rows in set (0.000 sec)
-------------------------------------------------------------------------------------------------
MariaDB [casestudy1]> select dept_name, name,salary from dept d
    -> join emp e on d.id=e.D_id
    -> order by e.salary desc, name asc
    -> limit 3;
+-----------+-------+--------+
| dept_name | name  | salary |
+-----------+-------+--------+
| IT        | jim   |  90000 |
| IT        | max   |  90000 |
| SALES     | henry |  80000 |
+-----------+-------+--------+
3 rows in set (0.000 sec)
-------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------
 
