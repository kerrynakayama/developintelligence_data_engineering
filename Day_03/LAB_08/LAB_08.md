# Lab 08 
## Transformations with dbt

### Take the data from LAB 1 and let's actually design some of the items we discussed 


![Alt Text](https://media.giphy.com/media/cvbGraL8NJJZu/giphy.gif) 
-
-
-
### I have connected you all with dbt.  You can choose to use the tool for this lab but if you choose not to it will not affect your ability to complete.  Understanding the usability of what the tool helps is the most important aspect of this lab. 
## The idea of this Lab is to figure out how you will use the funtionality of dbt to benefit you with the data that we have been working on all week
### First let's take a look at what happens when you run init after you connect the data source and git repository 
This can be done via dbt cloud which is pretty much laid out for you as you walk through dbt 
You can also do it through the command line interface (CLI) 
-
-
-
So this is what it will look like... 

![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/dbt1.png)

-
-
-
You will get a bunch of folders created in with .gitkeep files in them.  
** What is .gitkeep???? ** 

-
-
-
Basically it is just a place holder since git repositories do not allow for empty folders
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
Data is for store small mapping tables that you might use but don't have already in your data warehouse 
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


dbt requires raw data to be in database


#### First create a basic CTE script that breaks up your large transaction table into at least one table of your erd models.
- 

If we were going to do this in dbt we would create a new model in the models folder with a .sql file type and copy or completely write the script into this file
-

Now as much as I would like you all to get a perfectly working script ... I'm more interested in the idea of understanding how dbt works.  So for the bare minimum mock up a CTE with at least 4 staging tables to ultimately build one table based on joins at the end of your CTE. 

#### Now that we have created the first model let's create refferences back to this model

Take some of your intermediate staging tables and create a new file with that staging table name as the file.  Copy the select statement into this new .sql file and remove the orginal select from your CTE.  Now you are still refferencing this in your CTE so you will need to create a refference back to the new .sql file.  Write SELECT * FROM using the double curly brackets with the word ref parenthesis and the name of your new model.  Close this out with it's matching paranthesis and double brackets.  
-

#### You have just created a staged model! 

Now you can use this piece of code anywhere else in any of your other models.  The benefit is that it only runs one time which saves on resources when you get much more complicated models.  You can also share these across teams as long as it is clearly documented.  
- 




