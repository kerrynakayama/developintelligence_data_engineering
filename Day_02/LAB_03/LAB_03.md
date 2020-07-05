# Lab Three
## Caching and Concurrency

We just got done talking about Execution plans and Query profile.  Let's take a look at the query profile and discuss how you can optimize your execution plans by thinking about how Snowflake Caches your Data.  Once we do a walk through of this we willl also do a walkthrough of how Snowflake keeps ACID in a concurrency Lab.   

### First thing is First who remembers the structure of Snowflake? 

There are 3 layers 
![Image of 3 layers](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)

Now that we see that let's set our selves up.  Let's create a data bases where we can play with some sample data and let's also go ahead and create a few virtual data warehouses.  
*Remember that Snowflake has roles so depending on which role you are using you will be able to see the DB's or use certain Data warehouses*

```sql

CREATE OR REPLACE warehouse usaa_1 WITH 
warehouse_size ='X-SMALL'
auto_suspend = 180
auto_resume = true
initially_suspended=true;

```
```sql

CREATE OR REPLACE warehouse usaa_1 WITH 
warehouse_size ='LARGE'
auto_suspend = 900
auto_resume = true
initially_suspended=true;

```
It should automatically assign you the Data Warehouse that you just created but if not go ahead and assign it to yourself and pick the USAA Demo DB That we created too. 

 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/1%20create%20table.png)

 
```sql

CREATE OR REPLACE TRANSIENT TABLE SUPPLIER AS SELECT * FROM SNOWFLAKE_SAMPLE_DATA.TPCH_SF10000.SUPPLIER;

```
 
```sql

CREATE OR REPLACE TRANSIENT TABLE SUPPLIER2 CLONE SUPPLIER;

```

 
 Let's go ahead and SELECT "*" FROM the data 
 
 ```sql

SELECT * FROM SUPPLIER;

```
 
*When I was first doing this lab I selected all from the sample data but it should not matter where you selet your data from* 

Let's take a look at the Query Profile to see this in action.  
click on the history tab and then click on the query that you just ran.
The Query Profile will open up to the Details page.  

![Image of copy data over](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%20update%20before%20update.png) 

Go ahead and click on the Profile tab a the top so that we can see all the processes running.  

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/2%20cach_select.png)

We can see the Local disk and remote disk io both being used. 
we can see the scan progress, bytes scanned, 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/3%20detailsQP.png)

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/4%20profileQP.png)

0 bytes have been scanned from the cache

your pruning of the partitions scanned.  We will go more indetail about this in lab 7 when we do micro partitions and clustering

Ok so it takes about 1 and half minutes to run 

great let's run it again! 

![Alt Text](https://media.giphy.com/media/dzvcOaG7QsRd6/giphy.gif)


![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/5%20QP%20run%20select%20cache%20CS.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/6%20QP%20cache%20CS.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/7%20compare%20first%20run.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/8%20fast%20return.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/9%20CS%20query%20reuse.png)

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/10%20CS%20query%20reuse%20show%20Cluster.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/11%20Turn%20off%20CS%20cache.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/12%20original%20query.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/12%20rerun%20table%20with%20CS%20off.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/13%20no%20local%20disk%20using%20vwh%20Cache.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/14%20still%20takes%20a%20while%20to%20run.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/15%20VWH%20is%20off%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/16%20QH%20show%20clusters.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/16%20QP%20VWH%20cache.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/17%20QP%20100cache%20VWH.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/18%20start%20concurrency%20delete%20file.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/19%20Select%20from%20table%20read%20.png)

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%20running%20same%20VWH.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%20update%20before%20update.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/21%20update%20after%20update.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/22%20update%20blocked.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/23%20delete%20table%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/24%20update%20running%20but%20will%20fail.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/25%20update%20running%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%20QP%20update%20failed%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%20QP%20update%20happening.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%20query%20failed.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%20update%20failed.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/27%20query%20fail.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/27%20read%20after%20delete.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/28%20delete%20after%20running%20read%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/29%20read%20works%20.png)

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/31%20insert%20rows%20concurrency%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/32%20update%20after%20inserting%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/33%20select%20how%20many%20rows%20before%20update%20is%20complete.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/34%20update%20is%20complete%20concurrency.png)

### Let's create some tables
We will take data that Snowflake already has in the **Snowflake_sample_data**
First we need to take a look at what the data looks like
*it is very important to understand the data before you build a table*
Right click on the table **Customers** and choose *preview data*
This is what it will look like
![Image of data in the customer table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_07/IMAGES/1_describe_table.png)
We see: 
- customerkey 
- name (which is just the custkey with customer in front of it) 
- address
- nation key
- phone number 
- account balance
- marketing segemnt 
- comments 

**Question** 
At first glance what might be a good column to cluster the data on?

![Image of creating the a table without clusters](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_07/IMAGES/2_create_nocluster_table.png)
This the SQL code 
```sql
