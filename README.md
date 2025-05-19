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

 ![image](https://github.com/user-attachments/assets/e515ab84-f40e-4a94-892f-6874ab9d2691)

 
_**•	Query result**_

![image](https://github.com/user-attachments/assets/1db281c9-4f45-4342-9604-4ed615538dee)

 
**Query 02: Bounce rate per traffic source in July 2017 (Bounce_rate = num_bounce/total_visit) (order by total_visit DESC)**

_**•	SQL code**_

![image](https://github.com/user-attachments/assets/b20ed17c-34c8-4f07-8334-6dd76136235e)

 
_**•	Query result**_

![image](https://github.com/user-attachments/assets/9303e1a8-374f-477a-bb22-ab4b9e37d057)

 
**Query 3: Revenue by traffic source by week, by month in June 2017**

_**•	SQL code**_

![image](https://github.com/user-attachments/assets/ebb2846f-68b8-4c4d-8e0f-15a6ed79b716)
![image](https://github.com/user-attachments/assets/f9f92774-61bb-470a-ade2-aff84be5ea68)

_**•	Query result**_

![image](https://github.com/user-attachments/assets/0c1b14e7-19fa-49be-8ad7-8714e8ae50a2)


**Query 04: Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017.**

_**•	SQL code**_

![image](https://github.com/user-attachments/assets/bb765202-3bf4-4e1c-a1be-baaa0dd18fba)
![image](https://github.com/user-attachments/assets/c3fe3537-f823-443d-b2dc-3a62ff948152)


_**•	Query result**_

![image](https://github.com/user-attachments/assets/5ef9393b-7dcc-4e41-89d8-c7d92244b91c)


**Query 05: Average number of transactions per user that made a purchase in July 2017**

_**•	SQL code**_

![image](https://github.com/user-attachments/assets/00110667-e2f9-4993-908a-57e143c76b59)


_**•	Query result**_

![image](https://github.com/user-attachments/assets/2e23edf8-d369-49d5-84f0-bbb18eb44daa)

**Query 06: Average amount of money spent per session. Only include purchaser data in July 2017**

_**•	SQL code**_

![image](https://github.com/user-attachments/assets/f3cffc32-d4a9-4901-805c-2781b415fe49)


_**•	Query result**_

![image](https://github.com/user-attachments/assets/0ed3c61d-ef90-46cb-bd63-7bbeb049a021)


**Query 07: Other products purchased by customers who purchased product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered.**

_**•	SQL code**_

![image](https://github.com/user-attachments/assets/9430b187-0358-4cb4-b503-4e7dc7a0eba9)


_**•	Query result**_

![image](https://github.com/user-attachments/assets/5cc97533-7a64-4f0f-a441-08749bfab5c3)


**Query 08: Calculate cohort map from product view to addtocart to purchase in Jan, Feb and March 2017. For example, 100% product view then 40% add_to_cart and 10% purchase.**

_**•	SQL code**_

![image](https://github.com/user-attachments/assets/a6150830-89d2-43e8-8e68-7e57f239f347)
![image](https://github.com/user-attachments/assets/f7c102cc-64a6-46d6-96e8-46bbf515f1d1)
![image](https://github.com/user-attachments/assets/389726fc-809e-4536-bcb8-be2a281057ae)
![image](https://github.com/user-attachments/assets/a7b37a35-fd83-4b0b-9a89-02222d19bae7)

_**•	Query result**_

 ![image](https://github.com/user-attachments/assets/efead5c1-e308-4741-b024-854cf156e36e)

