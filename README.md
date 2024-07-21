# SQL-NULL-HANDLING

**SQL NULL HANDLING - ISNULL, COALESCE, IS NULL, IS NOT NULL, CASE, NULLIF**

**DIFFERENT WAYS OF NULL HANDLING**

1. SELECT * FROM EMPLOYEE WHERE DEPTID **IS NULL**
2. SELECT * FROM EMPLOYEE WHERE DEPTID **IS NOT NULL**
3. SELECT **COALESCE**(FIRSTNAME, LASTNAME, 'NOT EXISTS') FROM EMPLOYEE
4. SELECT **ISNULL**(FIRSTNAME, 'NOT EXISTS') FROM EMPLOYEE
5. SELECT FIRSTNAME, 
**CASE** 
    WHEN FIRSTNAME **IS NULL** THEN LASTNAME 
	WHEN LASTNAME **IS NULL **THEN 'LAST NAME NOT EXISTS'
    ELSE 'NA' 
**END** AS DEPARTMENT 
FROM EMPLOYEE;

6. **INSERTING DEFAULT VALUES FOR NULL FIELDS WHEN DATA IS INSERTED INTO THE TABLE.**
   
CREATE TABLE EMPLOYEES (
    ID INT,
    NAME VARCHAR(100),
  **DEPARTMENT VARCHAR(100) DEFAULT 'UNASSIGNED'**
);

7. **RETURNING NULL IF SALARY IS 100 (THE NULLIF FUNCTION RETURNS NULL IF THE TWO ARGUMENTS ARE EQUAL.)**

SELECT NAME, **NULLIF**(SALARY, 100) AS SALARY FROM EMPLOYEES;

8. **GROUP BY WITH NULL VALUES**

**WHEN USING GROUP BY, NULL VALUES ARE TREATED AS A SINGLE GROUP. ALL ROWS WITH NULL VALUES IN THE GROUPED COLUMN ARE GROUPED TOGETHER.**

10. **TIPS FOR HANDLING NULL WITH AGGREGATES**

11. **DEFAULT VALUES: USE FUNCTIONS LIKE COALESCE TO REPLACE NULL VALUES WITH DEFAULT VALUES IN AGGREGATES.**

12. **SUM OF SALARIES, REPLACING NULL WITH 0**

i. SELECT SUM(**COALESCE**(SALARY, 0)) FROM EMPLOYEE;

ii. SELECT SUM(**ISNULL**(SALARY, 0)) FROM EMPLOYEE;

13. **FILTERING NULLS: FILTER OUT NULL VALUES EXPLICITLY IF NEEDED.**

i. **COUNT NON-NULL SALARIES**

SELECT COUNT(*) FROM EMPLOYEE WHERE SALARY **IS NOT NULL**;

14 **NULL HANDLING IN GROUPING: BE AWARE THAT NULLS ARE GROUPED TOGETHER IN GROUP BY QUERIES.**

i. **GROUP BY DEPARTMENT AND HANDLE NULL**

SELECT **COALESCE**(DEPARTMENT, 'UNKNOWN') AS DEPARTMENT, COUNT(*) FROM EMPLOYEE GROUP BY DEPTID

ii. **HANDLING NULL VALUES IN JOIN CONDITIONS (NULL IS NOT EQUALS TO NULL IN SQL)**

SELECT E.FIRSTNAME, D.DEPTNAME
FROM EMPLOYEE E
LEFT JOIN DEPARTMENT D ON **ISNULL**(E.DEPTID, 0) = **ISNULL**(D.ID, 0);

ii. **HANDLING NULL VALUES IN JOIN CONDITIONS**

SELECT E.FIRSTNAME, D.DEPTNAME FROM EMPLOYEE E
LEFT JOIN DEPARTMENT D 
ON (CASE 
    WHEN E.DEPTID **IS NULL** THEN 1
    ELSE E.DEPTID 
    END) = D.DEPTNAME;
