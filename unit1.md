
# 1. Characteristics of Database Approach

**Slide/Page:** Introduction to DBMS PDF — p.11 to p.18
**Marks:** 8M

The database approach is different from the traditional file-processing approach. In a database system, data is stored in a centralized and organized way, and the DBMS controls access, structure, security, sharing, and recovery.

### Main characteristics

### 1. Self-describing nature of database system

A database system contains not only the actual data, but also the complete description of the database structure and constraints. This description is called **metadata**.

Metadata is stored in the **DBMS catalog**. The catalog stores information such as relation names, column names, data types, constraints, and relationships.

Example:
In a UNIVERSITY database, the DBMS catalog may store that the STUDENT relation has columns like Name, Student_number, Class, and Major.

### 2. Insulation between programs and data

In traditional file systems, the structure of data is written inside application programs. If the file structure changes, the program must also be changed.

In DBMS, data structure is stored separately in the catalog. So changes in data structure do not always affect application programs. This property is called **program-data independence**.

Example:
If a new column is added to a STUDENT table, old programs may still work without modification.

### 3. Data abstraction

DBMS hides internal storage details from users. Users only see a simple logical view of data. They do not need to know how data is physically stored on disk.

Example:
A user writes:

```sql
SELECT Name FROM STUDENT;
```

The user does not need to know where the file is stored or how indexes are used.

### 4. Support for multiple views

Different users can have different views of the same database. Each user sees only the data required for their work.

Example:
A student may see marks and attendance.
An admin may see fee details.
A teacher may see student performance.

### 5. Sharing of data and multiuser transaction processing

A DBMS allows multiple users to access the database at the same time. It uses concurrency control to make sure data remains correct.

Example:
In a banking system, many users may withdraw or deposit money at the same time. DBMS ensures correct updates.

**Conclusion:**
The database approach provides centralized control, reduced redundancy, better security, data independence, multiple views, and safe multiuser access. 

---

# 2. Three Schema Architecture with Neat Diagram

**Slide/Page:** Introduction to DBMS PDF — Three Schema Architecture section
**Marks:** 8M

Three schema architecture separates a database system into three levels. It helps in achieving **data independence**.

```text
              External Level
        User View 1 | User View 2 | User View 3
                    ↓
        External / Conceptual Mapping
                    ↓
              Conceptual Level
        Complete logical structure of database
                    ↓
        Conceptual / Internal Mapping
                    ↓
               Internal Level
        Physical storage of database
```

## 1. External Level

External level is also called the **view level**. It describes the part of the database that a particular user group is interested in.

Example:
A student view may contain only Name, USN, Marks.
An admin view may contain Name, Fees, Address.

It hides the rest of the database from that user.

## 2. Conceptual Level

The conceptual level describes the complete logical structure of the whole database. It includes entities, attributes, relationships, data types, and constraints.

Example:

```text
STUDENT(USN, Name, Age, Department)
COURSE(CourseID, CourseName, Credits)
ENROLLED(USN, CourseID, Grade)
```

It hides physical storage details.

## 3. Internal Level

The internal level describes how data is physically stored in the system. It includes file organization, indexes, storage paths, and record placement.

Example:
STUDENT relation may be stored as an unordered file with an index on USN.

## Need for mappings

Mappings are required to convert requests from one level to another.

There are two mappings:

1. **External/Conceptual mapping**
   Converts user views into conceptual schema.

2. **Conceptual/Internal mapping**
   Converts conceptual schema into physical storage details.

## Data Independence

Data independence means the ability to change schema at one level without changing schema at the next higher level.

### Types:

### 1. Logical Data Independence

Ability to change conceptual schema without affecting external views or application programs.

Example:
Adding a new column Email to STUDENT should not affect old user views.

### 2. Physical Data Independence

Ability to change internal schema without affecting conceptual schema.

Example:
Adding an index or changing file organization should not affect tables seen by users.

**Conclusion:**
Three schema architecture separates user view, logical structure, and physical storage. It provides security, abstraction, and data independence. 

---

# 3. Constraints

**Slide/Page:** Relational Model PDF — p.27 onwards
**Marks:** 6M / 8M

Constraints are rules applied on a database to maintain correctness and validity of data.

## Types of constraints

### 1. Domain Constraint

A domain is a set of valid atomic values for an attribute. Domain constraint ensures that an attribute can take only valid values.

Example:

```text
Age must be between 16 and 60.
Phone_number must contain 10 digits.
```

In SQL:

```sql
Age INT CHECK (Age >= 16 AND Age <= 60)
```

### 2. Key Constraint

A key is an attribute or set of attributes that uniquely identifies a tuple in a relation.

Example:

```text
STUDENT(USN, Name, Age)
```

Here, USN can be the key because every student has a unique USN.

### 3. Entity Integrity Constraint

Entity integrity says that the **primary key cannot be NULL**.

Reason:
A primary key is used to identify each tuple. If it is NULL, the tuple cannot be uniquely identified.

Example:

```sql
USN VARCHAR(20) PRIMARY KEY
```

### 4. Referential Integrity Constraint

Referential integrity is maintained using a foreign key. A foreign key value must either match a primary key value in the referenced table or be NULL if allowed.

Example:

```text
DEPARTMENT(Dnumber, Dname)
EMPLOYEE(EmpID, Name, Dno)
```

Here, `Dno` in EMPLOYEE is a foreign key referencing `Dnumber` in DEPARTMENT.

### 5. Application-based Constraint

These are business rules that cannot always be directly represented in the database schema.

Example:

```text
Employee salary must not exceed manager salary.
A student cannot register for more than 6 courses.
```

**Conclusion:**
Constraints help maintain accuracy, consistency, and integrity of data in a database. 

---

# 4. Symbols Used in ER Diagram

**Slide/Page:** ER Model PDF — ER Diagram section
**Marks:** 4M / 6M

An ER diagram uses different symbols to represent entities, attributes, relationships, and constraints.

| Symbol               | Meaning                  |
| -------------------- | ------------------------ |
| Rectangle            | Entity type              |
| Double rectangle     | Weak entity type         |
| Oval                 | Attribute                |
| Underlined oval/text | Key attribute            |
| Double oval          | Multivalued attribute    |
| Dashed oval          | Derived attribute        |
| Diamond              | Relationship             |
| Double diamond       | Identifying relationship |
| Single line          | Partial participation    |
| Double line          | Total participation      |
| 1, N, M              | Cardinality ratio        |

## Explanation

### Entity

An entity is represented using a rectangle.

Example:

```text
STUDENT
EMPLOYEE
DEPARTMENT
```

### Weak Entity

A weak entity does not have its own primary key. It depends on an owner entity. It is represented using a double rectangle.

Example:

```text
DEPENDENT
```

### Attribute

An attribute describes an entity. It is represented using an oval.

Example:

```text
Name, Age, Address
```

### Key Attribute

A key attribute uniquely identifies an entity. It is underlined.

Example:

```text
USN for STUDENT
SSN for EMPLOYEE
```

### Multivalued Attribute

An attribute that can have multiple values is shown using a double oval.

Example:

```text
Phone_Number
College_Degree
```

### Derived Attribute

A derived attribute is calculated from another attribute. It is shown using a dashed oval.

Example:

```text
Age can be derived from Date_of_Birth.
```

### Relationship

A relationship is represented using a diamond.

Example:

```text
EMPLOYEE works_for DEPARTMENT
```

### Total and Partial Participation

Total participation is shown using a double line. Partial participation is shown using a single line. 

---

# 5. ER Diagram

**Slide/Page:** ER Model PDF — ER diagram and company database section
**Marks:** 8M / 10M

An **Entity Relationship Diagram** is a high-level conceptual diagram used to represent the structure of a database. It shows entities, attributes, relationships, keys, cardinality, and participation constraints.

## Main components of ER diagram

### 1. Entity

An entity is a real-world object with independent existence.

Example:

```text
STUDENT, EMPLOYEE, DEPARTMENT, COURSE
```

### 2. Attribute

Attributes describe properties of an entity.

Example:

```text
STUDENT → USN, Name, Age, Department
```

### 3. Relationship

A relationship represents association between entities.

Example:

```text
STUDENT enrolls in COURSE
EMPLOYEE works for DEPARTMENT
```

### 4. Key Attribute

A key attribute uniquely identifies each entity.

Example:

```text
USN uniquely identifies STUDENT.
```

### 5. Cardinality Ratio

Cardinality tells how many entities can participate in a relationship.

Types:

```text
1:1
1:N
M:N
```

Example:

```text
One department has many employees → 1:N
```

### 6. Participation Constraint

Participation tells whether all or only some entities participate in a relationship.

Types:

```text
Total participation
Partial participation
```

## Example ER diagram

```text
 STUDENT                         COURSE
+---------+                     +----------+
| USN     |                     | CourseID |
| Name    |                     | CName    |
| Age     |                     | Credits  |
+---------+                     +----------+
     \                              /
      \                            /
       -------- ENROLLS ----------
```

Here, STUDENT and COURSE are entities. ENROLLS is the relationship.

**Conclusion:**
ER diagram is used during conceptual database design. It helps convert real-world requirements into a clear database structure. 

---

# 6. Types of Attributes in ER Model

**Slide/Page:** ER Model PDF — p.13 to p.17
**Marks:** 6M / 8M

Attributes describe the properties of an entity. In ER modeling, attributes are classified into different types.

## 1. Simple Attribute

A simple attribute cannot be divided into smaller parts.

Example:

```text
Age
Salary
Gender
```

## 2. Composite Attribute

A composite attribute can be divided into smaller subparts.

Example:

```text
Address → Street, City, State, Zip
Name → FirstName, MiddleName, LastName
```

## 3. Single-valued Attribute

A single-valued attribute has only one value for each entity.

Example:

```text
Age of a person
Date_of_Birth
```

## 4. Multivalued Attribute

A multivalued attribute can have more than one value.

Example:

```text
Phone_Number
College_Degree
Car_Color
```

In ER diagram, it is represented using a double oval.

## 5. Stored Attribute

A stored attribute is directly stored in the database.

Example:

```text
Date_of_Birth
```

## 6. Derived Attribute

A derived attribute is calculated from another attribute.

Example:

```text
Age is derived from Date_of_Birth.
```

It is represented using a dashed oval.

## 7. Key Attribute

A key attribute uniquely identifies an entity.

Example:

```text
USN for STUDENT
SSN for EMPLOYEE
```

It is represented by underlining the attribute name.

## 8. Complex Attribute

A complex attribute is a combination of composite and multivalued attributes.

Example:

```text
Previous_Degrees may include Degree, Year, College, Specialization.
```

**Conclusion:**
Attributes help describe entities in detail. Correct identification of attribute types is important for designing a proper ER model. 

---

# 7. Characteristics of a Relation

**Slide/Page:** Relational Model PDF — p.14 to p.21
**Marks:** 6M / 8M

In relational model, data is represented as relations. A relation looks like a table with rows and columns.

A row is called a **tuple**.
A column is called an **attribute**.
A table is called a **relation**. 

## Characteristics

### 1. Ordering of tuples does not matter

A relation is a set of tuples. Since sets do not have order, rows in a relation do not have any fixed order.

Example:

```text
STUDENT rows can be displayed in any order.
```

The meaning of the relation remains the same.

### 2. Ordering of values in tuple

At the logical level, order of attributes is not important as long as attribute-value correspondence is maintained.

Example:

```text
(Name = Ravi, Age = 20)
```

is same as:

```text
(Age = 20, Name = Ravi)
```

if attributes are clearly identified.

### 3. Atomic values

Each value in a tuple must be atomic, meaning it cannot be divided further.

Example:

```text
Phone_Number should not contain multiple phone numbers in one cell.
```

Multivalued attributes must be stored in a separate relation.

### 4. NULL values

A relation may contain NULL values. NULL means value is unknown, unavailable, or not applicable.

Example:

```text
OfficePhone may be NULL if a student has no office phone.
```

### 5. No duplicate tuples

Since a relation is a set, duplicate tuples are not allowed.

### 6. Meaning of relation

Each relation should have a clear meaning. A relation schema can be considered as an assertion, and each tuple is a fact.

Example:

```text
STUDENT(Name, USN, Age)
```

Each tuple represents one student fact.

**Conclusion:**
A good relation must have atomic values, no duplicate tuples, clear meaning, and proper handling of NULL values. 

---

# 8. ER-to-Relational Mapping Algorithm

**Slide/Page:** Relational Model PDF — ER-to-Relational Mapping section
**Marks:** 6M / 8M

ER-to-Relational Mapping converts an ER diagram into relational tables.

## Step 1: Mapping Regular Entity Types

For each strong entity type, create a relation.

Example:

```text
EMPLOYEE(SSN, Name, Salary)
```

The key attribute of the entity becomes the primary key of the relation.

## Step 2: Mapping Weak Entity Types

For each weak entity, create a relation including its attributes and the primary key of the owner entity as foreign key.

Example:

```text
DEPENDENT(EmpSSN, DependentName, Age)
```

Primary key may be:

```text
(EmpSSN, DependentName)
```

## Step 3: Mapping Binary 1:1 Relationship

For a 1:1 relationship, include the primary key of one relation as foreign key in the other relation. Prefer the side with total participation.

Example:

```text
EMPLOYEE manages DEPARTMENT
```

Add `ManagerSSN` to DEPARTMENT.

## Step 4: Mapping Binary 1:N Relationship

For a 1:N relationship, add the primary key of the 1-side as foreign key in the N-side.

Example:

```text
DEPARTMENT has EMPLOYEE
```

Add `Dnumber` in EMPLOYEE.

```text
EMPLOYEE(EmpID, Name, Dnumber)
```

## Step 5: Mapping Binary M:N Relationship

For M:N relationship, create a new relation. Include primary keys of both participating entities as foreign keys.

Example:

```text
STUDENT enrolls COURSE
```

Create:

```text
ENROLLS(USN, CourseID, Grade)
```

## Step 6: Mapping Multivalued Attributes

For each multivalued attribute, create a separate relation.

Example:

```text
EMPLOYEE has multiple phone numbers
```

Create:

```text
EMP_PHONE(EmpID, PhoneNo)
```

## Step 7: Mapping N-ary Relationship

For relationship involving more than two entities, create a new relation containing primary keys of all participating entities.

Example:

```text
SUPPLIER supplies PART to PROJECT
```

Create:

```text
SUPPLY(SupplierID, PartID, ProjectID, Quantity)
```

**Conclusion:**
This algorithm converts conceptual ER design into relational database schema systematically. 

---

# 9. Relational Constraints and Relational Algebra

**Slide/Page:** Relational Model PDF — constraints and relational algebra sections
**Marks:** 8M / 10M

## Part A: Relational Constraints

### 1. Domain Constraint

Each attribute must take values from a valid domain.

Example:

```text
Age must be integer.
Salary must be numeric.
```

### 2. Key Constraint

A key uniquely identifies tuples in a relation.

Example:

```text
USN in STUDENT table.
```

### 3. Entity Integrity

Primary key cannot be NULL.

Example:

```text
A STUDENT tuple cannot have NULL USN.
```

### 4. Referential Integrity

A foreign key must refer to an existing primary key value.

Example:

```text
EMPLOYEE.Dno must match DEPARTMENT.Dnumber.
```

## Part B: Relational Algebra

Relational algebra is a procedural query language. It uses operators to retrieve data from relations.

### 1. SELECT operation — σ

Used to select rows satisfying a condition.

Example:

```text
σ Age > 18 (STUDENT)
```

### 2. PROJECT operation — π

Used to select specific columns.

Example:

```text
π Name, USN (STUDENT)
```

### 3. UNION — ∪

Combines tuples from two union-compatible relations.

Example:

```text
CS_STUDENTS ∪ EC_STUDENTS
```

### 4. SET DIFFERENCE — −

Returns tuples in one relation but not in another.

Example:

```text
ALL_STUDENTS − PASSED_STUDENTS
```

### 5. CARTESIAN PRODUCT — ×

Combines every tuple of one relation with every tuple of another.

Example:

```text
STUDENT × COURSE
```

### 6. JOIN

Combines related tuples from two relations.

Example:

```text
STUDENT ⨝ ENROLLS
```

### 7. RENAME — ρ

Used to rename relation or attributes.

### 8. DIVISION

Used for queries involving “for all”.

Example:
Find students who enrolled in all courses.

**Conclusion:**
Relational constraints maintain correctness, while relational algebra provides operations to retrieve and manipulate data. 

---

# 10. DBMS

**Slide/Page:** Introduction to DBMS PDF — p.7 to p.9
**Marks:** 4M / 6M

A **Database Management System** is a general-purpose software system that allows users to define, construct, manipulate, and share databases.

## Functions of DBMS

### 1. Defining

Specifying data types, structures, and constraints.

Example:

```sql
CREATE TABLE STUDENT(
  USN VARCHAR(20),
  Name VARCHAR(30)
);
```

### 2. Constructing

Storing actual data on storage media.

Example:
Saving student records on disk.

### 3. Manipulating

Querying, updating, inserting, deleting, and generating reports.

Example:

```sql
SELECT * FROM STUDENT;
```

### 4. Sharing

Allowing multiple users and applications to access the database concurrently.

### 5. Protection

DBMS provides security and authorization.

Example:
Only admin can update marks.

### 6. Maintaining

DBMS allows database evolution, backup, and recovery.

## Database System

A database and DBMS together are called a **database system**.

```text
Database System = Database + DBMS Software
```

**Conclusion:**
DBMS is software that helps users store, manage, retrieve, secure, and share data efficiently. 

---

# 11. Disadvantages of File System compared to DBMS

**Slide/Page:** MSE Question Bank — Q6
**Marks:** 6M / 8M

Traditional file systems have many disadvantages compared to DBMS.

## 1. Data Redundancy

Same data may be stored in multiple files.

Example:
Student name may be stored in marks file, attendance file, and fee file.

## 2. Data Inconsistency

Because of redundancy, same data may have different values in different files.

Example:
One file shows student address as Udupi, another shows Mangalore.

## 3. Difficulty in Accessing Data

In file systems, new programs must be written for new queries.

Example:
To find students with marks above 90, a separate program may be needed.

## 4. Poor Data Integrity

It is difficult to enforce rules in file systems.

Example:

```text
Marks should not be greater than 100.
Age should not be negative.
```

## 5. Poor Security

File systems do not provide strong access control.

Example:
Unauthorized users may access sensitive data.

## 6. Atomicity Problems

If a transaction fails midway, file system may not restore data properly.

Example:
During money transfer, amount is debited but not credited due to failure.

## 7. Concurrent Access Problems

Multiple users accessing the same file can create inconsistency.

Example:
Two users updating the same bank balance at the same time.

**Conclusion:**
DBMS solves these problems using centralized control, constraints, security, concurrency control, backup, and recovery. 

---

# 12. Data Models

**Slide/Page:** Introduction PDF Data Models section / MSE QB Q7
**Marks:** 6M

A data model is a collection of concepts used to describe the structure of a database.

It describes:

```text
Data
Relationships
Constraints
Operations
```

## Types of Data Models

### 1. High-level or Conceptual Data Model

This model is close to how users see data. It uses concepts like entities, attributes, and relationships.

Example:

```text
STUDENT has attributes USN, Name, Age.
STUDENT enrolls in COURSE.
```

ER model is an example of conceptual data model.

### 2. Low-level or Physical Data Model

This model describes how data is physically stored in the computer.

It includes:

```text
Record formats
File organization
Indexes
Access paths
```

Example:
Student records stored in sorted file with index on USN.

### 3. Representational or Implementation Data Model

This model is between conceptual and physical models. It represents data using record structures.

Examples:

```text
Relational model
Network model
Hierarchical model
Object-relational model
```

The most common is the relational model, where data is stored as tables.

**Conclusion:**
Data models help in designing and understanding databases at different levels: user level, logical level, and physical level. 

---

# 13. Types of Integrity

**Slide/Page:** Relational Model PDF — constraints section / MSE QB Q8
**Marks:** 6M

Integrity means maintaining accuracy and consistency of data in the database.

## 1. Entity Integrity

Entity integrity states that primary key values cannot be NULL.

Reason:
A primary key is used to uniquely identify each tuple.

Example:

```text
STUDENT(USN, Name, Age)
```

USN cannot be NULL.

SQL example:

```sql
USN VARCHAR(20) PRIMARY KEY
```

## 2. Referential Integrity

Referential integrity states that a foreign key value must refer to an existing primary key value in another table.

Example:

```text
DEPARTMENT(Dnumber, Dname)
EMPLOYEE(EmpID, Name, Dno)
```

Here, `Dno` in EMPLOYEE must match `Dnumber` in DEPARTMENT.

SQL example:

```sql
FOREIGN KEY (Dno) REFERENCES DEPARTMENT(Dnumber)
```

## 3. Domain Integrity

Domain integrity ensures that values of attributes are from a valid domain.

Example:

```text
Age must be integer.
Gender must be Male/Female/Other.
Marks must be between 0 and 100.
```

SQL example:

```sql
Marks INT CHECK (Marks >= 0 AND Marks <= 100)
```

## 4. Key Integrity

Key integrity ensures that candidate keys uniquely identify tuples.

Example:

```text
No two students can have the same USN.
```

**Conclusion:**
Integrity constraints protect the database from invalid, duplicate, inconsistent, and meaningless data. 

---

# 14. Total Participation and Partial Participation

**Slide/Page:** ER Model PDF — Structural Constraints section
**Marks:** 6M

Participation constraint specifies whether all entities or only some entities participate in a relationship.

## 1. Total Participation

Total participation means every entity in an entity set must participate in a relationship.

It is also called **existence dependency**.

In ER diagram, total participation is shown by a **double line**.

Example:

```text
Every employee must work for a department.
```

Diagram:

```text
EMPLOYEE == WORKS_FOR -- DEPARTMENT
```

Here, EMPLOYEE has total participation in WORKS_FOR if every employee must belong to a department.

## 2. Partial Participation

Partial participation means only some entities in an entity set participate in a relationship.

In ER diagram, partial participation is shown by a **single line**.

Example:

```text
Only some employees manage departments.
```

Diagram:

```text
EMPLOYEE -- MANAGES == DEPARTMENT
```

Here, not every employee is a manager, so EMPLOYEE participation in MANAGES is partial.

## Difference

| Total Participation                          | Partial Participation                      |
| -------------------------------------------- | ------------------------------------------ |
| Every entity must participate                | Some entities may participate              |
| Shown by double line                         | Shown by single line                       |
| Existence dependent                          | Not existence dependent                    |
| Example: Every employee works for department | Example: Some employees manage departments |

**Conclusion:**
Participation constraints help specify minimum relationship requirements in ER modeling. 

---

# 15. Aggregate Functions

**Slide/Page:** SQL PPT — slides 99 to 113
**Marks:** 6M

Aggregate functions are used to perform calculations on a group of values and return a single value.

## Common aggregate functions

| Function | Meaning                         |
| -------- | ------------------------------- |
| COUNT    | Counts number of rows or values |
| SUM      | Finds total                     |
| AVG      | Finds average                   |
| MAX      | Finds maximum                   |
| MIN      | Finds minimum                   |

## 1. COUNT

COUNT returns the number of tuples or values.

Example:

```sql
SELECT COUNT(*) FROM EMPLOYEE;
```

This returns the total number of employees.

## 2. SUM

SUM returns the total of numeric values.

Example:

```sql
SELECT SUM(SALARY) FROM EMPLOYEE;
```

This returns total salary of all employees.

## 3. AVG

AVG returns the average value.

Example:

```sql
SELECT AVG(SALARY) FROM EMPLOYEE;
```

This returns average salary.

## 4. MAX

MAX returns the highest value.

Example:

```sql
SELECT MAX(SALARY) FROM EMPLOYEE;
```

This returns maximum salary.

## 5. MIN

MIN returns the lowest value.

Example:

```sql
SELECT MIN(SALARY) FROM EMPLOYEE;
```

This returns minimum salary.

## Aggregate functions with GROUP BY

GROUP BY is used to divide tuples into groups and apply aggregate functions to each group.

Example:

```sql
SELECT DNO, COUNT(*), AVG(SALARY)
FROM EMPLOYEE
GROUP BY DNO;
```

This gives number of employees and average salary for each department.

## Aggregate functions with HAVING

HAVING is used to apply condition on groups.

Example:

```sql
SELECT DNO, COUNT(*)
FROM EMPLOYEE
GROUP BY DNO
HAVING COUNT(*) > 5;
```

This displays departments having more than 5 employees.

**Conclusion:**
Aggregate functions are useful for summary calculations such as count, total, average, maximum, and minimum. They are commonly used with GROUP BY and HAVING. 

---

## Unit 1 Quick Revision Order

1. Characteristics of Database Approach
2. Three Schema Architecture
3. DBMS + File System Disadvantages
4. Data Models
5. ER Diagram Symbols
6. Types of Attributes
7. Total and Partial Participation
8. ER-to-Relational Mapping
9. Relational Constraints
10. Characteristics of Relation
11. Aggregate Functions
