# MySQL-Assignment-9
This  Assignment explains about the stored procedures in MySQL
-- Consider the Worker table with following fields: Worker_Id INT FirstName CHAR(25), LastName CHAR(25), 
-- Salary INT(15), JoiningDate DATETIME, Department CHAR(25)) 
create table worker(Worker_Id INT,First_Name CHAR(25),Last_Name CHAR(25), Salary INT , Joining_Date DATETIME, Department CHAR(25));
desc worker;
# 1. Create a stored procedure that takes in IN parameters for all the columns in the Worker table 
-- and adds a new record to the table and then invokes the procedure call. 
DELIMITER //
CREATE PROCEDURE Workers(IN Worker_Id INT,IN First_Name CHAR(25),IN Last_Name CHAR(25),IN Salary INT ,IN Joining_Date DATETIME,IN Department CHAR(25))
BEGIN
INSERT INTO Worker (worker_id, First_Name, Last_Name, salary,Joining_Date,Department)
VALUES (1,'ajay','cr',50000,'2024-11-11 09:00:00','sales'),(2,'arun','kumar',60000,'2024-10-11 09:02:00','Hr'),
(3,'swathi','krishna',45000,'2024-09-11 09:10:12','Marketing'),(4,'Agosh','menon',40000,'2024-10-11 09:15:00','IT'),
(5,'Santhosh','Raj',30000,'2024-06-11 09:00:00','Finance'),(6,'premjith','kp',40000,'2024-07-11 09:18:00','Hr'),
(7,'swaraj','nair',45000,'2022-05-11 09:20:00','Finance'),(8,'sukumar','joshi',55000,'2023-07-11 08:20:00','Marketing'),
(9,'sarang','kumar',25000,'2024-011-11 09:30:00','Marketing'),(10,'rejisha','ram',45000,'2022-05-11 09:30:00','IT');
END //
DELIMITER ;
CALL Workers (5,'Santhosh','Raj',30000,'2024-06-11 09:00:00','Finance');
SELECT * FROM Worker;

# 2. Write stored procedure takes in an IN parameter for WORKER_ID and an OUT parameter for SALARY.
-- It should retrieve the salary of the worker with the given ID and returns it in the p_salary parameter. Then make the procedure call. 
DELIMITER //

CREATE PROCEDURE Worker_Salary (IN workers_code INT,OUT workers_salary DECIMAL(10, 2))
BEGIN
SELECT salary INTO workers_salary FROM Worker WHERE Worker_Id =workers_code ;
END //
DELIMITER ;

SET @new_salary = 45000;
CALl Worker_Salary (3, @new_salary);
SELECT @new_salary AS Worker_Salary;

# 3. Create a stored procedure that takes in IN parameters for WORKER_ID and DEPARTMENT. 
-- It should update the department of the worker with the given ID. Then make a procedure call. 
DELIMITER //

CREATE PROCEDURE Update_Worker_Department (IN workers_code INT,IN workers_department CHAR(25))
BEGIN
UPDATE Worker SET department = workers_department  WHERE worker_id = workers_code;
END //
DELIMITER ;


CALL Update_Worker_Department (1,'sales');
SELECT * FROM Worker WHERE Worker_Id = 1;

# 4. Write a stored procedure that takes in an IN parameter for DEPARTMENT and an OUT parameter for p_workerCount. 
-- It should retrieve the number of workers in the given department and returns it in the p_workerCount parameter. Make procedure call. 
DELIMITER //
CREATE PROCEDURE Worker_Count_By_Department (IN P_department VARCHAR(50),OUT P_workerCount INT)
BEGIN
SELECT COUNT(*) INTO P_workerCount FROM Worker WHERE department = P_department;
END //
DELIMITER ;

SET @workers_Count = 0;
CALL Worker_Count_By_Department ('Finance', @workers_Count);
SELECT @workers_Count AS Worker_Count;

#5. Write a stored procedure that takes in an IN parameter for DEPARTMENT and an OUT parameter for p_avgSalary. 
-- It should retrieve the average salary of all workers in the given department and returns it in the p_avgSalary parameter and call the procedure.
DELIMITER //
CREATE PROCEDURE Average_Salary_By_Department (IN p_department VARCHAR(50),OUT p_avgSalary double)
BEGIN
SELECT AVG(salary) INTO p_avgSalary FROM Worker WHERE department = p_department;
END //
DELIMITER ;

SET @avg_salary =0 ;
CALL Average_Salary_By_Department('Marketing', @avg_salary);
SELECT @avg_salary AS Average_Salary;
