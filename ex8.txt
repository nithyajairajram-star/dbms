CREATE DATABASE StoreDB;
USE StoreDB;

CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(50),
    Price DECIMAL(10,2),
    Quantity INT
);

CREATE TABLE ProductLogs (
    LogID INT AUTO_INCREMENT PRIMARY KEY,
    EventType VARCHAR(20),
    EventDetails TEXT,
    EventTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

DELIMITER $$

CREATE TRIGGER trg_Product_Insert
BEFORE INSERT ON Products
FOR EACH ROW
BEGIN
    IF NEW.Price > 1000 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Price cannot exceed 1000';
    END IF;
END $$

DELIMITER ;

INSERT INTO Products VALUES
(1, 'Product A', 500, 10);

INSERT INTO Products VALUES
(2, 'Product B', 200, 15);

INSERT INTO Products VALUES
(3, 'Product C', 300, 20);

INSERT INTO Products VALUES
(4, 'Product X', 1500, 5);

DELIMITER $$

CREATE TRIGGER trg_Product_Update
BEFORE UPDATE ON Products
FOR EACH ROW
BEGIN
    DECLARE msg VARCHAR(255);

    SET msg = CONCAT(
        'Product Price Changed: Old = ',
        OLD.Price,
        ', New = ',
        NEW.Price
    );

    INSERT INTO ProductLogs(EventType, EventDetails, EventTime)
    VALUES ('UPDATE', msg, NOW());
END $$

DELIMITER ;

UPDATE Products
SET Price = 600
WHERE ProductID = 3;

SELECT * FROM ProductLogs;

DELIMITER $$

CREATE TRIGGER trg_Product_Delete
BEFORE DELETE ON Products
FOR EACH ROW
BEGIN
    DECLARE msg VARCHAR(255);

    SET msg = CONCAT(
        'Product with ID ',
        OLD.ProductID,
        ' deleted.'
    );

    INSERT INTO ProductLogs(EventType, EventDetails, EventTime)
    VALUES ('DELETE', msg, NOW());
END $$

DELIMITER ;

DELETE FROM Products
WHERE ProductID = 2;

SELECT * FROM ProductLogs;

SELECT * FROM Products;

SELECT * FROM ProductLogs;
