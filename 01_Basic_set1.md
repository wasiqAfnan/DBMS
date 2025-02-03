# Employee Table SQL Queries

# Table Schema
### EMPLOYEE(ID, NAME, DEPT, SALARY)
## Table Creation & Constraints

### Q1: Create the Employee Table
```sql
CREATE TABLE EMPLOYEE(ID INT, NAME VARCHAR(30), DEPT VARCHAR(20), SALARY INT);
```

### Q2: Set ID as Primary Key
```sql
ALTER TABLE EMPLOYEE ADD PRIMARY KEY(ID);
```

### Q3: Ensure Salary is >= 20000
```sql
ALTER TABLE EMPLOYEE ADD CONSTRAINT CONS1 CHECK(SALARY >= 20000);
```

### Q4: Restrict DEPT to 'HR' and 'IT'
```sql
ALTER TABLE EMPLOYEE ADD CONSTRAINT CONS2 CHECK(DEPT IN ('HR','IT'));
```

## Data Manipulation

### Q5: Insert 5 Records
```sql
INSERT INTO EMPLOYEE(ID, NAME, DEPT, SALARY) VALUES
(101, 'SHUBJASH', 'IT', 20000),
(102, 'MODASSIR', 'IT', 25000),
(103, 'SONU', 'IT', 22000),
(104, 'RINKU', 'HR', 28000),
(105, 'PINTU', 'HR', 27500);
```

### Q6: Display All Employee Data
```sql
SELECT * FROM EMPLOYEE;
```

### Q7: Show Table Structure
```sql
DESC EMPLOYEE;
```

## Aggregations & Queries

### Q8: Count Employees
```sql
SELECT COUNT(*) FROM EMPLOYEE;
```

### Q9: Average Salary
```sql
SELECT AVG(SALARY) FROM EMPLOYEE;
```

### Q10: Name Filters
```sql
SELECT * FROM EMPLOYEE WHERE NAME LIKE 'S%';
SELECT * FROM EMPLOYEE WHERE NAME LIKE '%H';
```

### Q11: Employee with Max Salary (Nested Query)
```sql
SELECT * FROM EMPLOYEE WHERE SALARY=(SELECT MAX(SALARY) FROM EMPLOYEE);
```

### Q12: Sort by Salary (Asc & Desc)
```sql
SELECT * FROM EMPLOYEE ORDER BY SALARY ASC;
SELECT * FROM EMPLOYEE ORDER BY SALARY DESC;
```

### Q13: Employee with Min Salary (Nested Query)
```sql
SELECT * FROM EMPLOYEE WHERE SALARY=(SELECT MIN(SALARY) FROM EMPLOYEE);
```

### Q14: Employees with Salary > Average (Nested Query)
```sql
SELECT * FROM EMPLOYEE WHERE SALARY>=(SELECT AVG(SALARY) FROM EMPLOYEE);
```

### Q15: Count Employees Per Department
```sql
SELECT DEPT, COUNT(*) AS EMPLOYEE FROM EMPLOYEE GROUP BY DEPT;
```

### Q16: Update MODASSIR's Data
```sql
UPDATE EMPLOYEE SET DEPT = 'HR', SALARY = 30000 WHERE NAME ='MODASSIR';
```

### Q17: Run Q15 Again
```sql
SELECT DEPT, COUNT(*) AS EMPLOYEE FROM EMPLOYEE GROUP BY DEPT;
```

### Q18: Total Salary per Department
```sql
SELECT DEPT, SUM(SALARY) AS TOTAL_SALARY FROM EMPLOYEE GROUP BY DEPT;
```

### Q19: Max Salary in HR Dept
```sql
SELECT DEPT, MAX(SALARY) AS MAX_SALARY FROM EMPLOYEE WHERE DEPT = 'HR';
```

## Adding New Department

### Q20: Add 'MANAGER' to DEPT Constraint & Insert Employees
```sql
ALTER TABLE EMPLOYEE ADD CONSTRAINT CONS2 CHECK(DEPT IN('HR', 'IT', 'MANAGER'));

INSERT INTO EMPLOYEE(ID, NAME, DEPT, SALARY) VALUES
(106, 'ROHAN', 'MANAGER', 25700),
(107, 'MOHAN', 'MANAGER', 25700);
```

## Additional Queries

### Q21: Employees in 2-Character Long DEPT
```sql
SELECT * FROM EMPLOYEE WHERE LENGTH(DEPT) = 2;
```

### Q22: Employee with Second Highest Salary
```sql
SELECT * FROM (
    SELECT ID, NAME, DEPT, SALARY, RANK() OVER (ORDER BY SALARY DESC) AS RANKING
    FROM EMPLOYEE
) AS TEMP_TABLE WHERE RANKING = 2;
```

### Q23: Delete ROHAN
```sql
DELETE FROM EMPLOYEE WHERE NAME = 'ROHAN';
```

### Q24: Employee with Third Highest Salary
```sql
SELECT * FROM (
    SELECT ID, NAME, DEPT, SALARY, RANK() OVER (ORDER BY SALARY DESC) AS RANKING
    FROM EMPLOYEE
) AS TEMP_TABLE WHERE RANKING = 3;
```

### Q25: Employees with Salary Between 20,000 and 25,000
```sql
SELECT * FROM EMPLOYEE WHERE SALARY BETWEEN 20000 AND 25000;
```

### Q26: Employees Not in 'HR' or 'IT' Departments
```sql
SELECT * FROM EMPLOYEE WHERE DEPT NOT IN ('HR', 'IT');
```

### Q27: Max Salary in Each Department
```sql
SELECT DEPT, MAX(SALARY) AS MAX_SALARY FROM EMPLOYEE GROUP BY DEPT HAVING DEPT = 'HR';
```

### Q28: Department with Maximum Employees
```sql
SELECT DEPT FROM (
    SELECT DEPT, COUNT(*) AS EMPLOYEE_COUNT
    FROM EMPLOYEE
    GROUP BY DEPT
    ORDER BY EMPLOYEE_COUNT DESC
    LIMIT 1
) AS TEMP_TABLE;
