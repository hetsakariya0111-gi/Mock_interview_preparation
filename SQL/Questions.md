SQL Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. What is SQL & command categories
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SQL = Structured Query Language, used to communicate with relational databases
DDL (Data Definition Language) — defines structure: CREATE, ALTER, DROP, TRUNCATE
DML (Data Manipulation Language) — manipulates data: INSERT, UPDATE, DELETE, SELECT (sometimes classified separately as DQL)
DCL (Data Control Language) — controls access: GRANT, REVOKE
TCL (Transaction Control Language) — manages transactions: COMMIT, ROLLBACK, SAVEPOINT

2. DBMS vs. RDBMS
~~~~~~~~~~~~~~~~~

DBMS — software to store/manage data, doesn't enforce relationships or a strict tabular structure (e.g. file systems, some NoSQL)
RDBMS — a DBMS based on the relational model, data stored in tables with rows/columns, enforces relationships via keys
Main RDBMS features: ACID compliance, data integrity constraints, relationships via foreign keys, normalization support, SQL-based querying

3. Key types
~~~~~~~~~~~~

Primary Key — uniquely identifies each row, cannot be NULL, one per table
Foreign Key — references a primary key in another table, enforces referential integrity
Unique Key — ensures all values in a column are distinct, can allow one NULL (DB-dependent)
Composite Key — primary key made of two or more columns combined
Surrogate Key — an artificial, system-generated key (like an auto-increment ID) with no business meaning, used instead of a natural key

4. Referential Integrity, ON DELETE CASCADE/SET NULL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Referential Integrity — ensures foreign key values always correspond to an existing primary key value in the referenced table (no orphaned records)
ON DELETE CASCADE — automatically deletes child rows when the referenced parent row is deleted
ON DELETE SET NULL — sets the foreign key column to NULL in child rows when the parent row is deleted (column must be nullable)

5. Functional Dependency (FD)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A relationship where one attribute (or set of attributes) uniquely determines another attribute — written as A → B (B is functionally dependent on A)
Role in design: forms the basis for normalization — identifying FDs helps eliminate redundancy and design properly structured tables

6. Types of Functional Dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Trivial — B is a subset of A (e.g. {name} → {name})
Non-Trivial — B is not a subset of A
Full — B depends on the entire composite key, not just part of it
Partial — B depends on only part of a composite key (violates 2NF)
Transitive — B depends on A indirectly, through another attribute C (A → C → B) (violates 3NF)

7. Normalization & Anomalies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Normalization — organizing tables to reduce data redundancy and improve data integrity, done by splitting tables and defining relationships based on FDs
Necessary to avoid Anomalies:
Insertion — can't add data without unrelated data also being present
Update — same data duplicated in multiple places, risk of inconsistency if not all updated
Deletion — deleting one piece of data unintentionally removes other needed data

8. 1NF, 2NF, 3NF
~~~~~~~~~~~~~~~~

1NF — each column holds atomic (indivisible) values, no repeating groups/arrays in a single field
2NF — must be in 1NF + no partial dependency (non-key attributes depend on the whole composite primary key, not just part of it)
3NF — must be in 2NF + no transitive dependency (non-key attributes depend only on the primary key, not on other non-key attributes)

9. BCNF vs. 3NF
~~~~~~~~~~~~~~~

BCNF (Boyce-Codd Normal Form) — a stricter version of 3NF
Rule: for every functional dependency A → B, A must be a super key
Difference from 3NF: 3NF allows some edge cases where a non-prime attribute determines part of a candidate key; BCNF removes all such anomalies, even in those edge cases

10. SQL Joins
~~~~~~~~~~~~~

INNER JOIN — returns only matching rows from both tables
sql
  SELECT * FROM A INNER JOIN B ON A.id = B.a_id;
LEFT JOIN — all rows from left table + matched rows from right (unmatched = NULL)
RIGHT JOIN — all rows from right table + matched rows from left
FULL JOIN — all rows from both tables, matched where possible, NULL where not
SELF JOIN — a table joined with itself, using aliases
sql
  SELECT a.name, b.name FROM emp a, emp b WHERE a.manager_id = b.id;
CROSS JOIN — Cartesian product, every row from A paired with every row from B

11. WHERE vs. HAVING
~~~~~~~~~~~~~~~~~~~~

WHERE — filters rows before grouping/aggregation, cannot use aggregate functions
HAVING — filters groups after aggregation (used with GROUP BY), can use aggregate functions like COUNT, SUM
Yes, they can be used together — WHERE filters rows first, then GROUP BY groups them, then HAVING filters the groups
sql
  SELECT dept, COUNT(*) FROM emp WHERE age > 25 GROUP BY dept HAVING COUNT(*) > 5;

12. DELETE vs. TRUNCATE vs. DROP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

DELETE — removes specific rows (with WHERE), logged row-by-row, slower, can be rolled back, doesn't reset auto-increment
TRUNCATE — removes all rows at once, minimal logging, faster, resets auto-increment, rollback capability is DB-dependent (limited)
DROP — removes the entire table structure and data permanently, cannot be rolled back (in most DBs)

13. Subqueries: Correlated vs. Non-correlated
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Subquery — a query nested inside another query
Non-correlated — runs independently of the outer query, executed once
sql
  SELECT * FROM emp WHERE salary > (SELECT AVG(salary) FROM emp);
Correlated — references columns from the outer query, executed once per row of the outer query
sql
  SELECT * FROM emp e WHERE salary > (SELECT AVG(salary) FROM emp WHERE dept = e.dept);

14. UNION vs. UNION ALL vs. INTERSECT vs. EXCEPT/MINUS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

UNION — combines results from two queries, removes duplicates
UNION ALL — combines results, keeps duplicates, faster (no dedup step)
INTERSECT — returns only rows common to both queries
EXCEPT/MINUS — returns rows from the first query that don't appear in the second

15. Window Functions
~~~~~~~~~~~~~~~~~~~~

Perform calculations across a set of rows related to the current row, without collapsing rows like GROUP BY does
ROW_NUMBER() — assigns a unique sequential number to each row within a partition
RANK() — assigns rank, skips numbers after ties (e.g. 1, 2, 2, 4)
DENSE_RANK() — assigns rank, no gaps after ties (e.g. 1, 2, 2, 3)
LEAD()/LAG() — accesses the value from the next/previous row within the partition
sql
  SELECT name, salary, RANK() OVER (ORDER BY salary DESC) FROM emp;

16. ACID Properties
~~~~~~~~~~~~~~~~~~~

Atomicity — a transaction is all-or-nothing; if one part fails, the whole transaction rolls back
Consistency — a transaction brings the database from one valid state to another, respecting all constraints
Isolation — concurrent transactions don't interfere with each other, as if executed sequentially
Durability — once committed, changes persist even after a system crash

17. Isolation Levels & Concurrency Problems
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Isolation levels (weakest to strongest): Read Uncommitted → Read Committed → Repeatable Read → Serializable
Dirty Read — reading uncommitted changes from another transaction that might get rolled back
Non-repeatable Read — reading the same row twice in a transaction and getting different values because another transaction modified it in between
Phantom Read — a query run twice returns different sets of rows because another transaction inserted/deleted rows matching the condition

18. Index: Clustered vs. Non-Clustered
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Index — a data structure that improves the speed of data retrieval, at the cost of extra storage and slower writes
Clustered — determines the physical storage order of table data; a table can have only one (since data can only be sorted one way)
Non-Clustered — a separate structure that holds a sorted reference (pointer) to the actual data rows; a table can have multiple

19. Views: Standard vs. Materialized
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

View — a virtual table based on the result of a stored SQL query
Standard View — doesn't store data, runs the underlying query fresh every time it's accessed — always up to date but slower for complex queries
Materialized View — stores the actual query result physically, needs periodic refreshing — faster to read, but can serve stale data until refreshed

20. Triggers
~~~~~~~~~~~~

Special stored procedures that automatically execute in response to specific events (INSERT, UPDATE, DELETE) on a table
BEFORE — runs before the triggering event (e.g. validate/modify data before insert)
AFTER — runs after the event completes (e.g. logging, updating related tables)
INSTEAD OF — replaces the triggering event entirely, commonly used on views to make them updatable
Common use cases: auditing/logging changes, enforcing complex business rules, maintaining derived/summary data

21. Nth highest salary query
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using subquery:
sql
  SELECT MAX(salary) FROM emp 
  WHERE salary < (SELECT MAX(salary) FROM emp);  -- 2nd highest example
Using window function (more flexible, works for any N):
sql
  SELECT DISTINCT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk FROM emp
  ) t WHERE rnk = N;

22. NVL() vs. IFNULL() vs. COALESCE() vs. ISNULL()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

NVL() — Oracle-specific, replaces NULL with a specified value, takes exactly 2 arguments
IFNULL() — MySQL-specific, similar to NVL, takes exactly 2 arguments
COALESCE() — ANSI SQL standard, works across most databases, returns the first non-NULL value from a list of any length
ISNULL() — SQL Server-specific, takes 2 arguments, similar to NVL/IFNULL (careful: MySQL's ISNULL() is different — checks if a value IS NULL, returns boolean)

23. SQL Injection & Prevention
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SQL Injection — an attack where malicious SQL code is inserted into input fields to manipulate or access the database improperly
Prevention:
Prepared Statements/Parameterized Queries — treats input as data, not executable code
ORMs (like Mongoose, Sequelize, Prisma) — abstract raw SQL, reducing direct injection risk
Input validation/sanitization, least-privilege database accounts, avoiding string concatenation for queries

24. Deadlock
~~~~~~~~~~~~

Occurs when two or more transactions are each waiting for a resource locked by the other, creating a cycle where none can proceed
Detection: the DBMS periodically checks for cycles in a "wait-for graph" of transactions and locked resources
Resolution: the DBMS picks a "victim" transaction (usually the one with least cost/progress) and forcibly rolls it back, releasing its locks so the others can proceed

25. Denormalization vs. Normalization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Normalization — organizing data to reduce redundancy, improve integrity, splitting into related tables
Denormalization — intentionally introducing redundancy by combining tables/duplicating data, to reduce the number of joins needed
When to denormalize: read-heavy systems where query performance matters more than write efficiency/storage — e.g. reporting/analytics systems, caching frequently-joined data, high-traffic APIs needing fast reads