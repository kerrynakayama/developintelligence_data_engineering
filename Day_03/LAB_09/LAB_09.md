
# LAB 9 

### LAST LAB!!!! 

![Alt Text](https://media.giphy.com/media/Zd6BEIy9xmcwRH5jNJ/giphy.gif) 


### Unless you are doing the learning Spikes... 


![Alt Text](https://media.giphy.com/media/RSL4AGXEfy19K/giphy.gif) 


# But really let's get this Lab done! 

![Alt Text](https://media.giphy.com/media/l2R06WPHU4ae0H4LC/giphy.gif) 
## Create at least 5 test on the data


![Alt Text](https://media.giphy.com/media/6276KRcJ7tTNrXOi5E/giphy.gif)
## Mock up a YAML file that will start to order the creation of these tables

![Alt Text](https://media.giphy.com/media/ErdfMetILIMko/giphy.gif) 

## Use Jinja to remove a CASE WHEN loop on the data (*hint payment type makes an easy casewhen statement*) 

![Alt Text](https://media.giphy.com/media/l1J9LMNeWISnddECA/giphy.gif)
## Create at least one macro

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
