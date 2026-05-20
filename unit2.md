
# 1. Informal Design Guidelines for Relational Schema

**Marks:** 8M
**Slide:** Database Design PPT — slides 4 to 24

Database design is used to produce good relation schemas. A good relation schema should be easy to understand, should avoid redundancy, should reduce NULL values, and should not generate spurious tuples.

According to the PPT, there are **four informal design guidelines**:

## Guideline 1: Semantics of Attributes

When we create a relation schema, the attributes in that relation should have a clear real-world meaning. The relation should represent either one entity type or one relationship type clearly.

A relation schema is good if its meaning is easy to explain.

Example of good design:

```text
EMPLOYEE(SSN, Name, Salary, Address, Dno)
DEPARTMENT(Dnumber, Dname, ManagerSSN)
```

Here, EMPLOYEE stores employee details, and DEPARTMENT stores department details. The meaning is clear.

Example of poor design:

```text
EMP_DEPT(SSN, Name, Salary, Dnumber, Dname, ManagerSSN)
```

This relation mixes employee details and department details in one table. This violates the first guideline because it combines attributes from multiple entity types.

**Rule:**
Do not combine attributes from different entity types and relationship types into a single relation.

## Guideline 2: Reduce Redundant Information and Update Anomalies

Redundancy means storing the same information repeatedly.

Example:

```text
EMP_DEPT(SSN, Name, Dnumber, Dname, ManagerSSN)
```

If many employees work in the same department, then department name and manager SSN will be repeated for every employee.

This causes three types of anomalies:

### 1. Insertion Anomaly

Insertion anomaly occurs when we cannot insert some data unless some other data is also available.

Example:
If a new department has no employees yet, we cannot insert department details in EMP_DEPT because employee SSN is required.

### 2. Deletion Anomaly

Deletion anomaly occurs when deleting one record accidentally removes useful information.

Example:
If the last employee of a department is deleted, then the department information is also lost.

### 3. Modification Anomaly

Modification anomaly occurs when the same data is repeated many times and must be updated everywhere.

Example:
If the manager of department 5 changes, we must update all employee tuples of department 5. If one tuple is missed, inconsistency occurs.

**Rule:**
Design relation schemas so that insertion, deletion, and modification anomalies are avoided.

## Guideline 3: Reduce NULL Values in Tuples

NULL means value is unknown, unavailable, or not applicable. Too many NULL values create problems.

Problems caused by NULL values:

```text
1. Wastage of storage space
2. Difficulty in understanding meaning
3. Problems in JOIN operations
4. Problems in aggregate functions like COUNT, SUM, AVG
```

Example of bad design:

```text
EMPLOYEE(SSN, Name, Salary, CarLicenseNo, OfficeNo, ParkingSlot)
```

If many employees do not have a car or office, then many values will be NULL.

Better design:

```text
EMPLOYEE(SSN, Name, Salary)
EMP_CAR(SSN, CarLicenseNo, ParkingSlot)
EMP_OFFICE(SSN, OfficeNo)
```

**Rule:**
Avoid placing attributes in a relation if their values are frequently NULL.

## Guideline 4: Avoid Spurious Tuples

Spurious tuples are incorrect extra tuples generated after joining decomposed relations.

This happens when a relation is decomposed badly and then joined again.

Example:

```text
EMP_PROJ(SSN, PNUMBER, HOURS, PLOCATION)
```

Bad decomposition:

```text
EMP_PROJ1(SSN, PNUMBER, HOURS)
EMP_LOCS(PLOCATION, PNUMBER)
```

If we join them incorrectly, extra invalid rows may be generated.

**Rule:**
Decompose relations only in such a way that joining them again gives the original relation without extra tuples.

This is called **lossless join** or **non-additive join** property.

## Final Answer Summary

The four informal design guidelines are:

```text
1. Give clear meaning to relation attributes.
2. Reduce redundant information and update anomalies.
3. Reduce NULL values.
4. Avoid spurious tuples after decomposition.
```

These guidelines help produce good relational database schemas. The PPT lists exactly these four guidelines and explains redundancy, NULL values, and spurious tuples as major design problems. 

---

# 2. Functional Dependency + Armstrong’s Axioms + Closure Algorithm

**Marks:** 6M + 10M
**Slide:** Database Design PPT — Functional Dependency section

## Functional Dependency

A **functional dependency** is a relationship between attributes of a relation.

A functional dependency is written as:

```text
X → Y
```

It means that the value of attribute set **X** uniquely determines the value of attribute set **Y**.

In simple words:
If two tuples have the same value of X, then they must have the same value of Y.

Example:

```text
STUDENT(USN, Name, Department, Phone)
```

Here:

```text
USN → Name
USN → Department
USN → Phone
```

Because USN uniquely identifies a student.

## Formal Meaning

For a relation R, the functional dependency X → Y holds if:

```text
For any two tuples t1 and t2:
If t1[X] = t2[X], then t1[Y] = t2[Y].
```

Example:

| USN | Name | Department |
| --- | ---- | ---------- |
| 101 | Ravi | CSE        |
| 102 | Aman | ECE        |

USN determines Name because each USN has only one student name.

## Types of Functional Dependency

### 1. Trivial Functional Dependency

A functional dependency X → Y is trivial if Y is a subset of X.

Example:

```text
{USN, Name} → USN
```

This is trivial because USN is already part of `{USN, Name}`.

### 2. Non-Trivial Functional Dependency

X → Y is non-trivial if Y is not a subset of X.

Example:

```text
USN → Name
```

Name is not part of USN.

### 3. Full Functional Dependency

X → Y is a full functional dependency if no attribute can be removed from X and still determine Y.

Example:

```text
{SSN, PNUMBER} → HOURS
```

Both SSN and PNUMBER are needed to determine HOURS.

### 4. Partial Dependency

X → Y is partial if some attribute can be removed from X and the dependency still holds.

Example:

```text
{SSN, PNUMBER} → ENAME
```

If:

```text
SSN → ENAME
```

then ENAME depends only on part of the composite key. So it is partial dependency. The PPT uses this same idea in the 2NF explanation. 

---

## Armstrong’s Axioms

Armstrong’s axioms are inference rules used to derive new functional dependencies from given functional dependencies.

The three main Armstrong axioms are:

## 1. Reflexivity Rule

If Y is a subset of X, then:

```text
X → Y
```

Example:

```text
{USN, Name} → USN
```

Because USN is part of `{USN, Name}`.

## 2. Augmentation Rule

If:

```text
X → Y
```

then:

```text
XZ → YZ
```

Example:

If:

```text
USN → Name
```

then:

```text
USN, Department → Name, Department
```

Same attribute Department is added on both sides.

## 3. Transitivity Rule

If:

```text
X → Y
Y → Z
```

then:

```text
X → Z
```

Example:

```text
USN → Department
Department → HOD
```

Therefore:

```text
USN → HOD
```

## Additional Inference Rules

These are derived from Armstrong’s axioms.

### 1. Decomposition Rule

If:

```text
X → YZ
```

then:

```text
X → Y
X → Z
```

Example:

```text
USN → Name, Department
```

So:

```text
USN → Name
USN → Department
```

### 2. Union Rule

If:

```text
X → Y
X → Z
```

then:

```text
X → YZ
```

Example:

```text
USN → Name
USN → Department
```

Therefore:

```text
USN → Name, Department
```

### 3. Pseudotransitivity Rule

If:

```text
X → Y
WY → Z
```

then:

```text
WX → Z
```

Example:

```text
USN → Department
Department, Course → Faculty
```

Therefore:

```text
USN, Course → Faculty
```

---

## Closure of Attribute Set: X⁺

The closure of an attribute set X, written as **X⁺**, is the set of all attributes that can be functionally determined by X using the given functional dependencies.

Example:

Relation:

```text
R(A, B, C, D, E)
```

Functional dependencies:

```text
A → B
B → C
C → D
D → E
```

Find A⁺.

Solution:

```text
A⁺ = {A}
A → B, so add B
A⁺ = {A, B}
B → C, so add C
A⁺ = {A, B, C}
C → D, so add D
A⁺ = {A, B, C, D}
D → E, so add E
A⁺ = {A, B, C, D, E}
```

So:

```text
A⁺ = {A, B, C, D, E}
```

Therefore, A is a candidate key because A determines all attributes.

---

## Closure Algorithm

To find X⁺:

```text
Step 1: Start with X⁺ = X.
Step 2: Check all functional dependencies.
Step 3: If Y → Z and Y is already inside X⁺, then add Z to X⁺.
Step 4: Repeat until no new attributes can be added.
Step 5: Final X⁺ is the closure of X.
```

## Example

Given:

```text
R(A, B, C, D, E, F)
F = {A → B, B → C, CD → E, A → D, E → F}
```

Find A⁺.

Solution:

```text
A⁺ = {A}

A → B, add B
A⁺ = {A, B}

B → C, add C
A⁺ = {A, B, C}

A → D, add D
A⁺ = {A, B, C, D}

CD → E, since C and D are present, add E
A⁺ = {A, B, C, D, E}

E → F, add F
A⁺ = {A, B, C, D, E, F}
```

Final answer:

```text
A⁺ = {A, B, C, D, E, F}
```

So A is a key.

## Importance of Closure

Closure is used to:

```text
1. Find candidate keys
2. Check whether a functional dependency is valid
3. Test normal forms
4. Find minimal cover
5. Perform decomposition
```

The uploaded PPT also includes many closure/minimal cover type problems in the functional dependency section. 

---

# 3. Normalization: 1NF, 2NF, 3NF with Examples

**Marks:** 8M
**Slide:** Database Design PPT — slides 62 to 83

## What is Normalization?

Normalization is the process of analyzing relation schemas using functional dependencies and primary keys to improve database design.

Main goals:

```text
1. Minimize redundancy
2. Minimize insertion anomalies
3. Minimize deletion anomalies
4. Minimize update anomalies
```

If a relation schema has problems, it is decomposed into smaller relations with better properties. The PPT says normalization provides a formal framework for analyzing relation schemas using keys and functional dependencies. 

---

## Important Terms

## Prime Attribute

An attribute is called a **prime attribute** if it is part of any candidate key.

Example:

```text
WORKS_ON(SSN, PNUMBER, HOURS)
```

If the key is:

```text
{SSN, PNUMBER}
```

Then:

```text
Prime attributes = SSN, PNUMBER
```

## Non-Prime Attribute

An attribute is non-prime if it is not part of any candidate key.

In the above example:

```text
Non-prime attribute = HOURS
```

The PPT defines prime and non-prime attributes before explaining normal forms. 

---

# First Normal Form — 1NF

## Definition

A relation is in **First Normal Form** if all attribute values are atomic, simple, and indivisible.

1NF does not allow:

```text
1. Multivalued attributes
2. Composite attributes
3. Nested relations
4. Repeating groups
```

The PPT states that 1NF allows only single atomic values. 

## Example: Not in 1NF

```text
DEPARTMENT(DNUMBER, DNAME, DLOCATIONS)
```

| DNUMBER | DNAME    | DLOCATIONS        |
| ------- | -------- | ----------------- |
| 5       | Research | Bengaluru, Mumbai |
| 6       | Admin    | Delhi             |

Here, DLOCATIONS has multiple values. So this relation is not in 1NF.

## Convert to 1NF

Best method: remove multivalued attribute and create a separate relation.

```text
DEPARTMENT(DNUMBER, DNAME)
DEPT_LOCATIONS(DNUMBER, DLOCATION)
```

| DNUMBER | DNAME    |
| ------- | -------- |
| 5       | Research |
| 6       | Admin    |

| DNUMBER | DLOCATION |
| ------- | --------- |
| 5       | Bengaluru |
| 5       | Mumbai    |
| 6       | Delhi     |

Now every value is atomic.

## Other Methods Mentioned in PPT

The PPT gives three methods to convert to 1NF:

```text
1. Remove multivalued attribute and place it in a separate relation.
2. Expand the key and create one tuple for each value.
3. Replace the multivalued attribute by fixed atomic attributes like DLOCATION1, DLOCATION2, DLOCATION3.
```

The first method is best because it avoids redundancy and is general. 

---

# Second Normal Form — 2NF

## Before 2NF: Full and Partial Dependency

### Full Functional Dependency

A functional dependency X → Y is full if removing any attribute from X makes the dependency invalid.

Example:

```text
{SSN, PNUMBER} → HOURS
```

Here both SSN and PNUMBER are needed to determine HOURS.

### Partial Dependency

A dependency is partial if a non-prime attribute depends only on part of a composite key.

Example:

```text
{SSN, PNUMBER} → ENAME
```

If:

```text
SSN → ENAME
```

then ENAME depends only on SSN, not on full key `{SSN, PNUMBER}`. So it is partial dependency.

The PPT gives this same example for full and partial dependency. 

## Definition of 2NF

A relation schema is in **Second Normal Form** if:

```text
1. It is in 1NF.
2. Every non-prime attribute is fully functionally dependent on the primary key.
```

In simple words:
No non-prime attribute should depend on only part of a composite primary key.

The PPT defines 2NF as every non-prime attribute being fully functionally dependent on the primary key. 

## Example: Not in 2NF

```text
EMP_PROJ(SSN, PNUMBER, ENAME, PNAME, HOURS)
```

Primary key:

```text
{SSN, PNUMBER}
```

Functional dependencies:

```text
{SSN, PNUMBER} → HOURS
SSN → ENAME
PNUMBER → PNAME
```

Here:

```text
ENAME depends only on SSN
PNAME depends only on PNUMBER
```

So ENAME and PNAME are partially dependent on the composite key. Therefore, EMP_PROJ is not in 2NF.

## Convert to 2NF

Decompose into:

```text
EMPLOYEE(SSN, ENAME)
PROJECT(PNUMBER, PNAME)
WORKS_ON(SSN, PNUMBER, HOURS)
```

Now:

```text
SSN → ENAME
PNUMBER → PNAME
{SSN, PNUMBER} → HOURS
```

Each non-prime attribute depends on the full key of its relation.

## Final Point

2NF mainly removes **partial dependency** and reduces redundancy.

---

# Third Normal Form — 3NF

## Before 3NF: Transitive Dependency

A transitive dependency occurs when a non-prime attribute depends on another non-prime attribute.

General form:

```text
X → Z
Z → Y
Therefore, X → Y
```

Here, Y is transitively dependent on X through Z.

The PPT defines transitive dependency and gives an EMP_DEPT example. 

## Definition of 3NF

A relation schema is in **Third Normal Form** if:

```text
1. It is in 2NF.
2. No non-prime attribute is transitively dependent on the primary key.
```

General definition:

For every non-trivial FD:

```text
X → A
```

Either:

```text
X is a superkey
OR
A is a prime attribute
```

The PPT gives this general 3NF definition. 

## Example: Not in 3NF

```text
EMP_DEPT(SSN, ENAME, DNUMBER, DNAME, DMGRSSN)
```

Primary key:

```text
SSN
```

Functional dependencies:

```text
SSN → ENAME
SSN → DNUMBER
DNUMBER → DNAME
DNUMBER → DMGRSSN
```

Here:

```text
SSN → DNUMBER
DNUMBER → DNAME
```

Therefore:

```text
SSN → DNAME
```

Also:

```text
SSN → DNUMBER
DNUMBER → DMGRSSN
```

Therefore:

```text
SSN → DMGRSSN
```

DNAME and DMGRSSN are transitively dependent on SSN through DNUMBER. So EMP_DEPT is not in 3NF. The PPT gives this same EMP_DEPT example. 

## Convert to 3NF

Decompose into:

```text
EMPLOYEE(SSN, ENAME, DNUMBER)
DEPARTMENT(DNUMBER, DNAME, DMGRSSN)
```

Now:

```text
SSN → ENAME, DNUMBER
DNUMBER → DNAME, DMGRSSN
```

No transitive dependency exists inside a single relation.

## Final Point

3NF removes **transitive dependency** and further reduces redundancy and update anomalies.

---

# Difference between 1NF, 2NF, and 3NF

| Normal Form | Main Rule                | Removes                              |
| ----------- | ------------------------ | ------------------------------------ |
| 1NF         | Values must be atomic    | Multivalued and composite attributes |
| 2NF         | No partial dependency    | Partial dependency                   |
| 3NF         | No transitive dependency | Transitive dependency                |

---

# Exam-Ready 8 Mark Answer: Normalization

Normalization is the process of analyzing relation schemas based on functional dependencies and keys to reduce redundancy and avoid insertion, deletion, and update anomalies. A relation is in 1NF if all attribute values are atomic and there are no multivalued or composite attributes. A relation is in 2NF if it is in 1NF and every non-prime attribute is fully dependent on the entire primary key. A relation is in 3NF if it is in 2NF and no non-prime attribute is transitively dependent on the primary key.

Example:

```text
EMP_PROJ(SSN, PNUMBER, ENAME, PNAME, HOURS)
Key = {SSN, PNUMBER}
FDs:
SSN → ENAME
PNUMBER → PNAME
{SSN, PNUMBER} → HOURS
```

This is not in 2NF because ENAME and PNAME depend on part of the composite key. Convert into:

```text
EMPLOYEE(SSN, ENAME)
PROJECT(PNUMBER, PNAME)
WORKS_ON(SSN, PNUMBER, HOURS)
```

For 3NF:

```text
EMP_DEPT(SSN, ENAME, DNUMBER, DNAME, DMGRSSN)
FDs:
SSN → DNUMBER
DNUMBER → DNAME, DMGRSSN
```

Here DNAME and DMGRSSN are transitively dependent on SSN. Convert into:

```text
EMPLOYEE(SSN, ENAME, DNUMBER)
DEPARTMENT(DNUMBER, DNAME, DMGRSSN)
```

Thus normalization improves database design by reducing redundancy and avoiding anomalies.

---

## Unit 2 Super Quick Revision

Study these in order:

1. Four informal design guidelines
2. Insertion, deletion, modification anomalies
3. Functional dependency definition
4. Armstrong’s axioms: reflexivity, augmentation, transitivity
5. Closure algorithm X⁺
6. 1NF: atomic values
7. 2NF: remove partial dependency
8. 3NF: remove transitive dependency

