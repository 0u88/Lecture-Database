---
marp: true
theme: default
class: invert
size: 16:9
paginate: true
footer: 國立陽明交通大學 電子與光子學士學位學程
headingDivider: 1
style: |
  section::after {
    content: attr(data-marpit-pagination) '/' attr(data-marpit-pagination-total);
  }
  
  .middle-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }
  .middle-grid img {
    width: 75%;
  }
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }
  .grid img {
    width: 100%;
  }
  .red-text {
    color: red;
  }
  
  .blue-text {
    color: blue;  
  }

  .brown-text {
    color: brown;  
  }

  .small-text {
    font-size: 0.80rem;
  }
---
# Chapter6: Normalization of Database Tables
- Good database design must be matched to good table structures. 
- Learn to evaluate and design good table structures to **control data redundancies** thereby avoiding data anomalies.
- The process that yields such desirable results is known as **normalization**.

# Database Tables and Normalization
- <span class="blue-text">**Normalization**</span> is a process for evaluating and correcting table structures to <span class="brown-text">**minimize data redundancies**</span> 
  - Reduce data anomalies
  - Assigns attributes to tables based on determination
- Normalization works through a series of stages called normal forms
![bg right:50% w:100%](https://www.c-sharpcorner.com/UploadFile/nipuntomar/normalization-and-its-types/Images/Norm.gif)

# A Sample Report Layout
![bg right:70% w:100%](restricted/CTable06_01.jpg)

# Poor Table Structure
![bg right:60% w:100%](restricted/CFig06_01.jpg)
- Data inconsistency
- Difficult to update
- Data redundant

# Enough Normalization
- From a structural point of view, higher normal forms are better than lower normal forms
- For most purposes in business database design, 3NF is as high as you need to go in the normalization process
- Denormalization produces a lower normal form to increase performance but greater data redundancy

# The Need for Normalization
- Database designers commonly use normalization in the following two situations:
  - When designing a new database structure
  - To analyze the relationship among the attributes within each entity and determine if the structure can be improved through normalization
- The main goal of normalization is to eliminate data anomalies by eliminating unnecessary or unwanted data redundancies
- Normalization uses the concept of functional dependencies to identify which attribute determines other attributes

# The Normalization Process
The objective of normalization is to ensure:
- Each table represents a single subject
- Each row/column intersection contains only one value and not a group of values
- No data item will be unnecessarily stored in more than one table. Data is updated in only one place
- All non-prime attributes in a table are dependent on the primary key
- Each table has no insertion, update, or deletion anomalies
- Ensure that all tables are in at least in 3NF in business environment
- Work one relation at a time, identifying the determination and functional dependencies of a relation (table)

# Normal Forms
![bg right:70% w:100%](restricted/CTable06_02.jpg)

# Normalization Base: Functional Dependency
- Normalization starts by identifying the dependencies of a given table
- Them progressively breaking up the table into a set of new tables based on the identified dependencies.
- Functional Dependency: The attribute B is fully functionally dependent on the attribute A if each value of A determines one and only one value of B.
- Example: 
  - PROJ_NUM → PROJ_ NAME (read as PROJ_ NUM functionally determines PROJ_NAME)
  - Attribute PROJ_NUM is known as the determinant attribute
  - Attribute PROJ_NAME is known as the dependent attribute.

# Functional Dependency Type: Partial dependency
When there is a functional dependence in which the determinant is only part of the PK
- The assumption is that there is only one candidate key 
- Partial dependencies tend to be straightforward and easy to identify

Example
- functional dependency: (A, B) &rarr; (C, D)
- functional dependency: B &rarr; C
- primary key: (A, B)
- We will call B &rarr; C is partial dependency 

# Functional Dependency - Transitive dependency
When the attribute is dependent on another attribute that is not part of the primary key
- Transitive dependencies are more difficult to identify among a set of data
- They occur only when a functional dependence exists among non-prime attributes
Example
- functional dependencies X → Y
- functional dependencies Y → Z
- primary key: X 
- the dependency X → Z is a transitive dependency because X determines the value of Z via Y. 
- Y → Z signals that a transitive dependency exists because Y is not a PK.

# Why Do We Do Database Normalization?
![bg right:70% w:100%](https://cdn.hackr.io/uploads/posts/attachments/1666888816mdnYlrMoEE.png)

# Conversion to First Normal Form (1NF)
A table in the first normal form means
- All key attributes are defined
- There are no repeating groups in the table
- All attributes are dependent on the primary key

Converting to 1NF starts with three steps
1. Eliminate the repeating groups 
2. Identify the primary key 
3. Identify all dependencies

# 1NF Step1 - Eliminate Repeating Groups
Repeating group: a group of entries existing for a single key value

<div class="grid">
    <img src="restricted/CFig06_01.jpg" alt="">
    <img src="restricted/CFig06_02.jpg" alt="">
</div> 

# 1NF Step2 - Identify PK 
PK: an identifier composed of one or more attributes that uniquely identifies a row
<span class="brown-text">PROJ_NUM + EMP_NUM</span>

![bg right:70% w:100%](restricted/CFig06_02.jpg)

# 1NF Step3 - Identify all Dependencies
- According to PK, a dependency exist
  - PROJ_NUM, EMP_NUM → PROJ_NAME, EMP_NAME, JOB_CLASS, CHG_HOUR, HOURS
  - *PROJ_NAME, EMP_NAME, JOB_CLASS, CHG_HOUR, HOURS* depends on **PROJ_NUM, EMP_NUM**
- (partial dependency) PROJ_NUM → PROJ_NAME
- (partial dependency) EMP_NUM → EMP_NAME, JOB_CLASS, CHG_HOUR, <span class="brown-text">(not HOURS)</span>
- (transitive dependency) JOB_CLASS → CHG_HOUR

# Dependency Diagram
Dependency diagram shows all dependencies found within given table structure
![bg right:70% w:90%](restricted/CFig06_03.jpg)

# After 1NF
- All relational tables satisfy 1NF requirements
  - All key attributes are defined
  - There are no repeating groups in the table
  - All attributes are dependent on the primary key
- Some tables may contain partial and transitive dependencies

# Conversion to Second Normal Form (2NF)
A table in the second normal form means
- it is in 1NF
- it does not include partial dependencies

Conversion to 2NF occurs only when the 1NF has a composite primary key
- If the 1NF has a single-attribute primary key, then the table is automatically in 2NF

Converting to 2NF starts with two steps
1. Make new tables to eliminate partial dependencies
2. Reassign corresponding dependent attributes

# 2NF Step1 Make New Tables to Eliminate Partial Dependencies
- Separate composite PK (PROJ_NUM + EMP_NUM) into different PKs
  - PK1: PROJ_NUM
  - PK2: EMP_NUM
  - PK3: PROJ_NUM + EMP_NUM
- Create tables based on new PK
  - Table1: PROJECT, PK is PROJ_NUM
  - Table2: EMPLOYEE, PK is EMP_NUM
  - Table3: ASSIGNMENT, PK is PROJ_NUM + EMP_NUM

# 2NF Step2 Reassign Corresponding Dependent Attributes
- Table PROJECT(**PROJ_NUM**, PROJ_NAME)
- Table EMPLOYEE(**EMP_NUM**, EMP_NAME, JOB_CLASS, CHG_HOUR)
- Table ASSIGNMENT(**PROJ_NUM**, **EMP_NUM**, <span class="brown-text">ASSIGN_HOUR</span>)
(any attributes that are not dependent in partial dependency will remain in the original table)

# Dependency Diagram
![bg right:70% w:90%](restricted/CFig06_04.jpg)

# After 2NF
- All relational tables satisfy 2NF requirements
  - it is in 1NF
  - it does not include partial dependencies
  - If the 1NF has a single-attribute primary key, then the table is automatically in 2NF
- Some tables may contain transitive dependencies

# Conversion to Third Normal Form (3NF)
A table in the third normal form means
- it is in 2NF
- it does not include transitive dependencies

Converting to 3NF starts with two steps
1. Make new tables to eliminate transitive dependencies 
2. Reassign corresponding dependent attributes

# 3NF Step1 Make New Tables to Eliminate Transitive Dependencies
A transitive dependency: JOB_CLASS → CHG_HOUR 
- Make determinant (JOB_CLASS) as a PK of a new table
- Create tables based on new PK
  - Table JOB(**JOB_CLASS**, CHG_HOUR)

# 3NF Step2 Reassign corresponding dependent attributes
- Table EMPLOYEE(**EMP_NUM**, EMP_NAME, JOB_CLASS)
- Table JOB(**JOB_CLASS**, CHG_HOUR)
- Table PROJECT(**PROJ_NUM**, PROJ_NAME)
- Table ASSIGNMENT(**PROJ_NUM**, **EMP_NUM**, ASSIGN_HOUR)

# Dependency Diagram
![bg right:70% w:90%](restricted/CFig06_05.jpg)

# After 3NF
- it is in 2NF
- it does not include transitive dependencies

# Improving the design
- Normalization form only focus on avoiding data redundancy
- Beyond normalization, there are still various issues we need to address
  1. Minimize data entry errors
  2. Evaluate naming conventions
  3. Refine attribute atomicity
  4. Identify new attributes
  5. Identify new relationships
  6. Refine primary keys as required for data granularity
  7. Maintain historical accuracy
  8. Evaluate using derived attributes

# The Completed Database After Design Improvement
<div class="grid">
    <img src="restricted/CFig06_06a.jpg" alt="">
    <img src="restricted/CFig06_06b.jpg" alt="">
</div> 

# Minimize data entry errors
- When a new database designer on board, we need insert a record in EMPLOYEE table. Thus, we enter data into JOB_CLASS. 
- However, sometime we may enter either Database Designer, DB Designer or database designer. It easily makes data entry errors
- Reduce the data enter errors by adding a <span class="brown-text">surrogate key</span> JOB_CODE <br>   JOB_CODE → JOB_CLASS, CHG_HOUR 
- Surrogate key is an artificial key introduced by DB designer
  - simplify PK design
  - usually numeric
  - often generated automatically by DBMS

# Evaluate Naming Conventions
- CHG_HOUR changed to JOB_CHG_HOUR
- JOB_CLASS changed to JOB_DESCRIPTION
- HOURS changed to ASSIGN_HOURS

# Refine Attribute atomicity
- Atomicity: not being able to be divided into small units
- An atomic attribute is an attribute that cannot be further subdivided
- EMP_NAME divided into EMP_LNAME, EMP_FNAME, EMP_INITIAL

# Identify New attributes
- Consider if any other attributes could be added into table
- Social Security Number, Hire Date,....

# Identify New Relationships
- Add EMP_NUM attribute into PROJECT as a foreign key to keep project manager information

# Refine PKs as Required for Data Granularity (1/3)
- How often an employee reports hours work on a project and at what level of granularity (many times per day, once a day, once a week, at the end of project)
- <span class="brown-text">Granularity</span> refers to the level of detail represented by the values stored in a table’s row


- After 3NF
  ASSIGNMENT(**PROJ_NUM**, **EMP_NUM**, ASSIGN_HOUR)
<style scoped>
table {
  font-size: 25px;
}
</style>
PROJ_NUM|EMP_NUM|ASSIGN_HOUR
--------|-------|-----------
15|103|2.6
18|118|1.4

Report hours at the end of project

# Refine PKs as Required for Data Granularity (2/3)

- Add ASSIGN_DATE attribute
  ASSIGNMENT(**PROJ_NUM**, **EMP_NUM**, **ASSIGN_DATE**, ASSIGN_HOUR)

<style scoped>
table {
  font-size: 25px;
}
</style>
PROJ_NUM|EMP_NUM|ASSIGN_DATE|ASSIGN_HOUR
--------|-------|-----------|-----------
15|103|06-Mar-22|2.6
18|118|06-Mar-22|1.4

Report hours once a day

# Refine PKs as Required for Data Granularity (3/3)
- Add ASSIGN_NUM as a surrogate key
  ASSIGNMENT(**ASSIGN_NUM**, PROJ_NUM, EMP_NUM, ASSIGN_DATE, ASSIGN_HOUR)

<style scoped>
table {
  font-size: 25px;
}
</style>
ASSIGN_NUM|PROJ_NUM|EMP_NUM|ASSIGN_DATE|ASSIGN_HOUR
--------|--------|-------|-----------|-----------
1001|15|103|06-Mar-22|2.6
1002|18|118|06-Mar-22|1.4

- Report hours anytime
- Lower granularity yields greater flexibility

# Maintain historical accuracy
- Add jog charge per hour (ASSIGN_CHG_HOUR) into ASSIGNMENT table is important to maintain historical accuracy of the data
- JOB_CHG_HOUR in JOB and ASSIGN_CHG_HOUR in ASSIGNMENT. they may the same in a time period. 
- Due to salary raise, JOB_CHG_HOUR will be changed
- ASSIGN_CHG_HOUR keep historical data and only reflect the charge hour whey employee report hours

# Evaluate Using Derived Attributes