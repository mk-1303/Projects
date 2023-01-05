# Project on a hr database with employee table

--Viewing all tables from db
SELECT * FROM all_tables;
--Backing up employee table/TRUNCATING table/adding values to the table from other table
CREATE TABLE emp_bkup as SELECT * FROM employees; -- CREATING BACKUP TABLE
TRUNCATE TABLE emp_bkup;--TRUNCATING BACKUP TABLE
SELECT * FROM emp_bkup;
--INSERT
INSERT INTO emp_bkup (SELECT * FROM employees);--INSERTING RECORDS INTO THE BACK UP TABLE
CLEAR SCREEN
---Viewing data from employee table
SELECT * FROM employees;
--Viewing data from departments table
SELECT * FROM departments;
--Checking total no of data on the employee tables
SELECT COUNT(employee_id) FROM employees;
--Joining employee tables and departments table and viewing whose salary is greater than 6000
SELECT e.

*,d.department_name FROM employees e INNER JOIN departments d
ON e.department_id=d.department_id WHERE SALARY >6000;
--Joining 3 different tables
SELECT e.employee_id,concat(e.first_name,e.last_name),d.department_name,l.city FROM employees e JOIN
departments d ON e.department_id=d.department_id
JOIN locations l
ON d.location_id=l.location_id
--Group by
SELECT department_id,sum(salary) FROM employees GROUP BY department_id having SUM(salary)>50000 ORDER BY department_id;
--Sub query
SELECT * FROM employees WHERE salary=(SELECT max(salary) FROM employees);--single row
SELECT * FROM employees WHERE salary IN (SELECT MAX(salary) FROM employees GROUP BY department_id);--Multi row sub query
--Aggregate functions
SELECT COUNT(*

) FROM employees;--Total count of employees
SELECT SUM(salary) FROM employees;--Total sum of salary of all employees
SELECT department_id,SUM(salary) FROM employees GROUP BY department_id;--Total sum of salary in each dept
SELECT AVG(salary) FROM employees;--Avg of salary
SELECT MIN(salary) FROM employees;--Min salary
--Salar function
SELECT CONCAT(first_name,CONCAT(' ',last_name)) FROM employees;--concat will combine two strings
SELECT first_name FROM employees WHERE UPPER(first_name)='LAURA';--Upper the first name into upper case
SELECT substr(first_name,2,4) FROM employees;--Substr function will return based on the index given
--Analytical function/Window function
SELECT concat(first_name,concat(' ',last_name)),salary,rank() over(order by salary) from employees;--ranking based on salary
SELECT concat(first_name,concat(' ',last_name)),dense_rank() over(order by salary) FROM employees;--dense rank based on salary
SELECT concat(first_name,concat(' ',last_name)) Name,round(avg(salary) OVER(ORDER BY employee_id),2) avg_salary FROM employees;-- Running avg salary
SELECT concat(first_name,concat(' ',last_name)) Name,department_id,round(sum(salary)
OVER(PARTITION BY department_id  ORDER BY employee_id),2) avg_salary FROM employees;--Running sum of salary

**PLSQL**
--Ananymous block if else
set SERVEROUTPUT ON;
clear screen
DECLARE
a NUMBER(2):=10;
b NUMBER(2):=20;
BEGIN
if a>b then
dbms_output.put_line(a||' is greater than '||b);
elsif a<b then
dbms_output.put_line(b||' is greater than '||a);
else
dbms_output.put_line(b||' and '||a||'is equal');
end if;
END;
--Check employee hired in leap year or not
SELECT * FROM employees;
clear screen
DECLARE
v_emp_id NUMBER :=&employee_id;
v_hire_year NUMBER(4);
BEGIN
SELECT to_number(to_char(hire_date,'YYYY')) INTO v_hire_year FROM employees
WHERE employee_id=v_emp_id;
if mod(v_hire_year,4)=0 then
dbms_output.put_line(v_emp_id||' is hired in a leap year');
else
dbms_output.put_line(v_emp_id||'is not hired in a leap year');
end if;
END;
