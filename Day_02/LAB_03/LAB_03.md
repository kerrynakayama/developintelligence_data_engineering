# Lab Three
## Caching and Concurrency

We just got done talking about Execution plans and Query profile.  Let's take a look at how Snowflake Caches your data so that you can reuse it.  

As we remember about snow flake there are 3 layers 
![Image of 3 layers](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)


The table consists of 24 rows stored across 4 micro-partitions, with the rows divided equally between each micro-partition. Within each micro-partition, the data is sorted and stored by column, which allows Snowflake to prune by:
1. micro-partition fisrt 
2. columns second 

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
