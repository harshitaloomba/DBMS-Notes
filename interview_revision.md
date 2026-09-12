# DBMS — Interview Revision Short Notes

## 1. DBMS Basics

### What is DBMS?
- DBMS = **Database Management System**.
- Software used to store, manage, retrieve, update and delete data.
- Examples: MySQL, PostgreSQL, Oracle, SQL Server.

### DBMS vs RDBMS
- **DBMS:** General database management system.
- **RDBMS:** Stores data mainly in related tables.
- Examples of RDBMS: MySQL, PostgreSQL, Oracle.

### Why DBMS?
- Reduces data redundancy.
- Maintains consistency.
- Provides security.
- Supports multiple users.
- Provides backup and recovery.

---

# 2. Relational Model

### Table
- Collection of related data.
- Also called a **relation**.

### Row
- One complete record.
- Also called a **tuple**.

### Column
- A property of the data.
- Also called an **attribute**.

### Relationship
- Connection between tables.
- Usually created using keys.

---

# 3. Keys

### Super Key
- Any set of columns that uniquely identifies a row.
- May contain extra columns.

### Candidate Key
- **Minimal Super Key**.
- No unnecessary column.

### Primary Key
- Candidate key selected to uniquely identify rows.
- Unique + NOT NULL.

### Alternate Key
- Candidate key that was **not selected** as the primary key.

### Foreign Key
- Column that refers to a key in another table.
- Used to connect tables and maintain referential integrity.

### Composite Key
- Key made using **2 or more columns**.

### Easy Flow

```text
Super Key
   ↓ remove unnecessary columns
Candidate Key
   ↓ choose one
Primary Key
```

---

# 4. SQL Commands

### SELECT
Get data.

```sql
SELECT * FROM Employee;
```

### INSERT
Add data.

```sql
INSERT INTO Employee VALUES (1, 'Alice');
```

### UPDATE
Change data.

```sql
UPDATE Employee
SET salary = 50000
WHERE emp_id = 1;
```

### DELETE
Remove data.

```sql
DELETE FROM Employee
WHERE emp_id = 1;
```

### WHERE
Filters **rows**.

```sql
WHERE salary > 50000
```

### GROUP BY
Creates groups.

```sql
SELECT dept_id, COUNT(*)
FROM Employee
GROUP BY dept_id;
```

### HAVING
Filters **groups**.

```sql
HAVING COUNT(*) > 5
```

### ORDER BY
Sorts results.

```sql
ORDER BY salary DESC;
```

### Important Memory

```text
WHERE   → Rows
GROUP BY → Groups
HAVING  → Groups after grouping
ORDER BY → Sorting
```

---

# 5. Aggregate Functions

Used to calculate values from multiple rows.

```text
COUNT() → Number of rows
SUM()   → Total
AVG()   → Average
MIN()   → Minimum
MAX()   → Maximum
```

Example:

```sql
SELECT dept_id, AVG(salary)
FROM Employee
GROUP BY dept_id;
```

---

# 6. Joins

Joins combine data from multiple tables.

### INNER JOIN
- Returns only matching rows.

```text
INNER → Matches only
```

### LEFT JOIN
- Returns all rows from left table.
- Matching rows from right table.
- No match → NULL.

```text
LEFT → Everything from left
```

### RIGHT JOIN
- Returns all rows from right table.

```text
RIGHT → Everything from right
```

### FULL OUTER JOIN
- Returns all rows from both tables.
- Unmatched values become NULL.
- MySQL does not directly support FULL OUTER JOIN.

### CROSS JOIN
- Every row of first table combines with every row of second table.
- If A has 3 rows and B has 4 rows → 12 rows.

### SELF JOIN
- A table is joined with itself.
- Common example: Employee → Manager.

### Join Memory

```text
INNER → Matching
LEFT  → All Left
RIGHT → All Right
FULL  → All Both
CROSS → Every Combination
SELF  → Same Table
```

---

# 7. Normalization

### Why normalization?
- Reduce duplicate data.
- Avoid anomalies.
- Improve consistency.

### Insert Anomaly
- Cannot add some information without unrelated information.

### Update Anomaly
- Same information appears in multiple rows.
- Must update it everywhere.

### Delete Anomaly
- Deleting one record accidentally removes other useful information.

---

## Normal Forms

### 1NF
- Values must be **atomic/single**.
- No multiple values in one cell.

```text
1NF → Atomic
```

### 2NF
- Must be in 1NF.
- No **partial dependency**.
- Mainly matters when there is a composite key.

```text
2NF → No Partial Dependency
```

### 3NF
- Must be in 2NF.
- No **transitive dependency**.

Example:

```text
emp_id → dept_id
dept_id → dept_name

Therefore:
emp_id → dept_name
```

Fix by separating Employee and Department.

```text
3NF → No Transitive Dependency
```

### BCNF
- Stronger than 3NF.
- For every dependency `X → Y`, **X must be a Super Key**.

```text
BCNF → Every Determinant is a Super Key
```

### 4NF
- Deals with problematic **multivalued dependencies**.

```text
4NF → No Multivalued Dependency
```

### 5NF
- Deals with problematic **join dependencies**.
- Decomposition should be lossless.

```text
5NF → No Join Dependency
```

### Normalization Memory

```text
1NF  → Atomic
2NF  → No Partial
3NF  → No Transitive
BCNF → Determinant is Key
4NF  → No Multivalued
5NF  → No Join Dependency
```

---

# 8. Transactions

### What is a Transaction?
- A group of database operations treated as one unit.

Example: Bank transfer

```text
1. Deduct ₹1000 from A
2. Add ₹1000 to B
```

Both should happen, or neither should happen.

---

# 9. ACID

### Atomicity
- **All or nothing**.

### Consistency
- Database remains valid according to its rules.

### Isolation
- Concurrent transactions should not improperly interfere.

### Durability
- Once committed, data remains even after a crash.

### Memory

```text
A → All or Nothing
C → Correct/Valid State
I → Independent Transactions
D → Data Stays
```

---

# 10. Concurrency

Concurrency = Multiple transactions running at the same time/overlapping.

## Problems

### Dirty Read
- Reading **uncommitted** data.

```text
T1 writes → T2 reads → T1 rollback
```

T2 read data that was never committed.

### Non-Repeatable Read
- Same row is read twice.
- Value changes between the reads.

```text
Read → Update by another transaction → Read again
```

### Phantom Read
- Same query returns a **different set of rows**.

```text
First query → 5 rows
Another transaction inserts a matching row
Second query → 6 rows
```

### Memory

```text
Dirty Read       → Uncommitted value
Non-repeatable   → Same row, different value
Phantom          → Different set of rows
```

---

# 11. Locks

### Shared Lock (S)
- Used for reading.
- Multiple transactions can generally read simultaneously.

```text
S → Read
```

### Exclusive Lock (X)
- Used for writing/modifying.
- Prevents conflicting access.

```text
X → Write
```

---

# 12. Deadlock

Deadlock = Transactions wait for each other forever.

Example:

```text
T1 locks A
T2 locks B

T1 waits for B
T2 waits for A
```

Both are stuck.

### Solution ideas
- Detect deadlock.
- Roll back one transaction.
- Timeout.
- Use prevention strategies.

---

# 13. Two-Phase Locking (2PL)

Two phases:

### Growing Phase
- Acquire locks.
- Cannot release locks.

### Shrinking Phase
- Release locks.
- Cannot acquire new locks.

```text
Growing  → LOCK
Shrinking → UNLOCK
```

---

# 14. Indexes

### What is an Index?
- Data structure that makes searching/retrieving data faster.
- Similar to the index of a book.

Example:

```sql
CREATE INDEX idx_name
ON Employee(emp_name);
```

### Advantages
- Faster searching.
- Can improve queries using WHERE, JOIN and ORDER BY.

### Disadvantages
- Takes extra storage.
- INSERT/UPDATE/DELETE can become slower because indexes also need updating.

### B-Tree / B+ Tree
- Balanced tree structures used for efficient searching.
- B+ Trees are especially useful for ordered and range searches.

---

# 15. Clustered vs Non-Clustered Index

### Clustered Index
- Determines how table data is organized around the indexed key, depending on the DBMS.
- Typically one clustered organization per table.

### Non-Clustered Index
- Separate index structure.
- Points/references to the actual table rows.
- Multiple can usually exist.

### Memory

```text
Clustered     → Data organized with index
Non-Clustered → Separate index + row reference
```

---

# 16. Composite Index

Index on multiple columns.

```sql
CREATE INDEX idx_dept_salary
ON Employee(dept_id, salary);
```

For:

```text
(dept_id, salary)
```

the index generally works best when the query uses the **leftmost column(s)**.

```text
(dept_id, salary)
     ↑
  leftmost
```

---

# 17. ER Model

ER = **Entity Relationship Model**

Used for designing a database.

### Entity
- Real-world object.
- Example: Student, Employee, Course.

### Attribute
- Property of an entity.
- Example: Student → ID, Name, Age.

### Relationship
- Association between entities.

```text
Student ─── ENROLLS ─── Course
```

### Cardinality
Shows how many entities can be related.

```text
1:1 → One-to-One
1:N → One-to-Many
M:N → Many-to-Many
```

Example:

```text
Department 1 ─── N Employee
```

One department can have many employees.

### Many-to-Many

```text
Student N ─── N Course
```

Usually implemented using a junction table:

```text
Enrollment(student_id, course_id)
```

---

# 18. SQL vs NoSQL

### SQL
- Relational/table-based.
- Structured schema.
- Strong support for joins and relationships.
- Examples: MySQL, PostgreSQL, Oracle.

### NoSQL
- Non-relational data models.
- Often flexible schema.
- Common models: Document, Key-Value, Wide-Column, Graph.
- Examples: MongoDB, Redis, Cassandra, Neo4j.

### Quick Comparison

| SQL | NoSQL |
|---|---|
| Tables | Document/key-value/etc. |
| Structured schema | Often flexible schema |
| Strong joins | Joins vary |
| Strong transaction support | Capabilities vary |
| Relational data | Flexible/distributed workloads |

---

# 19. CAP Theorem

CAP is important for **distributed systems**.

```text
C → Consistency
A → Availability
P → Partition Tolerance
```

When a network partition occurs, a distributed system cannot guarantee both **strong consistency and availability** at the same time.

### Consistency
- Reads see the appropriate/latest value according to the system's consistency guarantee.

### Availability
- Every request receives a response.

### Partition Tolerance
- System continues despite network communication failure between nodes.

---

# 20. CP

```text
CP = Consistency + Partition Tolerance
```

During a partition:

- Protect consistency.
- Some requests may be rejected or delayed.

Memory:

```text
CP → Correct data is more important than always responding
```

---

# 21. AP

```text
AP = Availability + Partition Tolerance
```

During a partition:

- Continue responding.
- Temporary inconsistent/stale values may be returned.

Memory:

```text
AP → Keep responding, consistency may be temporary
```

---

# 22. Eventual Consistency

- Replicas may temporarily have different values.
- If updates stop and the system continues working, replicas eventually converge.

Example:

```text
Node A → 20
Node B → 15

After synchronization:

Node A → 20
Node B → 20
```

---

# 23. ACID vs CAP

### ACID
Deals mainly with **transaction properties**:

```text
Atomicity
Consistency
Isolation
Durability
```

### CAP
Deals with **distributed systems and network partitions**:

```text
Consistency
Availability
Partition Tolerance
```

Important:
- The word "Consistency" appears in both.
- The concepts are not exactly the same.

---

# 24. Most Important Interview One-Liners

### DBMS
> DBMS is software used to store, manage and retrieve data from a database.

### Primary Key
> A primary key uniquely identifies each row and cannot contain NULL values.

### Foreign Key
> A foreign key is used to create a relationship between tables and maintain referential integrity.

### Normalization
> Normalization organizes data to reduce redundancy and avoid anomalies.

### 1NF
> 1NF means values are atomic and there are no repeating groups.

### 2NF
> 2NF means 1NF + no partial dependency on a part of a composite key.

### 3NF
> 3NF means 2NF + no transitive dependency.

### BCNF
> BCNF requires every determinant to be a super key.

### Transaction
> A transaction is a logical unit of database operations.

### ACID
> ACID stands for Atomicity, Consistency, Isolation and Durability.

### Index
> An index is a data structure that speeds up data retrieval but requires extra storage and maintenance.

### Deadlock
> Deadlock occurs when transactions wait for each other indefinitely.

### INNER JOIN
> Returns only matching rows from both tables.

### LEFT JOIN
> Returns all rows from the left table and matching rows from the right table.

### WHERE vs HAVING
> WHERE filters rows before grouping, while HAVING filters groups after GROUP BY.

### CAP
> CAP says that during a network partition, a distributed system cannot simultaneously guarantee strong consistency and availability.

---

# 25. Last-Minute DBMS Revision

```text
KEYS
Super → Candidate → Primary
Foreign → Connects tables
Composite → Multiple columns

SQL
WHERE → Rows
GROUP BY → Groups
HAVING → Groups
ORDER BY → Sort

JOINS
INNER → Match
LEFT → All Left
RIGHT → All Right
FULL → All
CROSS → Every Combination
SELF → Same Table

NORMALIZATION
1NF → Atomic
2NF → No Partial
3NF → No Transitive
BCNF → Determinant is Key
4NF → No Multivalued
5NF → No Join Dependency

ACID
A → All or Nothing
C → Valid State
I → No Improper Interference
D → Committed Data Stays

CONCURRENCY
Dirty → Uncommitted value
Non-repeatable → Same row, different value
Phantom → Different rows

LOCKS
S → Read
X → Write

2PL
Growing → Acquire
Shrinking → Release

INDEX
Faster reads
More storage
Slower writes

ER
Entity → Object
Attribute → Property
Relationship → Association
Cardinality → 1:1, 1:N, M:N

DISTRIBUTED
CAP → C, A, P
CP → Consistency + Partition Tolerance
AP → Availability + Partition Tolerance
Eventual → Replicas eventually converge
```
