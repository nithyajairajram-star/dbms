CREATE DATABASE CollegeDB;
USE CollegeDB;

CREATE TABLE Employees (
    EmpID INT PRIMARY KEY,
    FirstName VARCHAR(30),
    LastName VARCHAR(30),
    DepartmentID INT
);

CREATE TABLE Departments (
    DepartmentID INT PRIMARY KEY,
    DeptName VARCHAR(50)
);

CREATE TABLE Projects (
    ProjectID INT PRIMARY KEY,
    ProjectName VARCHAR(50),
    DepartmentID INT,
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);

INSERT INTO Employees (EmpID, FirstName, LastName, DepartmentID)
VALUES
(1, 'Alice', 'Johnson', 101),
(2, 'Bob', 'Smith', 102),
(3, 'Charlie', 'Brown', 103),
(4, 'Daisy', 'Wills', NULL);

INSERT INTO Departments (DepartmentID, DeptName)
VALUES
(101, 'HR'),
(102, 'IT'),
(103, 'Finance'),
(104, 'Marketing');

INSERT INTO Projects (ProjectID, ProjectName, DepartmentID)
VALUES
(1, 'Recruitment Drive', 101),
(2, 'Website Revamp', 102),
(3, 'Audit FY25', 103),
(4, 'Campaign Launch', 104);

ALTER TABLE Projects RENAME COLUMN DepartmentID TO DeptID;

ALTER TABLE Departments RENAME COLUMN DepartmentID TO DeptID;

SELECT CONCAT(FirstName, ' ', LastName) AS FullName,
       DeptName,
       ProjectName
FROM Employees
NATURAL JOIN Departments
NATURAL JOIN Projects
ORDER BY FullName;

SELECT CONCAT(e.FirstName, ' ', e.LastName) AS EmployeeName,
       IFNULL(d.DeptName, 'No Department') AS Department,
       IFNULL(p.ProjectName, 'No Project Assigned') AS Project
FROM Employees e
JOIN Departments d
ON e.DepartmentID = d.DeptID
LEFT JOIN Projects p
ON d.DeptID = p.DeptID
ORDER BY e.EmpID;

SELECT CONCAT(e.FirstName, ' ', e.LastName) AS EmployeeName,
       IFNULL(d.DeptName, 'Unassigned') AS Department,
       IFNULL(p.ProjectName, 'Not Allocated') AS Project
FROM Employees e
LEFT JOIN Departments d
ON e.DepartmentID = d.DeptID
LEFT JOIN Projects p
ON d.DeptID = p.DeptID
ORDER BY Department;

SELECT d.DeptName,
       CONCAT(IFNULL(e.FirstName, 'No'), ' ',
              IFNULL(e.LastName, 'Employee')) AS EmployeeName
FROM Departments d
RIGHT JOIN Employees e
ON e.DepartmentID = d.DeptID
WHERE e.DepartmentID IS NULL
   OR d.DeptName IS NOT NULL;

SELECT e.EmpID,
       CONCAT(e.FirstName, ' ', e.LastName) AS EmployeeName,
       d.DeptName
FROM Employees e
LEFT JOIN Departments d
ON e.DepartmentID = d.DeptID

UNION

SELECT e.EmpID,
       CONCAT(IFNULL(e.FirstName, 'Unknown'), ' ',
              IFNULL(e.LastName, '')) AS EmployeeName,
       d.DeptName
FROM Employees e
RIGHT JOIN Departments d
ON e.DepartmentID = d.DeptID
ORDER BY EmployeeName;
