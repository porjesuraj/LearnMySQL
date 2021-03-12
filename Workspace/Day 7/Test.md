### Stored Procedure
```sql
CREATE TABLE result_tbl
(
    id INT
);
```
#### Variable declaration
```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc1( )
    BEGIN
        DECLARE number INT;
        SET number = 10;
        INSERT INTO result_tbl VALUES( number );
    END $$
DELIMITER ;

CALL sp_proc1( );
SELECT * FROM result_tbl;
```
```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc2( )
    BEGIN
        DECLARE number INT DEFAULT 20;
        INSERT INTO result_tbl VALUES( number );
    END $$
DELIMITER ;

CALL sp_proc2( );
SELECT * FROM result_tbl;
```

```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc3( )
    BEGIN
        DECLARE number INT;
        SELECT 30 INTO number;
        INSERT INTO result_tbl VALUES( number );
    END $$
DELIMITER ;

CALL sp_proc3( );
SELECT * FROM result_tbl;
```

```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc3( )
    BEGIN
        DECLARE number INT;
        SELECT 30 INTO number;
        INSERT INTO result_tbl VALUES( number );
    END $$
DELIMITER ;

CALL sp_proc3( );
SELECT * FROM result_tbl;
```

```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc4( )
    BEGIN
        DECLARE year INTEGER DEFAULT 2020;
        DECLARE days INTEGER DEFAULT 28;
        IF year mod 4 = 0 THEN
            SET days = 29;
        END IF;
        INSERT INTO result_tbl VALUES ( days );
    END $$
DELIMITER ;

CALL sp_proc4( );
SELECT * FROM result_tbl;
```
```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc5( )
    BEGIN
        DECLARE year INTEGER DEFAULT 2021;
        DECLARE days INTEGER DEFAULT 0;
        IF year mod 4 = 0 THEN
            SET days = 29;
        ELSE
            SET days = 28;
        END IF;
        INSERT INTO result_tbl VALUES ( days );
    END $$
DELIMITER ;

CALL sp_proc5( );
SELECT * FROM result_tbl;
```

```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc6( )
    BEGIN
        DECLARE month INTEGER DEFAULT 7;
        DECLARE days INTEGER DEFAULT 0;
        IF month = 1 THEN
            SET days = 31;
        ELSEIF month = 2 THEN
            SET days = 28;
        ELSEIF month = 3 THEN
            SET days = 31;
        ELSEIF month = 4 THEN
            SET days = 30;
        ELSEIF month = 5 THEN
            SET days = 31;
        ELSEIF month = 6 THEN
            SET days = 30;
        ELSEIF month = 7 THEN
            SET days = 31;
        ELSEIF month = 8 THEN
            SET days = 31;
        ELSEIF month = 9 THEN
            SET days = 30;
        ELSEIF month = 10 THEN
            SET days = 31;
        ELSEIF month = 11 THEN
            SET days = 30;
        ELSEIF month = 12 THEN
            SET days = 31;
        ELSE
            SET days = 0;
        END IF;
        INSERT INTO result_tbl VALUES ( days );
    END $$
DELIMITER ;

CALL sp_proc6( );
SELECT * FROM result_tbl;
```

```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc7( )
    BEGIN
        DECLARE month INTEGER DEFAULT 7;
        DECLARE days INTEGER DEFAULT 0;
        CASE month
            WHEN 1 THEN
                SET days = 31;
            WHEN 2 THEN
                SET days = 28;
            WHEN 3 THEN
                SET days = 31;
            WHEN 4 THEN
                SET days = 30;
            WHEN 5 THEN
                SET days = 31;
            WHEN 6 THEN
                SET days = 30;
            WHEN 7 THEN
                SET days = 31;
            WHEN 8 THEN
                SET days = 31;
            WHEN 9 THEN
                SET days = 30;
            WHEN 10 THEN
                SET days = 31;
            WHEN 11 THEN
                SET days = 30;
            WHEN 12 THEN
                SET days = 31;
            ELSE
                SET days = 0;
        END CASE;
        INSERT INTO result_tbl VALUES ( days );
    END $$
DELIMITER ;

CALL sp_proc7( );
SELECT * FROM result_tb7;
```

```SQL
DELIMITER $$
CREATE PROCEDURE sp_proc8( )
    BEGIN
        DECLARE count INT DEFAULT 0;
        DECLARE result INT DEFAULT 0;
        WHILE count <=10 DO
            SET result = result + count;
            SET count = count + 1;
        END WHILE;
        INSERT INTO result_tbl VALUES( result );
    END $$
DELIMITER ;

CALL sp_proc8( );
SELECT * FROM result;
```

```sql
DELIMITER $$
CREATE PROCEDURE sp_proc9( )
    BEGIN
        DECLARE count INT DEFAULT 0;
        DECLARE result INT DEFAULT 0;
        REPEAT
            SET result = result + count;
            SET count = count + 1;
        UNTIL count > 10
        END REPEAT;
        INSERT INTO result_tbl VALUES( result );
    END $$
DELIMITER ;
CALL sp_proc9( );
SELECT * FROM result;
```

```sql
DELIMITER $$
CREATE PROCEDURE sp_proc9( )
    BEGIN
        DECLARE count INT DEFAULT 0;
        DECLARE result INT DEFAULT 0;
        REPEAT
            SET result = result + count;
            SET count = count + 1;
        UNTIL count > 10
        END REPEAT;
        INSERT INTO result_tbl VALUES( result );
    END $$
DELIMITER ;
CALL sp_proc9( );
SELECT * FROM result;
```

```sql
DELIMITER $$
CREATE PROCEDURE sp_proc10( )
    BEGIN
        DECLARE count INT DEFAULT 0;
        DECLARE result INT DEFAULT 0;
        label: LOOP
            SET result = result + count;
            SET count = count + 1;
            IF COUNT = 11 THEN
                LEAVE label;
            END IF;
        END LOOP;
        INSERT INTO result_tbl VALUES( result );
    END $$
DELIMITER ;

CALL sp_proc10( );
SELECT * FROM result_tbl;
```
```sql
CREATE TABLE account_tbl
(
    number INT,
    name VARCHAR( 50 ),
    balance FLOAT
);
INSERT INTO account_tbl VALUES ( 101, 'prathamesh', 75000 );
INSERT INTO account_tbl VALUES ( 102, 'soham', 30000 );
```
```sql
DELIMITER $$
CREATE PROCEDURE sp_proc11( IN srcNum INT, IN destNum INT, IN amount FLOAT, OUT srcBal FLOAT, OUT destBal FLOAT )
    BEGIN 
        UPDATE account_tbl SET balance = balance - amount WHERE number = srcNum;
        UPDATE account_tbl SET balance = balance + amount WHERE number = destNum;
        SELECT balance INTO srcBal FROM account_tbl WHERE number = srcNum;
        SELECT balance INTO destBal FROM account_tbl WHERE number = destNum;
        END$$
DELIMITER ;
```

```sql
DELIMITER $$
CREATE PROCEDURE sp_proc12( IN number INT, IN name VARCHAR(50), amount FLOAT )
    BEGIN 
        DECLARE EXIT HANDLER FOR 1062
        BEGIN
            INSERT INTO results VALUES( number, 'Already present');
        END;
        INSERT INTO account_tbl VALUES( number, name, amount );    
    END$$
DELIMITER ;
```

```sql
DELIMITER $$
CREATE PROCEDURE sp_proc13( IN number INT, IN name VARCHAR(50), amount FLOAT )
    BEGIN 
        DECLARE EXIT HANDLER FOR 1062 INSERT INTO results VALUES( number, 'Already present');
        
        INSERT INTO account_tbl VALUES( number, name, amount );    
    END$$
DELIMITER ;
```
```sql
DELIMITER $$
CREATE PROCEDURE sp_proc14( IN number INT, IN name VARCHAR(50), amount FLOAT )
    BEGIN 
        DECLARE EXIT HANDLER FOR 1062 SELECT('duplicare account entries are not allowed');
        INSERT INTO account_tbl VALUES( number, name, amount );    
    END$$
DELIMITER ;
```
```sql
DELIMITER $$
CREATE PROCEDURE sp_proc15( IN number INT, IN name VARCHAR(50), amount FLOAT )
    BEGIN 
        DECLARE EXIT HANDLER FOR SQLSTATE '23000' SELECT('duplicare account entries are not allowed');
        INSERT INTO account_tbl VALUES( number, name, amount );    
    END$$
DELIMITER ;
```
```sql
DELIMITER $$
CREATE PROCEDURE sp_proc1( INOUT bookList VARCHAR(4000) )
    BEGIN
        DECLARE FINISHED INTEGER DEFAULT 0; 
        DECLARE bookName VARCHAR( 250 ) DEFAULT "";
        DECLARE cur CURSOR FOR SELECT name FROM books;
        DECLARE CONTINUE HANDLER FOR NOT FOUND SET FINISHED = 1;
        OPEN cur;
        label:LOOP
            FETCH cur INTO bookName;
            IF FINISHED = 1 THEN
                LEAVE label;
            END IF;
            SET bookList = CONCAT( bookName, ',', bookList);
        END LOOP;
        CLOSE cur;
    END$$
DELIMITER ;    
```
```sql
DELIMITER $$
CREATE FUNCTION TO_UPPER_CASE( str VARCHAR( 50 ) )
RETURNS VARCHAR(50) DETERMINISTIC
    BEGIN
        SET str = UPPER( str );
        RETURN str;
    END $$
DELIMITER ;
```

```sql
DELIMITER $$
CREATE TRIGGER test AFTER UPDATE ON books FOR EACH ROW
BEGIN
    INSERT INTO results (first, second ) VALUES( OLD.PRICE, "Price before update");
    INSERT INTO results (first, second ) VALUES( NEW.PRICE, "Price after update");
END $$
DELIMITER ;
```

```sql
DELIMITER $$
CREATE PROCEDURE sp_proc1( )
BEGIN
    DECLARE EXIT HANDLER FOR 1062
    BEGIN

    END;
END $$
DELIMITER ;
```