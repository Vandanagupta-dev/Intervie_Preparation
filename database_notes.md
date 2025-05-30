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

---
Sure! Here's how you can **find the second or third highest salary in SQL Server**, using different methods:

---

## ✅ 1. **Using `OFFSET-FETCH` with `ORDER BY`**

### 🔹 Second Highest Salary:

```sql
SELECT DISTINCT Salary
FROM Employees
ORDER BY Salary DESC
OFFSET 1 ROW FETCH NEXT 1 ROW ONLY;
```

### 🔹 Third Highest Salary:

```sql
SELECT DISTINCT Salary
FROM Employees
ORDER BY Salary DESC
OFFSET 2 ROWS FETCH NEXT 1 ROW ONLY;
```

> ✅ Note: `OFFSET` starts from `0`, so `OFFSET 1` skips the first (highest) salary.

---

## ✅ 2. **Using Subqueries**

### 🔹 Second Highest Salary:

```sql
SELECT MAX(Salary) AS SecondHighest
FROM Employees
WHERE Salary < (SELECT MAX(Salary) FROM Employees);
```

### 🔹 Third Highest Salary:

```sql
SELECT MAX(Salary) AS ThirdHighest
FROM Employees
WHERE Salary < (
    SELECT MAX(Salary)
    FROM Employees
    WHERE Salary < (SELECT MAX(Salary) FROM Employees)
);
```

---

## ✅ 3. **Using `DENSE_RANK()` (Best for Ties and More Control)**

### 🔹 Second Highest Salary:

```sql
WITH SalaryRanks AS (
    SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS SalaryRank
    FROM Employees
)
SELECT Salary
FROM SalaryRanks
WHERE SalaryRank = 2;
```

### 🔹 Third Highest Salary:

```sql
WITH SalaryRanks AS (
    SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS SalaryRank
    FROM Employees
)
SELECT Salary
FROM SalaryRanks
WHERE SalaryRank = 3;
```

---

Let me know if your table has a different name or structure (like employee name, department, etc.) — I can customize the queries accordingly.

---Sure! Here's a collection of **common SQL commands** with **syntax examples** for each category:

---

### 🔹 1. **Data Definition Language (DDL)**

```sql
-- Create a table
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(100),
    Salary DECIMAL(10,2),
    DepartmentID INT
);

-- Alter a table (add a column)
ALTER TABLE Employees ADD Email VARCHAR(100);

-- Drop a table
DROP TABLE Employees;

-- Truncate a table (delete all rows, but keeps structure)
TRUNCATE TABLE Employees;

-- Rename a table (syntax may vary by DBMS)
ALTER TABLE Employees RENAME TO Staff;
```

---

### 🔹 2. **Data Manipulation Language (DML)**

```sql
-- Insert a new row
INSERT INTO Employees (EmployeeID, Name, Salary, DepartmentID)
VALUES (1, 'John Doe', 50000.00, 2);

-- Update existing data
UPDATE Employees
SET Salary = 55000.00
WHERE EmployeeID = 1;

-- Delete specific data
DELETE FROM Employees
WHERE EmployeeID = 1;
```

---

### 🔹 3. **Data Query Language (DQL)**

```sql
-- Select all columns
SELECT * FROM Employees;

-- Select specific columns with condition
SELECT Name, Salary FROM Employees
WHERE DepartmentID = 2;
```

---

### 🔹 4. **Transaction Control Language (TCL)**

```sql
BEGIN TRANSACTION;

-- Sample DML operations
UPDATE Employees SET Salary = 60000 WHERE EmployeeID = 2;

-- Commit the changes
COMMIT;

-- Or rollback if something goes wrong
ROLLBACK;
```

---

### 🔹 5. **Data Control Language (DCL)**

```sql
-- Grant privileges
GRANT SELECT, INSERT ON Employees TO User1;

-- Revoke privileges
REVOKE INSERT ON Employees FROM User1;
```

---

### 🔹 6. **Joins and Advanced Queries**

```sql
-- INNER JOIN
SELECT e.Name, d.DepartmentName
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID;

-- LEFT JOIN
SELECT e.Name, d.DepartmentName
FROM Employees e
LEFT JOIN Departments d ON e.DepartmentID = d.DepartmentID;

-- Aggregate functions with GROUP BY
SELECT DepartmentID, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY DepartmentID
HAVING AVG(Salary) > 50000;
```

---

### 🔹 7. **Other Useful Clauses**

```sql
-- ORDER BY
SELECT Name, Salary FROM Employees
ORDER BY Salary DESC;

-- LIKE
SELECT * FROM Employees
WHERE Name LIKE 'J%';

-- IN
SELECT * FROM Employees
WHERE DepartmentID IN (1, 2, 3);

-- BETWEEN
SELECT * FROM Employees
WHERE Salary BETWEEN 40000 AND 60000;

-- LIMIT (MySQL/PostgreSQL) / TOP (SQL Server)
SELECT * FROM Employees
LIMIT 5;

SELECT TOP 5 * FROM Employees;
```

---
Bahut achha sawal hai! Interview mein agar aapse poocha jaaye:

> **"Function database mein kyu use kiya jata hai?"**
> ya
> **"SQL mein function ka kya use hota hai?"**

Toh aap niche diye gaye **points ke sath example** dekar confidently jawab de sakte ho:

---

## ✅ **Database Function ki Need / Zarurat Kyu Hoti Hai?**

### 1. ✅ **Code Reusability (Ek hi logic baar-baar use karna)**

* Agar koi calculation ya check baar-baar karna hai, toh function mein likh ke reuse kar sakte hain.

🧠 Example:

```sql
CREATE FUNCTION dbo.GetFullName (@firstName NVARCHAR(50), @lastName NVARCHAR(50))
RETURNS NVARCHAR(100)
AS
BEGIN
    RETURN @firstName + ' ' + @lastName
END
```

Isse aap har jagah use kar sakte ho:

```sql
SELECT dbo.GetFullName(FirstName, LastName) FROM Users;
```

---

### 2. ✅ **Modular Code (Chhote-chhote blocks mein divide karna)**

* Complex queries ko chhote logical blocks (functions) mein todna asaan hota hai.
* Readability aur maintainability badh jaati hai.

---

### 3. ✅ **Consistency in Logic (Ek jaisi calculation har jagah)**

* Suppose discount ya tax calculation same logic se karna hai — function se consistent result milega.

🧠 Example:

```sql
CREATE FUNCTION dbo.CalculateGST (@amount DECIMAL(10,2))
RETURNS DECIMAL(10,2)
AS
BEGIN
    RETURN @amount * 0.18
END
```

---

### 4. ✅ **Performance Improvement (Kabhi-kabhi)**

* Scalar functions agar sahi tarike se likhe ho, to performance improve ho sakti hai.
* Lekin **complex UDFs (User-Defined Functions)** ko optimize karna padta hai.

---

### 5. ✅ **Security (Encapsulation of logic)**

* Aap function ke through business logic ko secure rakh sakte ho, direct table access se bacha ke.

---

### 6. ✅ **Use in Views, Stored Procedures, Triggers**

* Functions ko aap **SELECT**, **WHERE**, **JOIN**, **VIEW**, **SP**, sab mein use kar sakte ho.

---

## 🧠 Real-world Example Question (Interview Style):

### 👨‍💼 **Interviewer:**

> "Agar aapko kisi employee ka annual salary calculate karna ho har jagah, to aap kya karoge?"

### ✅ **You:**

> "Main ek SQL function banaunga jo monthly salary le kar annual return kare. Usse main reports, views, procedures sab jagah use kar sakta hoon."

```sql
CREATE FUNCTION dbo.GetAnnualSalary(@monthly DECIMAL(10,2))
RETURNS DECIMAL(10,2)
AS
BEGIN
    RETURN @monthly * 12
END
```

---

### 📌 Conclusion:

> **"Database function ka use hota hai business logic ko reuse karne, simplify karne, aur consistent banane ke liye — especially jab same calculation multiple jagah use karna ho."**

---

🔸 Table ke data ke sath:

SELECT 
    EmpName,
    MonthlySalary,
    dbo.GetAnnualSalary(MonthlySalary) AS AnnualSalary
FROM 
    Employees;
---
SELECT 
    ProductName,
    Price,
    dbo.CalculateGST(Price) AS GST_18,
    Price + dbo.CalculateGST(Price) AS FinalPrice
FROM 
    Products;


---
🎯 Function kahan use karein (and kyun)?
✅ 1. SELECT ke andar filter ya calculation:
sql
Copy
Edit
SELECT Name, dbo.CalculateGST(Price) AS GST
FROM Products;
➡ SP se possible nahi, function hi chahiye.

✅ 2. Views ke andar reusability:
Views ya computed columns me only functions use kiye ja sakte hain, SP nahi.

✅ 3. JOINs and WHERE clause ke sath:
sql
Copy
Edit
SELECT * 
FROM Orders o
JOIN dbo.GetTopSalaries(5) s ON o.EmpId = s.EmployeeId
➡ Ye sirf Table-Valued Function se hi possible hai.

❗ Toh phir SP kab use karein?
Jab aapko:

Data insert/update/delete karna ho

Multiple result sets return karne ho

Dynamic SQL execute karna ho

Transactions handle karne ho

✅ Summary for Interview Answer:
"Sir, jab mujhe modular, reusable, aur SELECT ke andar calculation chahiye hoti hai — jaise filtering, joining, ya scalar value nikalna — tab main Function use karta hoon. Lekin jab mujhe data manipulation (Insert/Update/Delete) ya complex business logic likhna ho, tab main Stored Procedure prefer karta hoon. Functions SELECT ke andar use ho sakte hain, jo SP se possible nahi hota — isiliye unka use alag scenario me hota hai."


---
बहुत अच्छा सवाल!
**"हमें Trigger बनाने की ज़रूरत कब पड़ती है?"** – इसका जवाब जानना **practical understanding** और **interview दोनों** के लिए जरूरी है।

---

## ✅ **Trigger की ज़रूरत कब पड़ती है?**

नीचे 6 Real-Life Scenarios दिए गए हैं जहाँ हमें Triggers की ज़रूरत पड़ती है:

---

### 🔹 **1. Audit Trail (Log रखना)**

👉 जब हमें पता रखना हो कि कौन-सी row कब insert/update/delete हुई।

**उदाहरण:**

```sql
AFTER INSERT / UPDATE trigger
→ Employee table में नया insert होते ही EmployeeAudit table में log डालना।
```

📌 *Use case:*
Company में अगर कोई employee की salary update करे तो उसका पुराना data log होना चाहिए – यही काम trigger करता है।

---

### 🔹 **2. Business Rule Enforcement**

👉 जब हमें कुछ business rules **automatically enforce** करने हों।

**उदाहरण:**

```sql
AFTER UPDATE
→ अगर कोई order की quantity 0 कर दे, तो status 'Cancelled' हो जाए।
```

📌 *Use case:*
User गलती से invalid data ना डाल सके, तो Trigger उस value को modify या reject कर सकता है।

---

### 🔹 **3. Preventing Invalid Operations**

👉 जब हमें कुछ actions से **restrict** करना हो।

**उदाहरण:**

```sql
INSTEAD OF DELETE
→ किसी VIP Customer को delete करने की कोशिश पर रोक लगाना।
```

📌 *Use case:*
कुछ खास rows को accidentally delete करने से रोकने के लिए trigger काम आता है।

---

### 🔹 **4. Cascading Operations**

👉 जब एक table पर action होने पर **दूसरे table** में भी कुछ change करना हो।

**उदाहरण:**

```sql
AFTER DELETE on Orders
→ Related OrderItems को भी delete करना।
```

📌 *Use case:*
Manual foreign key cascade की जगह triggers से भी ये किया जा सकता है।

---

### 🔹 **5. Notifications या Alerts Trigger करना**

👉 जैसे ही कुछ specific action हो, कोई **notification trigger** हो जाए।

**उदाहरण:**

```sql
AFTER INSERT on LeaveRequests
→ एक log table में entry करे और HR को notify करे।
```

📌 *Use case:*
Leave request डालते ही manager को notify करना।

---

### 🔹 **6. Synchronizing Tables (Audit/Backup)**

👉 जब एक table में change होने पर **दूसरे table** में भी copy रखना हो।

**उदाहरण:**

```sql
AFTER INSERT
→ Same data को Backup_Employees में भी insert कर दो।
```

📌 *Use case:*
Change log या history version बनाने के लिए।

---

## 🧠 Interview में कैसे बोलें:

> *“Sir, Trigger तब use करते हैं जब हमें data change hone par automatically kuch logic run karna हो — जैसे audit logs maintain करना, rules enforce करना, data sync करना, ya invalid actions से रोकना। ये behind-the-scenes automation provide करता है और manual errors को रोकता है.”*

---
Great question! Let's break down why `NVARCHAR` is used instead of `VARCHAR`, and why `PhoneNumber` is stored as a `NVARCHAR(15)` instead of `INT`.

---

### 🔹 1. `NVARCHAR` vs `VARCHAR`

#### ✅ Why `NVARCHAR`?

* `NVARCHAR` (National Variable Character) supports **Unicode** characters.
* This means it can store **characters from all languages**: English, Hindi, Chinese, Arabic, etc.
* It is a **best practice** when you expect to support multiple languages, especially for names or addresses.

#### Example:

If someone enters a name like `"张伟"` or `"संदीप"`, `VARCHAR` will **fail to store it correctly**, but `NVARCHAR` will **preserve it**.

> 🟡 So for `FullName`, `Email`, `Department` — using `NVARCHAR` ensures better compatibility and future-proofing.

---

### 🔹 2. Why not store `PhoneNumber` as `INT`?

#### 🚫 `INT` is not suitable for phone numbers because:

1. **Leading Zeros Will Be Lost**
   E.g., `0801234567` → stored as `801234567` in `INT`.

2. **Phone Numbers Are Not Numbers for Math**
   You will never add, subtract, or calculate with phone numbers. They are more like **identifiers** than actual numbers.

3. **International Codes / Special Characters**
   E.g., `+91-9876543210` or `(123) 456-7890` cannot be stored in an `INT`.

#### ✅ Correct Choice:

* `NVARCHAR(15)` or `VARCHAR(15)` is perfect for phone numbers.
* It keeps formatting and works internationally.

---

### ✅ Summary:

| Field         | Chosen Type     | Reason                                                               |
| ------------- | --------------- | -------------------------------------------------------------------- |
| `FullName`    | `NVARCHAR(100)` | Supports names in any language (Unicode)                             |
| `Email`       | `NVARCHAR(100)` | Emails may contain international characters                          |
| `PhoneNumber` | `NVARCHAR(15)`  | Stores formatting, supports `+`, `-`, and avoids integer limitations |
| `Department`  | `NVARCHAR(100)` | Allows department names in multiple languages                        |

---

If your application will **only support English** and **ASCII characters**, then `VARCHAR` is fine too — it’s a little faster and uses less storage. But using `NVARCHAR` is a safer default.

Let me know if you want to convert these types to `VARCHAR` for English-only use!

