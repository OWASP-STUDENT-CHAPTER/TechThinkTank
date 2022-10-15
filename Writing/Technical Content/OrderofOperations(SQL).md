**SQL Order of Operations**



SQL is not a traditional programming language in which you write a sequence of instructions in a given order of execution. Instead, SQL is a "declarative" language, which means that by writing a SQL query, you declare what data you expect as a result of the query, but you don't indicate how to obtain it.

Six Operations to Order: SELECT, FROM, WHERE, GROUP BY, HAVING, and ORDER BY

By using examples, we will explain the execution order of the six most common operations or pieces in an SQL query. Because the database executes query components in a specific order, it's helpful for the developer to know this order. It's similar to following a recipe: you need to know the ingredients and what to do with ingredients, but you also need to know in which order do the tasks. If the database follows a different order of operations, the performance of the query can decrease dramatically.

The best way to learn SQL order of operations is through practice. I recommend LearnSQL.com's SQL Practice track. It contains over 600 hands-on exercises to practice your SQL skills. Or select the best course for you from over 30 interactive SQL courses we offer at various levels of proficiency.

LearnSQL.com is a great place to learn SQL. LearnSQL.com offers 30 interactive courses that range in difficulty from beginner to advanced. You can choose from a full learning track, mini-tracks to sharpen targeted skills, and individual courses.
The Employee Database

In this article, we will work with a database for a typical company that has employees distributed in different departments. Each employee has an ID, a name, and a salary and belongs to a department, as we can see in the following tables.

Sample of the EMPLOYEE table:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT
100	James	Smith	78,000	ACCOUNTING
101	Mary	Sexton	82,000	IT
102	Chun	Yen	80,500	ACCOUNTING
103	Agnes	Miller	95,000	IT
104	Dmitry	Komer	120,000	SALES
Sample of the DEPARTMENT table:

DEPT_NAME	MANAGER	BUDGET
ACCOUNTING	100	300,000
IT	101	250,000
SALES	104	700,000
In this article, we will also use frequent SQL queries used in a company: "Obtain the names of employees working for the IT department" or "Obtain the number of employees in each department with a salary higher than 80.000." For each of these queries, we will analyze the order of execution of its components.

Let's start with a simple query to obtain the names of the employees in the IT department:

SELECT LAST_NAME, FIRST_NAME
  FROM EMPLOYEE
 WHERE DEPARTMENT = 'IT'
First, we execute FROM EMPLOYEE, which retrieves this data:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT
100	James	Smith	78,000	ACCOUNTING
101	Mary	Sexton	82,000	IT
102	Chun	Yen	80,500	ACCOUNTING
103	Agnes	Miller	95,000	IT
104	Dmitry	Komer	120,000	SALES
Second, we execute WHERE DEPARTMENT = 'IT', which narrows it down to this:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT
101	Mary	Sexton	82,000	IT
103	Agnes	Miller	95,000	IT
Finally, we apply SELECT FIRST_NAME, LAST_NAME, producing the final result of the query:

FIRST_NAME	LAST_NAME
Mary	Sexton
Agnes	Miller
Great! After completing our first query dissection, we can conclude that the order of execution for simple queries with SELECT, FROM, and WHERE is:

SELECT, FROM, and WHERE 
Do you want to practice your SQL skills online? Check out our SQL Practice track!.
Changes in the Order of Operations If We Add ORDER BY

Suppose your boss receives a report based on the query in the previous example and rejects it, because the employee names are not in alphabetical order. To fix it, you need to add an ORDER BY clause to the previous query:

  SELECT LAST_NAME, FIRST_NAME
    FROM EMPLOYEE
   WHERE DEPARTMENT = 'IT'
ORDER BY FIRST_NAME
The execution process of this query is almost the same as in the previous example. The only change is at the end, when the ORDER BY clause is processed. The final result of this query orders the entries by FIRST_NAME, as shown below:

FIRST_NAME	LAST_NAME
Agnes	Miller
Mary	Sexton
So, if we have SELECT with FROM, WHERE, and ORDER BY, the order of execution is:

SELECT with FROM, WHERE, and ORDER BY 
Adding GROUP BY and HAVING Clauses to the Query

In this example, we will use a query with GROUP BY. Suppose we want to obtain how many employees in each department have a salary higher than 80,000, and we want the result in descending order by the number of people in each department. The query for this situation is:

  SELECT DEPARTMENT, COUNT(*)
    FROM EMPLOYEES
   WHERE SALARY > 80000
GROUP BY DEPARTMENT
ORDER BY COUNT(*) DESC
Again, we first execute FROM EMPLOYEE, which retrieves this data:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT
100	James	Smith	78,000	ACCOUNTING
101	Mary	Sexton	82,000	IT
102	Chun	Yen	80,500	ACCOUNTING
103	Agnes	Miller	95,000	IT
104	Dmitry	Komer	120,000	SALES
Second, we execute WHERE SALARY > 80000, which narrows it down to this:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT
101	Mary	Sexton	82,000	IT
102	Chun	Yen	80,500	ACCOUNTING
103	Agnes	Miller	95,000	IT
104	Dmitry	Komer	120,000	SALES
Third, GROUP BY is applied, generating one record for each distinct value in the GROUP BY columns. In our example, we create one record for each distinct value in DEPARTMENT:

DEPARTMENT
ACCOUNTING
IT
SALES
Fourth, we apply SELECT with COUNT(*), producing this intermediate result:

DEPARTMENT	COUNT(*)
ACCOUNTING	1
IT	2
SALES	1
Finally, we apply the ORDER BY clause, producing the final result of the query:

DEPARTMENT	COUNT(*)
IT	2
ACCOUNTING	1
SALES	1
The order of execution in this example is:

1-FROM  2-WHERE  3-GROUP BY    4-SELECT      5-ORDER BY 
In this next example, we will add the HAVING clause. HAVING is not as commonly used in SQL as the other clauses we've looked at so far. The best way to describe HAVING is that it's like the WHERE clause for GROUP BY. In other words, it is a way to filter or discard some of the groups of records created by GROUP BY.

Suppose we now want to obtain all the departments, except the SALES department, with an average salary higher than 80,000. The query for this situation is:

  SELECT DEPARTMENT
    FROM EMPLOYEES
   WHERE DEPARTMENT <> 'SALES'
GROUP BY DEPARTMENT
  HAVING AVG(SALARY) > 80000
Again, we first execute FROM EMPLOYEE, which retrieves this data:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT
100	James	Smith	78,000	ACCOUNTING
101	Mary	Sexton	82,000	IT
102	Chun	Yen	80,500	ACCOUNTING
103	Agnes	Miller	95,000	IT
104	Dmitry	Komer	120,000	SALES
Second, the WHERE clause, excluding the SALES records, is processed:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT
100	James	Smith	78,000	ACCOUNTING
101	Mary	Sexton	82,000	IT
102	Chun	Yen	80,500	ACCOUNTING
103	Agnes	Miller	95,000	IT
Third, GROUP BY is applied, generating the following records:

DEPARTMENT	AVG(SALARY)
ACCOUNTING	79,250
IT	88,500
Fourth, HAVING AVG(SALARY) > 80000 is applied to filter the group of records generated by GROUP BY:

DEPARTMENT	AVG(SALARY)
IT	88,500
Finally, the SELECT clause is applied, producing the final result of the query:

DEPARTMENT
IT
The order of execution in this example is:

1 - FROM, 2 - WHERE, 3 - GROUP BY, 4 - HAVING, 5 - SELECT 
Adding a New Operation: The JOIN Clause

The previous examples dealt with one table. Let's add a second table using the JOIN clause. Suppose we want to obtain the last names and employee IDs of employees working for departments with a budget higher than 275,000. The query for this situation is:

SELECT EMPLOYEE_ID, LAST_NAME
  FROM EMPLOYEES
  JOIN DEPARTMENT
    ON DEPARTMENT = DEPT_NAME
 WHERE BUDGET > 275000
Again, we first execute FROM EMPLOYEE, which retrieves this data:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT
100	James	Smith	78,000	ACCOUNTING
101	Mary	Sexton	82,000	IT
102	Chun	Yen	80,500	ACCOUNTING
103	Agnes	Miller	95,000	IT
104	Dmitry	Komer	120,000	SALES
Second, we apply the JOIN clause generating a new intermediate result combining both tables:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT	DEPT_NAME	MANAGER	BUDGET
100	James	Smith	78,000	ACCOUNTING	ACCOUNTING	100	300,000
101	Mary	Sexton	82,000	IT	IT	101	250,000
102	Chun	Yen	80,500	ACCOUNTING	ACCOUNTING	100	300,000
103	Agnes	Miller	95,000	IT	IT	101	250,000
104	Dmitry	Komer	120,000	SALES	SALES	104	700,000
Third, WHERE BUDGET > 275000 is applied:

EMPLOYEE_ID	FIRST_NAME	LAST_NAME	SALARY	DEPARTMENT	DEPT_NAME	MANAGER	BUDGET
100	James	Smith	78,000	ACCOUNTING	ACCOUNTING	100	300,000
102	Chun	Yen	80,500	ACCOUNTING	ACCOUNTING	100	300,000
104	Dmitry	Komer	120,000	SALES	SALES	104	700,000
Finally, SELECT EMPLOYEE_ID, LAST_NAME is executed, producing the final result of the query:

EMPLOYEE_ID	LAST_NAME
100	Smith
102	Yen
104	Komer
The order of execution in this example is:

1 - FROM, 2 - JOIN, 3 - WHERE, 4 - SELECT 
Closing Words

In this article, we covered the execution order in SQL queries through examples. From these examples, we can see that there is an order of execution, but this order may vary depending on which clauses are present in the query. As a general guideline, the order of execution is:

1 - FROM, 2 - JOIN, 3 - WHERE, 4 - GROUP BY, 5 - HAVING, 6 - SELECT, 7 - ORDER BY 
LearnSQL.com lets you learn SQL by writing SQL code on your own. You build your SQL skills gradually. Each new concept is reinforced by an interactive exercise. By actually writing SQL code, you build your confidence.
However, if one of these clauses is not present, the order of execution will be different. SQL is an easy, entry-level language, but once you are inside, there are a lot of exciting concepts to explore. Check out this online course to enter the fascinating world of SQL, and see where it can take you!
