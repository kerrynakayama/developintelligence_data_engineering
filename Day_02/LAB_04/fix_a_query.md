![Alt Text](https://media.giphy.com/media/IjD2bKEIiyLfi/giphy.gif) 
 
# Fix the Query

![Alt Text](https://media.giphy.com/media/l2SpZQK6yiANOohag/giphy.gif) 
 
![Alt Text](https://media.giphy.com/media/h4rLmOP2F6xh0bhYkF/giphy.gif)
 
  
## Answer these questions without using a select all
 
### Count how many rows are Approximately in this data set
 
### Sample the data to view what the data looks like
 
### You only want to see what comment and account balance columns look like
 

```sql

SELECT * FROM SNOWFLAKE_SAMPLE_DATA.TPCH_SF10000.SUPPLIER


```

 
## Bad Aliases
 
### Remember the show Alias... Nahhh Ok.. Well let's fix the bad ones anyways in this Query  


![Alt Text](https://media.giphy.com/media/KDnjGMh6CgTwNQeLNu/giphy.gif) 
![Alt Text](https://media.giphy.com/media/j2GPmQRh2A8yHMMX3Q/giphy.gif)
 
 
 ```sql

SELECT * 
FROM 
"SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."LINEITEM" A 
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."ORDERS" B
ON A.L_ORDERKEY = B.O_ORDERKEY
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."PART" C
ON A.L_PARTKEY = C.P_PARTKEY
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."SUPPLIER" D
ON A.L_SUPPKEY = D.S_SUPPKEY

```
 
![Alt Text](https://media.giphy.com/media/GiP2uv99mMCoo/giphy.gif) 

 
## There are too many joins 
 
### Not only were the Aliases bad but there is Too much is going on in this Query.  Break it up and remove some of the joins 
 
![Alt Text](https://media.giphy.com/media/Yq7n1sakPfO1d0O9Wn/giphy.gif) 
 
 ### To answer the following question do you need all these joins...
 ### How many items were supplied by the United States that were shipped by regular air in 1998 
  
```sql

SELECT * 
FROM 
"SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."LINEITEM" A 
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."ORDERS" B
ON A.L_ORDERKEY = B.O_ORDERKEY
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."PART" C
ON A.L_PARTKEY = C.P_PARTKEY
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."SUPPLIER" D
ON A.L_SUPPKEY = D.S_SUPPKEY

```
### Wait let's make the question a bit harder by adding one more level to it...
### How many items were supplied by the United States that were shipped by regular air in 1998 and the total order price was over 50K dollars
 
 ```sql

SELECT * 
FROM 
"SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."LINEITEM" A 
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."ORDERS" B
ON A.L_ORDERKEY = B.O_ORDERKEY
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."PART" C
ON A.L_PARTKEY = C.P_PARTKEY
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."SUPPLIER" D
ON A.L_SUPPKEY = D.S_SUPPKEY
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."NATION" F
ON A.S_NATIONKEY = F.N_NATIONKEY
```
 
 
## Order and Group by 
 
### What are you trying to order this Query by what???
 
![Alt Text](https://media.giphy.com/media/l2AYpTe5FOOMkBZ3I3/giphy.gif) 
 
### order nations descending (z-a) alphabetically 
```sql

SELECT * FROM 
"SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."NATION" F

```



   
![Alt Text](https://media.giphy.com/media/krM6ANSNvFg52/giphy.gif) 
 
### Count how many suppliers are from each nation and order it ascending  (a-z) alphabetically by nation 
```sql

SELECT * 
FROM 
"SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."SUPPLIER" D
ON A.L_SUPPKEY = D.S_SUPPKEY
LEFT JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF10000"."NATION" F
ON A.S_NATIONKEY = F.N_NATIONKEY
GROUP BY 4 
ORDER BY 9
```

![Alt Text](https://media.giphy.com/media/h6g5FA1zhKswcRsEi7/giphy.gif)
 
 
## Switch from Sub query to CTE format 

```sql

SELECT NVL(H.platform_name,T.platform_name) AS platform_name,
NVL(H.country_tier, T.country_tier) AS country_tier,
NVL(H.year_month,T.year_month) AS year_month, 
NVL(H.ad_type,T.ad_type) AS ad_type, 
NVL(H.ad_eligible,T.ad_eligible) AS ad_eligible, 
T.days_active,T.dau, T.ad_revenue, T.displays, T.snapshot_date, T.product_id,
NVL(H.LTR+T.ad_revenue,T.ad_revenue) AS LTR,
NVL(H.LT_dau+T.dau,T.dau) AS LT_dau,
NVL(H.LT_imp+T.displays,T.displays) AS LT_imp
FROM
(SELECT drcc.platform_name, drcc.country_tier, drcc.year_month, drcc.ad_type,
drcc.days_active, drcc.dau, drcc.ad_revenue, drcc.displays,
drcc.snapshot_date, drcc.product_id, drcc.ad_eligible,
drcc.LTR, drcc.LT_dau, drcc.LT_imp
FROM 
(SELECT 
platform_name,country_tier,year_month,ad_type,days_active,
dau,ad_revenue,displays,snapshot_date,product_id,ad_eligible,
LTR,LT_dau, LT_imp
FROM rpt_ads.daily_revenue_cohort_cumulative3
WHERE snapshot_date < DATE_SUB('${ds}',1)) drcc
INNER JOIN 
(SELECT 
platform_name, country_tier, ad_type, product_id,
ad_eligible, MAX(snapshot_date) as last_date
FROM rpt_ads.daily_revenue_cohort_cumulative3
WHERE snapshot_date < DATE_SUB('${ds}',1)
GROUP BY platform_name, country_tier, ad_type,
product_id, ad_eligible) drcc2 
ON drcc2.last_date = drcc.snapshot_date
AND drcc2.platform_name = drcc.platform_name
AND drcc2.country_tier = drcc.country_tier
AND drcc2.ad_type = drcc.ad_type
AND drcc2.product_id = drcc.product_id
AND drcc2.ad_eligible = drcc.ad_eligible) H
--insert old table
RIGHT OUTER JOIN
(SELECT 
platform_name,country_tier,year_month,ad_type,days_active,
ad_eligible, dau,ad_revenue,displays,snapshot_date,product_id
FROM rpt_ads.daily_revenue_cohort2
WHERE snapshot_date = '${ds}') T
ON H.platform_name = T.platform_name
AND H.country_tier = T.country_tier
AND H.year_month = T.year_month
AND H.ad_type = T.ad_type
AND H.product_id = T.product_id
AND H.ad_eligible = T.ad_eligible
AND H.snapshot_date <= DATE_SUB(T.snapshot_date,1)
AND H.days_active <= T.days_active-1

```


![Alt Text](https://media.giphy.com/media/FVMz65wDVfC2Q/giphy.gif)


