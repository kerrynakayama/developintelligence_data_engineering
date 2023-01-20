# LAB Five 
## Recursive CTE and Connect by 
![Image of title](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/snow.jpeg)

## In this past module we went over Subqueries and CTE formats.  I have provided some data that if you look at it contains hierarchal data of an organization. 

![Image of hierarchal_tree](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/tree.jpg)


### I want you to ... 
Build the script for some a really short set of data so that you can use this in the future
 

![Image of fibi snail](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/fibi.png)


*Recursion is a feature that is used in almost every coding language that exists. When the data platform offers recursion you can drill into an arbitrary level of depth in a hierarchy with very good performance*

![Image of recusive tree](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/fractal-tree-index-symmetry-recursion-love-tree-thumb.jpg)

## Break into groups of no more than 3 people and write the queries 

 - create a cte without it being recurvsive that selects all the employees from the engineering dept
 - build a recursive CTE that will visually describe the hierarchal nature of the data
 - use connect by instead of a recursive CTE to show hierarchy
 - visualize the hierarchal structure with a connect by
 
 Bonus -  Build a CTE to create a Fibonacci Sequence
```sql

create or replace file format my_csv_format
 COMPRESSION = 'AUTO' 
 FIELD_DELIMITER = ',' 
 RECORD_DELIMITER = '\n' 
 SKIP_HEADER = 1
 FIELD_OPTIONALLY_ENCLOSED_BY = '\042' 
 TRIM_SPACE = FALSE 
 ERROR_ON_COLUMN_COUNT_MISMATCH = FALSE
 ESCAPE = 'NONE' 
 ESCAPE_UNENCLOSED_FIELD = '\134' 
 DATE_FORMAT = 'mm/dd/yyyy' 
 TIMESTAMP_FORMAT = 'AUTO' 
 NULL_IF = ('NULL', 'null', '\N') 
 COMMENT = 'parse comma-delimited, double-quoted data';
  
  create or replace TABLE MY_PRACTICE_DB.PRACTICE.EMPLOYEE (
	EMPLOYEE_ID NUMBER(38,0),
	EMPLOYEE_NAME VARCHAR(500),
	DEPARTMENT VARCHAR(500),
	START_DATE  timestamp_tz,
	TITLE VARCHAR(500),
	MANAGER_ID NUMBER(38,0)
);


create or replace stage my_int_stage
     file_format = MY_CSV_FORMAT;
    -- copy_options = <copy_option>
    -- comment = '<comment>';
    
    
insert into MY_PRACTICE_DB.PRACTICE.EMPLOYEE 
values (12, 'Al Engemann', 'Engineering','2017-02-18 16:00:00', 'lead analyst', 9),
(4, 'Alana Robinson', 'Product','2001-03-06 16:00:00', 'Director Product', 1),
(2, 'Alissa Green', 'HR','2022-04-18 16:00:00', 'Head of HR', 7),
(14, 'Bryan Striet', 'Engineering','2022-07-18 16:00:00', 'analyst', 12),
(25, 'Carolyn Quelly', 'Engineering','2001-10-18 16:00:00', 'manager IT', 3),
(6, 'Casey Jo', 'Finance','2022-11-18 16:00:00', 'Head of Accounting', 7),
(24, 'Cassie Chernin', 'Finance','2022-12-18 16:00:00', 'accountant', 6),
(32, 'Farrell Tobo', 'Engineering','2017-01-18 16:00:00', 'data engineer', 15),
(30, 'Greg Lemiska', 'Product','2016-02-03 16:00:00', 'product manager', 4),
(3, 'Issa Nicolay', 'Engineering','2017-05-18 16:00:00', 'VP Engineering', 1),
(23, 'Jaimy Clark', 'HR','2018-07-18 16:00:00', 'director of people', 2),
(20, 'Jesus Lio', 'Engineering','2017-07-18 16:00:00', 'QA', 17),
(22, 'John Kim', 'HR','2017-08-18 16:00:00', 'recruiting lead', 2),
(19, 'Katelyn Inman', 'Engineering','2017-08-18 16:00:00', 'QA', 17),
(8, 'Kianna Moore', 'Product','2007-08-18 16:00:00', 'VP Product', 1),
(1, 'Kim Lord', 'Product','2017-09-18 16:00:00', 'CEO', 0),
(11, 'Kristy Cummins', 'Engineering','2017-05-18 16:00:00', 'BI manager', 9),
(5, 'Nate Graham', 'Product','2017-02-18 16:00:00', 'VP Design', 1),
(7, 'Nathan Polzin', 'Finance','2021-02-24 16:00:00', 'CFO', 1),
(26, 'Oleg Baranovsky', 'Engineering','2022-02-22 16:00:00', 'IT', 25),
(17, 'Patrick Putnam', 'Engineering','2017-03-18 16:00:00', 'lead QA', 3),
(15, 'Patrick Striet', 'Engineering','2017-08-18 16:00:00', 'lead engineer', 3),
(9, 'Peter Attardo', 'Engineering','2017-02-18 16:00:00', 'Director Analytics', 3),
(27, 'Ron Meltramo', 'Engineering','2018-08-28 16:00:00', 'IT', 25),
(28, 'Sara Strickland', 'HR','2005-02-14 16:00:00', 'recruiter', 22),
(31, 'Steve Lang', 'Product','2017-02-11 16:00:00', 'product manager', 4),
(29, 'Susan Chen', 'Product','2020-02-11 16:00:00', 'product manager', 4),
(16, 'Tracey Lipnicki', 'Engineering','2017-02-18 16:00:00', 'engineer', 15),
(18, 'Troy Donnell', 'Engineering','2015-02-18 16:00:00', 'QA', 17),
(10, 'Vitoria Pomberio', 'Engineering','2017-02-18 16:00:00', 'Bi analyst', 11),
(13, 'Wesley Thatcher', 'Engineering','2021-02-18 16:00:00', 'analyst', 12),
(21, 'Zach Gibson', 'Engineering','2019-02-18 16:00:00', 'data scientist', 9);

SELECT * FROM MY_PRACTICE_DB.PRACTICE.EMPLOYEE ;
```


![Image of recusive tree](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/Screen%20Shot%202020-07-12%20at%206.44.17%20PM.png)
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
![Alt Text](https://media.giphy.com/media/FcFs1TW9CMVyw/giphy.gif)
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
  - 
![Image of recusive tree](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/cte_without_recursive.png)
![Image of recusive tree](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/recursive_image.png)
![Image of recusive tree](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/connect_by.png)
![Image of recusive tree](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/connect_by_image.png)
![Image of recusive tree](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_05/IMAGES/fibi_query.png)

