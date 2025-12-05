### 1. How to optimize a query?

Best practices for optimizing MySQL queries:

Use indexes (especially on WHERE, JOIN, ORDER BY, GROUP BY)

Avoid SELECT \* — always specify required columns

Use LIMIT for large data

Use proper joins instead of subqueries wherever possible

Use EXPLAIN / ANALYZE

Avoid functions in WHERE clause (WHERE YEAR(date) etc.)

Use EXISTS instead of IN for large datasets

Normalize tables (but denormalize if necessary for performance)

Use caching (Redis / Query Cache)

Example:

```sql
EXPLAIN SELECT id, name FROM users WHERE email = 'test@gmail.com';
```

### 2. Difference between ENUM vs SET

ENUM SET
Only one value can be selected Multiple values can be selected
Index-based Bitmask-based
Faster for single choice Slightly slower
Example: status Example: tags
status ENUM('active','inactive')

skills SET('php','js','sql')

### 3. Composite Key vs Compound Key

✅ Both are the same concept — a primary key made up of more than one column.

PRIMARY KEY (user_id, role_id);

Used for many-to-many relationships.

### 4. Toggle is_active in a single query

Flip 1 → 0 and 0 → 1

```sql
UPDATE users
SET is_active = CASE
WHEN is_active = 1 THEN 0
ELSE 1
END;
```

OR more clean:

UPDATE users SET is_active = 1 - is_active;

### 5. Database Design Example – ICC Tournament System

```
tournament_types
id | name
1 | 50 Over World Cup
2 | 20 Over World Cup

teams
id | team_name
1 | India
2 | Sri Lanka

team_tournament (Many-to-Many)
id | tournament_type_id | team_id

players
id | name | address

player_teams
id | player_id | team_id

matches
id | match_no | tournament_id

match_schedule
id | match_id | team_id
1 | 1 | 1
2 | 1 | 2
```

✅ Normalized
✅ Avoids redundancy
✅ Supports scalability

### 6. What is a Trigger?

A Trigger is an automatic action executed on INSERT, UPDATE, or DELETE.

CREATE TRIGGER before_insert_users
BEFORE INSERT ON users
FOR EACH ROW
BEGIN
SET NEW.created_at = NOW();
END;

❗ Use carefully, can affect performance.

### 7. What is a Stored Procedure?

A Stored Procedure is a reusable, pre-compiled SQL block.

```sql
CREATE PROCEDURE getUserById(IN userId INT)
BEGIN
SELECT \* FROM users WHERE id = userId;
END;
```

Use:

```sql
CALL getUserById(5);
```

### 8. Self Join Example

A table joins to itself.

```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employee e1
LEFT JOIN employee e2 ON e1.manager_id = e2.id;
```

### 9. What is the N+1 problem in SQL?

Occurs when one query loads data and then runs separate queries in a loop.

❌ Bad example:

```sql
SELECT _ FROM users;
SELECT _ FROM orders WHERE user_id = 1;
SELECT \* FROM orders WHERE user_id = 2;
```

✅ Best Solution:

```sql
SELECT u._, o._
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

### 10. Clustered vs Non-Clustered Index

    Clustered Index Non-Clustered Index
    Reorders actual data Separate structure
    Only 1 per table Multiple allowed
    Faster for range queries Used for search
    Default on Primary Key Separate index

### 11. What is a Covering Index?

When index itself contains all the columns needed for the query.

```sql
CREATE INDEX idx_user ON users(email, name);

SELECT email, name FROM users WHERE email = 'a@gmail.com';
```

✅ No table lookup required → Faster

### 12. What is ACID in MySQL?

A – Atomicity: All or nothing

C – Consistency: Maintains rules

I – Isolation: Transactions don’t interfere

D – Durability: Data stays safe after commit

### 13. Difference between DELETE, TRUNCATE and DROP

    DELETE TRUNCATE DROP
    Removes rows Removes all rows Deletes table
    Can rollback Cannot rollback Cannot rollback
    Slow Very fast Structure removed
    Where allowed No WHERE Whole table removed

### 14. What are Index types in MySQL?

PRIMARY

UNIQUE

INDEX

FULLTEXT

COMPOSITE

CREATE INDEX idx_name ON users(name);

### 15. What is Normalization?

Process of structuring data to reduce redundancy.

1NF -> 2NF -> 3NF

✅ Less redundancy
✅ Better consistency

### 16. What is Denormalization?

Combining tables for:

Faster reads

Reporting

Analytics

Used in: Data warehouse, OLAP

### 17. Difference: WHERE vs HAVING

    WHERE HAVING
    Filters rows Filters groups
    Used with SELECT Used with GROUP BY
    Faster Slightly slower
    SELECT dept, COUNT(_) FROM emp
    GROUP BY dept
    HAVING COUNT(_) > 5;

### 18. Difference: INNER JOIN vs LEFT JOIN

    INNER JOIN LEFT JOIN
    Only matched rows All left rows
    Removes unmatched Keeps unmatched as NULL

### 19. What is Sharding & Replication?

    Replication

Copy of data to multiple servers

For read scaling

Sharding

Split data into parts

For write scaling

### 20. What is a Deadlock?

When two transactions wait for each other.

✅ Solution:

Keep transactions short

Access tables in the same order

Use proper indexing

### 21. What is Explain & Analyze?

```sql
    EXPLAIN SELECT \* FROM users WHERE email='test@gmail.com';
```

Shows:

Index used

No. of rows scanned

Query plan

### 22. What is a View in MySQL?

A virtual table created from a query.

```sql
CREATE VIEW active_users AS
SELECT \* FROM users WHERE is_active = 1;
```

### 23. Window Function Example

```sql
    SELECT
    name,
    salary,
    RANK() OVER(ORDER BY salary DESC) AS ranking
    FROM employees;
```

Used in reports

### 24. Recent Interview Questions (Very Important )

How do you optimize a query with 10 million rows?

When to use a composite index?

Difference between partitioning and sharding?

How to prevent deadlocks?

How does InnoDB handle transactions?

What is isolation level and which is default?

How do you debug a slow query?

What is the difference between MyISAM & InnoDB?

How do you handle database scaling in production?

Best practices for designing schema in high traffic apps?
