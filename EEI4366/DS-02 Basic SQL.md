SQL, which stands for Structured Query Language, is a specialized programming language used for managing and manipulating relational databases.
## Command Categories

1. **DDL (Data Definition Language)**:
    
    - DDL commands are used to define and manage the structure or schema of a database.
    - These commands include operations such as creating, altering, and dropping database objects like tables, indexes, and views.
    - Common DDL commands include `CREATE`, `ALTER`, `DROP`, `TRUNCATE`,`UPDATE`,`DELETE` and `RENAME`.
2. **DML (Data Manipulation Language)**:
    
    - DML commands are used to manipulate the data stored within a database.
    - These commands include operations such as querying data, inserting new records, updating existing records, and deleting records.
    - Common DML commands include `SELECT`, `INSERT`, `UPDATE`, and `DELETE`.
3. **DCL (Data Control Language)**:
    
    - DCL commands are used to control access and permissions within a database. They manage the security and access rights of users and roles.
    - These commands include operations such as granting or revoking privileges on database objects.
    - Common DCL commands include `GRANT` and `REVOKE`.

## Integrity

> [!tip]
> Constraints are rules used to limit the type of data that can go into a table.
> 

**Entity Integrity**:

Entity integrity is one of the fundamental principles of database design and is typically enforced using primary keys in SQL databases.
It ensures that each row in a table is uniquely identifiable, and no two rows have the same primary key value.

```sql
CREATE TABLE Books (
    BookID INT PRIMARY KEY,
    Title VARCHAR(255),
    Author VARCHAR(255),
    ISBN VARCHAR(13)
);
```

**Data Integrity:**

Data integrity ensures the <mark style="background: #FF5582A6;">accuracy and consistency of data</mark> within a single table. It's enforced through constraints that define rules for the data in the table.

1. **Primary Key Constraint:**
   
   Example: Creating a `Students` table with a primary key constraint on the `StudentID` column to ensure each student has a unique identifier.

   ```sql
   CREATE TABLE Students (
       StudentID INT PRIMARY KEY,
       FirstName VARCHAR(50),
       LastName VARCHAR(50)
   );
   ```

2. **Unique Constraint:**

   Example: Ensuring that email addresses in the `Users` table are unique.

   ```sql
   CREATE TABLE Users (
       UserID INT PRIMARY KEY,
       Email VARCHAR(100) UNIQUE,
       Password VARCHAR(50)
   );
   ```

3. **Check Constraint:**

   Example: Using a check constraint to ensure that ages in the `Employees` table are between 18 and 65.

   ```sql
   CREATE TABLE Employees (
       EmployeeID INT PRIMARY KEY,
       FirstName VARCHAR(50),
       LastName VARCHAR(50),
       Age INT CHECK (Age >= 18 AND Age <= 65)
       Gender CHAR CHECK (Gender IN ('M','F'))
   );
   ```

**Referential Integrity:**

Referential integrity <mark style="background: #FF5582A6;">ensures the consistency of relationships between tables.</mark> It's enforced using foreign key constraints and optionally specifying cascade actions.

1. **Foreign Key Constraint:**

   Example: Creating a `Orders` table with a foreign key constraint that references the `CustomerID` in the `Customers` table. This ensures that each order is associated with a valid customer.

   ```sql
   CREATE TABLE Customers (
       CustomerID INT PRIMARY KEY,
       CustomerName VARCHAR(100)
   );

   CREATE TABLE Orders (
       OrderID INT PRIMARY KEY,
       OrderDate DATE,
       CustomerID INT,
       FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
   );
   ```

2. **Cascade Actions (CASCADE UPDATE):**

   Example: Specifying a CASCADE UPDATE action so that when a `CustomerID` is updated in the `Customers` table, the corresponding `CustomerID` in the `Orders` table is automatically updated.

   ```sql
   CREATE TABLE Customers (
       CustomerID INT PRIMARY KEY,
       CustomerName VARCHAR(100)
   );

   CREATE TABLE Orders (
       OrderID INT PRIMARY KEY,
       OrderDate DATE,
       CustomerID INT,
       FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID) ON UPDATE CASCADE
   );
   ```

   If you update a customer's `CustomerID` in the `Customers` table, the change will automatically propagate to the `Orders` table.

3. **Cascade Actions (CASCADE DELETE):**

   Example: Specifying a CASCADE DELETE action so that when a customer is deleted from the `Customers` table, all related orders in the `Orders` table are also deleted.

   ```sql
   CREATE TABLE Customers (
       CustomerID INT PRIMARY KEY,
       CustomerName VARCHAR(100)
   );

   CREATE TABLE Orders (
       OrderID INT PRIMARY KEY,
       OrderDate DATE,
       CustomerID INT,
       FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID) ON DELETE CASCADE
   );
   ```

   If you delete a customer, all of their associated orders will also be deleted automatically.


## Join

JOIN operation is used to combine rows from two or more tables based on a related column between them. This operation allows you to retrieve data from multiple tables simultaneously, creating a virtual table or result set that combines information from the joined tables. 

There are several types of JOINs in SQL, including:

1. **INNER JOIN**: This is the most common type of JOIN. It returns only the rows <mark style="background: #FF5582A6;">where there is a match in both tables based on the specified join condition.</mark> Rows from one table that do not have a matching row in the other table are excluded from the result.

   ```sql
   SELECT Orders.OrderID, Customers.CustomerName
   FROM Orders
   INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
   ```

2. **LEFT JOIN (or LEFT OUTER JOIN)**: This returns <mark style="background: #FF5582A6;">all rows from the left (or first) table and the matched rows from the right</mark> (or second) table. If there is no match in the right table, NULL values are returned for the columns from the right table.

   ```sql
   SELECT Customers.CustomerName, Orders.OrderID
   FROM Customers
   LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
   ```

3. **RIGHT JOIN (or RIGHT OUTER JOIN)**: This is similar to a LEFT JOIN but returns <mark style="background: #FF5582A6;">all rows from the right (or second) table and the matched rows from the left (or first) table.</mark> Rows from the left table with no match in the right table result in NULL values for the columns from the left table.

   ```sql
   SELECT Customers.CustomerName, Orders.OrderID
   FROM Customers
   RIGHT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
   ```

4. **FULL JOIN (or FULL OUTER JOIN)**: This returns <mark style="background: #FF5582A6;">all rows when there is a match in either the left or the right table.</mark> If there is no match in one of the tables, NULL values are returned for the columns from the table without a match.

   ```sql
   SELECT Customers.CustomerName, Orders.OrderID
   FROM Customers
   FULL JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
   ```

5. **CROSS JOIN**: This type of JOIN produces a <mark style="background: #FF5582A6;">Cartesian product of all rows from the first table and all rows from the second table.</mark> It is used when you want to combine every row from one table with every row from another table, resulting in a large result set.

   ```sql
   SELECT Customers.CustomerName, Products.ProductName
   FROM Customers
   CROSS JOIN Products;
   ```

6. **SELF JOIN**: This is a JOIN operation where a table is joined with itself. It is often used when dealing with hierarchical data or when you need to compare rows within the same table.

   ```sql
   SELECT e1.EmployeeName, e2.EmployeeName AS ManagerName
   FROM Employees e1
   LEFT JOIN Employees e2 ON e1.ManagerID = e2.EmployeeID;
   ```