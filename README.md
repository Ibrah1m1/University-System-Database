# 🎓 University System Database

> **Database Design & SQL Implementation - University Coursework**

<div align="center">

![SQL](https://img.shields.io/badge/SQL-003B57?style=for-the-badge&logo=postgresql&logoColor=white)
![Database](https://img.shields.io/badge/Database-Design-336791?style=for-the-badge&logo=google&logoColor=white)
![ERD](https://img.shields.io/badge/ERD-Normalization-FF6B6B?style=for-the-badge)
![3NF](https://img.shields.io/badge/Normalization-3NF-4CAF50?style=for-the-badge)

</div>

---

## 📌 Overview

A comprehensive **relational database management system** designed to streamline university course registration, student enrollment, and academic reporting. This project replaces manual registration processes with an automated, accurate, and efficient DBMS.

**Team Project** | University of Jeddah | Software Engineering

---

## 🎯 Problem Statement

Manual course registration systems commonly suffer from:
- ❌ Schedule conflicts between courses
- ❌ Inaccurate class lists and enrollment records
- ❌ Time-consuming report generation
- ❌ Data inconsistency and redundancy

---

## 💡 Solution

A normalized relational database that:
- ✅ Centralizes student, instructor, and course data
- ✅ Automates enrollment validation and conflict checking
- ✅ Ensures data accuracy through constraints and normalization
- ✅ Simplifies academic reporting with structured queries

---

## 🛠️ Technologies & Tools

| Technology | Purpose |
|------------|---------|
| **SQL** | Database queries and management |
| **MySQL/PostgreSQL** | Relational database management |
| **Draw.io/Lucidchart** | Entity-Relationship Diagram (ERD) design |
| **Git** | Version control and collaboration |

---



## 📊 Database Schema

### Entities & Relationships

| Table | Primary Key | Foreign Keys | Attributes |
|-------|------------|--------------|------------|
| **STUDENT** | StudentID | — | FName, LName, Email |
| **INSTRUCTOR** | InstructorID | — | FName, LName, Dept |
| **COURSE** | CourseCode | — | Title, Credits |
| **CLASS** | ClassID | CourseCode, InstructorID | Semester, Year, Time |
| **ENROLLMENT** | (StudentID, ClassID) | StudentID, ClassID | Grade |

### Relationships

- **STUDENT** ←→ **ENROLLMENT**: One student enrolls in many classes
- **CLASS** ←→ **ENROLLMENT**: One class contains many students  
- **INSTRUCTOR** ←→ **CLASS**: One instructor teaches many classes
- **COURSE** ←→ **CLASS**: One course is offered as many sections



-------------------------------------------------------------------------

### Normalization



✅ All tables are in **3NF (Third Normal Form)**

### Table Structure

| Table | Primary Key | Foreign Keys | Description |
|-------|------------|--------------|-------------|
| **STUDENT** | StudentID | — | Student demographic data |
| **INSTRUCTOR** | InstructorID | — | Instructor qualifications |
| **COURSE** | CourseCode | — | Course details (title, credits) |
| **CLASS** | ClassID | CourseCode, InstructorID | Specific class sections |
| **ENROLLMENT** | (StudentID, ClassID) | StudentID, ClassID | Student-course enrollment records |

---

## 🔧 Normalization

All tables are normalized to **Third Normal Form (3NF)**:

| Normal Form | Requirement | Status |
|-------------|-------------|--------|
| **1NF** | All columns contain atomic values | ✅ Achieved |
| **2NF** | No partial dependencies on composite keys | ✅ Achieved |
| **3NF** | No transitive dependencies (non-key → non-key) | ✅ Achieved |

### Functional Dependencies
STUDENT: StudentID → FName, LName, Email INSTRUCTOR: InstructorID → FName, LName, Dept COURSE: CourseCode → Title, Credits CLASS: ClassID → CourseCode, InstructorID, Semester, Year, Time ENROLLMENT: (StudentID, ClassID) → Grade


---

## 💻 SQL Implementation

### Table Creation Example
```sql
CREATE TABLE STUDENT (
    StudentID INT PRIMARY KEY,
    FName VARCHAR(50) NOT NULL,
    LName VARCHAR(50) NOT NULL,
    Email VARCHAR(100) UNIQUE
);

CREATE TABLE ENROLLMENT (
    StudentID INT,
    ClassID INT,
    Grade DECIMAL(3,2),
    PRIMARY KEY (StudentID, ClassID),
    FOREIGN KEY (StudentID) REFERENCES STUDENT(StudentID),
    FOREIGN KEY (ClassID) REFERENCES CLASS(ClassID)
);
```
### Sample Queries

Query 1: Students in a Specific Class
```
Sample Queries

Query 1: Students in a Specific Class

SELECT s.FName, s.LName, s.Email
FROM STUDENT s
JOIN ENROLLMENT e ON s.StudentID = e.StudentID
JOIN CLASS c ON e.ClassID = c.ClassID
WHERE c.Semester = 'Fall' AND c.Year = 2025
ORDER BY s.LName;

```
Query 2: Average Credits per Department
```


SELECT i.Dept, AVG(co.Credits) AS AvgCredits
FROM INSTRUCTOR i
JOIN CLASS c ON i.InstructorID = c.InstructorID
JOIN COURSE co ON c.CourseCode = co.CourseCode
GROUP BY i.Dept;
```
Query 3: Instructor with Most Classes
```


SELECT i.FName, i.LName, COUNT(c.ClassID) AS ClassCount
FROM INSTRUCTOR i
JOIN CLASS c ON i.InstructorID = c.InstructorID
GROUP BY i.InstructorID
HAVING COUNT(c.ClassID) = (
    SELECT MAX(ClassCount)
    FROM (
        SELECT InstructorID, COUNT(*) AS ClassCount
        FROM CLASS
        GROUP BY InstructorID
    ) AS Counts
);
```
### Stored Procedures

Procedure 1: Get Classes by Instructor
```

DELIMITER //
CREATE PROCEDURE GetInstructorClasses(IN instrID INT)
BEGIN
    SELECT c.ClassID, co.Title, c.Semester, c.Year, c.Time
    FROM CLASS c
    JOIN COURSE co ON c.CourseCode = co.CourseCode
    WHERE c.InstructorID = instrID;
END //
DELIMITER ;
```
Procedure 2: Update Student Grade
```



DELIMITER //
CREATE PROCEDURE UpdateGrade(
    IN studID INT, 
    IN clsID INT, 
    IN newGrade DECIMAL(3,2)
)
BEGIN
    UPDATE ENROLLMENT
    SET Grade = newGrade
    WHERE StudentID = studID AND ClassID = clsID;
END //
DELIMITER ;
```

## 📁 Project Structure
```

University-System-Database/
├── schemas/
│   ├── erd_diagram.png
│   ├── schema.sql
│   └── normalization_proof.pdf
├── queries/
│   ├── basic_queries.sql
│   ├── advanced_queries.sql
│   └── stored_procedures.sql
├── docs/
│   ├── project_report.pdf
│   └── user_manual.pdf
├── data/
│   ├── sample_data.sql
```


### 🎓 Learning Outcomes

✅ Applied Linked Lists for efficient queue management (O(1) operations).

✅ Used HashMaps for fast stock lookup and updates (O(1) average case).

✅ Implemented stack-like structure for order history logging.

✅ Practiced Object-Oriented Programming principles (encapsulation, abstraction).

✅ Developed a complete console application with menu-driven interface.

✅ Collaborated on team-based software development workflow.

## 🚀 How to Run
```

# Clone the repository
git clone https://github.com/Ibrah1m1/Restaurant-Order-System.git

# Navigate to project directory
cd Restaurant-Order-System/src

# Compile all Java files
javac *.java classes/*.java utils/*.java

# Run the application
java Main
```


## 👥 Team

| Name | Role |
|------|------|
| **Ibrahim Eissa** |Leader,Database Design, SQL Implementation|
| **Yassen Sayari** | ERD Design, Documentation |
| **Mohammed Mamdouh** | Query Development, Testing |

## 📧 Contact

**Ibrahim Eissa Abu Alghayth**

-📧 ibrahim.abualg@gmail.com

-🔗 LinkedIn: https://linkedin.com/in/yourname

-🐙 GitHub: https://github.com/Ibrah1m1

-📄 Project Documentation

[Project_DS.pdf](https://github.com/user-attachments/files/25886669/Project_DS.pdf)


🔗 Related Links

## 🔗 Related Links

- [Java Documentation](https://docs.oracle.com/javase/)
- [Data Structures Tutorial](https://www.geeksforgeeks.org/data-structures/)
- [OOP Principles](https://www.geeksforgeeks.org/object-oriented-programming-oops-concept-in-java/)


Made with ❤️ by the Team

