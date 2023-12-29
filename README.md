# Creating-companydatabase
create table employee(
    emp_id int Primary Key,
    first_name VARCHAR(40),
    last_name Varchar(40),
    birth_day DATE,
    sex Varchar(1),
    salary Int,
    super_id Int,
    branch_id Int);

create table branch(
    branch_id INT Primary Key,
    branch_name Varchar(40),
    mgr_id Int,
    mgr_start_date DATE,
    Foreign Key(mgr_id) References employee(emp_id) on delete set NULL
);

Alter table employee
Add foreign Key(branch_id)
References branch(branch_id)
on delete set NULL;

Alter table employee
add foreign key(super_id)
references employee(emp_id)
On delete set NULL;

create table client (
    client_id Int Primary Key,
    client_name VARCHAR(40),
    branch_id Int,
    Foreign Key(branch_id) references branch(branch_id) on delete set NULL
);

create table works_with(
    emp_id INT,
    client_id Int,
    total_sales Int,
    Primary key(emp_id,client_id),
    foreign Key(emp_id) references employee(emp_id) On delete cascade,
    Foreign Key(client_id) references client(client_id) On delete cascade
 );

 -- Corporate
 Insert into employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL); 

 INSERT into branch VALUES(1, 'Corpoate', 100, '2006-02-09');

 update employee
 set branch_id =1
 where emp_id =100; 

 Insert into employee VALUES(101, 'Jan', 'levinson', '1961-05-11', 'F', 110000, 100, 1);

Select * from employee;

select*
from employee
order by salary Desc;
--find all employees ordered by sex then name
select* 
from employee
order by sex, first_name, last_name;
--find all the first 5 employees in the table
select*
from employee
limit 1;
-- find all first names and last names of employees
select first_name, last_name
from employee;
--find the fornames and surnames of all employees
select first_name As forename,last_name As surname
from employee;
-- find out all the different genders
select distinct sex 
from employee ;

select distinct branch_id 
from employee ;
-------Functions
--find the number of employees
select COUNT(emp_id)
from employee;
--find the number of female employees born after 1970
select COUNT(emp_id)
from employee
where sex='F'AND birth_date > '1971-01-01';
--find the average of all employee salaries
select AVG(salary)
from employee; 
-- find the sum of all employee salaries
select SUM(salary)
from employee;  
--find out how many amles and females there are
select COUNT(sex), sex 
from employee
group by sex ; 

--wild cards
--% =any # characters, _= one character
select *
from branch_supplier
where supplier_name LIKE '% Label%';
--find any employee born in may
select *
from employee
where birth_date LIKE '____-10%';
-- find any clients who are schools
select *
from client
where client_name LIKE '%school%';

--UNION
--Find a list of employee and branch names
select first_name 
from employee
Union
select branch_name
from branch;
-- same data types and similar number of columns are taken into consideration and can be done for 'n' number of times.
--Joins
--Find all branches and names of their managers
select employee.emp_id, employee.first_name, branch.branch_name
from employee
left join branch
on employee.emp_id=branch.mgr_id;

#Nested Queries
#Find the names of all employees who have
#sold over 30,000 to a single client
select employee.first_name,employee.last_name
from employee
where employee.emp_id in (
    select works_with.emp_id
    from works_with
    where works_with.total_sales >30000
);
OR 
select works_with.emp_id 
from works_with
where works_with.total_sales>30000
