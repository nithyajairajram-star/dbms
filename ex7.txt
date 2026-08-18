CREATE DATABASE CollegeDB;
USE CollegeDB;

CREATE TABLE Departments (
    DeptID INT PRIMARY KEY,
    DeptName VARCHAR(100),
    StudentCount INT,
    TeacherCount INT,
    Classrooms INT,
    Email VARCHAR(100)
);

START TRANSACTION;

INSERT INTO Departments VALUES
(1, 'Computer Science', 200, 15, 5, 'cs@college.edu');

SAVEPOINT sp1;

INSERT INTO Departments VALUES
(2, 'Information Technology', 180, 12, 4, 'it@college.edu');

SAVEPOINT sp2;

INSERT INTO Departments VALUES
(3, 'Cyber Security', 150, 10, 6, 'cyber@college.edu');

SAVEPOINT sp3;

INSERT INTO Departments VALUES
(4, 'Electronics', 150, 10, 6, 'ece@college.edu');

SELECT * FROM Departments;

UPDATE Departments
SET StudentCount = 220
WHERE DeptID = 1;

UPDATE Departments
SET StudentCount = 190
WHERE DeptID = 2;

SAVEPOINT sp4;

ROLLBACK TO sp2;

SELECT * FROM Departments;

INSERT INTO Departments VALUES
(5, 'Mechanical', 160, 11, 7, 'mech@college.edu');

DELETE FROM Departments
WHERE DeptID = 2;

COMMIT;

CREATE USER 'dept_user'@'localhost'
IDENTIFIED BY 'pass123';

GRANT SELECT, INSERT
ON CollegeDB.Departments
TO 'dept_user'@'localhost';

USE CollegeDB;

SELECT * FROM Departments;

INSERT INTO Departments VALUES
(6, 'Artificial Intelligence', 140, 9, 4, 'ai@college.edu');

DELETE FROM Departments
WHERE DeptID = 1;

GRANT DELETE, UPDATE
ON CollegeDB.Departments
TO 'dept_user'@'localhost';

REVOKE DELETE, INSERT, UPDATE
ON CollegeDB.Departments
FROM 'dept_user'@'localhost';

DROP USER 'dept_user'@'localhost';
