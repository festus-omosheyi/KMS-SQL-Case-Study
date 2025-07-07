# 📊 Kultra Mega Stores (KMS) SQL Case Study

## 🏢 Company Overview

**Kultra Mega Stores (KMS)**, headquartered in **Lagos, Nigeria**, specializes in **office supplies** and **furniture**. Its customer base includes:
- Individual consumers
- Small businesses (Retail)
- Large corporate clients (Wholesale)

You have been hired as a **Business Intelligence Analyst** to support the Abuja division. Your task is to use SQL to analyze order data from **2009 to 2012** and provide actionable insights to management.

---

## 📁 Data Files

This project uses two CSV files:
- `KMS_SQL_Case_Study.csv`: Contains order-level data with 21 columns.
- `Order_Status.csv`: Contains only returned order IDs.

These were imported into Microsoft SQL Server Management Studio (SSMS) using appropriate data types (e.g., `nvarchar` for text fields, `float` for sales/profit).

---

## 🧪 Case Scenario I – Business Insights

| Task | Objective |
|------|-----------|
| 1 | Identify the **product category** with the highest total sales. |
```sql
select top 1 product_category, sum(sales) as Total_sales
from kms_orders
group by product_category
order by Total_sales desc
```
| 2 | Determine the **Top 3** and **Bottom 3** performing regions by total sales. |
```sql
select top 3 region, sum(sales) as Total_sales
from kms_orders 
group by region
order by Total_sales desc

select top 3 region, sum(sales) as Total_sales
from kms_orders 
group by region
order by Total_sales asc
```
| 3 | Calculate **total appliance sales** in **Ontario**. |
```sql
select sum(sales) as Appliances_Sales_Ontario
from kms_orders
where product_sub_category = 'Appliances' and province = 'Ontario'
```
| 4 | List the **bottom 10 customers** by total sales and suggest revenue strategies. |
```sql
select top 10 customer_name, sum(sales) as Total_sales
from kms_orders
group by customer_name
order by Total_sales asc
-- Advice:- Target low-value customers with loyalty or upsell strategies
```
| 5 | Identify the **shipping method** that incurred the highest shipping cost. |
```sql
select ship_mode, sum(shipping_cost) as Total_Shipping_Cost
from kms_orders
group by ship_mode
order by Total_Shipping_Cost desc
```

---

## 👥 Case Scenario II – Customer Behavior Analysis

| Task | Objective |
|------|-----------|
| 6 | Find the **most valuable customers** and their most purchased product categories. |
```sql
select customer_name, sum(sales) as Total_sales, 
	(select STRING_AGG(product_category, ', ')
	from ( select distinct product_category
		from kms_orders as innerkms
		where innerkms.customer_name = outerkms.customer_name
	) as Category
	) as Category_bought
from kms_orders as outerkms
group by customer_name
order by Total_sales desc
```
| 7 | Identify the **small business customer** with the highest sales. |
```sql
select customer_name, sum(sales) as Total_sales
from kms_orders
where customer_segment = 'Small Business'
group by customer_name
order by total_sales desc
```
| 8 | Determine the **corporate customer** with the most order count. |
```sql
select customer_name, COUNT(distinct order_id) as Order_count
from KMS_Orders
where Customer_Segment = 'corporate'
group by Customer_Name
order by Order_count desc
```
| 9 | Find the **most profitable consumer customer**. |
```sql
select customer_name, sum(profit) as Total_profit
from KMS_Orders
where Customer_Segment = 'consumer'
group by Customer_Name
order by Total_profit desc
```
| 10 | Match **returned orders** with their customer names and segments. |
```sql
select distinct k.customer_name, k.customer_segment, o.status
from KMS_Orders k
join Order_Status o
on k.Order_ID = o.Order_ID
```
| 11 | Analyze if **shipping costs** were justified based on **Order Priority**. |
```sql
select order_priority, ship_mode, count(*) as Num_Orders, sum(shipping_cost) as Total_shipping_cost
from KMS_Orders
group by Order_Priority, Ship_Mode
order by Ship_Mode desc
```

---

## 🛠 Tools Used

- **SQL Server Management Studio (SSMS)**
- **SQL (T-SQL)**
- **Microsoft Excel** (for initial review and import)
- **GitHub** (for documentation and sharing)

---

## 📌 Key Learnings

- Used **aggregation functions** like `SUM`, `COUNT`, and `STRING_AGG`.
- Applied **JOINs** to merge order data with return status.
- Used **GROUP BY** and **ORDER BY** to analyze performance metrics.
- Interpreted business logic (e.g., high shipping cost vs low-priority orders).

---

## 🧠 Recommendations

- Offer **loyalty discounts** and **upsell higher-margin products** to bottom 10 customers.
- Monitor **shipping cost alignment** with **order priority** to reduce waste.
- Focus marketing efforts in **top-performing regions**, and improve service in the **bottom 3**.
- Personalize campaigns for **high-value customers** based on product preferences.

---

## ✅ How to Reproduce

1. Open SSMS and create a new database (e.g., `KMS_DB`).
2. Import both CSV files into tables: `KMS_Orders` and `Order_Status`.
3. Run the SQL script provided in this repository: [`kms_sql_case_study.sql`](./kms_sql_case_study.sql).
4. Use query results to build visual dashboards or reports.

---

## 🤝 Author

**Festus Omosheyi Godstime**  
Business Intelligence Analyst | SQL • Excel • Data Analytics  
🕊️ Committed to transforming raw data into actionable insights.

---

## 🧾 License

This project is open-source and free to use for learning and academic purposes.

