# Students and Courses SQL Queries

## Table Schema

### STUDENTS Table
| Column      | Data Type     | Constraints |
|------------|--------------|-------------|
| ID         | INT          | PRIMARY KEY |
| NAME       | VARCHAR(20)  |             |
| DATEOFBIRTH | DATE        | CHECK(DATEOFBIRTH > '2000-01-01') |
| COURSEID   | INT          | FOREIGN KEY REFERENCES COURSE(CID) |

### COURSE Table
| Column      | Data Type     | Constraints |
|------------|--------------|-------------|
| CID        | INT          | PRIMARY KEY |
| CNAME      | VARCHAR(30)  |             |
| CINSTRUCTOR | VARCHAR(20) |             |

## Table Creation

### Q1: Create STUDENTS Table
```sql
CREATE TABLE STUDENTS (
    ID INT PRIMARY KEY, 
    NAME VARCHAR(20), 
    DATE_OF_BIRTH DATE CHECK(DATE_OF_BIRTH > '2000-01-01'), 
    COURSEID INT, 
    FOREIGN KEY (COURSEID) REFERENCES COURSE(CID)
);
```

### Q2: Create COURSE Table
```sql
CREATE TABLE COURSE (
    CID INT PRIMARY KEY, 
    CNAME VARCHAR(30), 
    CINSTRUCTOR VARCHAR(20)
);
```

## Data Insertion

### Q3: Insert Data into STUDENTS Table
```sql
INSERT INTO STUDENTS (ID, NAME, DATE_OF_BIRTH, COURSEID) VALUES
(1, 'Alice', '2002-05-14', 101),
(2, 'Bob', '2001-07-22', 102),
(3, 'Charlie', '2003-03-18', 101),
(4, 'David', '2000-12-25', NULL),
(5, 'Eve', '2002-10-30', 103);
```

### Q4: Insert Data into COURSE Table
```sql
INSERT INTO COURSE (CID, CNAME, CINSTRUCTOR) VALUES
(101, 'Computer Science', 'Dr. Smith'),
(102, 'Mathematics', 'Prof. Johnson'),
(103, 'Physics', 'Dr. Brown');
```

## Queries

### Q5: Find the course taught by "Prof. Johnson"
```sql
SELECT * FROM COURSE WHERE CINSTRUCTOR = 'Prof. Johnson';
```

### Q6: Display students with their courses, show "No Course" if not enrolled
```sql
SELECT STUDENTS.NAME, IFNULL(COURSE.CNAME, 'No Course') AS COURSENAME 
FROM STUDENTS 
LEFT JOIN COURSE ON STUDENTS.COURSEID = COURSE.CID;
```

### Q7: Find the names of students with their course instructors
```sql
SELECT STUDENTS.ID, STUDENTS.NAME, STUDENTS.DATE_OF_BIRTH, 
       IFNULL(COURSE.CNAME, 'NO COURSE') AS COURSENAME, 
       IFNULL(COURSE.CINSTRUCTOR, 'NO INSTRUCTOR') AS CINSTRUCTOR 
FROM STUDENTS 
LEFT OUTER JOIN COURSE ON STUDENTS.COURSEID = COURSE.CID;
```

### Q8: Count students per course, including courses with no students
```sql
SELECT CNAME, COUNT(STUDENTS.ID) AS StudentCount 
FROM COURSE 
LEFT OUTER JOIN STUDENTS ON COURSE.CID = STUDENTS.COURSEID 
GROUP BY CNAME;
```

### Q9: Students enrolled in "Computer Science"
```sql
SELECT COURSE.CNAME, STUDENTS.NAME 
FROM COURSE 
INNER JOIN STUDENTS ON COURSE.CID = STUDENTS.COURSEID 
WHERE COURSE.CNAME = 'Computer Science';
```

### Q10: Students younger than 21 with their course details
```sql
SELECT STUDENTS.ID, STUDENTS.NAME, STUDENTS.DATE_OF_BIRTH, 
       IFNULL(COURSE.CNAME, 'NO COURSE') AS COURSENAME, 
       IFNULL(COURSE.CINSTRUCTOR, 'NO INSTRUCTOR') AS CINSTRUCTOR
FROM STUDENTS 
LEFT JOIN COURSE ON STUDENTS.COURSEID = COURSE.CID
WHERE TIMESTAMPDIFF(YEAR, STUDENTS.DATE_OF_BIRTH, CURDATE()) < 21;
```

### Q11: Students not enrolled in any course
```sql
SELECT * FROM STUDENTS WHERE COURSEID IS NULL;
```

### Q12: Courses with no students enrolled
```sql
SELECT * FROM COURSE WHERE CID NOT IN (SELECT DISTINCT COURSEID FROM STUDENTS);
```

### Q13: Students enrolled in courses taught by "Dr. Smith"
```sql
SELECT STUDENTS.NAME, COURSE.CNAME 
FROM STUDENTS 
JOIN COURSE ON STUDENTS.COURSEID = COURSE.CID 
WHERE COURSE.CINSTRUCTOR = 'Dr. Smith';
```

### Q14: Youngest student in each course
```sql
SELECT COURSEID, NAME, DATE_OF_BIRTH 
FROM STUDENTS 
WHERE DATE_OF_BIRTH = (SELECT MIN(DATE_OF_BIRTH) FROM STUDENTS S WHERE S.COURSEID = STUDENTS.COURSEID);
```

### Q15: Students enrolled in courses starting with 'P'
```sql
SELECT STUDENTS.NAME, COURSE.CNAME 
FROM STUDENTS 
JOIN COURSE ON STUDENTS.COURSEID = COURSE.CID 
WHERE COURSE.CNAME LIKE 'P%';
```

### Q16: Students older than 21 with their course details
```sql
SELECT STUDENTS.ID, STUDENTS.NAME, STUDENTS.DATE_OF_BIRTH, 
       IFNULL(COURSE.CNAME, 'NO COURSE') AS COURSENAME, 
       IFNULL(COURSE.CINSTRUCTOR, 'NO INSTRUCTOR') AS CINSTRUCTOR
FROM STUDENTS 
LEFT JOIN COURSE ON STUDENTS.COURSEID = COURSE.CID
WHERE TIMESTAMPDIFF(YEAR, STUDENTS.DATE_OF_BIRTH, CURDATE()) > 21;
```
