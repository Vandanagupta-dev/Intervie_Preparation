Sure! Here are **database interview questions for 1 year experience**, along with clear and concise answers:

---

### 1. **What is the difference between SQL and NoSQL databases?**

* **SQL databases** are relational, use structured tables, and support ACID transactions (e.g., SQL Server, MySQL).
* **NoSQL databases** are non-relational, schema-less, designed for unstructured data, and scale horizontally (e.g., MongoDB, Cassandra).

---

### 2. **What are primary keys and foreign keys? Why are they important?**

* **Primary Key** uniquely identifies each record in a table; it cannot be null.
* **Foreign Key** is a field in one table that references the primary key in another table to maintain referential integrity.
* They ensure data consistency and relationships between tables.

---

### 3. **What is normalization? Explain different normal forms.**

* Normalization is organizing data to reduce redundancy and improve data integrity.
* **1NF:** Eliminate repeating groups, ensure atomicity.
* **2NF:** Remove partial dependencies (depends on full primary key).
* **3NF:** Remove transitive dependencies (non-key fields depend only on primary key).

---

### 4. **How do you write a SQL query to find duplicate records in a table?**

```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 1;
```

---

### 5. **What are joins? Explain different types of joins with examples.**

* Joins combine rows from two or more tables based on related columns.
* **INNER JOIN:** Returns matching records.
* **LEFT JOIN:** Returns all from left table + matched from right.
* **RIGHT JOIN:** Returns all from right table + matched from left.
* **FULL OUTER JOIN:** Returns all records when there is a match in either table.

---

### 6. **What is indexing? How does it improve query performance?**

* Indexing creates a data structure (like a book’s index) to speed up data retrieval without scanning the entire table.
* It reduces query execution time by quickly locating rows.

---

### 7. **What is a stored procedure? Have you created or used stored procedures?**

* A stored procedure is a precompiled set of SQL statements stored in the database for reuse.
* It helps improve performance, security, and maintainability.
* *Example:* Yes, I have created stored procedures for CRUD operations in projects.

---

### 8. **What is a transaction? Explain ACID properties.**

* A transaction is a sequence of operations performed as a single logical unit.
* **ACID**:

  * *Atomicity*: All or nothing.
  * *Consistency*: Data remains valid before and after.
  * *Isolation*: Transactions don’t interfere.
  * *Durability*: Once committed, data is permanent.

---

### 9. **How do you perform pagination in SQL Server?**

```sql
SELECT columns
FROM table_name
ORDER BY column_name
OFFSET @PageSize * (@PageNumber - 1) ROWS
FETCH NEXT @PageSize ROWS ONLY;
```

---

### 10. **What is Entity Framework? How does it interact with the database?**

* EF is an Object-Relational Mapper (ORM) for .NET, allowing developers to work with data as objects.
* It translates LINQ queries into SQL queries to interact with the database.

---

### 11. **What are the differences between DbContext and DbSet in Entity Framework?**

* **DbContext** represents the session with the database and manages entity objects.
* **DbSet** represents a collection/table of entities you can query or modify.

---

### 12. **How do you handle database migrations in EF Core?**

* Using `Add-Migration` to create migration files, and `Update-Database` to apply schema changes to the database.

---

### 13. **Explain the difference between ExecuteReader(), ExecuteScalar(), and ExecuteNonQuery() in ADO.NET.**

* **ExecuteReader()**: Executes a query that returns multiple rows (returns a DataReader).
* **ExecuteScalar()**: Executes a query that returns a single value (first column of first row).
* **ExecuteNonQuery()**: Executes commands that don’t return data (INSERT, UPDATE, DELETE) and returns affected rows count.

---

### 14. **What is a connection string? Can you explain its components?**

* A connection string contains info to connect to the database.
* Components: Data Source (server), Initial Catalog (database name), User ID, Password, Integrated Security.

---

### 15. **How do you prevent SQL Injection attacks in your applications?**

* Use parameterized queries or stored procedures.
* Avoid concatenating user input directly into SQL queries.
* Use ORM tools like EF which handle this by default.

---

### 16. **What are stored procedures, views, and triggers?**

* **Stored Procedure:** Predefined SQL commands stored in DB.
* **View:** Virtual table based on SQL query, used to simplify complex queries.
* **Trigger:** Special procedure that runs automatically in response to certain events (INSERT, UPDATE, DELETE).

---

### 17. **How do you optimize slow-running SQL queries?**

* Analyze with execution plans.
* Use indexes appropriately.
* Avoid SELECT \*; fetch only needed columns.
* Use joins efficiently and avoid unnecessary subqueries.

---

### 18. **Have you used transactions in your projects? How do you implement them in .NET?**

* Yes, using `SqlTransaction` in ADO.NET or transaction scopes in EF Core (`context.Database.BeginTransaction()`) to ensure atomic operations.

---

### 19. **What is the difference between clustered and non-clustered indexes?**

* **Clustered index** defines the physical order of data in the table (only one per table).
* **Non-clustered index** is a separate structure that points to data rows, can be multiple per table.

---

### 20. **How do you handle concurrency in databases?**

* Use **optimistic concurrency** by adding a timestamp/version column and checking it before updating.
* Or use **pessimistic concurrency** by locking records during transactions.

---

If you want, I can help prepare detailed answers or examples for any question!

