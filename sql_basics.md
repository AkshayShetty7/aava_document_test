# **SQL BASICS** 

A concise beginner’s guide to querying and managing relational data 

### **What you’ll learn** 

- Tables, rows, columns, and basic database concepts 

- SELECT, WHERE, sorting, and limiting results 

- Aggregate functions, GROUP BY, and HAVING 

- JOINs, data changes, table design, and constraints 

**Who this is for:** students and first-time SQL learners. Examples use broadly supported SQL; some details vary by database system. 

**How to practise:** run each query against a test database, inspect the result, and change one clause at a time. 

Read the query • Predict the result • Verify it 

SQL BASICS • BEGINNER GUIDE 

Page 1 of 6 

## **1. Databases and Basic Queries** 

A relational database stores related information in tables. A table has columns (attributes) and rows (records). SQL, or Structured Query Language, is used to retrieve and modify this data. 

### **Example table: Students** 

```
Students
```

|`+----+--------+-------+-------+`|
|---|
|`| id | name   | dept  | mark  |`|
|`+----+--------+-------+-------+`|
|`| 1  | Asha   | CSE   | 88    |`<br>|
|`| 2  | Ravi   | ECE   | 76    |`<br>|
|`| 3  | Noor   | CSE   | 93    |`|
|`+----+--------+-------+-------+`|



### **SELECT retrieves data** 

```
SELECT name, mark
FROM Students;
```

This returns the name and mark columns for every row. Use * to select all columns, though listing the needed columns is usually clearer. 

```
SELECT *
FROM Students;
```

### **Filter rows with WHERE** 

```
SELECT name, mark
FROM Students
WHERE mark >= 80;
```

Common comparison operators are =, <>, <, >, <=, and >=. Combine conditions with AND, OR, and NOT. Text values are usually written in single quotes. 

```
SELECT name, dept
FROM Students
WHERE dept = 'CSE' AND mark > 90;
```

SQL keywords are commonly written in uppercase for readability, but SQL is generally not case-sensitive for keywords. String comparisons and identifier rules depend on the database and its collation/settings. 

SQL BASICS • BEGINNER GUIDE 

Page 2 of 6 

## **2. Sorting, Filtering, and NULL** 

### **Sort results with ORDER BY** 

```
SELECT name, mark
FROM Students
ORDER BY mark DESC;
```

ASC sorts ascending and DESC sorts descending. If you omit the direction, ascending is usually the default. Add a second column to break ties. 

```
SELECT name, dept, mark
FROM Students
ORDER BY dept ASC, mark DESC;
```

### **Limit the number of rows** 

```
SELECT name, mark
FROM Students
ORDER BY mark DESC
LIMIT 2;
```

LIMIT is supported by systems such as SQLite, PostgreSQL, and MySQL. SQL Server commonly uses TOP or OFFSET/FETCH instead; syntax varies across database products. 

### **Match a set or pattern** 

```
SELECT name FROM Students
WHERE dept IN ('CSE', 'ECE');
```

```
SELECT name FROM Students
WHERE name LIKE 'A%';
```

IN matches any listed value. LIKE supports patterns: % means zero or more characters and _ means one character in common SQL pattern syntax. 

### **Understand NULL** 

```
SELECT name
FROM Students
WHERE mark IS NULL;
```

NULL represents missing or unknown data. Do not use = NULL; use IS NULL or IS NOT NULL. Comparisons involving NULL generally evaluate to UNKNOWN, so handle missing values explicitly. 

### **Remove duplicates** 

```
SELECT DISTINCT dept
FROM Students;
```

DISTINCT removes duplicate result rows for the selected columns. It does not modify the underlying table. 

SQL BASICS • BEGINNER GUIDE 

Page 3 of 6 

## **3. Aggregates and Grouping** 

Aggregate functions summarise multiple rows. Common functions include COUNT, SUM, AVG, MIN, and MAX. 

```
SELECT COUNT(*) AS student_count,
       AVG(mark) AS average_mark,
       MIN(mark) AS lowest_mark,
       MAX(mark) AS highest_mark
FROM Students;
```

COUNT(*) counts rows. COUNT(column) counts non-NULL values in that column. Most aggregates ignore NULL inputs; COUNT(*) is the main exception. 

### **GROUP BY** 

```
SELECT dept, COUNT(*) AS total,
       AVG(mark) AS avg_mark
FROM Students
GROUP BY dept;
```

GROUP BY creates one result group per distinct department, then computes aggregates for each group. Selected columns that are not aggregated generally need to appear in GROUP BY, subject to database-specific rules. 

### **Filter groups with HAVING** 

```
SELECT dept, AVG(mark) AS avg_mark
FROM Students
GROUP BY dept
HAVING AVG(mark) >= 80;
```

WHERE filters individual rows before grouping. HAVING filters groups after aggregation. Use WHERE for row-level conditions and HAVING for aggregate conditions. 

### **Typical query order** 

A useful mental model is: FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT. This is a logical processing model, not necessarily the physical execution order chosen by the database engine. 

Practice: count how many students are in each department, then display only departments with at least two students. Hint: COUNT(*) and HAVING. 

SQL BASICS • BEGINNER GUIDE 

Page 4 of 6 

## **4. JOINs and Table Relationships** 

A JOIN combines rows from related tables. Consider Students(id, name, dept_id) and Departments(id, dept_name). A foreign key can connect Students.dept_id to Departments.id. 

### **INNER JOIN** 

```
SELECT s.name, d.dept_name
FROM Students AS s
INNER JOIN Departments AS d
    ON s.dept_id = d.id;
```

INNER JOIN returns rows with matching keys in both tables. Aliases such as s and d make references shorter and clearer. 

### **LEFT JOIN** 

```
SELECT s.name, d.dept_name
FROM Students AS s
LEFT JOIN Departments AS d
    ON s.dept_id = d.id;
```

LEFT JOIN keeps every row from the left table. When there is no matching department, the department columns are NULL. RIGHT and FULL OUTER JOIN support varies by database. 

### **Create and change data** 

```
INSERT INTO Students (id, name, dept, mark)
VALUES (4, 'Meera', 'CSE', 85);
```

```
UPDATE Students
SET mark = 90
WHERE id = 4;
```

```
DELETE FROM Students
WHERE id = 4;
```

INSERT adds rows, UPDATE changes matching rows, and DELETE removes matching rows. Always verify the WHERE clause before UPDATE or DELETE; omitting it can affect every row. 

### **Transactions** 

```
BEGIN;
```

```
UPDATE Accounts SET balance = balance - 100 WHERE id = 1;
UPDATE Accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;  -- make changes permanent
-- ROLLBACK; would undo uncommitted changes
```

Transaction commands and exact syntax vary by database. Use transactions for related changes that should succeed or fail together. 

SQL BASICS • BEGINNER GUIDE 

Page 5 of 6 

## **5. Table Design, Safety, and Practice** 

### **Create a table** 

```
CREATE TABLE Students (
    id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    dept VARCHAR(20),
```

```
    mark INTEGER CHECK (mark BETWEEN 0 AND 100)
);
```

CREATE TABLE defines columns and constraints. A PRIMARY KEY uniquely identifies each row and cannot be NULL. NOT NULL requires a value. CHECK enforces a condition when supported and enabled by the database. 

### **Useful concepts** 

**Foreign key:** enforces a relationship to a key in another table. **UNIQUE:** prevents duplicate values in a column or column combination. **Index:** can speed up searches and joins but uses storage and adds overhead to writes. **Normalization:** structures data to reduce unnecessary duplication and update anomalies. 

### **Practical safety rules** 

I Test UPDATE and DELETE with a SELECT using the same WHERE condition first. 

I Use parameterized queries in applications; do not build SQL by concatenating untrusted input. 

I Back up important data and use least-privilege database accounts. 

I Check database-specific syntax and behaviour before using a query in production. 

### **Quick practice** 

I List all students scoring 80 or more, highest mark first. 

I Find the average mark for each department. 

I Count students per department and keep groups with at least 2 students. 

I Join a student table to a department table to show department names. 

I Create a table with a primary key and a NOT NULL column. 

Reference: PostgreSQL SQL documentation — https://www.postgresql.org/docs/current/sql.html 

SQL BASICS • BEGINNER GUIDE 

Page 6 of 6 

