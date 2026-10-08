# .NET Interview Preparation — Day 4
## SQL Server & Database Interview Questions

## 1. SQL Joins
A JOIN combines data from two or more tables based on a related column.

- INNER JOIN — only matching rows from both tables.
- LEFT JOIN — all left rows + matching right rows; otherwise NULL.
- RIGHT JOIN — all right rows + matching left rows; otherwise NULL.
- FULL OUTER JOIN — all rows from both tables.

**Shortcut:** INNER = matching only | LEFT = everything left | RIGHT = everything right | FULL = everything both

**Interview answer:**  
“A JOIN is used to combine data from two or more tables based on a related column. INNER JOIN returns only matching rows from both tables. LEFT JOIN returns all rows from the left table and matching rows from the right table. RIGHT JOIN returns all rows from the right table and matching rows from the left table. FULL OUTER JOIN returns all rows from both tables, including matching and non-matching rows.”

## 2. Primary Key vs Foreign Key
**Primary Key:** Uniquely identifies each row; cannot contain duplicate or NULL values.  
**Foreign Key:** Creates a relationship by referencing a key in another table, usually the primary key.

**Shortcut:** Primary Key = Who is this row? | Foreign Key = Which related record does it belong to?

## 3. Index in SQL Server
An Index improves query performance by making data retrieval faster.

**Clustered Index**
- Determines the order of data rows based on the index key.
- A table can have only one clustered index.
- A Primary Key is not always clustered, although SQL Server commonly creates a clustered index for it by default when appropriate.

**Non-Clustered Index**
- Separate structure containing indexed values and references to actual rows.
- A table can have multiple non-clustered indexes.

**Shortcut:** Clustered → Data order → Only 1 | Non-Clustered → Separate lookup structure → Multiple

## 4. Composite Index
A Composite Index is created on two or more columns.

```sql
CREATE NONCLUSTERED INDEX IX_Employee_Department_Status
ON Employee(DepartmentId, Status);
```

Use it when queries frequently filter/search using multiple columns together.

**Important:** Column order matters; `(DepartmentId, Status)` is not always equivalent to `(Status, DepartmentId)`.

**Shortcut:** Composite = Multiple columns in one index.

## 5. Indexing Strategy — 10 Million Rows
For:

```sql
SELECT *
FROM Employee
WHERE DepartmentId = 10
  AND Status = 'Active';
```

A suitable approach is:

```sql
CREATE NONCLUSTERED INDEX IX_Employee_Department_Status
ON Employee(DepartmentId, Status);
```

It can help SQL Server find required records faster instead of scanning the whole table.

## 6. Stored Procedure
A Stored Procedure is a predefined set of SQL statements stored in SQL Server. It can accept parameters and perform SELECT, INSERT, UPDATE, and DELETE.

```sql
CREATE PROCEDURE GetEmployeesByDepartment
    @DepartmentId INT
AS
BEGIN
    SELECT EmployeeId, Name, DepartmentId
    FROM Employee
    WHERE DepartmentId = @DepartmentId;
END
```

Execute:
```sql
EXEC GetEmployeesByDepartment @DepartmentId = 10;
```

**Why:** reusable database logic, centralized complex queries, security, simpler database operations.

**Interview answer:**  
“A Stored Procedure is a predefined set of SQL statements stored in SQL Server. It can accept parameters and perform operations like SELECT, INSERT, UPDATE, and DELETE. We use stored procedures to make database logic reusable, centralize complex queries, improve security, and simplify database operations.”

## 7. View
A View is a **virtual table based on a SELECT query**.

```sql
CREATE VIEW vw_ActiveEmployees
AS
SELECT EmployeeId, Name, DepartmentId
FROM Employee
WHERE Status = 'Active';
```

**Why:** simplify complex queries, reuse query logic, provide controlled access to data.

**View vs Stored Procedure**
- View: virtual table, queried like a table, mainly SELECT, no direct parameters.
- Stored Procedure: executable SQL logic, can accept parameters and perform broader operations.

**Shortcut:** View = Virtual table + SELECT | Stored Procedure = Executable SQL logic + parameters

## 8. Function
A Function performs a specific operation and **returns a value or a table**.

**Function vs Stored Procedure**
- Function must return a value/table.
- Function can be used in a SELECT statement.
- Stored Procedure does not have to return a value.
- Stored Procedure can perform SELECT, INSERT, UPDATE, DELETE.

**Shortcut:** Function → Must return something | Procedure → Performs an operation

**Remember:** “Function returns; Procedure performs.”

## 9. Trigger
A Trigger automatically executes when a specific event occurs, such as INSERT, UPDATE, or DELETE.

**Uses:** auditing, logging, automatically maintaining related data.

**Interview answer:**  
“A Trigger is a database object that automatically executes when a specific event occurs, such as INSERT, UPDATE, or DELETE. We commonly use triggers for auditing, logging, or automatically maintaining related data.”

**Shortcut:** Trigger = Event happens → Trigger automatically executes.

## 10. CTE — Common Table Expression
A CTE is a temporary named result set defined using `WITH` and used within a single SQL statement.

```sql
WITH ActiveEmployees AS
(
    SELECT EmployeeId, Name, DepartmentId
    FROM Employee
    WHERE Status = 'Active'
)
SELECT *
FROM ActiveEmployees;
```

**Why:** simplify complex queries, improve readability, reuse logic within the statement, recursive queries.

**Shortcut:** CTE → WITH → Temporary result → One SQL statement.

## 11. Temp Table vs Table Variable
Both temporarily store data.

**Temp Table:** `#TempTable` — generally better for larger temporary datasets and complex operations; supports indexes and statistics.

**Table Variable:** `@TableVariable` — generally suitable for smaller datasets and simpler operations; more limited.

**Shortcut:** `#` → larger/complex | `@` → smaller/simple

## 12. Normalization vs Denormalization
**Normalization:** Organize data into multiple related tables to reduce duplication and improve consistency.

**Denormalization:** Intentionally combine/duplicate data to improve read/query performance, especially in read-heavy systems.

**Shortcut:** Normalization → Less duplication → Better consistency | Denormalization → More duplication → Faster reads

## 13. Transactions
A Transaction is a group of database operations treated as a **single unit of work**.

```sql
BEGIN TRANSACTION;

UPDATE Account
SET Balance = Balance - 1000
WHERE AccountId = 1;

UPDATE Account
SET Balance = Balance + 1000
WHERE AccountId = 2;

COMMIT TRANSACTION;
```

If something fails:
```sql
ROLLBACK TRANSACTION;
```

**COMMIT:** permanently saves changes.  
**ROLLBACK:** undoes transaction changes.

**Shortcut:** BEGIN → Operations → COMMIT = Save | Error → ROLLBACK = Undo

## 14. ACID Properties
- **A — Atomicity:** all operations succeed or all roll back.
- **C — Consistency:** database remains valid/consistent.
- **I — Isolation:** concurrent transactions do not incorrectly interfere.
- **D — Durability:** committed changes remain saved after failure.

**Shortcut:** A = All or nothing | C = Correct state | I = Independent transactions | D = Data stays saved

## 15. Isolation Levels
Isolation level controls how concurrent transactions see each other’s changes.

1. **Read Uncommitted** — can read uncommitted changes; dirty reads possible.
2. **Read Committed** — reads only committed data; default isolation level in SQL Server.
3. **Repeatable Read** — prevents changes to rows already read until the transaction completes.
4. **Serializable** — highest standard isolation; strongest protection but can reduce concurrency.

SQL Server also supports **Snapshot** and **Read Committed Snapshot**.

**Shortcut:**  
Read Uncommitted → Dirty reads allowed  
Read Committed → Only committed data  
Repeatable Read → Same row stays consistent  
Serializable → Strongest standard isolation

## Day 4 Quick Revision

| Topic | Remember |
|---|---|
| JOIN | Combine related table data |
| Primary Key | Uniquely identifies a row |
| Foreign Key | Creates table relationship |
| Index | Faster data retrieval |
| Clustered | Data order, one per table |
| Non-Clustered | Separate lookup structure, multiple |
| Composite Index | Two or more columns |
| Stored Procedure | Reusable SQL logic |
| View | Virtual table / SELECT |
| Function | Must return value/table |
| Trigger | Automatically executes on an event |
| CTE | `WITH` + temporary named result |
| Temp Table | `#`, generally larger/complex temporary data |
| Table Variable | `@`, generally smaller/simple data |
| Normalization | Reduce duplication |
| Denormalization | Improve read performance |
| Transaction | Unit of work |
| COMMIT | Save changes |
| ROLLBACK | Undo changes |
| ACID | Atomicity, Consistency, Isolation, Durability |
| Isolation | Controls concurrent transaction behavior |

## Important Interview Corrections
1. Index does not mean unique identification; its main purpose is faster data retrieval.
2. Primary Key is not always a clustered index.
3. Composite Index = two or more columns.
4. View = virtual table based on a SELECT query.
5. Function must return a value or table.
6. Trigger reacts automatically to an event.
7. CTE exists for one SQL statement.
8. Transaction = one unit of work; COMMIT saves, ROLLBACK undoes.
9. SQL Server default isolation level = Read Committed.
10. Function returns; Procedure performs.
