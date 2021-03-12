## mongo
-------------------------------------
sudo systemctl start mongod
mongo
db;
use S_db;
show dbs;
db.stats()
----------------------------





##Stored Procedure

create a table student rn,nm,mrks
Define stored procedure to insert record into student table
Define stored procedure to update record into student table
Define stored procedure to DELETE record from student table


--- sql
CREATE TABLE student
(
roll_no INT,
sname VARCHAR(50),
smarks FLOAT
);
-----

----sql
INSERT INTO student
VALUES
(1,"SURAJ",80),
   (2,"YESH",81),
   (3,"MOHIT",82),
   (4,"SHAUNAK",83);
--------------------

DELIMITER $$
DROP PROCEDURE IF EXISTS sp_insert;
CREATE PROCEDURE sp_insert(IN roll_no INT,
IN sname VARCHAR(50),
IN smarks FLOAT)
BEGIN
INSERT INTO student
VALUES(roll_no,sname,smarks);
END $$
DELIMITER ;


CALL sp_insert(5,"pavan",85);
SELECT * FROM student;
----------------------------------

-------------------------
DELIMITER $$
DROP PROCEDURE IF EXISTS sp_update;
CREATE PROCEDURE sp_update(IN ROLL INT,
IN MARKS FLOAT)
BEGIN
UPDATE student
SET smarks = MARKS
WHERE roll_no = ROLL;
END $$
DELIMITER ;

CALL sp_update(5,90);



--------------------------------

------------------


DELIMITER $$
DROP PROCEDURE IF EXISTS sp_delete;
CREATE PROCEDURE sp_delete(IN ROLL INT)
BEGIN
DELETE FROM student
WHERE roll_no = ROLL;
END $$
DELIMITER ;

CALL sp_delete(5);
select * from student;
----------------------------------------
--- handler


DELIMITER $$
DROP PROCEDURE IF EXISTS sp_insert;
CREATE PROCEDURE sp_insert(IN roll_no INT,
IN sname VARCHAR(50),
IN smarks FLOAT)
BEGIN
DECLARE status INTEGER DEFAULT 1;
DECLARE CONTINUE HANDLER FOR 1062
BEGIN 
   SET status = 0;
END;
INSERT INTO student
VALUES(roll_no,sname,smarks);
END $$
DELIMITER ;


CALL sp_insert(5,"pavan",85);
SELECT * FROM student; 

## cursor

Declare variables as per requirement.
2. Declare Cursor
3. Declare NOT FOUND handler
4. Open cursor
5. FETCH record into variable
6. Close Cursor

-------------------------------------
DELIMITER $$
CREATE PROCEDURE sp_fetch_name( INOUT name_list VARCHAR(500) )
BEGIN
DECLARE finished INTEGER DEFAULT 0;
DECLARE name VARCHAR(50) DEFAULT '';
-- Declare cursor
DECLARE cur CURSOR FOR SELECT sname FROM student;
-- Declare NOT FOUND Handler
DECLARE CONTINUE HANDLER FOR NOT FOUND
SET finished = 1;
-- Open Cursor
OPEN cur;
-- define labled loop
label:LOOP
FETCH cur INTO name;
IF finished = 1 
THEN
LEAVE label;
END IF;
SET name_list = CONCAT( name,',',name_list);
END LOOP label;
-- Close Cursor
CLOSE cur;
END $$
DELIMITER ;
---------------------------------------------------

SET @list = '';
CALL sp_fetch_name( @list );
SELECT @list FROM DUAL;
-------------------------------------------------

##Stored Function

it is a server side program, 
which is used to achieve reusability,
when pre defined function are insufficient to execute 
business logic 
so we create stored function

here parameter ais IN only, 
we cant change it explicitely also,
unlike stored procedure .
-> we cant execute function through CALL,
for invoking a stored function,
 we need to refer function in its expression i.e select statement;

CREATE FUNCTION sp_name (params,... )
RETURNS type [NOT] DETERMINISTIC
BEGIN
routine_body
END
------------------------

### for lower function stored procedure

DELIMITER $$
CREATE FUNCTION sf_lowerstr (str VARCHAR(50))
RETURNS VARCHAR(50) DETERMINISTIC
BEGIN
DECLARE temp VARCHAR(50) default '';
SET temp = CONCAT('LIFE','',LOWER(STR));
RETURN temp;
END$$
DELIMITER ;

SELECT sf_lowerstr('WORLD') from DUAL;

-----------------------------------------

## FUNCTION
FUNCTION cannot return multiple values,
only a single one, for multiple values as output use 
stored procedures.

types of stored function:
1 deterministic function
-> a function in which for  same parameters result is always the same .
eg BIN(1);

2 NON deterministic function:
a function in which for same parameters result can change .
eg: datefdiff(currdate(),hire_date);

##trigger
trigger is a named database object associated with only permanent table, which activates when a particular event occurs for the table.


-----------------------------------------------
CREATE
    TRIGGER trigger_name
    trigger_time trigger_event
    ON tbl_name FOR EACH ROW
    [trigger_order]
    BEGIN
    trigger_body
    END
------------------------------------------------

trigger_time: { BEFORE | AFTER }

trigger_event: { INSERT | UPDATE | DELETE }

trigger_order: { FOLLOWS | PRECEDES } other_trigger_name

-------------------------------------------------

1. POSSIBLE trigger_event: 
1. INSERT
   THE trigger activates whenever a new row is inserted into the table.
2. UPDATE
 the trigger activates whenever a row is updated
3.DELETE 
the trigger activates whenever a row is deleted from the table

TCL operation are not permitted on trigger
OLD and NEW are implicit variable available for trigger.
it doesnt take any parameter or return values.
#4 to maintain logs of DML operations we use trigger
--------------------------------------------------


CREATE TABLE accounts
(
number INTEGER,
name VARCHAR(50),
balance FLOAT,
type VARCHAR(50)
);
CREATE TABLE transactions
(
number INTEGER,
balance FLOAT,
type VARCHAR( 50 )
);
INSERT INTO accounts
VALUES
(101,'Amol',85000,'Saving'),
(102,'Rupesh',30000,'Current'),
(103,'Sandeep',185000,'Loan');
---------------------------------------------

DELIMITER $$
CREATE TRIGGER trans_insert
    AFTER INSERT ON accounts FOR EACH ROW
    BEGIN
     INSERT INTO transactions
     VALUES(NEW.number,NEW.balance,NEW.type);
    END $$
    DELIMITER ;

    SHOW TRIGGERS;

-------------------------------------------
DELIMITER $$
CREATE TRIGGER trans_update
    AFTER UPDATE ON accounts FOR EACH ROW
    BEGIN
     INSERT INTO transactions
     VALUES(OLD.number,OLD.balance,OLD.type);
     INSERT INTO transactions
     VALUES(NEW.number,NEW.balance,NEW.type);
    END $$
    DELIMITER ;

    SHOW TRIGGERS;












