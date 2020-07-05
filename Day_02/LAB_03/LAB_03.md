# Lab Three
## Caching and Concurrency

We just got done talking about Execution plans and Query profile.  Let's take a look at how Snowflake Caches your data so that you can reuse it.  

As we remember about Snowflake there are 3 layers 
![Image of 3 layers](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)


![Image of 3 layers](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)


![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/1%20create%20table.png)
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
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/2%20cach_select.png)
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
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/3%20detailsQP.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/31%20insert%20rows%20concurrency%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/32%20update%20after%20inserting%20.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/33%20select%20how%20many%20rows%20before%20update%20is%20complete.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/34%20update%20is%20complete%20concurrency.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/4%20profileQP.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/5%20QP%20run%20select%20cache%20CS.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/6%20QP%20cache%20CS.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/7%20compare%20first%20run.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/8%20fast%20return.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/9%20CS%20query%20reuse.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)



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
