
mysql> CREATE DATABASE student_course_db;
Query OK, 1 row affected (0.04 sec)

mysql> USE student_course_db;
Database changed

mysql> CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    age INT,
    CONSTRAINT chk_age CHECK(age >= 18)
);
Query OK, 0 rows affected (0.22 sec)

mysql> CREATE TABLE courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL,
    course_code VARCHAR(20) UNIQUE NOT NULL
);
Query OK, 0 rows affected (0.08 sec)

mysql> SHOW TABLES;
+-----------------------------+
| Tables_in_student_course_db |
+-----------------------------+
| courses                     |
| students                    |
+-----------------------------+
2 rows in set (0.03 sec)

mysql> DESCRIBE students;
+--------------+--------------+------+-----+---------+----------------+
| Field        | Type         | Null | Key | Default | Extra          |
+--------------+--------------+------+-----+---------+----------------+
| student_id   | int          | NO   | PRI | NULL    | auto_increment |
| student_name | varchar(100) | NO   |     | NULL    |                |
| email        | varchar(100) | NO   | UNI | NULL    |                |
| age          | int          | YES  |     | NULL    |                |
+--------------+--------------+------+-----+---------+----------------+
4 rows in set (0.01 sec)

mysql> INSERT INTO students(student_name,email,age)
    VALUES
    ('Rahul','rahul@gmail.com',20),
    ('Priya','priya@gmail.com',21),
    ('Kiran','kiran@gmail.com',22);
Query OK, 3 rows affected (0.02 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> INSERT INTO courses(course_name,course_code)
    VALUES
    ('Database Systems','DB101'),
    ('Python Programming','PY102'),
    ('Java Programming','JA103');
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM students;
+------------+--------------+------------------+------+
| student_id | student_name | email            | age  |
+------------+--------------+------------------+------+
| 1          | Rahul        | rahul@gmail.com  | 20   |
| 2          | Priya        | priya@gmail.com  | 21   |
| 3          | Kiran        | kiran@gmail.com  | 22   |
+------------+--------------+------------------+------+
3 rows in set (0.00 sec)

mysql> SELECT * FROM courses;
+-----------+--------------------+-------------+
| course_id | course_name        | course_code |
+-----------+--------------------+-------------+
| 1         | Database Systems   | DB101       |
| 2         | Python Programming | PY102       |
| 3         | Java Programming   | JA103       |
+-----------+--------------------+-------------+
3 rows in set (0.00 sec)

mysql> INSERT INTO students(student_name,email,age)
    VALUES('Arun','rahul@gmail.com',20);
ERROR 1062 (23000): Duplicate entry 'rahul@gmail.com' for key 'students.email'

mysql> INSERT INTO students(student_name,email,age)
    VALUES(NULL,'new@gmail.com',20);
ERROR 1048 (23000): Column 'student_name' cannot be null

mysql> INSERT INTO students(student_name,email,age)
    VALUES('Ravi','ravi@gmail.com',15);
ERROR 3819 (HY000): Check constraint 'chk_age' is violated.
```
