# A place to learn some .. *DRUM ROLL* .. SQL! 


Now, I particularly use PostgreSQL, using pgAdmin 4 dashboard.
You could use Mysql, or other sorts of databases that run sql queries. It doesn't matter.
However, there are slight differences in each and I suggest doing research to figure out which one is 
best for you and your situation.

Wanna download it? Go [here](https://www.postgresql.org) 

Upon setup, you will need to create a password. *DO NOT FORGET THAT PASSWORD*. You will need it to be able to use pg admin.


Let's go....


# Using mockstaff.sql file 

You can run this query to see information about a specific table. Once it's downloaded and opened in postgreSQL.

```sql
SELECT * FROM company_divisions
```
The select is a function stating you want to select something. The star means to grab all of information stored
and company_divisions is a table that we have in our database. We want to extract information (all of it, hence the star) from
a specific table. 

```sql
SELECT * FROM company_regions
```
Returns us (all) the information from the table company regions!. Hence the * 
We did not place restrictions on what information we want requested. The star just says give us everything. 

Now you could imagine you could just change company_regions with any other table.


# Using the count_min_max.sql file and mockstaff.file should be already loaded.

Okay. We have a large table! What if you don't want to see  eveything at once... What if you wanted to see just a hint of what the whole table shows..

```sql
SELECT * FROM staff LIMIT 10
```
There you go. Using the LIMIT clause, you should see 10 rows from the table returned instead of 1000 that is in the table.

Now once you load the file and run the query say, ```Select * FROM staff``` 
A notification will appear in pgadmin showing how many rows are returned. It should say 1000.

BUT...

Let's say you forgot and it would be really costly to run the function again, but you still want to know how many staff employees you have. USE THE COUNT FUNCTION!
```sql
SELECT count(*) FROM staff 
```
This returns (you guessed it) 1000. 1000 for the number of employees. 

Ahhh. BUT HOW MANY OF THEM ARE WOMEN YOU ASK? 
NEVER FEAR SQL IS HERE! 
```sql
SELECT count(*) FROM staff GROUP BY gender
```
First things first. Gender has to be a column in the table(staff) for you to perform this. It has gender values as Male or Female. 
Second, GROUP BY is basically saying, yes give me the count from staff, but I want it to be returned in a different way. 
Count all these employees (hence the star!) but now I want you to count them by their gender and group them! 

AHA! We do get returned the right values, BUT!!!! It doesn't tell us which is which! 496 and 504 of what?
We can fix this. 
```sql
SELECT gender, count(*) FROM staff GROUP BY gender
```
Well why does this fix it. Well remember we wrote in our query to select on the count. In reality, this means return us just the count.
When we specify select gender, we are specifically stating to select it with our count as well.
Now we know that the number of males are 504 and females are 496.

- Getting the hang of it so far? If you come this far, I would like to recommend this [video](https://www.youtube.com/watch?v=yPu6qV5byu4). VERY efficient video about mysql, but many of the queries are the same. It is amazing. According to someone in the comments, they landed a job by watching that video.

Anyways...let's keep going.

## How many employees do we have per deparment? 
- - - - - - - - - - - - - - - - - - - - - - -
(TRY IT YOURSELF FIRST, SEE IF YOU CAN GET IT!)

- ANSWER BELOW 
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -
- - - - - - - - - - - - - - - - - - - - - - -






```sql
SELECT department, count(*) FROM staff GROUP BY department
```
Select department! Select their Count as well (for all employees, hence the star!). FROM my staff table inside my database.
THESE VALUES SELECTED WILL BE RETURNED!
NOW RETURN THEM TO ME BY GROUPING THEM BY THEIR SPECIFIC DEPARTMENT! 

Hows that for an all caps explanation? 22 rows for all 22 departments. We are too clever, aren't we?! 

## What is the highest salary of our employess..? 
```sql
SELECT max(salary) FROM staff
``` 
Yes! Very clever of you. Just change max with min and there you have it. The lowest salary will be returned! 
max is just one of the many "math" functions you could run.

Wanna see them at the same time? BUT you also want to see them based on employees in each department? You must be a very efficient person.

```sql
SELECT department, min(salary), max(salary)
FROM staff
GROUP BY department
``` 
Oh you must have wanted it specific to gender too? 

```sql
SELECT gender, min(salary), max(salary)
FROM staff
GROUP BY gender
``` 
There you have it. Go to your boss and claim you demand a higher salary! (Just kidding!)

Let's move on to statistical functions!
# Using statistics.sql file!

How much money is our company spending to pay our employees? Hey! Let's sum up all the salaries in the staff table!
```sql
SELECT sum(salary)
FROM staff
``` 
97.3 million paid per year across all staff. How much does each department pay in salary?

```sql
SELECT department, sum(salary)
FROM staff
GROUP BY department
``` 
22 rows for each of our 22 departments! What's the average?!

```sql
SELECT department, avg(salary)
FROM staff
GROUP BY department
``` 

Variance, Standard Deviations ?

```sql
SELECT department, var_pop(salary)
FROM staff
GROUP BY department
``` 
```sql
SELECT department, stddev_pop(salary)
FROM staff
GROUP BY department
``` 
We get way too many decimal places though, so lets round them and say we want 2 decimal places.
```sql
SELECT department, round(stddev_pop(salary),2)
FROM staff
GROUP BY department
``` 

# Using filter_group.sql file!
Filtering our data and grouping data.

Almost like booleans! If your familiar with coding. 
If not, its like adding clauses to our queries. Greater than, less than, equal to.

## Let's see all salaries greater then $100,000, by their name and department they work in.

```sql
SELECT last_name, department, salary
FROM staff
WHERE
salary > 100000
``` 
We used WHERE function, which is giving a restriction, or a specific request. 

OR we might want to see just one department.
just add ```WHERE department = 'Tools' ```
It is important to add the quotation marks, because tools in the database is loaded as a CHAR VAR which is a character variable, commonly known as a string in other coding languages.

If you have 2 restrictions? Just use  "AND" 

```sql
SELECT last_name, department, salary
FROM staff
WHERE
salary > 100000
AND
department = 'Tools'
``` 
We want only the department of tools and only the people with high salaries.

We can use OR as well! We will have some people in other departments. We will also have people in tools department with salaries lower than 100000 because we said OR. Give me this ... OR ... give me that.

```sql
SELECT last_name, department, salary
FROM staff
WHERE
department = 'Tools'
OR
salary > 100000
``` 

ALL DEPARMENTS that begin with letter B and sum of their salaries.
```sql
SELECT department, sum(salary)
FROM staff
WHERE
deparment LIKE 'B%'
GROUP BY department
``` 
WE used the LIKE function that basically says, WHERE something looks like "this". In our case B. The percent symbol is important here. Putting it before the B will change everything. We want the word to start with B and the % says and of the other letters could be arbitrary, as long as it starts with B, we want it returned. 

Beginning with letters Bo..?  ``` LIKE 'Bo%' ```

Baby or Beauty?  ```LIKE 'B%y' ```

# REFORMATTING data  
- file: reformat.sql

We can reformat data to get it into a consisten format. You could change state names into their abbreviations to save space and have easier convenience of use.

```sql
SELECT DISTINCT department
FROM staff
```
DISTINCT allows us to return a specific in our query just once. There are many staff members in Beauty deparment. BUT
We do not want Beauty to appear 78 times. We want it to appear once, so we use DISTINCT.
Uppercase? Just use ``` UPPER(department) ```

Job title to combine with department name?

CONCAT IT MAN!
```sql
SELECT
job_title || '-' || department
FROM staff
```
We could give names (titles) to columns we created.

```sql
SELECT
job_title || '-' || department title_dept
FROM staff
```
USE TRIM to remove whitespaces or extra characters.
```sql
SELECT
trim('   Sofware Engineer   ')
``` 
We cut out the leading and trailing whitespaces. 
Check it by 
```sql
SELECT length('   Software Engineer   ') 
``` 
with 
```sql
SELECT length(trim('   Software Engineer   ')) 
```
Length was reduced! :-)













