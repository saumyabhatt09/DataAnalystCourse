
## Basic SQL

### Create Table
---
Table 1 Query:

    Create Table EmployeeDemographics 
    (EmployeeID int, 
    FirstName varchar(50), 
    LastName varchar(50), 
    Age int, 
    Gender varchar(50)
    )

Table 2 Query:

    Create Table EmployeeSalary 
    (EmployeeID int, 
    JobTitle varchar(50), 
    Salary int
    )



Table 1 Insert:

    Insert into EmployeeDemographics VALUES
    (1001, 'Jim', 'Halpert', 30, 'Male'),
    (1002, 'Pam', 'Beasley', 30, 'Female'),
    (1003, 'Dwight', 'Schrute', 29, 'Male'),
    (1004, 'Angela', 'Martin', 31, 'Female'),
    (1005, 'Toby', 'Flenderson', 32, 'Male'),
    (1006, 'Michael', 'Scott', 35, 'Male'),
    (1007, 'Meredith', 'Palmer', 32, 'Female'),
    (1008, 'Stanley', 'Hudson', 38, 'Male'),
    (1009, 'Kevin', 'Malone', 31, 'Male')

Table 2 Insert:

    Insert Into EmployeeSalary VALUES
    (1001, 'Salesman', 45000),
    (1002, 'Receptionist', 36000),
    (1003, 'Salesman', 63000),
    (1004, 'Accountant', 47000),
    (1005, 'HR', 50000),
    (1006, 'Regional Manager', 65000),
    (1007, 'Supplier Relations', 41000),
    (1008, 'Salesman', 48000),
    (1009, 'Accountant', 42000)

### Select query
---
    SELECT * 
    	FROM EmployeeDemographics
    
    SELECT  *
    	FROM  Test.dbo.EmployeeDemographics
    
    SELECT FirstName, LastName
    	FROM EmployeeDempgraphics

* Other functions -
	* AVG() - To get the average
	* Count() - To return count of 
	* AS - To rename a column
	* DISTINCT - To return  without duplicates
	* MAX and MIN - To calculate maximum and minimum

### Where 
---
    Select * 
    	from EmployeeDemographics
    	WHERE Gender = 'Male'
	
* Other functions -
	* <> - Not equals to
	* And - And operator
	* Or - Or operator
	* Like - To match text mostly
		* SELECT *
			FROM EmployeeDemographics
			WHERE LastName LIKE '%s'  (% - wildcard)
	* Null and Not Null - To return null and not null
	* In - = for multiple things
		* SELECT *
			FROM EmployeeDemographics
			WHERE LastName IN ('Jim', 'Pam')
			
### Group and Order By
---
Used to arrange the data in a particular manner.
**Note: The Aggregation functions are usually used with Group by statement**

    SELECT Gender, Count(Gender)
    	FROM dbo.EmployeeDemographics
    	Group by (Gender)
    
    SELECT *
    	FROM dbo.EmployeeDemographics
    	Order by (FirstName) - Will display in alphabetical order


**Incorrect**

    SELECT FirstName, Gender
    FROM dbo.EmployeeDemographics
    Group by (Gender)
Error: Column 'dbo.EmployeeDemographics.FirstName' is invalid in the select list because it is not contained in either an aggregate function or the GROUP BY clause. This error happens because while using the group by, everything in the select function must be a part of either aggregate function or the group by clause. In the query above the database won't know what first-name to fetch for each gender 

### Joins
---
    Select FirstName, LastName from
    Test.dbo.EmployeeDemographics
    INNER JOIN Test.dbo.EmployeeSalary
    ON Test.dbo.EmployeeDemographics.EmployeeID = Test.dbo.EmployeeSalary.EmployeeID
    WHERE Gender = 'Male'

 1. Inner Join = returns everything matching from both tables
 2. Full Outer Join = returns everything matching and which does not match and combines it to one table
 3. Left and Right Outer Join = returns everything in the left table and matching from right table and vise versa.

If we want to return EmployeeID we have to specify which table's EmployeeID we want

### Unions
---
Used usually when we have all the same columns in both the tables and we want to merge the tables together.
It usually matches using the data type so have to be careful while applying the Unions.

### Case Statements
---
    Select FirstName, LastName,
    Case
    	WHEN Age >30 THEN 'Young'
    	ELSE 'Baby'
    End as AgeBracket
    from Test.dbo.EmployeeDemographics
    INNER JOIN Test.dbo.EmployeeSalary
    ON Test.dbo.EmployeeDemographics.EmployeeID = Test.dbo.EmployeeSalary.EmployeeID
    WHERE Gender = 'Male'

### Having 
---

The SQL HAVING Clause -  The _HAVING clause_ was added to _SQL_ because the WHERE keyword cannot be used with aggregate functions.

The `WHERE` clause in SQL is used to filter rows before any grouping or aggregation is performed. Because of this, you cannot directly use aggregate functions within the `WHERE` clause. Instead, SQL provides the `HAVING` clause, which is specifically designed to filter groups after aggregation.

#### Differences Between `WHERE` and `HAVING`

    Select FirstName, LastName
    from Test.dbo.EmployeeDemographics
    INNER JOIN Test.dbo.EmployeeSalary
    ON Test.dbo.EmployeeDemographics.EmployeeID = Test.dbo.EmployeeSalary.EmployeeID
    WHERE Gender = 'Male'
    Group By FirstName, LastName, Salary
    HAVING MAX(Salary) > 2000

- **`WHERE` Clause**: Filters rows before any grouping and aggregation. It cannot contain aggregate functions.
- **`HAVING` Clause**: Filters groups after aggregation. It is used with aggregate functions.

### Update and Delete
---

    UPDATE dbo.EmployeeDemographics
    	SET FirstName = 'Jimmy'
    	Where FirstName = 'Jim'
    SELECT * from dbo.EmployeeDemographics

    DELETE FROM dbo.EmployeeDemographics
    WHERE EmployeeID = 1002
    SELECT * from dbo.EmployeeDemographics

**To safeguard from deleting needed data execute a select query first before delete query so that we know what exactly are we deleting**


### Aliasing
---

    SELECT FirstName + ' '+LastName AS FullName
    From dbo.EmployeeDemographics

Useful when we are executing bigger queries and when we have aggregate functions which return a derived column.

We can also assign an alias name to table and it makes the script look clean and easy to understand. 

### Partition By
---
Comes in very useful when we want to apply aggregate function on just a selected column and display other columns alongside in the result set.

    SELECT FirstName, LastName, Count(Gender) OVER (PARTITION BY Gender) as MaximumSalary
    FROM dbo.EmployeeDemographics
    JOIN dbo.EmployeeSalary
    ON dbo.EmployeeDemographics.EmployeeID = dbo.EmployeeSalary.EmployeeID

ORDER by Salary DESC

Resource: https://www.youtube.com/watch?v=h3Jx4CoNgL8

### CTE (Common Table Expression)
---

    WITH Employee as
    	(Select FirstName, LastName, Count(Gender) OVER (Partition by Gender) as TotalGender
    	 From dbo.EmployeeDemographics
    	)
    Select FirstName from Employee

A CTE, or Common Table Expression, is a temporary named result set that you can reference within a SQL statement. It's like creating a temporary table that exists only for the duration of the query execution.
This means that the Employee is a kind of temp table being created in the memory that can be used in the subqueries.
The CTE has to be generated every time to used in the subsequent queries.

###  Temp Tables
---
A temporary SQL table, also known as a  **temp table**, is a table that is created and used within the context of a specific session or transaction in a database management system. It is designed to store temporary data that is needed for a short duration and does not require a permanent storage solution.

Temporary tables are created on-the-fly and are typically used to perform complex calculations, store intermediate results, or manipulate subsets of data during the execution of a query or a series of queries.

    Create TABLE #Employee 
    	(
    		FirstName varchar(100),
    		LastName varchar (100),
    		Gender varchar(15),
    		Salary int
    	)
    
    Insert into #Employee 
    Select FirstName, LastName, Gender, Salary 
    from EmployeeDemographics
    INNER JOIN EmployeeSalary
    ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
    Where Gender = 'Male'
    
    Select * from #Employee

### String Functions
---

 - Drop Table EmployeeErrors:

Query:
    

    CREATE TABLE EmployeeErrors (
        EmployeeID varchar(50)
        ,FirstName varchar(50)
        ,LastName varchar(50)
        )
    
    
    Insert into EmployeeErrors Values 
    ('1001  ', 'Jimbo', 'Halbert')
    ,('  1002', 'Pamela', 'Beasely')
    ,('1005', 'TOby', 'Flenderson - Fired')
    
    Select *
    From EmployeeErrors

- Using Trim, LTRIM, RTRIM

Query:

    Select EmployeeID, TRIM(employeeID) AS IDTRIM
    FROM EmployeeErrors 
    
    Select EmployeeID, RTRIM(employeeID) as IDRTRIM
    FROM EmployeeErrors 
    
    Select EmployeeID, LTRIM(employeeID) as IDLTRIM
    FROM EmployeeErrors 

- Using Replace

Query:

    Select LastName, REPLACE(LastName, '- Fired', '') as LastNameFixed
    FROM EmployeeErrors


- Using Substring

Query:

    Select Substring(err.FirstName,1,3), Substring(dem.FirstName,1,3), Substring(err.LastName,1,3), Substring(dem.LastName,1,3)
    FROM EmployeeErrors err
    JOIN EmployeeDemographics dem
    	on Substring(err.FirstName,1,3) = Substring(dem.FirstName,1,3)
    	and Substring(err.LastName,1,3) = Substring(dem.LastName,1,3)



- Using UPPER and lower

Query:

    Select firstname, LOWER(firstname)
    from EmployeeErrors
    
    Select Firstname, UPPER(FirstName)
    from EmployeeErrors

### Stored Procedures

A stored procedure is a prepared SQL code that you can save, so the code can be reused over and over again.

So if you have an SQL query that you write over and over again, save it as a stored procedure, and then just call it to execute it.

You can also pass parameters to a stored procedure, so that the stored procedure can act based on the parameter value(s) that is passed.

    CREATE PROCEDURE Temp_Employee
    AS
    DROP TABLE IF EXISTS #temp_employee
    Create table #temp_employee (
    JobTitle varchar(100),
    EmployeesPerJob int ,
    AvgAge int,
    AvgSalary int
    )
    
    Insert into #temp_employee
    SELECT JobTitle, Count(JobTitle), Avg(Age), AVG(salary)
    FROM SQLTutorial..EmployeeDemographics emp
    JOIN SQLTutorial..EmployeeSalary sal
    	ON emp.EmployeeID = sal.EmployeeID
    group by JobTitle
    
    Select * 
    From #temp_employee
    GO;
    
    
    CREATE PROCEDURE Temp_Employee2 
    @JobTitle nvarchar(100)
    AS
    DROP TABLE IF EXISTS #temp_employee3
    Create table #temp_employee3 (
    JobTitle varchar(100),
    EmployeesPerJob int ,
    AvgAge int,
    AvgSalary int
    )
    
    
    Insert into #temp_employee3
    
    SELECT JobTitle, Count(JobTitle), Avg(Age), AVG(salary) FROM SQLTutorial..EmployeeDemographics emp JOIN SQLTutorial..EmployeeSalary sal 	ON emp.EmployeeID = sal.EmployeeID where JobTitle = @JobTitle --- make sure to change this in this script from original above group by JobTitle
    
    Select * 
    From #temp_employee3
    GO;
    
    exec Temp_Employee2 @jobtitle = 'Salesman'
    exec Temp_Employee2 @jobtitle = 'Accountant'

### Sub-Queries

Reference: https://www.youtube.com/watch?v=m1KcNV-Zhmc&list=PLUaB-1hjhk8FE_XZ87vPPSfHqb6OcM0cF&index=18

    Select EmployeeID, JobTitle, Salary
    From EmployeeSalary

- Subquery in Select

Query:

    Select EmployeeID, Salary, (Select AVG(Salary) From EmployeeSalary) as AllAvgSalary
    From EmployeeSalary

- How to do it with Partition By

Query:

    Select EmployeeID, Salary, AVG(Salary) over () as AllAvgSalary
    From EmployeeSalary

- Why Group By doesn't work

Query:

    Select EmployeeID, Salary, AVG(Salary) as AllAvgSalary
    From EmployeeSalary
    Group By EmployeeID, Salary
    order by EmployeeID


- Subquery in From

Query:

    Select a.EmployeeID, AllAvgSalary
    From 
    	(Select EmployeeID, Salary, AVG(Salary) over () as AllAvgSalary
    	 From EmployeeSalary) a
    Order by a.EmployeeID


- Subquery in Where

Query:

    Select EmployeeID, JobTitle, Salary
    From EmployeeSalary
    where EmployeeID in (
    	Select EmployeeID 
    	From EmployeeDemographics
    	where Age > 30)


### Window Function

Window functions in SQL are used to perform calculations across a set of table rows that are somehow related to the current row. Unlike aggregate functions, which return a single value for a group of rows, window functions can return multiple rows for each group and maintain the original row details. This makes window functions extremely useful for a variety of analytical and reporting tasks.

    select id, firstname, lastname, sum(salary) over (partion by gender)
    from employee_info empi
    JOIN employee_salary emps
    ON empi.id = emps.id

`ROW_NUMBER`, `RANK`, and `DENSE_RANK` are all window functions in SQL that are used to assign unique numbers to rows within the result set based on the order specified in the `ORDER BY` clause. Each of these functions serves a different purpose and behaves differently when dealing with duplicate values. Here’s a detailed explanation of each function and their differences:

#### 1. `ROW_NUMBER()`

The `ROW_NUMBER()` function assigns a unique sequential integer to rows within a partition of the result set, starting at 1 for the first row in each partition.

-   **Behavior with Duplicates:** Each row gets a unique number, even if there are duplicate values in the columns specified in the `ORDER BY` clause.

Query:

    SELECT
        sales_id,
        employee_id,
        region,
        sales_amount,
        ROW_NUMBER() OVER (PARTITION BY region ORDER BY sales_amount DESC) AS row_num
    FROM
        sales;
#### 2. `RANK()`
The `RANK()` function assigns a rank to each row within a partition of the result set. The rank of a row is its position in the result set after ordering. Rows with the same values receive the same rank, and the next rank value is skipped based on the number of tied rows.

-   **Behavior with Duplicates:** Tied rows receive the same rank, and gaps are left in the rank sequence.

Query:

    SELECT
        sales_id,
        employee_id,
        region,
        sales_amount,
        RANK() OVER (PARTITION BY region ORDER BY sales_amount DESC) AS rank
    FROM
        sales;

 

#### 3. `DENSE_RANK()`

The `DENSE_RANK()` function is similar to `RANK()`, but it does not leave gaps in the rank sequence. Rows with the same values receive the same rank, and the next rank value is consecutive.

-   **Behavior with Duplicates:** Tied rows receive the same rank, and no gaps are left in the rank sequence.

Query:

    SELECT
        sales_id,
        employee_id,
        region,
        sales_amount,
        DENSE_RANK() OVER (PARTITION BY region ORDER BY sales_amount DESC) AS dense_rank
    FROM
        sales;
