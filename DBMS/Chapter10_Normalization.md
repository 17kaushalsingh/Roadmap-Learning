# Chapter 10: Normalization 📋

## Introduction to Normalization

**Normalization is a step towards DB optimization** - a systematic approach to organize data in a database efficiently.

### Why Normalization?
- **To avoid redundancy** in the database
- **Eliminate anomalies** caused by redundant data
- **Improve database performance** and reduce size

---

## Functional Dependency (FD)

### Definition
A functional dependency is a relationship between attributes, typically from the primary key to other attributes.

**Notation**: `X → Y`
- **X** = Determinant (left side)
- **Y** = Dependent (right side)

### Types of Functional Dependencies

#### 1. Trivial FD
A → B has trivial functional dependency if B is a subset of A.

**Examples**:
- `A → A`
- `B → B`
- `{A, B} → A` (since A ⊆ {A, B})

#### 2. Non-trivial FD
A → B has non-trivial functional dependency if B is not a subset of A.

**Example**:
- `{Student_ID} → Student_Name` (Student_Name ∉ {Student_ID})

---

## Armstrong's Axioms (FD Rules)

### 1. Reflexive Rule
If A is a set of attributes and B is a subset of A, then A → B holds.

**Formula**: If A ⊇ B then A → B

**Example**: `{Student_ID, Name} → Name`

### 2. Augmentation Rule
If B can be determined from A, then adding an attribute won't change anything.

**Formula**: If A → B holds, then AX → BX holds too

**Example**: If `Student_ID → Name`, then `{Student_ID, Course} → {Name, Course}`

### 3. Transitivity Rule
If A determines B and B determines C, then A determines C.

**Formula**: If A → B and B → C then A → C

**Example**: If `Student_ID → Dept_ID` and `Dept_ID → Dept_Name`, then `Student_ID → Dept_Name`

---

## Database Anomalies

### What are Anomalies?
Anomalies are abnormalities introduced by data redundancy that affect database operations.

### 1. Insertion Anomaly
When certain data cannot be inserted into the database without the presence of other data.

**Example**: Cannot insert a new course until a student enrolls in it.

### 2. Deletion Anomaly
Deletion of data results in unintended loss of other important data.

**Example**: Deleting a student's record might also delete course information if no other students are enrolled.

### 3. Updation (Modification) Anomaly
Update of a single data value requires multiple rows to be updated.

**Example**: Changing a professor's name requires updating all courses taught by that professor.

**Consequences**: Data inconsistency arises if updates are missed at some places.

---

## What is Normalization?

Normalization is a database optimization technique that:
- **Minimizes redundancy** from relations
- **Eliminates undesirable characteristics** (Insertion, Update, Deletion anomalies)
- **Divides composite attributes** into individual attributes
- **Breaks larger tables** into smaller tables linked by relationships

---

## Normal Forms

### 1. First Normal Form (1NF)

**Requirements**:
1. Every relation cell must have **atomic values**
2. Relation must not have **multi-valued attributes**

**Example - Before 1NF**:
```
Student_ID | Student_Name | Courses
101        | John         | {Math, Physics}
```

**Example - After 1NF**:
```
Student_ID | Student_Name | Course
101        | John         | Math
101        | John         | Physics
```

### 2. Second Normal Form (2NF)

**Requirements**:
1. Relation must be in **1NF**
2. There should be **no partial dependency**
   - All non-prime attributes must be **fully dependent** on Primary Key
   - Non-prime attributes cannot depend on **part of the Primary Key**

**Partial Dependency**: When non-key attribute depends on only part of composite primary key.

**Example - Before 2NF**:
```
{Student_ID, Course_ID} → Student_Name, Course_Name, Grade
```
Here `Student_Name` depends only on `Student_ID` (partial dependency).

**Example - After 2NF**:
```
Table1: {Student_ID} → Student_Name
Table2: {Course_ID} → Course_Name
Table3: {Student_ID, Course_ID} → Grade
```

### 3. Third Normal Form (3NF)

**Requirements**:
1. Relation must be in **2NF**
2. **No transitive dependency** exists
   - Non-prime attribute should not determine another non-prime attribute

**Transitive Dependency**: A → B and B → C, where A is primary key.

**Example - Before 3NF**:
```
Student_ID → Student_Name, Dept_ID, Dept_Name
```
Here `Dept_ID → Dept_Name` (transitive dependency).

**Example - After 3NF**:
```
Table1: {Student_ID} → Student_Name, Dept_ID
Table2: {Dept_ID} → Dept_Name
```

### 4. Boyce-Codd Normal Form (BCNF)

**Requirements**:
1. Relation must be in **3NF**
2. For any FD A → B, **A must be a super key**
   - We must not derive prime attribute from any prime or non-prime attribute

**Strictest Normal Form**: BCNF is stronger than 3NF.

**Example - Violation of BCNF**:
```
{Student, Course} → Professor
Professor → Course
```
Here `Professor → Course` but `Professor` is not a super key.

**Example - After BCNF**:
```
Table1: {Student, Course} → Professor
Table2: {Professor} → Course
```

---

## Advantages of Normalization

### 1. **Minimize Data Redundancy**
- Eliminates duplicate data storage
- Reduces storage requirements

### 2. **Greater Overall Database Organization**
- Structured and logical data arrangement
- Clear relationships between entities

### 3. **Data Consistency**
- Maintains data integrity
- Reduces update anomalies
- Ensures accurate and reliable data

### 4. **Improved Performance**
- Faster query execution
- Efficient storage utilization
- Better maintenance capabilities

---

## Practical Examples

### Example 1: Library Database

**Before Normalization**:
```
Book_ID | Title | Author_Name | Author_Email | Category | Category_Desc
1       | SQL   | John        | john@email   | DB       | Database
2       | C++   | John        | john@email   | PL       | Programming
```

**Problems**:
- Redundant author information
- Redundant category information
- Update anomalies

**After 3NF**:
```
Books: {Book_ID} → Title, Author_ID, Category_ID
Authors: {Author_ID} → Author_Name, Author_Email
Categories: {Category_ID} → Category, Category_Desc
```

### Example 2: University Database

**Unnormalized Table**:
```
Student_ID | Name | Courses | Grades
101        | John | {Math, Physics} | {A, B}
102        | Jane | {Math, Chemistry} | {A, A}
```

**After 1NF**:
```
Student_ID | Name | Course | Grade
101        | John | Math   | A
101        | John | Physics| B
102        | Jane | Math   | A
102        | Jane | Chemistry| A
```

**After 2NF & 3NF**:
```
Students: {Student_ID} → Name
Enrollments: {Student_ID, Course} → Grade
Courses: {Course} → Course_Info
```

---

## Interview Questions

### Q1. What is the difference between 3NF and BCNF?
**Answer**:
- **3NF**: Removes transitive dependencies but allows redundancy
- **BCNF**: Stronger form where every determinant must be a super key
- **BCNF is a subset of 3NF**: Every BCNF relation is in 3NF, but not vice versa

### Q2. When should we denormalize a database?
**Answer**:
- For read-heavy applications
- To improve query performance
- For reporting and analytics
- When join operations are too costly
- Trade-off: Performance vs. Data integrity

### Q3. Find normal form of given relation:
```
R(A, B, C, D) with FDs: {A → B, BC → D}
```

**Solution**:
- **Candidate Key**: {A, C} (since A, C can determine all attributes)
- **Prime Attributes**: A, C
- **Non-prime Attributes**: B, D

**Check 1NF**: Atomic values ✓
**Check 2NF**:
- A → B is partial dependency (A is part of candidate key)
- **Not in 2NF**

**After decomposition to 2NF**:
```
R1(A, B) with A → B
R2(A, C, D) with AC → AD
```

### Q4. Identify normal form:
```
R(A, B, C, D) with FDs: {AB → C, C → D}
```

**Solution**:
- **Candidate Key**: {A, B}
- **Prime Attributes**: A, B
- **Non-prime Attributes**: C, D

**Check 1NF**: ✓
**Check 2NF**: ✓ (No partial dependency)
**Check 3NF**:
- C → D is transitive dependency (non-prime → non-prime)
- **Not in 3NF**

**After decomposition to 3NF**:
```
R1(A, B, C) with AB → C
R2(C, D) with C → D
```

---

## Quick Reference Table

| Normal Form | Requirement | What it Removes |
|-------------|-------------|------------------|
| 1NF | Atomic values, no multi-valued attributes | Repeating groups |
| 2NF | 1NF + No partial dependency | Partial dependencies |
| 3NF | 2NF + No transitive dependency | Transitive dependencies |
| BCNF | 3NF + Every determinant is super key | Remaining anomalies |

---

## Normalization Checklist

### For Each Normal Form:

**1NF Checklist:**
- [ ] All attributes contain atomic values
- [ ] No multi-valued attributes
- [ ] No repeating groups

**2NF Checklist:**
- [ ] Satisfies 1NF
- [ ] No partial dependencies
- [ ] All non-key attributes fully dependent on primary key

**3NF Checklist:**
- [ ] Satisfies 2NF
- [ ] No transitive dependencies
- [ ] No non-key attributes depend on other non-key attributes

**BCNF Checklist:**
- [ ] Satisfies 3NF
- [ ] Every determinant is a super key
- [ ] No redundancy in functional dependencies

---

## Key Takeaways

1. **Normalization is a systematic process** to organize data efficiently
2. **Functional dependencies** are the foundation of normalization theory
3. **Armstrong's axioms** help derive new functional dependencies
4. **Each normal form** addresses specific types of anomalies
5. **BCNF is the strongest** commonly used normal form
6. **Trade-offs exist** between normalization and performance
7. **Real-world applications** may require denormalization for performance

**Remember**: The goal is to balance data integrity with performance requirements!

---

**Interview Tip**: Always identify candidate keys first, then check for partial and transitive dependencies systematically when determining normal forms.