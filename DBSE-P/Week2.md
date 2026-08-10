
mysql> CREATE DATABASE bookflow_db;
Query OK, 1 row affected (0.02 sec)

mysql> USE bookflow_db;
Database changed

mysql> CREATE TABLE Books (
    -> book_id INT PRIMARY KEY,
    -> title VARCHAR(100) NOT NULL,
    -> isbn VARCHAR(20) UNIQUE,
    -> published_year INT CHECK (published_year < 2027)
    -> );
Query OK, 0 rows affected (0.10 sec)

mysql> INSERT INTO Books (book_id, title, isbn, published_year)
    -> VALUES
    -> (1,'The Great Gatsby','9780743273565',1925),
    -> (2,'To Kill a Mockingbird','9780061120084',1960),
    -> (3,'1984','9780451524935',1949);
Query OK, 3 rows affected (0.02 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Books;
+---------+----------------------+---------------+----------------+
| book_id | title                | isbn          | published_year |
+---------+----------------------+---------------+----------------+
| 1       | The Great Gatsby     | 9780743273565 | 1925           |
| 2       | To Kill a Mockingbird| 9780061120084 | 1960           |
| 3       | 1984                 | 9780451524935 | 1949           |
+---------+----------------------+---------------+----------------+
3 rows in set (0.00 sec)

mysql> CREATE TABLE Members (
    -> member_id INT PRIMARY KEY,
    -> full_name VARCHAR(100),
    -> email VARCHAR(100) UNIQUE
    -> );
Query OK, 0 rows affected (0.06 sec)

mysql> INSERT INTO Members (member_id, full_name, email)
    -> VALUES
    -> (101,'John Smith','john.smith@email.com'),
    -> (102,'Emma Wilson','emma.wilson@email.com'),
    -> (103,'Michael Brown','michael.brown@email.com');
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Members;
+-----------+---------------+--------------------------+
| member_id | full_name     | email                    |
+-----------+---------------+--------------------------+
| 101       | John Smith    | john.smith@email.com     |
| 102       | Emma Wilson   | emma.wilson@email.com    |
| 103       | Michael Brown | michael.brown@email.com  |
+-----------+---------------+--------------------------+
3 rows in set (0.00 sec)

mysql> CREATE TABLE Loans (
    -> loan_id INT PRIMARY KEY,
    -> member_id INT,
    -> book_id INT,
    -> loan_date DATE,
    -> FOREIGN KEY (member_id) REFERENCES Members(member_id),
    -> FOREIGN KEY (book_id) REFERENCES Books(book_id)
    -> );
Query OK, 0 rows affected (0.08 sec)
mysql> INSERT INTO Loans (loan_id, member_id, book_id, loan_date)
    -> VALUES
    -> (1,101,1,'2025-01-10'),
    -> (2,102,2,'2025-01-12'),
    -> (3,103,3,'2025-01-15');
Query OK, 3 rows affected (0.02 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Loans;
+---------+-----------+---------+------------+
| loan_id | member_id | book_id | loan_date  |
+---------+-----------+---------+------------+
| 1       | 101       | 1       | 2025-01-10 |
| 2       | 102       | 2       | 2025-01-12 |
| 3       | 103       | 3       | 2025-01-15 |
+---------+-----------+---------+------------+
3 rows in set (0.00 sec)

mysql> SELECT
    -> Members.full_name,
    -> Books.title,
    -> Loans.loan_date
    -> FROM Loans
    -> JOIN Members ON Loans.member_id = Members.member_id
    -> JOIN Books ON Loans.book_id = Books.book_id;
+---------------+----------------------+------------+
| full_name     | title                | loan_date  |
+---------------+----------------------+------------+
| John Smith    | The Great Gatsby     | 2025-01-10 |
| Emma Wilson   | To Kill a Mockingbird| 2025-01-12 |
| Michael Brown | 1984                 | 2025-01-15 |
+---------------+----------------------+------------+
3 rows in set (0.00 sec)

mysql> SELECT member_id, COUNT(*) AS total_books
    -> FROM Loans
    -> GROUP BY member_id;
+-----------+-------------+
| member_id | total_books |
+-----------+-------------+
| 101       | 1           |
| 102       | 1           |
| 103       | 1           |
+-----------+-------------+
3 rows in set (0.00 sec)

mysql> SELECT title, published_year
    -> FROM Books
    -> WHERE published_year > 1950;
+----------------------+----------------+
| title                | published_year |
+----------------------+----------------+
| To Kill a Mockingbird| 1960           |
+----------------------+----------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM Members
    -> WHERE email LIKE '%@email.com';
+-----------+---------------+--------------------------+
| member_id | full_name     | email                    |
+-----------+---------------+--------------------------+
| 101       | John Smith    | john.smith@email.com     |
| 102       | Emma Wilson   | emma.wilson@email.com    |
| 103       | Michael Brown | michael.brown@email.com  |
+-----------+---------------+--------------------------+
3 rows in set (0.00 sec)
mysql> START TRANSACTION;
Query OK, 0 rows affected (0.00 sec)

mysql> UPDATE Books
    -> SET published_year = 2020
    -> WHERE book_id = 1;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT * FROM Books;
+---------+----------------------+---------------+----------------+
| book_id | title                | isbn          | published_year |
+---------+----------------------+---------------+----------------+
| 1       | The Great Gatsby     | 9780743273565 | 2020           |
| 2       | To Kill a Mockingbird| 9780061120084 | 1960           |
| 3       | 1984                 | 9780451524935 | 1949           |
+---------+----------------------+---------------+----------------+
3 rows in set (0.00 sec)

mysql> ROLLBACK;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM Books;
+---------+----------------------+---------------+----------------+
| book_id | title                | isbn          | published_year |
+---------+----------------------+---------------+----------------+
| 1       | The Great Gatsby     | 9780743273565 | 1925           |
| 2       | To Kill a Mockingbird| 9780061120084 | 1960           |
| 3       | 1984                 | 9780451524935 | 1949           |
+---------+----------------------+---------------+----------------+
3 rows in set (0.00 sec)

mysql> START TRANSACTION;
Query OK, 0 rows affected (0.00 sec)

mysql> UPDATE Members
    -> SET full_name = 'John A. Smith'
    -> WHERE member_id = 101;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> COMMIT;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM Members;
+-----------+---------------+--------------------------+
| member_id | full_name     | email                    |
+-----------+---------------+--------------------------+
| 101       | John A. Smith | john.smith@email.com     |
| 102       | Emma Wilson   | emma.wilson@email.com    |
| 103       | Michael Brown | michael.brown@email.com  |
+-----------+---------------+--------------------------+
3 rows in set (0.00 sec)

mysql> CREATE INDEX idx_book_title
    -> ON Books(title);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> SHOW INDEX FROM Books;
+-------+------------+----------------+--------------+-------------+
| Table | Non_unique | Key_name       | Seq_in_index | Column_name |
+-------+------------+----------------+--------------+-------------+
| Books |          0 | PRIMARY        |            1 | book_id     |
| Books |          0 | isbn           |            1 | isbn        |
| Books |          1 | idx_book_title |            1 | title       |
+-------+------------+----------------+--------------+-------------+
3 rows in set (0.00 sec)

mysql> SHOW TABLES;
+----------------------+
| Tables_in_bookflow_db|
+----------------------+
| Books                |
| Loans                |
| Members              |
+----------------------+
3 rows in set (0.00 sec)
