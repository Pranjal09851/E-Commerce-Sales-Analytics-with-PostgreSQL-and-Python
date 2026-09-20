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
