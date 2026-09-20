# E-Commerce-Sales-Analytics-with-PostgreSQL-and-Python
End-to-end analysis of ~100K Brazilian e-commerce orders: a relational PostgreSQL database, SQL analysis with CTEs and window functions, and Python (pandas, seaborn) for visualisation, RFM customer segmentation and cohort retention.

1. Business Problem
An online marketplace wants to understand where its revenue comes from, how well it delivers, and whether customers come back. This project answers:

What are the headline KPIs (revenue, orders, customers, average order value)?
How is revenue trending month over month?
Which product categories and states drive the most revenue?
How good is delivery performance, and do late deliveries hurt customer reviews?
How do customers pay?
How many customers buy again, and which customer segments are most valuable?
Which sellers contribute the most revenue?

2. Dataset
Source: Brazilian E-Commerce Public Dataset by Olist (Kaggle)
Size: ~100K orders placed between September 2016 and October 2018. Trend analysis uses January 2017 to August 2018, because the first and last months are sparse.
Currency: Brazilian Reais (BRL)
Table	Description
customers	Customer location. customer_id is per order, customer_unique_id identifies the real person
orders	Order status and purchase, approval, delivery and estimated-delivery timestamps
order_items	Products in each order, seller, price and freight value
order_payments	Payment type, instalments and value
order_reviews	Review score and comments
products	Product category and dimensions
sellers	Seller location
category_translation	Portuguese to English category names

The geolocation file is not used in this project.

3. Tools Used
PostgreSQL and pgAdmin: database design, SQL analysis
Python: pandas, SQLAlchemy, psycopg2, matplotlib, seaborn
Jupyter Notebook (Anaconda)
SQL techniques: joins, CTEs, views, window functions (LAG, RANK, SUM() OVER), conditional aggregation

4. Database Schema
<img width="822" height="417" alt="image" src="https://github.com/user-attachments/assets/fc7cfefd-d776-481d-a758-9b361e0847fe" />


Design notes
order_items has a composite primary key (order_id, order_item_id), and order_payments has (order_id, payment_sequential).
order_reviews has no primary key because review_id is not unique in the source data.
category_translation is joined with a LEFT JOIN and has no foreign key, because a few product categories have no translation.
A view, vw_delivered_items, joins orders, customers and items for delivered orders only, so all revenue queries use one consistent definition.

Revenue definition: SUM(price) from orders with status delivered, excluding freight.

5. Key Findings

All figures are in BRL and come from delivered orders between January 2017 and August 2018, unless stated otherwise.

Headline KPIs: total revenue [13221498.11], [96478] orders, [93358] unique customers, average order value [137.04].

5.1 Revenue trend
<img width="465" height="786" alt="image" src="https://github.com/user-attachments/assets/a7359398-c572-41a8-a716-7aa19bf9d2c2" />
Observation: Monthly revenue moved from [11179836: Jan 2017 value] to [838576.64: Aug 2018 value]. The peak was in [FEB: month] at [234223.40: value].

5.2 Categories and geography
<img width="500" height="362" alt="image" src="https://github.com/user-attachments/assets/d72d8a8b-48ed-4f7a-9ed6-303edbfa966a" />
<img width="502" height="486" alt="image" src="https://github.com/user-attachments/assets/e3d895c9-bde2-4b71-b2ca-ec74cbcad4c1" />
Observation: The top 3 categories ([health_beuaty,watches_gifts,bed_bath_tables: names]) account for [123313.72,1166176.98,1023434.76]% of revenue. [SP,RJ,MG: state] alone generates [5067633.16,1759651.13,1552481.83]% of revenue.
]

5.3 Delivery performance and customer satisfaction
<img width="590" height="458" alt="download" src="https://github.com/user-attachments/assets/730e2b28-9778-4232-b297-dd5ba4916685" />
Observation: Average review score is [4.29] for on-time deliveries versus [2.57] for late ones. [1.72]% of orders arrive after the estimated date. The slowest states are [SP], at about [8.8] days on average.


5.4 Customer retention
<img width="915" height="684" alt="download" src="https://github.com/user-attachments/assets/71c45f13-9ee6-4a41-b0b6-147f36e8edcd" />
Observation: Only [3]% of customers placed more than one order. Month-1 retention is below [0.72]% for every cohort.


5.5 RFM segments
Segment	              Customers	 share	Avg spend (BRL)				
High-Value New	        22292		231.6	     39.1
At Risk (High Spenders)	22044		233.7	     39.0
Lost / Low Value	      23377		47.2	     8.4
New / Recent	          22844		47.0	     8.1
Champions	              1543		264.3	     3.1
Loyal / Repeat	        1258		254.9	     2.4

5.6 Other observations
Payments: [credit card share_pct:78.3]% of payment value.
Sellers: The top 10 sellers contribute 1754800.0 of revenue.

6. Recommendations
#	Finding	Recommendation
1	Late deliveries reduce review scores	Set more realistic delivery estimates in the slowest states and flag consistently late sellers
2	Very low repeat purchase rate	Launch post-purchase campaigns (e.g. a coupon for a second order within 30 days)
3	Revenue concentrated in a few states/categories	

Actions by RFM segment
High-Value New: second-purchase coupon within 30 days
At Risk (High Spenders): personalised win-back offer
Loyal / Repeat and Champions: loyalty programme, early access to new products
Lost / Low Value: low-cost automated campaigns only




