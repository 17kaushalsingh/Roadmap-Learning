# SQL Fast Revision 📊

## Why SQL is #1 Priority 🎯

**A lot of companies ask database-related queries. This is often the first subject interviewers test to check your practical knowledge.**

### **Key Interview Focus**:
- **Query writing ability** (not just theory)
- **Understanding of optimization** techniques
- **Connection to DBMS concepts**
- **Practical problem-solving** with data

---

## Core SQL Concepts 📋

### **1. Basic Queries & Operations** 🔍

#### **Essential Commands**:
```sql
-- Basic SELECT
SELECT column1, column2 FROM table_name WHERE condition;

-- Aggregation Functions
SELECT COUNT(*), AVG(salary), MAX(salary) FROM employees;

-- GROUP BY and HAVING
SELECT department, COUNT(*) FROM employees
GROUP BY department HAVING COUNT(*) > 5;
```

#### **Key Points**:
- **WHERE** filters rows before grouping
- **HAVING** filters groups after aggregation
- **GROUP BY** combines rows with same values
- **ORDER BY** sorts final results

### **2. JOIN Operations** 🔗

#### **Types of JOINs**:
```sql
-- INNER JOIN (default)
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;

-- LEFT JOIN (all from left table)
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;

-- RIGHT JOIN (all from right table)
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;

-- FULL OUTER JOIN (all from both tables)
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;
```

#### **Interview Tips**:
- **Visualize JOINs** as Venn diagrams
- **Understand NULL handling** in different JOINs
- **Performance considerations** for large datasets
- **Multiple JOINs** in single query

### **3. Subqueries & Nested Queries** 📊

#### **Types of Subqueries**:
```sql
-- Scalar Subquery (returns single value)
SELECT name FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Multi-row Subquery with IN
SELECT name FROM employees
WHERE department_id IN (SELECT id FROM departments WHERE location = 'NY');

-- Correlated Subquery
SELECT e1.name, e1.salary FROM employees e1
WHERE e1.salary > (SELECT AVG(e2.salary) FROM employees e2
                   WHERE e2.department_id = e1.department_id);
```

#### **Performance Optimization**:
- **EXISTS** often better than **IN** for subqueries
- **Correlated subqueries** can be slow
- **Consider JOINs** instead of subqueries when possible

---

## Common Interview Questions & Solutions 💼

### **Question 1: Find Second Highest Salary** 🏆

#### **Multiple Approaches**:

**Approach 1: Using LIMIT and OFFSET**
```sql
SELECT DISTINCT salary FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

**Approach 2: Using Subquery**
```sql
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

**Approach 3: Using DENSE_RANK**
```sql
SELECT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as rank
    FROM employees
) ranked WHERE rank = 2;
```

**Follow-up Questions**:
- **What if there are duplicates?** (Use DENSE_RANK)
- **Find Nth highest salary?** (Change LIMIT OFFSET or rank value)
- **Performance comparison** between approaches

### **Question 2: Employees with Higher Salary than Department Average** 📈

```sql
SELECT e.name, e.salary, e.department_id, dept_avg.avg_salary
FROM employees e
JOIN (
    SELECT department_id, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department_id
) dept_avg ON e.department_id = dept_avg.department_id
WHERE e.salary > dept_avg.avg_salary;
```

**Key Points**:
- **Subquery in FROM clause** creates derived table
- **JOIN condition** links employee to department average
- **GROUP BY** calculates department averages

### **Question 3: Find Duplicate Records** 🔍

```sql
-- Find duplicates based on email
SELECT email, COUNT(*) as count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Show complete duplicate records
SELECT * FROM users u1
WHERE EXISTS (
    SELECT 1 FROM users u2
    WHERE u2.email = u1.email AND u2.id != u1.id
);
```

**Variations**:
- **Delete duplicates** keeping one record
- **Find duplicates across multiple columns**
- **Handle NULL values** in duplicate detection

### **Question 4: Department with Most Employees** 👥

```sql
-- Using LIMIT
SELECT d.department_name, COUNT(e.id) as employee_count
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.id, d.department_name
ORDER BY employee_count DESC
LIMIT 1;

-- Using subquery
SELECT department_name, employee_count
FROM (
    SELECT d.department_name, COUNT(e.id) as employee_count,
           RANK() OVER (ORDER BY COUNT(e.id) DESC) as rank
    FROM departments d
    LEFT JOIN employees e ON d.id = e.department_id
    GROUP BY d.id, d.department_name
) ranked WHERE rank = 1;
```

### **Question 5: Monthly Sales Report** 📊

```sql
SELECT
    DATE_FORMAT(order_date, '%Y-%m') as month,
    COUNT(*) as total_orders,
    SUM(amount) as total_sales,
    AVG(amount) as avg_order_value
FROM orders
WHERE order_date >= '2023-01-01'
GROUP BY DATE_FORMAT(order_date, '%Y-%m')
ORDER BY month;
```

**Key Functions**:
- **DATE_FORMAT** for date manipulation
- **Aggregate functions** for calculations
- **Date filtering** with WHERE clause

---

## Advanced SQL Topics 🚀

### **1. Window Functions** 📊

#### **Common Window Functions**:
```sql
-- Running Total
SELECT order_date, amount,
       SUM(amount) OVER (ORDER BY order_date) as running_total
FROM orders;

-- Row Number
SELECT name, salary,
       ROW_NUMBER() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- Lead and Lag
SELECT order_date, amount,
       LAG(amount) OVER (ORDER BY order_date) as prev_day_amount
FROM orders;
```

#### **Interview Applications**:
- **Top N records per group**
- **Running totals and averages**
- **Year-over-year comparisons**
- **Gap analysis**

### **2. Common Table Expressions (CTE)** 🔄

```sql
-- Recursive CTE for hierarchy
WITH RECURSIVE employee_hierarchy AS (
    SELECT id, name, manager_id, 1 as level
    FROM employees WHERE manager_id IS NULL

    UNION ALL

    SELECT e.id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy;
```

### **3. Indexes & Performance** ⚡

#### **Index Types**:
- **B-Tree Index**: Default for most queries
- **Hash Index**: For exact matches
- **Composite Index**: Multiple columns
- **Partial Index**: With WHERE condition

#### **When to Create Index**:
```sql
-- Single column index
CREATE INDEX idx_employee_email ON employees(email);

-- Composite index
CREATE INDEX idx_employee_dept_salary ON employees(department_id, salary);

-- Unique index
CREATE UNIQUE INDEX idx_employee_ssn ON employees(ssn);
```

#### **Performance Tips**:
- **Index columns used in WHERE clauses**
- **Avoid indexing frequently updated columns**
- **Use EXPLAIN** to analyze query performance
- **Covering indexes** can avoid table lookups

---

## Database Design & Normalization 🏗️

### **Normalization Forms** 📋

#### **1NF (First Normal Form)**:
- **Each cell contains atomic value**
- **No repeating groups**
- **Primary key defined**

#### **2NF (Second Normal Form)**:
- **1NF + No partial dependencies**
- **All non-key attributes depend on entire primary key**

#### **3NF (Third Normal Form)**:
- **2NF + No transitive dependencies**
- **Non-key attributes don't depend on other non-key attributes**

#### **BCNF (Boyce-Codd Normal Form)**:
- **Stronger version of 3NF**
- **Every determinant is a candidate key**

### **Practical Example**:
```sql
-- Before Normalization (violates 1NF)
CREATE TABLE bad_orders (
    order_id INT,
    customer_name VARCHAR(100),
    products TEXT -- "product1,product2,product3"
);

-- After Normalization (3NF)
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100)
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    price DECIMAL(10,2)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    price DECIMAL(10,2),
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

---

## Transactions & ACID Properties 🔒

### **ACID Properties Explained**:

#### **Atomicity (A)**:
- **All or nothing** execution
- **Transaction completes fully** or not at all
- **Rollback** on failure

#### **Consistency (C)**:
- **Database remains valid** after transaction
- **Constraints maintained**
- **Referential integrity preserved**

#### **Isolation (I)**:
- **Transactions don't interfere** with each other
- **Concurrent execution** appears serial
- **Prevents dirty reads, phantom reads**

#### **Durability (D)**:
- **Changes persist** after commit
- **Survive system failures**
- **Written to permanent storage**

### **Transaction Commands**:
```sql
START TRANSACTION;

-- Bank transfer example
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

-- Commit if successful
COMMIT;

-- Or rollback if error
ROLLBACK;
```

---

## SQL Cheat Sheet 📝

### **Quick Reference Commands**:

#### **Basic Operations**:
```sql
-- Create table
CREATE TABLE table_name (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert data
INSERT INTO table_name (id, name) VALUES (1, 'John');

-- Update data
UPDATE table_name SET name = 'Jane' WHERE id = 1;

-- Delete data
DELETE FROM table_name WHERE id = 1;
```

#### **Query Optimization Tips**:
1. **Use INDEXES** on frequently queried columns
2. **Avoid SELECT *** - specify columns
3. **Use EXISTS instead of IN** for subqueries
4. **Use LIMIT** for large result sets
5. **Analyze with EXPLAIN** plan

#### **Common Functions**:
```sql
-- String functions
UPPER(string), LOWER(string), LENGTH(string)
CONCAT(string1, string2), SUBSTRING(string, start, length)

-- Date functions
NOW(), CURRENT_DATE(), DATE_FORMAT(date, format)
DATE_ADD(date, INTERVAL value unit)

-- Aggregate functions
COUNT(), SUM(), AVG(), MAX(), MIN()
```

---

## Interview Preparation Checklist ✅

### **Must-Know Concepts**:
- [ ] **Basic CRUD operations** (CREATE, READ, UPDATE, DELETE)
- [ ] **JOIN types** (INNER, LEFT, RIGHT, FULL OUTER)
- [ ] **Subqueries** (scalar, multi-row, correlated)
- [ ] **Group BY and HAVING**
- [ ] **Window functions** (ROW_NUMBER, RANK, DENSE_RANK)
- [ ] **Index types and usage**
- [ ] **ACID properties**
- [ ] **Normalization forms** (1NF, 2NF, 3NF, BCNF)

### **Common Problems to Practice**:
- [ ] **Second highest salary**
- [ ] **Department-wise employee counts**
- [ ] **Duplicate record detection**
- [ ] **Running totals and cumulative sums**
- [ ] **Date-based reporting**
- [ ] **Hierarchical data queries**
- [ ] **Pivot table transformations**

### **Performance Topics**:
- [ ] **Query optimization techniques**
- [ ] **Index usage strategies**
- [ ] **EXPLAIN plan analysis**
- [ ] **Common anti-patterns**

---

## Key Takeaways 💡

1. **Practice Writing Queries**: Cannot write SQL in interviews without practice
2. **Understand Optimization**: Know how indexes and query plans work
3. **Master JOINs**: Essential for combining data from multiple tables
4. **Know Window Functions**: Powerful for advanced analytics
5. **Understand ACID**: Critical for transaction-based systems
6. **Normalization Matters**: Shows database design understanding
7. **Performance Awareness**: Demonstrates practical knowledge
8. **Connect Theory to Practice**: Link SQL concepts to DBMS theory

**Remember**: SQL tests both theoretical knowledge and practical coding ability. Practice writing actual queries, not just reading about concepts! 🎯

---

**Next Steps**: Practice these concepts with real database scenarios and move on to OOP & LLD concepts for comprehensive interview preparation.