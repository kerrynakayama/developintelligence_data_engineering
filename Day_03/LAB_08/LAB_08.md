# Lab 08 
## Transformations with dbt

### Take the data from LAB 1 and let's actually design some of the items we discussed 


![Alt Text](https://media.giphy.com/media/cvbGraL8NJJZu/giphy.gif) 
-
-
-
## I have connected you all with dbt.  
### You can choose to use the tool for this lab but if you choose not to it will not affect your ability to complete.  
### Understanding the usability of what the tool helps is the most important aspect of this lab. 
-
# The idea of this Lab is to figure out how you will use the funtionality of dbt to benefit you with the data that we have been working on all week
 - 
 ![Alt Text](https://media.giphy.com/media/Zsx8ZwmX3ajny/giphy.gif)
  
   
### If you choose to click on the link you will get about 20+ emails from dbt... again you do not need dbt to do this lab and understand how it will work.  
 
 
![Alt Text](https://media.giphy.com/media/Q87XzlKSuHqnT2FEHE/giphy.gif)

-
-
-
-
-
### Let's do a basic run through of the setup 
-
-
-
#### When you first login if this the first time running dbt it will ask you to create a project.  If you login from the link I sent then most of the setup is already complete.  You will only have to add the loging credentials to your DB 
 
 
![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/newproject.png)

#### once you pick you project it will take you directly to the DB connector screen.  Choose the correct DB to work with.  dbt has a tutorial that connects you to Big Query.  I set you all up with Snowflake since that is the Data warehouse you will be working with.  

![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/database.png)

#### This information should already be ready to go except your personal credentials
#### If you were to set this up from scratch you would need to add the 
- account 
- role 
- database
- warehouse you want dbt to run on 
- and you can choose to keep alive (you will use this if you have some sort of scheduler that runs later)
- 

![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/database2.png)

#### Use your user name and new password that you all should have set up for Snowflake.  

![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/database3.png)

#### dbt creates a schema that it will put your tables and/or views into.  The default is dbt_username.  For this lab I have the teams setup to put it into dbt_staging.  


![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/repos.png)

#### The Repo set up gives you 3 options 
- managed dbt repository 
- github repository 
- or another git repository that you give the URL 

#### I created a repository for each group in github for the lab.  It should have read and write privileges from dbt set up by connecting the 3rd party connection.

![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/init.png)

#### Be sure that when you are setting up the repository that you do not initialize it with a readme or .gitignore.  dbt has its own init that will create all the folders that you will need to start out with.  More can be added or deleted. We will go over them in a few mins.

![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/init2.png) 


 
 
### Let's take a look at what happens after you run init and you connect the data source and git repository 
-
-
-
-
So this is what it will look like... 

![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/dbt1.png)

-
-
-
You will get a bunch of folders created in with .gitkeep files in them.  
-
### *What is .gitkeep????* 

-
-

![Alt Text](https://media.giphy.com/media/5wWf7HfQJzA8cze6CWc/giphy.gif)

-
 
**Basically it is just a place holder since git repositories do not allow for empty folders**
- 
- analysis 
- data
- dbt_modules
- logs
- macros
- models 
- snapshos
- targets
- tests 

Analysis is where you can store analytics scrips related to reporting but you should know that it will not get run.  It get's compiled to SQL querries that you can run in the dbt cloud IDE or another IDE 
-
Data is for store small mapping tables that you might use but don't have already in your data warehouse though most data should already be in your data warhouse
- 
dbt modules is where you will store all your SQL queries that will be used to create the structure
- 
Macros will be lists of functions that will be used to aid you in the creation of your projects 
- 
Models is where you will store all your SQL queries that will be used to create the structure
- 
Snapshots are versions of older SQL code or models that you would like to save so that you can go back and access 
- 
Targets are where all your compiled SQL models are put into actual SQL scripts that are run in the database when you use the dbt command "dbt run"
- 
Tests are going to be the items that you set up whether they are Schema tests or data tests 
- 
dbt.yml will be where you can find your dbt.yml files.  These will basically be your config files that will be used to create the order of your models and tests.  There is also a section called models in the yml file that dictates whether the item being created in your data warehouse will be a materialized views or table.  This can also be over written from code directly in your model
- 

![Alt Text](https://media.giphy.com/media/40DRc0W00UbgQ/giphy.gif) 

-
 
dbt requires raw data to be in database
-
 

## First create a basic CTE script that breaks up your large transaction table into at least one table of your erd models.
- 

If we were going to do this in dbt we would create a new model in the models folder with a .sql file type and copy or completely write the script into this file
-

Now as much as I would like you all to get a perfectly working script ... I'm more interested in the idea of understanding how dbt works.  So for the bare minimum mock up a CTE with at least 4 staging tables to ultimately build one table based on joins at the end of your CTE. 
-



## Now that we have created the first model let's create refferences back to this model
 

![Alt Text](https://media.giphy.com/media/e2E5DEXDNt280/giphy.gif)

Take some of your intermediate staging tables and create a new file with that staging table name as the file.  Copy the select statement into this new .sql file and remove the orginal select from your CTE.  Now you are still refferencing this in your CTE so you will need to create a refference back to the new .sql file.  Write SELECT * FROM using the double curly brackets with the word ref parenthesis and the name of your new model.  Close this out with it's matching paranthesis and double brackets.  
-

## You have just created a staged model! 
 
![Alt Text](https://media.giphy.com/media/kbQsZIE1OnBgmihgCl/giphy.gif) 
 
Now you can use this piece of code anywhere else in any of your other models.  The benefit is that it only runs one time which saves on resources when you get much more complicated models.  You can also share these across teams as long as it is clearly documented.  
- 


```sql

create or replace sequence seq_cust start = 1 increment = 1;
create or replace sequence seq_pay start = 1 increment = 1;
create or replace sequence seq_geo start = 1 increment = 1;

WITH TOTAL_CUSTOMERS AS 
(SELECT customer_id, payment,
SPLIT_PART(TRIM(GET(CUSTOMER_DATA,0),'"'),':',2) AS surname,
SPLIT_PART(TRIM(GET(CUSTOMER_DATA,3),'"'),':',2) AS Age,
SPLIT_PART(TRIM(GET(CUSTOMER_DATA,1),'"'),':',2) AS GEO,
SPLIT_PART(TRIM(GET(CUSTOMER_DATA,2),'"'),':',2) AS Gender,
SPLIT_PART(TRIM(GET(CUSTOMER_DATA,4),'"'),':',2) AS Tenure,
SPLIT_PART(TRIM(GET(CUSTOMER_DATA,5),'"'),':',2) AS Has_Save_card,  
SPLIT_PART(TRIM(GET(CUSTOMER_DATA,6),'"'),':',2) AS Is_member  
FROM "USAA_DEMOS"."RAWDATA"."TRANSACTIONS2" 
GROUP BY customer_id, payment, surname, age, geo, gender, tenure, has_save_card, is_member),

PAYMENT AS 
(SELECT seq_pay.nextval AS payment_id, payment
FROM 
(SELECT payment FROM "USAA_DEMOS"."RAWDATA"."TRANSACTIONS2" GROUP BY PAYMENT)),

GEOS AS 
(SELECT seq_geo.nextval AS geo_id, geo
FROM 
(SELECT DISTINCT GEO
FROM 
(SELECT SPLIT_PART(TRIM(GET(CUSTOMER_DATA,1),'"'),':',2) AS GEO FROM "USAA_DEMOS"."RAWDATA"."TRANSACTIONS2" GROUP BY GEO
UNION 
SELECT COUNTRY AS GEO FROM "USAA_DEMOS"."RAWDATA"."TRANSACTIONS2" GROUP BY GEO))),

TOTAL_CUSTOMER_NORMAL AS 
(SELECT customer_id, payment_id,TC.surname,TC.Age,GEO_id,TC.Gender,TC.Tenure,TC.Has_Save_card,TC.Is_member
FROM TOTAL_CUSTOMERS TC 
LEFT JOIN PAYMENT P 
ON TC.payment = P.payment 
LEFT JOIN GEOS G
ON TC.GEO = G.GEO),

INSTORE_CUSTOMERS AS 
(SELECT seq_cust.nextval AS customer_id,'INSTORE' AS Type, payment_id,surname,Age,GEO_ID,Gender,Tenure,Has_Save_card,Is_member
FROM TOTAL_CUSTOMER_NORMAL TCN
WHERE CUSTOMER_ID IS NULL),

ONLINE_CUSTOMERS AS 
(SELECT customer_id, 'ONLINE' AS Type, payment_id,surname,Age,GEO_ID,Gender,Tenure,Has_Save_card,Is_member
FROM TOTAL_CUSTOMER_NORMAL TCN
WHERE CUSTOMER_ID IS NOT NULL)

SELECT customer_id, Type, payment_id,surname,Age,GEO_ID,Gender,Tenure,Has_Save_card,Is_member
FROM INSTORE_CUSTOMERS
UNION ALL 
SELECT customer_id, Type, payment_id,surname,Age,GEO_ID,Gender,Tenure,Has_Save_card,Is_member
FROM ONLINE_CUSTOMERS
;
```

