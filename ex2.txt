CREATE DATABASE IF NOT EXISTS university_db;

USE university_db;

CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL,
    credits INT NOT NULL,
    CHECK (credits > 0 AND credits <= 6)
);

CREATE TABLE enrollments (
    enrollment_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    enrollment_date DATE NOT NULL DEFAULT (CURRENT_DATE),

    FOREIGN KEY (student_id) REFERENCES students(student_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE,

    FOREIGN KEY (course_id) REFERENCES courses(course_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE,

    UNIQUE (student_id, course_id)
);

INSERT INTO students (name, email)
VALUES
('Alice Johnson', 'alice@example.com'),
('Bob Smith', 'bob@example.com');

SELECT * FROM students;

INSERT INTO courses (course_name, credits)
VALUES
('Database Systems', 3),
('Computer Networks', 4);

SELECT * FROM courses;

INSERT INTO enrollments (student_id, course_id)
VALUES
(1, 1),
(2, 1),
(1, 2);

SELECT * FROM enrollments;

DELETE FROM students
WHERE student_id = 2;

DELETE FROM courses
WHERE course_id = 2;

SELECT * FROM students;

SELECT * FROM courses;

DROP TABLE IF EXISTS enrollments;

DROP TABLE IF EXISTS students;

DROP TABLE IF EXISTS courses;

DROP DATABASE university_db;
