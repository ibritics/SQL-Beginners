--EX_1
--SELECT THE 2nd Maximum Salary
SELECT ID,EMP
FROM (
  SELECT ID,EMP, DENSE_RANK() OVER(ORDER BY  PAY DESC) rn
  FROM SQL_Excercises
	)
WHERE rn=2


--EX_2
--Select the employees working on "Health App" project
SELECT emp FROM Table_1 WHERE EXISTS (SELECT * FROM Table_2 WHERE
EMP_id = ID and LOWER(Project) = "health app")


--EX_3
--Inner Join
SELECT emp_id, Project, Budget, pay
FROM Table_1 a
INNER JOIN Table_2 b
ON a.id = b.EMP_ID


--EX_4
--Left Join
SELECT *
FROM Table_1 a
LEFT JOIN Table_2 b
ON a.id = b.EMP_ID


--EX_5
--Right Join
SELECT *
FROM Table_2 b
RIGHT JOIN Table_1 a
ON a.id = b.EMP_ID


--EX_6
--SELECT THE TOP EARNING WORKER BY DEPARTMENT
WITH TOP_DEP as(
SELECT *, RANK() OVER (PARTITION BY department_id ORDER BY PAY DESC) RANK
FROM Table_1 A
INNER JOIN Table_3 B 
  ON A.ID=B.Emp_ID)
SELECT department_name, emp, pay
FROM TOP_DEP
WHERE rank = 1


--EX_7
--Assign an id to rows
SELECT *, ROW_NUMBER() over (ORDER BY ID) rowid
FROM Table_1

--EX_8
--Identify the row in which the null duplicate value is storing
WITH t as(SELECT *, ROW_NUMBER() over (ORDER BY ID) rowid FROM Table_1)
SELECT * from t where rowid NOT IN (select max(rowid) from t group by
ID)

--Note: Here we use the max() instead of other function (for instance sum) because
--we want to get only one value isolated. Try to substitude max with sum and you will see
--the difference.

--EX_8
--Delete a duplicate value
SELECT * from Table_1 where ROW_ID NOT in (select max(ROW_ID) from Table_1 group by
ID)


--EX_9
--Find distribution of male and female workers in a department
SELECT  count(DISTINCT a.ID) as num_unique_emp,  b.department_id dep_id, b.department_name dep_name, sum(CASE WHEN gender ='M' THEN 1 ELSE 0 END) MALE, sum(CASE WHEN gender ='F' THEN 1 ELSE 0 END) FEMALE
FROM Table_1 a
LEFT JOIN Table_3 b
ON ID=emp_id
GROUP BY dep_id, dep_name


--EX_10 
--Find a distribution by the classification of 6 figure salaries
Select department_name,
SUM (CASE WHEN Pay <100000 THEN 1 ELSE 0 END) AS Under_100k,
SUM(CASE WHEN Pay >=100000 THEN 1 ELSE 0 END) as Over_100k
From Table_1
LEFT JOIN Table_3
ON emp_id= id
GROUP BY 1 --it is the same as grouping by department_name


--EX_11
--From Table_4 table clean the transaction names such as to get first Paypal or first Netflix
SELECT account_id, 
	   transaction_name,
		CASE WHEN upper(transaction_name) LIKE  '%PAYPAL%' then 'PAYPAL' --First Paypal priority
		WHEN upper(transaction_name) LIKE  '%NETFLIX%' then 'NETFLIX'
        ELSE transaction_name 
        END as Transaction_2
FROM Table_4;


--EX_12
--Prove that the count of NULLS is 0 and sum of NULLS is NULL.
SELECT count(NULL) as COUNTS, sum(NULL) as SUMS


--EX_13
--Report event_id, rank 1 name(s), rank 2 name(s), rank 3 name(s). Order the contest by event_id.
--Name that share a rank should be ordered alphabetically and separated by a comma.

WITH 
t_1 as (SELECT event_id,participant_name, score,
        DENSE_RANK() OVER (PARTITION BY event_id ORDER BY score DESC) as rk 
        from table_5
        ),
t_2 AS (SELECt event_id, participant_name,score
        FROM t_1 
        where rk = 1
        order by participant_name
        ),
t_3 AS (SELECt event_id, participant_name
        FROM t_1 
        where rk = 2
        order by participant_name
        ),
t_4 AS (SELECt event_id, participant_name
        FROM t_1 
        where rk = 3
       	order by participant_name
        )


SELECT t_1.event_id , 
    GROUP_CONCAT(DISTINCT t_2.participant_name ) AS firt_place,
    GROUP_CONCAT(DISTINCT t_3.participant_name ) AS second_place,
    GROUP_CONCAT(DISTINCT t_4.participant_name ) AS third_place
FROM t_1
LEFT JOIN t_2 ON t_1.event_id=t_2.event_id
LEFT JOIN t_3 ON t_2.event_id=t_3.event_id
LEFT JOIN t_4 ON t_3.event_id=t_4.event_id
GROUP BY 1 ORDER BY 1;
 
--EX_14 
--Company has started to research the AI topics. It has assigned some of the employers to do this task
--and hired one ome employee all employee ids are in Table_6 the current employees with current tasks are in table_2. Find all employees which have switched to this project.

WITH
new_table as (
  SELECT t2.EMP_ID as EMP_ID, t6.EMP_ID as EMP_ID_1, t2.Project as Project, t6.Project as Project_1
  FROM Table_2 as t2
  FULL JOIN 
  Table_6  as t6
  on t2.EMP_ID=t6.EMP_ID
)
  SELECT coalesce(emp_id_1, emp_id) as Employer,
  		 coalesce(project_1,Project) as Project
  FROM new_table
