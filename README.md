# I. Introduction
Utilizing SQL in _**BigQuery**_ for optimized data management within the _**E-commerce sector**_, integrated data from multiple worksheets to generate comprehensive and accurate sales reports. This approach ensures efficient analysis and precise insights into the E-commerce landscape.
# II. Requirements
1. Google Cloud Platform account
2.	Project on Google Cloud Platform
3.	Google BigQuery API enabled
4.	SQL query editor or IDE
# III. Dataset Access
1.	Sign in to your Google Cloud Platform account and create a new project.
2.	Go to the BigQuery console and choose your newly created project.
3.	In the navigation menu, click on 'Add Data' and select 'Search a project.'
4.	Enter the project ID bigquery-public-data.google_analytics_sample.ga_sessions and press Enter.
5.	Open the 'ga_sessions_' table by clicking on it."
# IV. Exploring the Dataset
**Query 01: Calculate total visit, pageview, transaction for Jan, Feb and March 2017 (order by month)** 

_**•	SQL code**_

 ```SQL
select 
      format_date("%Y%m", PARSE_DATE("%Y%m%d", date)) as month
      ,count(totals.visits) as visits
      ,sum(totals.pageviews) as pageviews
      ,sum(totals.transactions) as transactions
from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
where _table_suffix between '0101' and '0331'
group by month
order by month;
```

 
_**•	Query result**_

![image](https://github.com/user-attachments/assets/1db281c9-4f45-4342-9604-4ed615538dee)

 
**Query 02: Bounce rate per traffic source in July 2017 (Bounce_rate = num_bounce/total_visit) (order by total_visit DESC)**

_**•	SQL code**_

```SQL
select 
      trafficSource.source as source
      ,count(totals.visits) as total_visits
      ,count(case when totals.bounces is not null then 1 
                         else null end) as total_no_of_bounces
      ,round(100*count(case when totals.bounces is not null then 1 
                         else null end)/count(totals.visits),2) as bounce_rate
from `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
group by source
order by total_visits desc;
```

 
_**•	Query result**_

![image](https://github.com/user-attachments/assets/9303e1a8-374f-477a-bb22-ab4b9e37d057)

 
**Query 3: Revenue by traffic source by week, by month in June 2017**

_**•	SQL code**_

```SQL
with data_June_month as (
  select
        'month' as time_type
        ,format_date("%Y%m", PARSE_DATE("%Y%m%d", date)) as time
        ,trafficSource.source as source
        ,sum(productRevenue)/1000000 as revenue
  from `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
  unnest (hits) hits,
  unnest (hits.product) product
  where productRevenue is not null
  group by time_type, time , source
  order by time, source, revenue desc
),
    data_June_day as (
      select
        'week' as time_type
        ,format_date("%Y%W", PARSE_DATE("%Y%m%d", date)) as time
        ,trafficSource.source as source
        ,sum(productRevenue)/1000000 as revenue
  from `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
  unnest (hits) hits,
  unnest (hits.product) product
  where productRevenue is not null
  group by time_type, time, source
  order by time, source, revenue desc
    )
select *
from data_June_month
union all
select *
from data_June_day
where source like '(direct)'
      or source like 'google'
```

_**•	Query result**_

![image](https://github.com/user-attachments/assets/0c1b14e7-19fa-49be-8ad7-8714e8ae50a2)


**Query 04: Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017.**

_**•	SQL code**_

```SQL
with type_purchasers as (
      select 
            format_date("%Y%m", PARSE_DATE("%Y%m%d", date)) as month
            ,count(distinct fullVisitorId)
            ,sum(totals.pageviews)
            ,sum(totals.pageviews)
            /count(distinct fullVisitorId) as avg_pageviews_purchase
      from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
      unnest (hits) hits,
      unnest (hits.product) product
      where _table_suffix between '0601' and '0731'
            and productRevenue is not null
            and totals.transactions >=1
      group by month
      order by month
),
     type_non_purchasers as ( 
      select 
            format_date("%Y%m", PARSE_DATE("%Y%m%d", date)) as month
            ,count(distinct fullVisitorId)
            ,sum(totals.pageviews)
            ,sum(totals.pageviews)
            /count(distinct fullVisitorId) as avg_pageviews_non_purchase
      from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
      unnest (hits) hits,
      unnest (hits.product) product
      where _table_suffix between '0601' and '0731'
            and productRevenue is null
            and totals.transactions is null 
      group by month
      order by month
     )
select
      type_purchasers.month
      ,avg_pageviews_purchase
      ,avg_pageviews_non_purchase
from type_purchasers
join type_non_purchasers using(month)
```


_**•	Query result**_

![image](https://github.com/user-attachments/assets/5ef9393b-7dcc-4e41-89d8-c7d92244b91c)


**Query 05: Average number of transactions per user that made a purchase in July 2017**

_**•	SQL code**_

```SQL
select 
      format_date("%Y%m", PARSE_DATE("%Y%m%d", date)) as month
      ,sum(totals.transactions)
        /count(distinct fullVisitorId) as Avg_total_transactions_per_user
from `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
unnest (hits) hits,
unnest (hits.product) product
where productRevenue is not null
      and totals.transactions >=1
group by month
```


_**•	Query result**_

![image](https://github.com/user-attachments/assets/2e23edf8-d369-49d5-84f0-bbb18eb44daa)

**Query 06: Average amount of money spent per session. Only include purchaser data in July 2017**

_**•	SQL code**_

```SQL
select 
      format_date("%Y%m", PARSE_DATE("%Y%m%d", date)) as month
      ,round((sum(productRevenue)
      /(count (totals.visits)))/1000000,2) as avg_revenue_by_user_per_visit
from `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
unnest (hits) hits,
unnest (hits.product) product
where productRevenue is not null
      and totals.transactions is not null
group by month;
```


_**•	Query result**_

![image](https://github.com/user-attachments/assets/0ed3c61d-ef90-46cb-bd63-7bbeb049a021)


**Query 07: Other products purchased by customers who purchased product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered.**

_**•	SQL code**_

```SQL
with raw_data_1 as(
      select 
            fullVisitorId as user_purchasers
      from `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
      unnest (hits) hits,
      unnest (hits.product) product
      where productRevenue is not null
      and v2ProductName = "YouTube Men's Vintage Henley"
)
 select
      v2ProductName as other_purchased_products
      ,count(productQuantity) as quanlity
from raw_data_1
inner join `bigquery-public-data.google_analytics_sample.ga_sessions_201707*` as t1
on raw_data_1.user_purchasers = t1.fullVisitorId
cross join 
      unnest (hits) hits
cross join 
      unnest (hits.product) product
where productRevenue is not null
group by other_purchased_products
order by quanlity desc;
```


_**•	Query result**_

![image](https://github.com/user-attachments/assets/5cc97533-7a64-4f0f-a441-08749bfab5c3)


**Query 08: Calculate cohort map from product view to addtocart to purchase in Jan, Feb and March 2017. For example, 100% product view then 40% add_to_cart and 10% purchase.**

_**•	SQL code**_

```SQL
with 
    raw_data_1 as (
        select
            format_date("%Y%m", PARSE_DATE("%Y%m%d", date)) as month
            ,eCommerceAction.action_type as action_type
            ,productRevenue
        from `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
        unnest (hits) hits,
        unnest (hits.product) product
        where _table_suffix between '0101' and '0331'
        order by action_type
),
    action_type as (
        select 
            month
            ,count(action_type) as num_product_view
        from raw_data_1
        where action_type = '2'
        group by month
        order by month
),
    action_add_to_cart as (
         select 
            month
            ,count(action_type) as num_addtocart
        from raw_data_1
        where action_type = '3'
        group by month
        order by month
),
    action_purchases as (
        select 
            month
            ,count(action_type) as num_purchase
        from raw_data_1
        where action_type = '6'
            and productRevenue is not null
        group by month
        order by month 
)
select
        action_type.month
        ,num_product_view
        ,num_addtocart
        ,num_purchase
        ,round((100*num_addtocart/num_product_view),2) as add_to_cart_rate
        ,round((100*num_purchase/num_product_view),2) as purchase_rate
from action_type
join action_add_to_cart on action_type.month =  action_add_to_cart.month
join action_purchases on action_add_to_cart.month = action_purchases.month
order by month asc;
```

_**•	Query result**_

 ![image](https://github.com/user-attachments/assets/efead5c1-e308-4741-b024-854cf156e36e)

