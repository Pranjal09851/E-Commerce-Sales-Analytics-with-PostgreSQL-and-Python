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

Headline KPIs: total revenue [FILL IN], [FILL IN] orders, [FILL IN] unique customers, average order value [FILL IN].

5.1 Revenue trend
<img width="1105" height="607" alt="image" src="https://github.com/user-attachments/assets/830a4297-0fec-41fe-b348-cb0a24e02eb3" />
Observation: Monthly revenue moved from [FILL IN: Jan 2017 value] to [FILL IN: Aug 2018 value]. The peak was in [FILL IN: month] at [FILL IN: value].
Why it matters: [FILL IN: e.g. seasonality, whether growth is slowing in 2018]

5.2 Categories and geography
<img width="1105" height="600" alt="image" src="https://github.com/user-attachments/assets/3aec7966-25a0-4faa-b906-dc1fea04e37c" />
Observation: The top 3 categories ([FILL IN: names]) account for [FILL IN]% of revenue. [FILL IN: state] alone generates [FILL IN]% of revenue.
Why it matters: [FILL IN: e.g. revenue concentration risk]

5.3 Delivery performance and customer satisfaction
<img width="735" height="568" alt="image" src="https://github.com/user-attachments/assets/38a36ab9-fd96-45a2-a765-1a1aef0aa1bc" />
Observation: Average review score is [FILL IN] for on-time deliveries versus [FILL IN] for late ones. [FILL IN]% of orders arrive after the estimated date. The slowest states are [FILL IN], at about [FILL IN] days on average.
Why it matters: [FILL IN: late delivery is the main driver of poor reviews / your own interpretation]

5.4 Customer retention
<img width="915" height="684" alt="download" src="https://github.com/user-attachments/assets/71c45f13-9ee6-4a41-b0b6-147f36e8edcd" />
Observation: Only [FILL IN]% of customers placed more than one order. Month-1 retention is below [FILL IN]% for every cohort.
Why it matters: [FILL IN: e.g. the business relies on acquiring new customers rather than retaining existing ones]

5.5 RFM segments
Segment	              Customers	 share	Avg spend (BRL)				
High-Value New	        22292		231.6	     39.1
At Risk (High Spenders)	22044		233.7	     39.0
Lost / Low Value	      23377		47.2	     8.4
New / Recent	          22844		47.0	     8.1
Champions	              1543		264.3	     3.1
Loyal / Repeat	        1258		254.9	     2.4

5.6 Other observations
Payments: [FILL IN: e.g. credit card share]% of payment value.
Sellers: The top 10 sellers contribute [FILL IN]% of revenue.

6. Recommendations
#	Finding	Recommendation
1	Late deliveries reduce review scores	Set more realistic delivery estimates in the slowest states and flag consistently late sellers
2	Very low repeat purchase rate	Launch post-purchase campaigns (e.g. a coupon for a second order within 30 days)
3	Revenue concentrated in a few states/categories	[FILL IN: e.g. targeted expansion in high-potential states]

Actions by RFM segment
High-Value New: second-purchase coupon within 30 days
At Risk (High Spenders): personalised win-back offer
Loyal / Repeat and Champions: loyalty programme, early access to new products
Lost / Low Value: low-cost automated campaigns only




