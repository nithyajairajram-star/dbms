mysql> CREATE DATABASE COMPANY;
Query OK, 1 row affected (0.01 sec)

mysql> 
mysql> USE COMPANY;
Database changed
mysql> 
mysql> CREATE TABLE EMPLOYEE ( Emp_no INT PRIMARY KEY, E_name VARCHAR(50), E_address VARCHAR(100), E_ph_no VARCHAR(15), Dept_no INT,    Dept_name VARCHAR(50), Job_id CHAR(10), Salary DECIMAL(10, 2)); 
Query OK, 0 rows affected (0.04 sec)

mysql> 
mysql> Describe EMPLOYEE; 
+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| Emp_no    | int           | NO   | PRI | NULL    |       |
| E_name    | varchar(50)   | YES  |     | NULL    |       |
| E_address | varchar(100)  | YES  |     | NULL    |       |
| E_ph_no   | varchar(15)   | YES  |     | NULL    |       |
| Dept_no   | int           | YES  |     | NULL    |       |
| Dept_name | varchar(50)   | YES  |     | NULL    |       |
| Job_id    | char(10)      | YES  |     | NULL    |       |
| Salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
8 rows in set (0.01 sec)

mysql> 
mysql> ALTER TABLE EMPLOYEE ADD HIREDATE DATE; 
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
mysql> ALTER TABLE EMPLOYEE MODIFY Job_id VARCHAR(10); 
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
mysql> ALTER TABLE EMPLOYEE RENAME COLUMN Emp_no TO E_no; 
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
mysql> ALTER TABLE EMPLOYEE MODIFY Job_id VARCHAR(20); 
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
mysql> ALTER TABLE EMPLOYEE ADD CONSTRAINT UQ_E_ph_no UNIQUE (E_ph_no); 
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
mysql> ALTER TABLE EMPLOYEE MODIFY E_name VARCHAR(50) NOT NULL; 
Query OK, 0 rows affected (0.08 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
mysql> ALTER TABLE EMPLOYEE ADD CONSTRAINT CHK_Salary 
    -> CHECK(Salary > 0); 
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
mysql> INSERT INTO EMPLOYEE (E_no, E_name, E_address, E_ph_no, Dept_no, Dept_name, Job_id, Salary, HIREDATE)VALUES (1, 'John Doe', '123 Main St', '555-1234', 101, 'Sales', 'J1001', 50000.00, '20-08-24'); 
Query OK, 1 row affected (0.01 sec)

mysql> 
mysql> INSERT INTO EMPLOYEE (E_no, E_name, E_address, E_ph_no, Dept_no, Dept_name, Job_id, Salary, HIREDATE)VALUES (2, 'Jane Smith', '456 Oak St', '555-5678', 102, 'Marketing', 'J1002', 60000.00, '4-06-18'); 
Query OK, 1 row affected (0.01 sec)

mysql> 
mysql> > INSERT INTO EMPLOYEE (E_no, E_name, E_address, E_ph_no, Dept_no, Dept_name, Job_id, Salary, HIREDATE)VALUES (3, 'Alice Johnson', '789 Pine St', '555-9012', 103, 'HR', 'J1003', 55000.00, '4-07-15'); 
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '> INSERT INTO EMPLOYEE (E_no, E_name, E_address, E_ph_no, Dept_no, Dept_name, Jo' at line 1
mysql> 
mysql> INSERT INTO EMPLOYEE (E_no, E_name, E_address, E_ph_no, Dept_no, Dept_name, Job_id, Salary, HIREDATE)VALUES (4, 'Alice Bob', '112 Apple St', '555-1112', 104, 'ADMIN', 'J1004', 45000.00, '4-03-15'); 
Query OK, 1 row affected (0.00 sec)

mysql> 
mysql> SELECT * FROM EMPLOYEE; 
+------+------------+--------------+----------+---------+-----------+--------+----------+------------+
| E_no | E_name     | E_address    | E_ph_no  | Dept_no | Dept_name | Job_id | Salary   | HIREDATE   |
+------+------------+--------------+----------+---------+-----------+--------+----------+------------+
|    1 | John Doe   | 123 Main St  | 555-1234 |     101 | Sales     | J1001  | 50000.00 | 2020-08-24 |
|    2 | Jane Smith | 456 Oak St   | 555-5678 |     102 | Marketing | J1002  | 60000.00 | 0004-06-18 |
|    4 | Alice Bob  | 112 Apple St | 555-1112 |     104 | ADMIN     | J1004  | 45000.00 | 0004-03-15 |
+------+------------+--------------+----------+---------+-----------+--------+----------+------------+
3 rows in set (0.00 sec)

mysql> 
mysql> UPDATE EMPLOYEE SET Salary = 55000.00 WHERE E_no = 1; 
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> 
mysql> UPDATE EMPLOYEE SET Dept_name = 'Digital Marketing' WHERE Dept_no = 102;  
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> 
mysql> SELECT * FROM EMPLOYEE; 
+------+------------+--------------+----------+---------+-------------------+--------+----------+------------+
| E_no | E_name     | E_address    | E_ph_no  | Dept_no | Dept_name         | Job_id | Salary   | HIREDATE   |
+------+------------+--------------+----------+---------+-------------------+--------+----------+------------+
|    1 | John Doe   | 123 Main St  | 555-1234 |     101 | Sales             | J1001  | 55000.00 | 2020-08-24 |
|    2 | Jane Smith | 456 Oak St   | 555-5678 |     102 | Digital Marketing | J1002  | 60000.00 | 0004-06-18 |
|    4 | Alice Bob  | 112 Apple St | 555-1112 |     104 | ADMIN             | J1004  | 45000.00 | 0004-03-15 |
+------+------------+--------------+----------+---------+-------------------+--------+----------+------------+
3 rows in set (0.00 sec)

mysql> 
mysql> DELETE FROM EMPLOYEE WHERE E_no = 3; 
Query OK, 0 rows affected (0.00 sec)

mysql> 
mysql> DELETE FROM EMPLOYEE WHERE Dept_no = 101; 
Query OK, 1 row affected (0.00 sec)

mysql> 
mysql> SELECT * FROM EMPLOYEE; 
+------+------------+--------------+----------+---------+-------------------+--------+----------+------------+
| E_no | E_name     | E_address    | E_ph_no  | Dept_no | Dept_name         | Job_id | Salary   | HIREDATE   |
+------+------------+--------------+----------+---------+-------------------+--------+----------+------------+
|    2 | Jane Smith | 456 Oak St   | 555-5678 |     102 | Digital Marketing | J1002  | 60000.00 | 0004-06-18 |
|    4 | Alice Bob  | 112 Apple St | 555-1112 |     104 | ADMIN             | J1004  | 45000.00 | 0004-03-15 |
+------+------------+--------------+----------+---------+-------------------+--------+----------+------------+
2 rows in set (0.00 sec)

mysql> 
mysql> TRUNCATE TABLE EMPLOYEE; 
Query OK, 0 rows affected (0.07 sec)

mysql> 
mysql> SELECT * FROM EMPLOYEE; 


