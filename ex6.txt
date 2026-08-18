CREATE DATABASE CompanyDB;
USE CompanyDB;

CREATE TABLE Employees (
    EmpID INT PRIMARY KEY,
    FirstName VARCHAR(30),
    LastName VARCHAR(30),
    Salary DECIMAL(10,2),
    Department VARCHAR(30)
);

CREATE TABLE Attendance (
    AttendanceID INT PRIMARY KEY AUTO_INCREMENT,
    EmpID INT,
    Date DATE,
    Status ENUM('Present', 'Absent'),
    FOREIGN KEY (EmpID) REFERENCES Employees(EmpID)
);

INSERT INTO Employees VALUES
(1, 'Alice', 'Thomas', 55000.00, 'HR'),
(2, 'Bob', 'Williams', 62000.00, 'IT'),
(3, 'Charlie', 'Smith', 47000.00, 'Finance'),
(4, 'Daisy', 'Johnson', 50000.00, 'IT');

INSERT INTO Attendance (EmpID, Date, Status) VALUES
(1, '2025-08-01', 'Present'),
(2, '2025-08-01', 'Absent'),
(3, '2025-08-01', 'Present'),
(4, '2025-08-01', 'Present'),
(2, '2025-08-02', 'Present');

CREATE VIEW IT_Employees AS
SELECT EmpID,
       CONCAT(FirstName, ' ', LastName) AS FullName,
       Salary
FROM Employees
WHERE Department = 'IT';

SELECT * FROM IT_Employees;

UPDATE IT_Employees
SET Salary = Salary + 5000
WHERE FullName = 'Bob Williams';

SELECT * FROM IT_Employees;

DELIMITER //

CREATE PROCEDURE GetSalaryRange(
    IN minSal DECIMAL(10,2),
    IN maxSal DECIMAL(10,2)
)
BEGIN
    SELECT EmpID,
           CONCAT(FirstName, ' ', LastName) AS FullName,
           Salary
    FROM Employees
    WHERE Salary BETWEEN minSal AND maxSal;
END //

DELIMITER ;

CREATE VIEW NameDetails AS
SELECT EmpID,
       CONCAT(FirstName, ' ', LastName) AS FullName,
       LENGTH(CONCAT(FirstName, ' ', LastName)) AS NameLength
FROM Employees;

CALL GetSalaryRange(48000.00, 60000.00);

DELIMITER //

CREATE PROCEDURE CheckAboveAverageSalary(IN emp_id INT)
BEGIN
    DECLARE empSal DECIMAL(10,2);
    DECLARE avgSal DECIMAL(10,2);

    SELECT Salary INTO empSal
    FROM Employees
    WHERE EmpID = emp_id;

    SELECT AVG(Salary) INTO avgSal
    FROM Employees;

    IF empSal > avgSal THEN
        SELECT CONCAT(
            'Employee ', emp_id,
            ' has salary above average.'
        ) AS Message;
    ELSE
        SELECT CONCAT(
            'Employee ', emp_id,
            ' has salary below average.'
        ) AS Message;
    END IF;
END //

DELIMITER ;

CALL CheckAboveAverageSalary(2);

DELIMITER //

CREATE FUNCTION AttendanceDays(emp_id INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE countDays INT;

    SELECT COUNT(*)
    INTO countDays
    FROM Attendance
    WHERE EmpID = emp_id
      AND Status = 'Present';

    RETURN countDays;
END //

DELIMITER ;

SELECT EmpID, AttendanceDays(EmpID) AS PresentDays FROM Employees;
