# Lab Three
## Caching and Concurrency

We just got done talking about Execution plans and Query profile.  Let's take a look at how Snowflake Caches your data so that you can reuse it.  

As we remember about Snowflake there are 3 layers 
![Image of 3 layers](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)


![Image of 3 layers](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%20Shot%202020-05-22%20at%2012.42.55%20PM.png)


![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/1%create%table.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/10%CS%query%reuse%show%Cluster.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/11%Turn%off%CS%cache.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/12%original%query.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/12%rerun%table%with%CS%off.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/13%no%local%disk%using%vwh%Cache.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/14%still%takes%a%while%to%run.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/15%VWH%is%off%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/16%QH%show%clusters.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/16%QP%VWH%cache.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/17%QP%100cache%VWH.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/18%start%concurrency%delete%file.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/19%Select%from%table%read%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/2%cach_select.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%running%same%VWH.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/20%update%before%update.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/21%update%after%update.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/22%update%blocked.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/23%delete%table%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/24%update%running%but%will%fail.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/25%update%running%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%QP%update%failed%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%QP%update%happening.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%query%failed.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/26%update%failed.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/27%query%fail.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/27%read%after%delete.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/28%delete%after%running%read%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/29%read%works%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/3%detailsQP.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/31%insert%rows%concurrency%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/32%update%after%inserting%.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/33%select%how%many%rows%before%update%is%complete.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/34%update%is%complete%concurrency.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/4%profileQP.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/5%QP%run%select%cache%CS.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/6%QP%cache%CS.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/7%compare%first%run.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/8%fast%return.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/9%CS%query%reuse.png)
![Image of create table](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_02/LAB_03/Images/Screen%Shot%2020-05-22%at%12.42.55%PM.png)




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
