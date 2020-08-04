# Let's take the Transactional Data From Yesterday and think through our ERD that we created.  
#### The transactions are event based on everytime an invoice is created.  It will contain ...
- Invoice id
- Stock code of the item that is on the invoice
- description of the stock item **hint - check through what these might be first as they might point out errors in the data **
- Quantity - number of stock items being purchased ... or something else 
- invoice data - date the invoice came in (looks like it comes in daily without a timestampt - does this affect your design)
- Price - price of the individual items 
- Customer id - id of the customer who bought something 
- payment - how did the customer pay for this invoice 
- user_agent - tons of data in this - tells what device they used - ** hint - might give more information about who this user is **
- customer data - array format that has more info about the user
- seller id - id of the person who sold the items to the customer 
- country the transaction took place
- 
- 
![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/Screen%20Shot%202020-07-05%20at%203.53.37%20PM.png)



-
-
-

## Take one Fact Table (It does not need to be the most complicated) And Write a simple CTE on how to create that table 

![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/erd.jpg)
![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/erd2.png)

### You will Use this tomorrow in dbt so I thought it would be helpful to get a head start.  
![dbt_github](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_03/LAB_08/IMAGES/dbt1.png)
