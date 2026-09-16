# DBMS Notes

## 1. Relational Model

### 1.1 What is a Database?

- A **database** is an organized collection of data.
- Example: A college database may store:
  - Student details
  - Course details
  - Faculty details
  - Marks
  - Attendance

### 1.2 What is DBMS?

- **DBMS = Database Management System**
- Software used to:
  - Store data
  - Retrieve data
  - Insert data
  - Update data
  - Delete data
  - Manage security
  - Handle multiple users
  - Recover data after failures

Examples:
- MySQL
- PostgreSQL
- Oracle
- SQL Server
- MongoDB (NoSQL DBMS)

---

## 2. Relational Model

A **relational database** stores data in the form of **tables**.

Example:

### Student Table

| student_id | name | age | dept |
|---|---|---:|---|
| 101 | Alice | 20 | CSE |
| 102 | Bob | 21 | ECE |
| 103 | John | 20 | CSE |

### 2.1 Table

- A table stores related data.
- In relational database terminology, a table is also called a **relation**.

### 2.2 Row

- A row represents one complete record.
- Also called a **tuple**.

Example:

```text
101 | Alice | 20 | CSE
```

is one row/tuple.

### 2.3 Column

- A column represents one property/attribute.
- Also called an **attribute**.

Example:

```text
student_id
name
age
dept
```

are columns/attributes.

### 2.4 Relationships

Relationships show how tables are connected.

Example:

```text
Student
student_id
name
dept_id
```

```text
Department
dept_id
dept_name
```

`Student.dept_id` can refer to `Department.dept_id`.

---

# 3. Keys

Keys are attributes used to **identify records** and establish relationships between tables.

## 3.1 Super Key

A **Super Key** is any set of one or more attributes that can uniquely identify a row.

Example:

```text
Student(student_id, email, name)
```

Possible Super Keys:

```text
student_id
email
student_id + name
email + name
student_id + email
```

Important:

- A Super Key may contain **extra/unnecessary attributes**.

---

## 3.2 Candidate Key

A **Candidate Key** is a **minimal Super Key**.

It uniquely identifies a row and contains no unnecessary attribute.

Example:

```text
Student(student_id, email, name)
```

If both `student_id` and `email` are unique:

```text
student_id → Candidate Key
email      → Candidate Key
```

---

## 3.3 Primary Key

The **Primary Key** is the candidate key selected to uniquely identify records.

Example:

```sql
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100)
);
```

Properties:

- Unique
- Cannot contain `NULL`
- One primary key constraint per table
- It can consist of multiple columns

---

## 3.4 Alternate Key

Candidate keys that are **not selected as the Primary Key** are called Alternate Keys.

Example:

```text
Candidate Keys:
- student_id
- email

Primary Key:
- student_id

Alternate Key:
- email
```

---

## 3.5 Foreign Key

A **Foreign Key** is a column that refers to a key in another table.

Example:

### Department

| dept_id | dept_name |
|---|---|
| 1 | CSE |
| 2 | ECE |

### Student

| student_id | name | dept_id |
|---|---|---:|
| 101 | Alice | 1 |
| 102 | Bob | 2 |

Here:

```text
Student.dept_id → Department.dept_id
```

Purpose:

- Connect tables
- Maintain **referential integrity**
- Prevent invalid references

---

## 3.6 Composite Key

A **Composite Key** uses two or more columns together to uniquely identify a row.

Example:

### Enrollment

| student_id | course_id | marks |
|---|---|---:|
| 101 | C01 | 90 |
| 101 | C02 | 85 |
| 102 | C01 | 88 |

Neither `student_id` nor `course_id` is unique alone.

Together:

```text
(student_id, course_id)
```

uniquely identifies an enrollment.

```sql
PRIMARY KEY (student_id, course_id)
```

---

## Key Summary

| Key | Meaning |
|---|---|
| Super Key | Any attribute set that uniquely identifies a row |
| Candidate Key | Minimal Super Key |
| Primary Key | Candidate Key selected as main key |
| Alternate Key | Candidate Key not selected as Primary Key |
| Foreign Key | Refers to a key in another table |
| Composite Key | Key made using multiple columns |

### Easy Memory

```text
Super Key
   ↓ remove unnecessary attributes
Candidate Key
   ↓ choose one
Primary Key
```

---

# 4. SQL

**SQL = Structured Query Language**

SQL is used to interact with relational databases.

---

## 4.1 SELECT

Used to retrieve data.

```sql
SELECT * FROM Employee;
```

Select specific columns:

```sql
SELECT emp_id, emp_name
FROM Employee;
```

---

## 4.2 INSERT

Used to add new records.

```sql
INSERT INTO Employee (emp_id, emp_name, dept_id)
VALUES (101, 'Alice', 10);
```

---

## 4.3 UPDATE

Used to modify existing records.

```sql
UPDATE Employee
SET dept_id = 20
WHERE emp_id = 101;
```

### Important

Always be careful with `UPDATE` without `WHERE`.

```sql
UPDATE Employee
SET dept_id = 20;
```

This updates **every row**.

---

## 4.4 DELETE

Used to remove records.

```sql
DELETE FROM Employee
WHERE emp_id = 101;
```

Without `WHERE`:

```sql
DELETE FROM Employee;
```

all rows are deleted.

---

## 4.5 WHERE

Used to filter individual rows.

```sql
SELECT *
FROM Employee
WHERE dept_id = 10;
```

Example:

```sql
SELECT *
FROM Employee
WHERE salary > 50000;
```

---

## 4.6 GROUP BY

Used to group rows having the same value.

Example:

```sql
SELECT dept_id, COUNT(*) AS employee_count
FROM Employee
GROUP BY dept_id;
```

Result:

| dept_id | employee_count |
|---:|---:|
| 10 | 5 |
| 20 | 3 |
| 30 | 7 |

Common aggregate functions:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

## 4.7 HAVING

Used to filter **groups** after `GROUP BY`.

Example:

```sql
SELECT dept_id, COUNT(*) AS employee_count
FROM Employee
GROUP BY dept_id
HAVING COUNT(*) > 5;
```

This returns only departments having more than 5 employees.

### WHERE vs HAVING

| WHERE | HAVING |
|---|---|
| Filters rows | Filters groups |
| Before GROUP BY | After GROUP BY |
| Usually does not use aggregate result | Commonly used with aggregate functions |

Memory:

```text
WHERE → rows
HAVING → groups
```

---

## 4.8 ORDER BY

Used to sort results.

Ascending:

```sql
SELECT *
FROM Employee
ORDER BY salary ASC;
```

Descending:

```sql
SELECT *
FROM Employee
ORDER BY salary DESC;
```

---

## SQL Query Execution Order

A useful conceptual order is:

```text
FROM
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
```

Note: This is the **logical processing order**, not necessarily the physical execution plan used internally by the database.

---

# 5. Joins

A **JOIN** combines rows from multiple tables using a related column or condition.

Example tables:

### Employee

| emp_id | emp_name | dept_id |
|---:|---|---:|
| 1 | Alice | 10 |
| 2 | Bob | 20 |
| 3 | John | 30 |

### Department

| dept_id | dept_name |
|---:|---|
| 10 | HR |
| 20 | IT |
| 40 | Finance |

---

## 5.1 INNER JOIN

Returns only matching rows from both tables.

```sql
SELECT e.emp_name, d.dept_name
FROM Employee e
INNER JOIN Department d
ON e.dept_id = d.dept_id;
```

Result:

| emp_name | dept_name |
|---|---|
| Alice | HR |
| Bob | IT |

John is excluded because department 30 has no matching department.

### Memory

```text
INNER = only matches
```

---

## 5.2 LEFT JOIN

Returns:

- All rows from the left table
- Matching rows from the right table
- `NULL` when there is no match

```sql
SELECT e.emp_name, d.dept_name
FROM Employee e
LEFT JOIN Department d
ON e.dept_id = d.dept_id;
```

Result:

| emp_name | dept_name |
|---|---|
| Alice | HR |
| Bob | IT |
| John | NULL |

### Memory

```text
LEFT JOIN = everything from LEFT
```

---

## 5.3 RIGHT JOIN

Returns:

- All rows from the right table
- Matching rows from the left table

```sql
SELECT e.emp_name, d.dept_name
FROM Employee e
RIGHT JOIN Department d
ON e.dept_id = d.dept_id;
```

Finance appears even though no employee belongs to department 40.

### Memory

```text
RIGHT JOIN = everything from RIGHT
```

---

## 5.4 FULL OUTER JOIN

Returns:

- All matching rows
- Unmatched rows from the left table
- Unmatched rows from the right table

Conceptually:

```text
LEFT JOIN + RIGHT JOIN
```

Important:

- PostgreSQL supports `FULL OUTER JOIN`.
- MySQL does not natively support `FULL OUTER JOIN`; it can be simulated using `LEFT JOIN` + `UNION` + `RIGHT JOIN`.

---

## 5.5 CROSS JOIN

Returns the **Cartesian product**.

If:

```text
Table A = 3 rows
Table B = 4 rows
```

Then:

```text
3 × 4 = 12 rows
```

Example:

```sql
SELECT *
FROM Employee
CROSS JOIN Department;
```

Every employee is combined with every department.

---

## 5.6 SELF JOIN

A table is joined with itself.

Example:

### Employee

| emp_id | emp_name | manager_id |
|---:|---|---:|
| 1 | Alice | NULL |
| 2 | Bob | 1 |
| 3 | John | 1 |

Query:

```sql
SELECT
    e.emp_name AS employee,
    m.emp_name AS manager
FROM Employee e
LEFT JOIN Employee m
ON e.manager_id = m.emp_id;
```

Result:

| employee | manager |
|---|---|
| Alice | NULL |
| Bob | Alice |
| John | Alice |

---

## Join Summary

| Join | Returns |
|---|---|
| INNER | Matching rows |
| LEFT | All left + matching right |
| RIGHT | All right + matching left |
| FULL | All rows from both sides |
| CROSS | Every combination |
| SELF | Table joined with itself |

---

# 6. Normalization

**Normalization** is the process of organizing tables to:

- Reduce data redundancy
- Avoid data anomalies
- Improve data consistency
- Make data easier to maintain

---

## 6.1 Data Anomalies

There are three major anomalies.

### Insert Anomaly

You cannot insert some information without inserting unrelated information.

Example:

Suppose one table contains:

| Student | Course | Instructor |
|---|---|---|
| Alice | DBMS | Sharma |
| Bob | DBMS | Sharma |

If a new course is created but no student has enrolled yet, it becomes difficult to store the course independently.

Memory:

```text
INSERT → Can't add something independently
```

---

### Update Anomaly

The same information exists in multiple rows, so it must be updated everywhere.

Example:

| Student | Course | Instructor |
|---|---|---|
| Alice | DBMS | Sharma |
| Bob | DBMS | Sharma |

If Sharma changes to Raj Sharma, multiple rows must be updated.

If one row is missed:

| Student | Course | Instructor |
|---|---|---|
| Alice | DBMS | Raj Sharma |
| Bob | DBMS | Sharma |

The database becomes inconsistent.

Memory:

```text
UPDATE → Repeated information → update everywhere
```

---

### Delete Anomaly

Deleting one record accidentally removes other useful information.

Example:

If John is the only student enrolled in a particular course and we delete John's enrollment, the course information may also disappear if both are stored in the same table.

Memory:

```text
DELETE → Deleting one thing removes useful information
```

---

# 7. Normal Forms

## 7.1 First Normal Form (1NF)

A table is in **1NF** if:

- Each cell contains a single/atomic value.
- There are no repeating groups or multi-valued cells.

### ❌ Not 1NF

| student_id | name | phone_numbers |
|---|---|---|
| 1 | Alice | 9876, 8765 |

`phone_numbers` contains multiple values.

### ✅ 1NF

| student_id | name | phone |
|---|---|---|
| 1 | Alice | 9876 |
| 1 | Alice | 8765 |

Memory:

```text
1NF → Atomic values
```

---

## 7.2 Second Normal Form (2NF)

A table is in **2NF** if:

1. It is in 1NF.
2. There is no **partial dependency** of a non-key attribute on part of a composite candidate key.

### What is Partial Dependency?

Suppose:

```text
Enrollment(
    student_id,
    course_id,
    student_name,
    course_name,
    marks
)
```

Candidate key:

```text
(student_id, course_id)
```

Dependencies:

```text
student_id → student_name
course_id → course_name
(student_id, course_id) → marks
```

`student_name` depends only on `student_id`.

`course_name` depends only on `course_id`.

They depend on **part of the composite key**, not the complete key.

Therefore, this violates 2NF.

### Fix

Split into:

```text
Student(student_id, student_name)

Course(course_id, course_name)

Enrollment(student_id, course_id, marks)
```

Memory:

```text
2NF → No partial dependency
```

Important:

- If a table has a single-attribute candidate key, partial dependency cannot occur.
- Therefore, a table in 1NF with only single-attribute candidate keys is automatically in 2NF.

---

## 7.3 Third Normal Form (3NF)

A table is in **3NF** if:

1. It is in 2NF.
2. It has no problematic **transitive dependency** of non-key attributes on a key.

Example:

```text
Employee(
    emp_id,
    emp_name,
    dept_id,
    dept_name
)
```

Dependencies:

```text
emp_id → emp_name, dept_id
dept_id → dept_name
```

Therefore:

```text
emp_id → dept_id → dept_name
```

`dept_name` indirectly depends on `emp_id` through `dept_id`.

This is a transitive dependency.

### Fix

Split into:

```text
Employee(
    emp_id,
    emp_name,
    dept_id
)
```

```text
Department(
    dept_id,
    dept_name
)
```

Memory:

```text
3NF → No transitive dependency
```

---

## 7.4 BCNF

**BCNF = Boyce-Codd Normal Form**

BCNF is stronger than 3NF.

### Rule

For every functional dependency:

```text
X → Y
```

`X` must be a **Super Key**.

In simple interview language:

> **Every determinant must be a candidate key/super key.**

### Example

```text
Employee(
    emp_id,
    emp_name,
    dept_id,
    dept_name
)
```

Dependencies:

```text
emp_id → emp_name, dept_id, dept_name
dept_id → dept_name
```

`emp_id` is a key, so:

```text
emp_id → ...
```

is fine.

But:

```text
dept_id → dept_name
```

is a problem because `dept_id` is not a key of the Employee table.

Therefore:

```text
Employee
```

is not in BCNF.

### Fix

```text
Employee(
    emp_id,
    emp_name,
    dept_id
)
```

```text
Department(
    dept_id,
    dept_name
)
```

Memory:

```text
BCNF → Every determinant must be a key
```

---

## 7.5 4NF

**4NF = Fourth Normal Form**

A table is in 4NF when it is in BCNF and has no problematic **multivalued dependencies**.

### Example

Suppose:

```text
Student(student, hobby, language)
```

A student can independently have:

- Multiple hobbies
- Multiple languages

Example:

| Student | Hobby | Language |
|---|---|---|
| Alice | Singing | English |
| Alice | Singing | Hindi |
| Alice | Running | English |
| Alice | Running | Hindi |

This creates unnecessary combinations.

### Fix

Split into:

```text
Student_Hobby(student, hobby)
```

```text
Student_Language(student, language)
```

Memory:

```text
4NF → No problematic multivalued dependency
```

---

## 7.6 5NF

# 5NF — Fifth Normal Form

## What is 5NF?

**5NF (Fifth Normal Form)** is a normalization level that deals with **Join Dependencies (JD)**.

A table is in **5NF** when:

- It is already in **4NF**.
- It cannot be further decomposed into smaller tables without losing information.
- Every **non-trivial join dependency** is implied by the **candidate keys**.

### Simple Definition

> **5NF removes redundancy caused by complex relationships between 3 or more attributes/tables.**

---

# Why Do We Need 5NF?

Sometimes a table may have **no functional dependency or multivalued dependency problem**, but it can still contain redundancy because of a **join dependency**.

5NF handles this problem.

---

# Join Dependency (JD)

A **Join Dependency** means that a table can be reconstructed by joining multiple smaller tables.

Suppose:

```text
R(A, B, C)
```

can be decomposed into:

```text
R1(A, B)
R2(B, C)
R3(A, C)
```

If joining these smaller tables gives exactly the original valid relationships, then a **join dependency** exists.

---

# Example

Consider:

### Supplier_Project_Part

| Supplier | Project | Part |
|---|---|---|
| S1 | P1 | PartA |
| S1 | P1 | PartB |
| S1 | P2 | PartA |
| S2 | P1 | PartA |

Suppose the business rule is:

> A supplier can supply a part to a project only when:
>
> - Supplier works on the Project
> - Supplier supplies the Part
> - Project uses the Part

Then instead of storing all three relationships together, we can split them.

### Supplier_Project

| Supplier | Project |
|---|---|
| S1 | P1 |
| S1 | P2 |
| S2 | P1 |

### Supplier_Part

| Supplier | Part |
|---|---|
| S1 | PartA |
| S1 | PartB |
| S2 | PartA |

### Project_Part

| Project | Part |
|---|---|
| P1 | PartA |
| P1 | PartB |
| P2 | PartA |

The original relationship can be reconstructed by joining these tables according to the business rules.

This is the type of complex redundancy that **5NF** deals with.

---

# 4NF vs 5NF

| Normal Form | Main Problem Solved |
|---|---|
| 1NF | Repeating groups / atomic values |
| 2NF | Partial dependency |
| 3NF | Transitive dependency |
| BCNF | Determinant is not a super key |
| 4NF | Multivalued dependency |
| 5NF | Join dependency |

### Easy Memory

```text
1NF → Atomic values
2NF → Partial dependency
3NF → Transitive dependency
BCNF → Every determinant is a super key
4NF → Multivalued dependency
5NF → Join dependency
```

---

# 5NF and Lossless Decomposition

A decomposition should be **lossless**.

### Lossless means:

After splitting the table, we should be able to join the smaller tables and get back the **correct original information**.

```text
Original Table
      ↓
Decompose
      ↓
Smaller Tables
      ↓
JOIN
      ↓
Original Information
```

We should not get:

- Missing valid rows
- Extra/spurious rows

---

# Important Point

Do **not** define 5NF simply as:

> "4NF + lossless decomposition"

That is incomplete.

The important condition is:

> **5NF ensures that every non-trivial join dependency is implied by candidate keys.**

---

# When is 5NF Useful?

5NF is mainly useful when:

- A table has relationships involving **3 or more entities**.
- Redundancy exists because of complex relationships.
- Decomposing the table can remove that redundancy.
- The decomposition remains lossless.

5NF is relatively **rare in normal application databases** because most practical schemas are adequately handled by 3NF or BCNF.

---

# Interview Answer

> **5NF, or Fifth Normal Form, deals with join dependencies. A relation is in 5NF when every non-trivial join dependency is implied by its candidate keys, so the table cannot be further losslessly decomposed to remove redundancy.**

---

# One-Line Revision

```text
5NF → Removes redundancy caused by JOIN dependencies.
```

### Memory Trick

```text
4NF → Multiple independent values → MVD
5NF → Multiple tables/relationships → JD
```
-----


### Normalization Summary

```text
1NF  → Atomic values
2NF  → No partial dependency
3NF  → No transitive dependency
BCNF → Every determinant is a super key
4NF  → No problematic multivalued dependency
5NF  → No problematic join dependency
```

---

# Denormalization

**Denormalization = intentionally adding some duplicate/redundant data to make data retrieval faster.**

Normally, **normalization** tries to remove duplicate data.

Denormalization does the opposite **when there is a performance reason**.

---

## Example

Suppose we have normalized tables:

### Employee

| emp_id | emp_name | dept_id |
|---|---|---|
| 1 | Alice | 10 |
| 2 | Bob | 10 |

### Department

| dept_id | dept_name |
|---|---|
| 10 | IT |

To get employee + department name, we need a **JOIN**:

```sql
SELECT e.emp_name, d.dept_name
FROM Employee e
JOIN Department d
ON e.dept_id = d.dept_id;
```

### After Denormalization

We might store:

### Employee

| emp_id | emp_name | dept_id | dept_name |
|---|---|---|---|
| 1 | Alice | 10 | IT |
| 2 | Bob | 10 | IT |

Now we don't need the JOIN for this particular query.

---

## Why Denormalize?

```text
Normalization
    ↓
Less redundancy
    ↓
More JOINs
    ↓
Better data consistency

Denormalization
    ↓
More redundancy
    ↓
Fewer JOINs
    ↓
Faster reads
```

---

## Advantages

- Faster **read/query performance**
- Fewer JOINs
- Useful for reporting and analytics
- Can simplify frequently used queries

---

## Disadvantages

- Duplicate data
- More storage required
- Updates become harder
- Higher risk of inconsistent data

Example:

If the department name changes:

```text
IT → Information Technology
```

you may need to update it in **many Employee rows**.

---

## When Do We Use Denormalization?

Use it when:

- Reads are much more frequent than writes.
- JOINs are expensive.
- The same data is repeatedly needed together.
- Query performance is more important than minimizing redundancy.

---

## Interview Answer

> **Denormalization is the process of intentionally introducing redundancy into a database to reduce JOINs and improve read performance. The trade-off is increased storage and a higher risk of data inconsistency.**

---

## Easy Memory

```text
Normalization   → Remove redundancy
Denormalization → Add redundancy for speed
```
---

# 8. Transactions

A **transaction** is a logical unit of work consisting of one or more database operations.

Example:

Transfer ₹1000 from Account A to Account B:

```text
1. Deduct ₹1000 from A
2. Add ₹1000 to B
```

Both operations should succeed together.

---

# 9. ACID Properties

ACID properties ensure reliable transactions.

## 9.1 Atomicity

> All operations happen, or none happen.

Example:

```text
A: -₹1000
B: +₹1000
```

If adding money to B fails, deduction from A should also be rolled back.

Memory:

```text
Atomicity = All or Nothing
```

---

## 9.2 Consistency

A transaction must move the database from one valid state to another valid state.

Example:

If total money before transfer is:

```text
₹10,000
```

it should remain:

```text
₹10,000
```

after transferring money between accounts.

Memory:

```text
Consistency = Rules remain valid
```

---

## 9.3 Isolation

Concurrent transactions should not incorrectly interfere with each other.

Example:

Two users try to update the same bank account at the same time.

Isolation controls how their operations interact.

Memory:

```text
Isolation = Transactions don't improperly interfere
```

---

## 9.4 Durability

Once a transaction is committed, its changes should survive failures such as a system crash.

Memory:

```text
Durability = Committed data stays
```

---

## ACID Summary

| Property | Meaning |
|---|---|
| Atomicity | All or nothing |
| Consistency | Database remains valid |
| Isolation | Concurrent transactions don't improperly interfere |
| Durability | Committed changes persist |

---

# 10. Concurrency

**Concurrency** means multiple transactions execute at the same time or overlap in execution.

Example:

```text
Transaction A ────────
Transaction B ────────
```

Concurrency improves performance but can cause problems if not controlled.

---

# 11. Concurrency Problems

## 11.1 Dirty Read

Transaction A reads data written by Transaction B **before B commits**.

Example:

```text
A updates salary: 50,000 → 70,000
B reads 70,000
A rolls back
```

B read data that was never permanently committed.

Memory:

```text
Dirty Read = Read uncommitted data
```

---

## 11.2 Non-Repeatable Read

A transaction reads the same row twice and gets different values because another transaction updated it between the reads.

Example:

```text
First read: 50,000

Another transaction changes it to 70,000

Second read: 70,000
```

Same query, different value.

Memory:

```text
Non-repeatable = Same row, different value
```

---

## 11.3 Phantom Read

A transaction executes the same query twice and gets a different **set of rows** because another transaction inserted/deleted matching rows.

Example:

First query:

```sql
SELECT *
FROM Employee
WHERE salary > 50000;
```

Returns 5 employees.

Another transaction inserts an employee with salary 60,000.

Second query returns 6 employees.

The new row is a **phantom row**.

Memory:

```text
Phantom = New/disappearing rows
```

---

# 12. Locks

Locks control access to data when transactions run concurrently.

## 12.1 Shared Lock (S)

Used for reading.

Multiple transactions can generally hold shared locks on the same item at the same time.

```text
T1 → READ
T2 → READ
```

Both can read.

---

## 12.2 Exclusive Lock (X)

Used when modifying data.

An exclusive lock prevents conflicting concurrent access.

```text
T1 → WRITE
```

Other transactions generally cannot read/write that item in a conflicting way until the lock is released.

### Memory

```text
S = Shared = Read
X = Exclusive = Write
```

---

# 13. Deadlock

A **deadlock** occurs when transactions wait for each other forever.

Example:

```text
T1 locks A
T2 locks B

T1 waits for B
T2 waits for A
```

Diagram:

```text
T1 → waiting for B
↑             ↓
A             B
↓             ↑
T2 → waiting for A
```

Neither transaction can proceed.

### Ways to Handle Deadlock

- Deadlock detection
- Timeout
- Rollback one transaction
- Prevention/avoidance strategies

---

# 14. Two-Phase Locking (2PL)

2PL is a concurrency-control protocol.

It has two phases:

## Phase 1: Growing Phase

- Transaction can acquire locks.
- Transaction cannot release locks.

```text
LOCK ↑
```

## Phase 2: Shrinking Phase

- Transaction can release locks.
- Transaction cannot acquire new locks.

```text
UNLOCK ↓
```

Memory:

```text
Growing  → Acquire
Shrinking → Release
```

---

# DBMS Indexes — Complete README

## 1. What is an Index?

An **index** is a data structure used by a database to **find rows faster** without scanning the entire table.

### Without Index

Suppose we have 1 million employees:

```text
Employee Table
     ↓
Scan Row 1
     ↓
Scan Row 2
     ↓
Scan Row 3
     ↓
...
     ↓
Find required row
```

This can be slow.

### With Index

```text
Query
  ↓
Index
  ↓
Find key quickly
  ↓
Locate required row
```

### Simple Example

```sql
CREATE INDEX idx_emp_name
ON Employee(emp_name);
```

Now a query like:

```sql
SELECT *
FROM Employee
WHERE emp_name = 'Alice';
```

can potentially use the index to find `Alice` faster.

---

# 2. Why Do We Use Indexes?

Indexes are mainly used to improve **read/search performance**.

They can help with:

- `WHERE`
- `JOIN`
- `ORDER BY`
- `GROUP BY`
- Range queries such as `BETWEEN`, `<`, `>`

### Example

```sql
SELECT *
FROM Employee
WHERE salary > 50000;
```

An index on `salary` can potentially make this search faster.

---

# 3. Trade-off of Indexes

Indexes are not free.

### Advantages

- Faster searches
- Faster filtering
- Can improve JOIN performance
- Can improve sorting/range queries

### Disadvantages

- Require additional storage
- `INSERT` can become slower
- `UPDATE` can become slower
- `DELETE` can become slower

Why?

Because when table data changes, the database may also need to update the indexes.

### Memory Trick

```text
Index
→ Faster READ
→ More storage
→ Can slow WRITE
```

---

# 4. B-Tree / B+ Tree https://www.youtube.com/watch?v=BwUvgG29fPc

B-Tree and B+ Tree are **data structures used to implement database indexes**.

They are NOT simply "index types."

```text
Index
  ↓
Implementation / Data Structure
  ↓
B-Tree / B+ Tree
```

---

# 5. B-Tree

**B-Tree = Balanced Tree**

It keeps keys sorted and maintains a balanced structure.

Example:

```text
              [50]
            /      \
         [20]       [70]
        /    \      /    \
     [10]   [30] [60]   [80]
```

To search for `60`:

```text
60 > 50
   ↓
Go right

60 < 70
   ↓
Go left

Found 60
```

Instead of scanning every record, the database follows the tree.

### Complexity

Search is approximately:

```text
O(log n)
```

depending on implementation and workload.

---

# 6. B+ Tree

**B+ Tree is a variant of B-Tree and is commonly used for database indexes.**

### Main Characteristics

- Internal nodes mainly contain keys for navigation.
- Actual record pointers/data references are stored at the leaf level.
- Leaf nodes are linked.
- Excellent for range scans.

Conceptually:

```text
              [50]
            /      \
         [20]       [70]
        /    \      /    \
     [10]   [30] [60]   [80]
       ↓      ↓    ↓      ↓
     Data   Data Data   Data
```

Leaf nodes:

```text
[10] → [20] → [30] → [50] → [60] → [70] → [80]
```

### Why is this useful?

For:

```sql
WHERE emp_id BETWEEN 20 AND 70
```

the database can find `20` and then follow the linked leaf nodes:

```text
20 → 30 → 50 → 60 → 70
```

So range queries are efficient.

---

# 7. B-Tree vs B+ Tree

| Feature | B-Tree | B+ Tree |
|---|---|---|
| Balanced | Yes | Yes |
| Sorted keys | Yes | Yes |
| Data can be in internal nodes | Yes | Usually no |
| Data pointers at leaves | Yes | Yes |
| Leaf nodes linked | Usually no | Yes |
| Range queries | Good | Excellent |
| Common in database indexes | Yes | Very common |

### Memory Trick

```text
B+ Tree
→ Data at leaves
→ Internal nodes guide search
→ Leaves linked
→ Great for range queries
```

---

# 8. Clustered Index

A **clustered index** determines the table's row organization around the indexed key.

The exact implementation is **DBMS-specific**.

Example:

```sql
CREATE CLUSTERED INDEX idx_emp_id
ON Employee(emp_id);
```

Conceptually, rows are organized by:

```text
101 → Alice
102 → David
103 → John
104 → Sara
105 → Bob
```

### Important

Usually, a table can have **only one clustered organization**.

Why?

Because the table's rows cannot simultaneously be organized in multiple different ways.

### Useful for

- Point lookups
- Range queries
- Ordered access

### Memory

```text
Clustered
→ Table row organization
→ Usually ONE
```

---

# 9. Non-Clustered Index

A **non-clustered index** is a separate index structure containing:

```text
Indexed Value
      +
Row Reference
```

Example:

```sql
CREATE NONCLUSTERED INDEX idx_emp_name
ON Employee(emp_name);
```

Conceptually:

```text
Alice → Row Reference
Bob   → Row Reference
David → Row Reference
John  → Row Reference
Sara  → Row Reference
```

The actual table remains separate.

### Search Flow

```text
Query
  ↓
Non-Clustered Index
  ↓
Find indexed value
  ↓
Get row reference
  ↓
Fetch actual row
```

### Important

A table can generally have **multiple non-clustered indexes**, subject to DBMS limitations.

### Memory

```text
Non-Clustered
→ Separate index
→ Value + Row Reference
→ Multiple possible
```

---

# 10. Clustered vs Non-Clustered

| Feature | Clustered | Non-Clustered |
|---|---|---|
| Main idea | Organizes table rows around key | Separate index structure |
| Relationship to table | Tied to row organization | Separate from table organization |
| Number | Usually one | Generally multiple |
| Row reference | DBMS-dependent | Typically present |
| Range queries | Good | Good |
| Implementation | DBMS-specific | DBMS-specific |

### Easy Example

```text
Clustered:

Index
 ↓
Table rows organized around key


Non-Clustered:

Separate Index
 ↓
Value → Row Reference
 ↓
Actual Table Row
```

---

# 11. Composite Index

A **composite index** is an index created using **multiple columns**.

Example:

```sql
CREATE INDEX idx_dept_salary
ON Employee(dept_id, salary);
```

This index contains:

```text
(dept_id, salary)
```

### Useful Query

```sql
SELECT *
FROM Employee
WHERE dept_id = 10;
```

Also:

```sql
SELECT *
FROM Employee
WHERE dept_id = 10
AND salary > 50000;
```

---

# 12. Leftmost Prefix Rule

For:

```text
(dept_id, salary)
```

the order matters.

`dept_id` is the **leftmost column**.

### Good usage

```sql
WHERE dept_id = 10
```

```text
✅ Uses leading column
```

And:

```sql
WHERE dept_id = 10
AND salary > 50000
```

```text
✅ Uses leading columns
```

### Less useful

```sql
WHERE salary > 50000
```

```text
⚠️ Does not use the leading dept_id column
```

The exact optimizer behavior depends on the DBMS and query, but the leftmost-prefix rule is an important general principle.

### Memory

```text
Index: (A, B, C)

A       → Good
A, B    → Good
A, B, C → Good

B       → Usually less useful
C       → Usually less useful
B, C    → Usually less useful
```

---

## Composite Index

```sql
CREATE INDEX idx_dept_salary
ON Employee(dept_id, salary);
```

---

## Clustered Index — SQL Server

```sql
CREATE CLUSTERED INDEX idx_emp_id
ON Employee(emp_id);
```

---

## Non-Clustered Index — SQL Server

```sql
CREATE NONCLUSTERED INDEX idx_emp_name
ON Employee(emp_name);
```

---

## Drop Index

Syntax varies by DBMS.

For SQL Server:

```sql
DROP INDEX idx_emp_name ON Employee;
```

---

# 24. Which Index Should You Choose?

### Equality Search

```sql
WHERE emp_id = 101
```

B+ Tree or hash-based indexing can be useful, depending on the DBMS and workload.

### Range Search

```sql
WHERE salary BETWEEN 40000 AND 70000
```

B+ Tree is generally a good choice.

### Multiple Conditions

```sql
WHERE dept_id = 10
AND salary > 50000
```

A composite index can help:

```text
(dept_id, salary)
```

### Text Search

```text
Search inside article/product description
```

Consider:

```text
Full-Text Index
```

---



# 26. Final Revision Table

| Concept | Main Idea |
|---|---|
| **Index** | Speeds up data retrieval |
| **B-Tree** | Balanced tree structure |
| **B+ Tree** | Tree structure with data pointers at leaves and linked leaves |
| **Clustered** | Determines table row organization |
| **Non-Clustered** | Separate index with row references |
| **Composite** | Multiple columns |
----

# 20. ER Model

**ER = Entity-Relationship**

The ER model is used to design databases before implementing tables.

Main concepts:

```text
Entity
Attribute
Relationship
Cardinality
```

---

# 21. Entity

An **entity** is a real-world object about which we store information.

Examples:

```text
Student
Employee
Course
Department
Customer
Product
```

Example:

```text
Student
- student_id
- name
- age
```

---

# 22. Attribute

An **attribute** describes an entity.

For Employee:

```text
Employee
├── emp_id
├── emp_name
├── salary
└── dept_id
```

These are attributes.

Types commonly discussed:

- Simple attribute
- Composite attribute
- Single-valued attribute
- Multi-valued attribute
- Derived attribute

Example:

```text
DOB → Age
```

Age can be a derived attribute because it can be calculated from DOB.

---

# 23. Relationship

A relationship describes how entities are associated.

Example:

```text
Student ─── ENROLLS IN ─── Course
```

Another example:

```text
Employee ─── WORKS IN ─── Department
```

---

# 24. Cardinality

Cardinality describes how many instances of one entity can be associated with another.

## 24.1 One-to-One (1:1)

One person has one passport.

```text
Person 1 ─── 1 Passport
```

---

## 24.2 One-to-Many (1:N)

One department has many employees.

```text
Department 1 ─── N Employee
```

---

## 24.3 Many-to-Many (M:N)

Many students can take many courses.

```text
Student N ─── N Course
```

In a relational database, M:N relationships are usually implemented using a **junction/bridge table**.

Example:

```text
Student
   ↓
Enrollment
   ↓
Course
```

### Enrollment

| student_id | course_id |
|---|---|
| 101 | C01 |
| 101 | C02 |
| 102 | C01 |

---

# 25. Distributed Databases

## What is a Distributed Database?

A **distributed database** is a database where data is stored across **multiple machines/nodes**, but the system works together and appears to the user as **one database system**.

```text
                 Distributed Database
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
       Node 1          Node 2         Node 3
       India            USA           Europe
```

The nodes communicate with each other through a network.

---

# Simple Example

Suppose an online shopping company has users from different countries.

Instead of keeping all data in one location:

```text
India users    → India Node
USA users      → USA Node
Europe users   → Europe Node
```

This can reduce the distance data has to travel.

The user still interacts with:

```text
              One Database Service
                     ↓
          ┌──────────┼──────────┐
          ↓          ↓          ↓
       India        USA       Europe
       Node         Node        Node
```

---

# Why Use Distributed Databases?

## 1. Scalability

More machines can be added as the amount of data or traffic increases.

```text
More users
    ↓
More workload
    ↓
Add more nodes
```

---

## 2. High Availability

If one node goes down, another node may continue serving requests, depending on the system's replication/failover design.

```text
Node 1 ❌
   ↓
Node 2 ✅
   ↓
Service continues
```

---

## 3. Fault Tolerance

The system can continue operating even when some machines fail.

This is especially possible when data is **replicated** across multiple nodes.

---

## 4. Lower Latency

Data can be placed closer to users geographically.

Example:

```text
User in India
      ↓
India Node
      ↓
Less network distance
      ↓
Potentially lower latency
```

---

# Important Concepts

## 1. Replication

**Replication = keeping copies of data on multiple nodes.**

Example:

```text
User Data
   │
   ├──→ India Node
   ├──→ USA Node
   └──→ Europe Node
```

### Why?

- Higher availability
- Fault tolerance
- Faster local reads

But replication introduces a challenge:

> **How do we keep all copies consistent?**

---

# 2. Sharding

**Sharding = splitting data across different nodes.**

Example:

```text
Node 1 → Users 1–1,000,000
Node 2 → Users 1,000,001–2,000,000
Node 3 → Users 2,000,001–3,000,000
```

Each node stores a **different portion** of the data.

### Memory

```text
Replication → Copy data
Sharding    → Split data
```

---

# Replication vs Sharding

| Feature | Replication | Sharding |
|---|---|---|
| Meaning | Copy data | Split data |
| Data on nodes | Same/overlapping copies | Different portions |
| Main benefit | Availability | Scalability |
| Main challenge | Keeping copies consistent | Distributing/querying data |

---

# 3. Distributed Transactions

A transaction may involve **multiple nodes**.

Example:

```text
Transfer ₹10,000

Node 1 → Deduct money
Node 2 → Add money
```

Both operations should succeed together.

If one succeeds and the other fails:

```text
Node 1 → ₹10,000 deducted ✅
Node 2 → ₹10,000 added ❌
```

The system can become inconsistent.

Therefore, distributed transactions are more complicated than transactions involving a single database node.

---

# 4. Network Failure

Unlike a single-machine database, distributed databases depend heavily on the network.

For example:

```text
Node 1 ←──── Network ────→ Node 2
                    ❌
               Network Failure
```

Node 1 may not be able to communicate with Node 2.

This creates difficult questions:

- Is the other node down?
- Is only the network connection broken?
- Which data is correct?
- Should the system continue accepting writes?

---

# 5. Consistency

Suppose the same data exists on multiple nodes:

```text
Node 1 → Balance = ₹1000
Node 2 → Balance = ₹1000
```

A transaction changes it:

```text
Balance → ₹700
```

If replication is delayed:

```text
Node 1 → ₹700
Node 2 → ₹1000
```

For a short period, different nodes may return different values.

This leads to an important distributed-system trade-off between:

```text
Consistency
Availability
Partition Tolerance
```

This is related to the **CAP theorem**.

---

# Distributed Database vs Centralized Database

| Feature | Centralized | Distributed |
|---|---|---|
| Machines | Usually one main system | Multiple nodes |
| Data location | One location/system | Multiple locations |
| Scalability | More limited | Can scale across nodes |
| Fault tolerance | Lower if single point of failure | Can be higher with redundancy |
| Network complexity | Lower | Higher |
| System complexity | Simpler | More complex |

---

# Advantages

### 1. Scalability

Can distribute workload across multiple machines.

### 2. Availability

Multiple nodes can provide service when some nodes fail.

### 3. Fault Tolerance

Replication can protect against node failures.

### 4. Geographic Distribution

Data can be placed closer to users.

### 5. Load Distribution

Requests can be distributed across multiple nodes.

---

# Challenges

### 1. Network Failures

Nodes communicate over a network, which can fail or become slow.

### 2. Data Consistency

Keeping replicated data synchronized can be difficult.

### 3. Distributed Transactions

One transaction may involve multiple nodes.

### 4. Replication

Copies of data must be synchronized correctly.

### 5. Complexity

Designing, monitoring and debugging a distributed database is more difficult.

### 6. Partition Handling

The system must decide how to behave when nodes cannot communicate.

---

# Simple Real-Life Example

Imagine a food-delivery application.

Users are distributed across:

```text
India
USA
Europe
```

The company may have:

```text
India Server
     ↓
Indian users/data

USA Server
     ↓
US users/data

Europe Server
     ↓
European users/data
```

When a user in India places an order:

```text
User
 ↓
India Node
 ↓
Process Request
```

If important data is replicated, copies may also exist on other nodes.

---

# Distributed Database Flow

```text
                    Application
                         ↓
                   Distributed DB
                         ↓
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
       Node 1          Node 2         Node 3
       India            USA           Europe
          │              │              │
          └──────────────┼──────────────┘
                         ↓
                    Network
```

---

# Key Terms

| Term | Simple Meaning |
|---|---|
| **Node** | A machine/server participating in the database |
| **Replication** | Copying data to multiple nodes |
| **Sharding** | Splitting data across nodes |
| **Distributed Transaction** | Transaction involving multiple nodes |
| **Consistency** | Nodes have the expected/current data |
| **Availability** | System continues responding to requests |
| **Fault Tolerance** | System can handle failures |
| **Partition** | Network communication failure between nodes |

---

# 🧠 Important Memory Trick

```text
Distributed Database
        ↓
Multiple Machines
        ↓
Benefits:
Scalability
Availability
Fault Tolerance
Lower Geographic Latency

Challenges:
Network Failure
Consistency
Replication
Distributed Transactions
Complexity
```

### Most Important Difference

```text
Replication → COPY data
Sharding    → SPLIT data
```

---

# Interview Answer

> **A distributed database stores and manages data across multiple interconnected machines or locations while presenting it as a unified database system. It provides benefits such as scalability, availability and fault tolerance, but introduces challenges like network failures, data consistency, replication and distributed transactions.**

### One-Line Revision

> **Distributed Database = One logical database system running across multiple physical nodes.**


-----

# SQL AND NoSQL — COMPLETE NOTES

---

# 1. SQL

## What is SQL?

**SQL = Structured Query Language**

SQL is a language used to **store, retrieve, update, and manage data** in relational databases.

Examples of SQL databases:

- MySQL
- PostgreSQL
- Oracle
- SQL Server

### Basic Idea

```text
SQL Database
     ↓
  Tables
     ↓
Rows + Columns
     ↓
Relationships
```

---

# 2. Relational Database

SQL databases are generally **relational databases**.

Data is stored in tables.

### Example

```text
Student

+----+-------+-----+
| id | name  | age |
+----+-------+-----+
| 1  | Alice | 21  |
| 2  | Bob   | 22  |
+----+-------+-----+
```

- `id`, `name`, `age` → Columns
- Alice/Bob records → Rows
- `id` → can be a Primary Key

Different tables can be connected using relationships.

---

# 3. SQL Schema

A schema defines the structure of the database.

Example:

```sql
CREATE TABLE Student (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT
);
```

Here we define:

- Column names
- Data types
- Constraints
- Relationships

SQL databases generally use a **structured schema**.

---

# 5. Relationships in SQL

SQL databases are very good at representing relationships.

Example:

```text
Student
   |
   | enrolled in
   ↓
Course
```

Tables can be connected using:

- Primary Key
- Foreign Key
- JOINs

Example:

```sql
SELECT Student.name, Course.course_name
FROM Student
JOIN Course
ON Student.course_id = Course.course_id;
```

---

# 6. Advantages of SQL

- Structured data storage
- Strong support for relationships
- Powerful JOIN operations
- Strong transaction support
- Data integrity using constraints
- Standardized SQL language
- Good for complex queries

---

# 7. Disadvantages of SQL

- Schema is generally less flexible than document-style NoSQL models.
- Complex relational structures can require many JOINs.
- Scaling very large workloads can require additional architecture.
- Schema changes may require migrations.

---

# 8. When to Use SQL?

SQL is generally suitable when:

- Data is structured.
- Relationships are important.
- Complex JOINs are required.
- Strong transactional behavior is needed.
- Data integrity is important.
- Schema is relatively stable.

### Examples

- Banking systems
- Payroll systems
- Inventory systems
- College management systems
- E-commerce orders

---

# 9. NoSQL

## What is NoSQL?

**NoSQL = Not Only SQL**

NoSQL refers to **non-relational database systems** that use data models other than the traditional relational table model.

NoSQL databases can use:

- Document
- Key-Value
- Wide-Column
- Graph

Examples:

- MongoDB
- Redis
- Cassandra
- Neo4j

### Main Idea

> **NoSQL provides flexible data models and is commonly used for large-scale or distributed workloads.**

---

# 10. Document Database

### Example: MongoDB

Data is stored as documents, usually in a JSON-like format.

Example:

```json
{
  "student_id": 101,
  "name": "Alice",
  "age": 21,
  "courses": ["DBMS", "OS"]
}
```

Related information can be stored together.

### Memory

> **Document → MongoDB → JSON-like data**

---

# 11. Key-Value Database

### Example: Redis

Data is stored as:

```text
Key → Value
```

Example:

```text
"user101" → "Alice"
"age101"  → 21
```

The key is used to retrieve the value.

### Common Uses

- Caching
- Sessions
- Fast lookups
- Real-time applications

### Memory

> **Key-Value → Redis → Dictionary/Map**

---

# 12. Wide-Column Database

### Example: Cassandra

Uses a distributed wide-column data model.

It is commonly used for:

- Large datasets
- Distributed systems
- High availability
- High-throughput workloads

### Memory

> **Wide-Column → Cassandra → Distributed large-scale data**

---

# 13. Graph Database

### Example: Neo4j

Stores:

```text
Nodes + Relationships
```

Example:

```text
Alice
  |
  | FRIENDS_WITH
  ↓
Bob
  |
  | FRIENDS_WITH
  ↓
Charlie
```

Useful when relationships are central to the application.

### Examples

- Social networks
- Recommendation systems
- Fraud detection
- Network analysis

### Memory

> **Graph → Neo4j → Connections**

---

# 14. Flexible Schema

NoSQL databases often provide more flexible schemas.

Example:

Document 1:

```json
{
  "name": "Alice",
  "age": 21
}
```

Document 2:

```json
{
  "name": "Bob",
  "age": 22,
  "skills": ["C++", "Java"]
}
```

The documents do not necessarily have exactly the same fields.

### Important

Flexible schema **does not mean no structure at all**.

The application can still enforce its own rules.

---

# 15. Horizontal Scaling

NoSQL databases are often designed for horizontal scaling.

## Vertical Scaling

Make one machine more powerful.

```text
8 GB RAM
   ↓
64 GB RAM
```

**Vertical = Bigger machine**

---

## Horizontal Scaling

Add more machines.

```text
        Database
           |
    ----------------
    |       |      |
  Node 1  Node 2  Node 3
```

**Horizontal = More machines**

---

# 16. Replication

Replication means keeping copies of data on multiple nodes.

```text
          Data
         /    \
      Node 1  Node 2
       Copy    Copy
```

Benefits can include:

- Higher availability
- Fault tolerance
- Read scaling

### Memory

> **Replication = COPY data**

---

# 17. Sharding

Sharding means **splitting data across multiple nodes**.

Example:

```text
Users 1–1000
     ↓
   Node 1

Users 1001–2000
     ↓
   Node 2

Users 2001–3000
     ↓
   Node 3
```

### Memory

> **Sharding = SPLIT data**

---

# 18. Advantages of NoSQL

- Flexible data models
- Horizontal scaling
- Suitable for distributed workloads
- Can handle very large datasets
- Multiple data models available
- Can provide high availability through replication

---

# 19. Disadvantages of NoSQL

- Relationships may be less natural than in relational databases.
- JOIN support varies between databases.
- Data duplication may be used intentionally.
- Consistency and transaction guarantees vary.
- There is no single query language common to all NoSQL databases.
- Poor data modeling can lead to inefficient queries.

---

# 20. When to Use NoSQL?

NoSQL can be suitable when:

- Data structure changes frequently.
- Data is very large.
- Horizontal scaling is important.
- Workloads are distributed.
- High throughput is required.
- Data naturally fits a document, key-value, wide-column, or graph model.

### Examples

- Social media
- Caching
- IoT systems
- Real-time applications
- Recommendation systems
- Large distributed applications

---

# 21. SQL vs NoSQL

| Feature | SQL | NoSQL |
|---|---|---|
| Meaning | Structured Query Language | Not Only SQL |
| Database type | Relational | Non-relational / multiple data models |
| Data model | Tables | Document, Key-Value, Wide-Column, Graph |
| Schema | Usually structured | Often flexible |
| Relationships | Strong support | Depends on database |
| JOINs | Strong support | Varies |
| Transactions | Strong support | Varies |
| Scaling | Vertical + can support horizontal scaling | Often designed for horizontal scaling |
| Data structure | Structured | Flexible |
| Query language | SQL | Depends on database |
| Distributed workloads | Supported by some SQL systems | Common design goal |
| Examples | MySQL, PostgreSQL, Oracle | MongoDB, Redis, Cassandra, Neo4j |

---

# 22. SQL Example

Suppose we have:

```text
Student
+----+-------+
| id | name  |
+----+-------+
|101 | Alice |
+----+-------+
```

And:

```text
Enrollment
+------------+--------+
| student_id | course |
+------------+--------+
|101         | DBMS   |
|101         | OS     |
+------------+--------+
```

To get Alice's courses:

```sql
SELECT Student.name, Enrollment.course
FROM Student
JOIN Enrollment
ON Student.id = Enrollment.student_id;
```

SQL uses relationships and JOINs to combine information.

---

# 23. NoSQL Example

The same information could be stored together in a document:

```json
{
  "student_id": 101,
  "name": "Alice",
  "courses": [
    "DBMS",
    "OS"
  ]
}
```

The related information is stored inside one document.

---

# 24. Important Misconceptions

## ❌ "SQL cannot scale horizontally"

Wrong.

SQL databases can also scale horizontally depending on the database and architecture.

Examples of techniques include:

- Replication
- Sharding
- Partitioning
- Distributed SQL

---

## ❌ "NoSQL has no transactions"

Wrong.

Many NoSQL databases support transactions.

However, the exact transaction capabilities vary by database.

---

## ❌ "NoSQL has no schema"

Not exactly.

NoSQL often means a **more flexible schema**, not the complete absence of structure.

---

## ❌ "NoSQL is always faster"

Wrong.

Performance depends on:

- Query pattern
- Data model
- Indexes
- Workload
- Hardware
- Database implementation
- Architecture

---

# 25. SQL and NoSQL — Simple Example

Imagine a college application.

## SQL Approach

```text
Student
   ↓
Enrollment
   ↓
Course
```

Information is stored in separate related tables.

```text
Primary Key
     ↓
Foreign Key
     ↓
JOIN
```

---

## NoSQL Approach

A student document could contain:

```json
{
  "id": 101,
  "name": "Alice",
  "courses": [
    {
      "name": "DBMS",
      "teacher": "Dr. Sharma"
    },
    {
      "name": "OS",
      "teacher": "Dr. Singh"
    }
  ]
}
```

Related information can be stored together.

---

# 26. SQL vs NoSQL — Decision Guide

```text
                    Need a database?
                           |
                 ---------------------
                 |                   |
          Structured data?      Flexible data?
                 |                   |
                YES                 YES
                 |                   |
           Relationships?       Distributed/
                 |              large-scale?
          ------------             |
          |          |             |
         YES         NO           YES
          |          |             |
         SQL        SQL          NoSQL*
```

`*` NoSQL is not automatically the correct choice; the workload and specific database capabilities matter.

---

# 28. Final Revision

## SQL

```text
SQL
 ↓
Relational
 ↓
Tables
 ↓
Rows + Columns
 ↓
Relationships
 ↓
JOINs
 ↓
Structured Schema
```

### Examples

```text
MySQL
PostgreSQL
Oracle
SQL Server
```

---

## NoSQL

```text
NoSQL
 ↓
Non-Relational
 ↓
Multiple Data Models
 ↓
Document
Key-Value
Wide-Column
Graph
 ↓
Flexible Schema
 ↓
Distributed / Large-Scale Workloads
```

### Examples

```text
MongoDB  → Document
Redis    → Key-Value
Cassandra → Wide-Column
Neo4j    → Graph
```

---

# 29. Super-Easy Memory Trick

### SQL

> **SQL = Tables + Relationships + JOINs**

### NoSQL

> **NoSQL = Flexible Models + Distributed Scaling**

### Sharding vs Replication

> **Sharding = SPLIT data**

> **Replication = COPY data**

### NoSQL Types

> **MongoDB → Document**  
> **Redis → Key-Value**  
> **Cassandra → Wide-Column**  
> **Neo4j → Graph**


-----

# 27. CAP Theorem

CAP theorem applies to **distributed systems**.

It says that when a **network partition** occurs, a distributed system cannot simultaneously guarantee both:

- Strong **Consistency**
- **Availability**

while also tolerating that partition.

CAP:

```text
C = Consistency
A = Availability
P = Partition Tolerance
```

### Consistency

Every read receives the latest successful write according to the system's consistency model.

Simple idea:

```text
Write X = 10
Read → 10
```

---

### Availability

Every request receives a response, even if that response may not contain the newest data.

---

### Partition Tolerance

The system continues operating despite communication failures between nodes.

Example:

```text
Node A  X  Node B
     Network Failure
```

The system must tolerate this partition.

---

# 28. CP

**CP = Consistency + Partition Tolerance**

During a network partition, the system prioritizes consistency.

It may reject/delay some requests rather than return potentially stale data.

Simple idea:

```text
Partition occurs
      ↓
Protect consistency
      ↓
Some requests may fail/wait
```

---

# 29. AP

**AP = Availability + Partition Tolerance**

During a network partition, the system prioritizes availability.

It continues responding, but different nodes may temporarily have different values.

Later, the system can reconcile the data.

Simple idea:

```text
Partition occurs
      ↓
Keep responding
      ↓
Temporary inconsistency possible
```

---

# 30. Eventual Consistency

**Eventual consistency** means that if no new updates continue and the system remains operational, replicas will eventually converge to the same value.

Example:

```text
Node A → value = 20
Node B → value = 15
```

After replication:

```text
Node A → 20
Node B → 20
```

There may be a temporary period where different nodes return different values.

Commonly useful when:

- High availability is important
- Temporary stale reads are acceptable
- Large distributed systems need scalable replication

---

# 31. ACID vs CAP

Do not confuse these concepts.

### ACID

Deals primarily with **transaction properties**:

```text
Atomicity
Consistency
Isolation
Durability
```

### CAP

Deals with **distributed systems under network partitions**:

```text
Consistency
Availability
Partition Tolerance
```

The word **Consistency** appears in both, but it does not mean exactly the same thing.

---

# 32. Quick Revision

```text
DBMS
│
├── Relational Model
│   ├── Tables
│   ├── Rows
│   ├── Columns
│   └── Relationships
│
├── Keys
│   ├── Primary Key
│   ├── Foreign Key
│   ├── Candidate Key
│   ├── Super Key
│   ├── Alternate Key
│   └── Composite Key
│
├── SQL
│   ├── SELECT
│   ├── INSERT
│   ├── UPDATE
│   ├── DELETE
│   ├── WHERE
│   ├── GROUP BY
│   ├── HAVING
│   └── ORDER BY
│
├── Joins
│   ├── INNER
│   ├── LEFT
│   ├── RIGHT
│   ├── FULL
│   ├── CROSS
│   └── SELF
│
├── Normalization
│   ├── 1NF  → Atomic values
│   ├── 2NF  → No partial dependency
│   ├── 3NF  → No transitive dependency
│   ├── BCNF → Every determinant is a super key
│   ├── 4NF  → No problematic multivalued dependency
│   └── 5NF  → No problematic join dependency
│
├── Transactions
│   └── ACID
│       ├── Atomicity
│       ├── Consistency
│       ├── Isolation
│       └── Durability
│
├── Concurrency
│   ├── Dirty Read
│   ├── Non-repeatable Read
│   ├── Phantom Read
│   ├── Locks
│   ├── Deadlock
│   └── 2PL
│
├── Indexes
│   ├── B-Tree / B+ Tree
│   ├── Clustered
│   ├── Non-Clustered
│   └── Composite
│
├── ER Model
│   ├── Entity
│   ├── Attribute
│   ├── Relationship
│   └── Cardinality
│
└── Distributed Databases
    ├── SQL vs NoSQL
    ├── CAP
    ├── CP
    ├── AP
    └── Eventual Consistency
```

# 33. One-Line Memory Sheet

```text
Super Key      → Uniquely identifies row
Candidate Key  → Minimal Super Key
Primary Key    → Selected Candidate Key
Foreign Key    → Connects tables
Composite Key  → Multiple columns as a key

WHERE          → Filter rows
GROUP BY       → Make groups
HAVING         → Filter groups
ORDER BY       → Sort

INNER JOIN     → Matching rows
LEFT JOIN      → All left rows
RIGHT JOIN     → All right rows
FULL JOIN      → All rows from both
CROSS JOIN     → Every combination
SELF JOIN      → Table with itself

1NF            → Atomic
2NF            → No partial dependency
3NF            → No transitive dependency
BCNF           → Every determinant is a super key
4NF            → No problematic multivalued dependency
5NF            → No problematic join dependency

Atomicity      → All or nothing
Consistency    → Valid state
Isolation      → Transactions don't improperly interfere
Durability     → Committed data stays

Dirty Read     → Read uncommitted data
Non-repeatable → Same row gives different value
Phantom        → Different set of rows

Shared Lock    → Read
Exclusive Lock → Write
Deadlock       → Transactions wait for each other
2PL            → Growing then shrinking

Index          → Faster lookup, extra storage/write cost
B+ Tree        → Efficient search and range scans
Composite      → Index on multiple columns

Entity         → Real-world object
Attribute      → Property
Relationship    → Association
Cardinality     → Number of associations

CAP            → Consistency, Availability, Partition Tolerance
CP             → Consistency + Partition Tolerance
AP             → Availability + Partition Tolerance
Eventual       → Replicas eventually converge
```

# 34. Important Interview Questions

1. What is DBMS?
2. DBMS vs RDBMS?
3. What is a primary key?
4. Primary key vs unique key?
5. Primary key vs foreign key?
6. Super key vs candidate key?
7. What is a composite key?
8. What is normalization?
9. Explain 1NF, 2NF and 3NF.
10. What is BCNF?
11. Difference between 3NF and BCNF?
12. What are insert, update and delete anomalies?
13. What is a transaction?
14. Explain ACID properties.
15. What is a dirty read?
16. Non-repeatable read vs phantom read?
17. What is a deadlock?
18. What is 2PL?
19. What is an index?
20. Clustered vs non-clustered index?
21. What is a composite index?
22. What is a B+ tree?
23. What is an ER model?
24. Explain 1:1, 1:N and M:N relationships.
25. SQL vs NoSQL?
26. What is CAP theorem?
27. CP vs AP?
28. What is eventual consistency?
