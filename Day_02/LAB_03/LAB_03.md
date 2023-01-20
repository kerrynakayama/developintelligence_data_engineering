# Lab Three
## Caching Lab

We just got done talking about Execution plans and Query profile.  Let's take a look at the query profile and discuss how you can optimize your execution plans by thinking about how Snowflake Caches your Data.  Once we go over caching we will also do a walkthrough of how Snowflake keeps ACID in a concurrency Lab.   

### First thing is First who remembers the structure of Snowflake? 

There are 3 layers 
![Image of 3 layers](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)

Now that we see that let's set our selves up.  Let's create a data bases where we can play with some sample data and let's also go ahead and create a few virtual data warehouses.  
*Remember that Snowflake has roles so depending on which role you are using you will be able to see the DB's or use certain Data warehouses*

```sql

CREATE OR REPLACE warehouse lab3 WITH 
warehouse_size ='X-SMALL'
auto_suspend = 180
auto_resume = true
initially_suspended=true;

```
```sql

CREATE OR REPLACE warehouse lab3 WITH 
warehouse_size ='LARGE'
auto_suspend = 900
auto_resume = true
initially_suspended=true;

```
It should automatically assign you the Data Warehouse that you just created but if not go ahead and assign it to yourself and pick the USAA Demo DB That we created too. 

 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/1%20create%20table.png)

 
```sql

CREATE OR REPLACE TRANSIENT TABLE SUPPLIER AS SELECT * FROM SNOWFLAKE_SAMPLE_DATA.TPCH_SF1000.SUPPLIER;

```
 
```sql

CREATE OR REPLACE TRANSIENT TABLE SUPPLIER2 CLONE SUPPLIER;

```
![Image of copy data over](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%20update%20before%20update.png) 

 
 Let's go ahead and SELECT * FROM the data 
 
 ```sql

SELECT * FROM SUPPLIER;

```

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/2%20cach_select.png) 
*When I was first doing this lab I selected all from the sample data but it should not matter where you selet your data from* 


Let's take a look at the Query Profile to see this in action.  
click on the history tab and then click on the query that you just ran.
The Query Profile will open up to the Details page.  

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/3%20detailsQP.png)

Go ahead and click on the Profile tab a the top so that we can see all the processes running.  

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/4%20profileQP.png)

We can see the Local disk and remote disk io both being used. 
we can see the scan progress, bytes scanned, 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/8%20fast%20return.png)

0 bytes have been scanned from the cache

your pruning of the partitions scanned.  We will go more indetail about this in lab 7 when we do micro partitions and clustering

Ok so it takes about 1 and half minutes to run 

great let's run it again! 

![Alt Text](https://media.giphy.com/media/dzvcOaG7QsRd6/giphy.gif)

So it basically runs instantly!!!  

![Alt Text](https://media.giphy.com/media/SCJwTkBLSr5kI/giphy.gif)

![Image of Fast return](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/8%20fast%20return.png)

Ok let's go back to the Query profile and take a look at what's going on.  

![Image of Query reuse](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/5%20QP%20run%20select%20cache%20CS.png)
![Image of Query reuse zoom](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/9%20CS%20query%20reuse.png)
So it was a Query reuse result.  
Which is why it ran so fast.

### Who was expecting it to use the Cache like we saw earlier?  
We can go back and take a look at the older query profile just by clicking on the query 

![Image of QP of quick cach result](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/6%20QP%20cache%20CS.png)

### what do you think is going on?

Let's take a closer look at the History tab.  If we pull it out we can see more detail like rows scanned, bytes scanned and clusters.
![Image of no cluster used](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/10%20CS%20query%20reuse%20show%20Cluster.png)

Just as I expected there were no clusters used in the this query. 

What happened here is that our Cloud Services Layer also has a cache.  And running the query saves the results here while the VWH is stil alive 

Ok so let's turn this off 
![Image of trun of cach](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/11%20Turn%20off%20CS%20cache.png)

 ```sql

ALTER SESSION SET USE_CACHED_RESULT = false;

```

*make sure at least 180 seconds have passed and the Cluster has suto suspended itself* 

Ok so let's run this again
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/12%20rerun%20table%20with%20CS%20off.png)

![Alt Text](https://media.giphy.com/media/toB3AnUDkqE3GENKx0/giphy.gif)

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/12%20original%20query.png)

Wait a minute why did it still pull from the Data storage? 
Also it took pretty much as long as the first time we pulled it as it did this time

### What is happening here?

Well it looks like the VWH Auto suspended and now it has to pull the data back from the Storage layer. 
![Image of 3 layers](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)

Ok well let's not wait long this time and run the query again before the VWH auto suspends 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/13%20no%20local%20disk%20using%20vwh%20Cache.png)

We can see this time there is no remotes disk and it is 100% form the scanned cache.  
We can also see the local disk io is 0%

### Ok so how long did it take?

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/14%20still%20takes%20a%20while%20to%20run.png)

Woah nearly as long as  the first time we ran it.  So the Cache stops us from pulling from the storge but it still needs to process form the VWH and return to the services layer. 

So this Cache is helpful but can still be expensive! 

Ok! let' turn the Services layer Caching back on. 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/15%20VWH%20is%20off%20.png)

 ```sql

ALTER SESSION SET USE_CACHED_RESULT = true

```

Let's dig into this a bit more.  
You all might have some questions like...
### Will Snowflake still use the result cache if it is updated?

Let me update the table by deleting one row from it. 

![Image of deleting one row](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%20update%20before%20update.png)

We can see in the Query Profile that the row was deleted.

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/18%20start%20concurrency%20delete%20file.png)

Now let's run the SELECT * again. So since we update the table it now has to pull it once again from the Storage layer. It can no longer pull it from the cloud services cache.  

If there are any changes Snowflake will not use the Cached results.
Run all changes and update first before running select queries to check work.  This will get expensive! 

![Alt Text](https://media.giphy.com/media/BI2fB0DCdz2lG/giphy.gif) 


### Will it be shared across users? 

Let's run the query in the original user
We can see that it is using the cloud services cache since it returned almost instantly
Now let't switch roles and Data warehouses and run the same exact query.  

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%20running%20same%20VWH.png)

It too returns instantly since it is cached.  So even if users are runnning different warehouses as long as they run the exact query it will still be cached.  

### How long will the results be cached if the DWH doesn't auto suspend?

Result should last for 24 hours.  Think how this might be useful for reporting that is used in Tableau

## Concurrency Lab 

Ok let's take the table and run an update statement and then run a second update in another user.  

First update... 

 ```sql

UPDATE SUPPLIER2
SET S_NATIONKEY = 1200
WHERE S_NATIONKEY = 12


```
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%20update%20before%20update.png)

Second update using a different user and WH... 

 ```sql

UPDATE SUPPLIER
SET S_NATIONKEY = 1300
WHERE S_NATIONKEY = 12;


```

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/22%20update%20blocked.png)


Ok we can see the first one running and the second one is being blocked 

If we were to kill the first one the second one will start running since it is no longer being blocked.

So we can see that the first one was never completed so only the second one was able to run. 

Now let's try deleting the table.  

Let's run another update... 

 ```sql

UPDATE SUPPLIER2
SET S_NATIONKEY = 12
WHERE S_NATIONKEY = 1300


```
And let's delete the table from the other user. 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/23%20delete%20table%20.png)


 ```sql

DROP TABLE SUPPLIER2


```
Let's go back and check out the first user.  

Well it looks like it is still running...  

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/24%20update%20running%20but%20will%20fail.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%20QP%20update%20happening.png)


It can take some time before it let's the user know the table is dropped.  

Ok there we can see the Query failed.. 
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%20QP%20update%20failed%20.png)

We go back to the worksheet and we can see that it failed. 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%20query%20failed.png)


So let's go back and recreate the table and run a SELECT * from the first user rather than an update statement.

We are just reading from the table 

Now in the second user let's delete the table again.

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/28%20delete%20after%20running%20read%20.png)
### Do we expect this query to fail? 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/27%20read%20after%20delete.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/29%20read%20works%20.png)
No it doesn't since it is just a read and it was run before the delete happened.  

Ok last one let's insert some rows into the table after we update the table.  

First step is to update the table

 ```sql

UPDATE SUPPLIER2
SET S_NATIONKEY = 1300
WHERE S_NATIONKEY = 12


```

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/32%20update%20after%20inserting%20.png)

Second step will be to insert some rows
 ```sql

INSERT INTO SUPPLIER2
SELECT * FROM SNOWFLAKE_SAMPLE_DATA.TPCH_SF10000.SUPPLIER WHERE S_NATIONKEY = 12 LIMIT 100 


```

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/31%20insert%20rows%20concurrency%20.png)

Ok this was successful 

Now let's take a look at the table

The first time we select all the table is not done updating all the rows and over 4 million are there with nation key 12 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/33%20select%20how%20many%20rows%20before%20update%20is%20complete.png)

Now we will run it after both update are done 

![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/34%20update%20is%20complete%20concurrency.png)

There are only 100 rows now with nation key 12 just as we were expecting after the updates.  

### Why didn't the extra 100 rows get updated even though they were in there before the first update finished?? 

### Ok So what did we Learn??? 


![Alt Text](https://media.giphy.com/media/QyKwWQzeOSl9e/giphy.gif) 
