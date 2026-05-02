# 🛒 E-Commerce Sales Analysis (Advanced SQL Project)

> ⚠️ **Disclaimer:** This project uses a simulated dataset for demonstration purposes.

---

## 📌 Project Overview

This project analyzes e-commerce transactional data using advanced SQL techniques to uncover revenue trends, customer behavior, and product performance.

The goal is to transform raw transactional data into **actionable business insights** using SQL.

---

## 🎯 Business Problem

The business needs to understand:

- Who are the **top customers** driving revenue?
- Which products contribute most to sales?
- How is revenue trending over time?
- What opportunities exist to improve performance?

---

## 📊 Dataset Description

The dataset consists of four tables:

### 🧾 Customers

| Column        | Description |
|--------------|------------|
| customer_id  | Unique customer ID |
| name         | Customer name |
| city         | Customer location |

---

### 🧾 Orders

| Column      | Description |
|-------------|------------|
| order_id    | Unique order ID |
| customer_id | Linked customer |
| order_date  | Date of order |

---

### 🧾 Order Items

| Column      | Description |
|-------------|------------|
| order_id    | Order reference |
| product_id  | Product reference |
| quantity    | Units purchased |

---

### 🧾 Products

| Column       | Description |
|--------------|------------|
| product_id   | Unique product ID |
| product_name | Product name |
| category     | Product category |
| price        | Unit price |

---

## 🧠 Data Model

The dataset follows a **relational model**:

- Customers → Orders → Order_Items → Products  
- Primary keys and foreign keys are used to establish relationships  

---

## 🧮 SQL Analysis

---

### 📌 Total Revenue (JOIN)

```sql
SELECT 
    SUM(p.price * oi.quantity) AS total_revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id;
````

###  📌 Revenue by Customer (Multi-table JOIN)
```sql
SELECT 
    c.name,
    SUM(p.price * oi.quantity) AS revenue
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
GROUP BY c.name
ORDER BY revenue DESC;
````

### 📌 Top Products (Window Function)
```sql
SELECT 
    product_name,
    revenue,
    RANK() OVER (ORDER BY revenue DESC) AS rank
FROM (
    SELECT 
        p.product_name,
        SUM(p.price * oi.quantity) AS revenue
    FROM order_items oi
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY p.product_name
) t;
````

### 📌 Monthly Revenue Trend
```sql
SELECT 
    DATE_TRUNC('month', o.order_date) AS month,
    SUM(p.price * oi.quantity) AS revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
GROUP BY month
ORDER BY month;
````

### 📌 Running Revenue (Window Function)
````sql
SELECT 
    month,
    revenue,
    SUM(revenue) OVER (ORDER BY month) AS running_total
FROM (
    SELECT 
        DATE_TRUNC('month', o.order_date) AS month,
        SUM(p.price * oi.quantity) AS revenue
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY month
) t;
````

### 📌 Customer Lifetime Value (CTE)
````sql
WITH customer_revenue AS (
    SELECT 
        c.customer_id,
        c.name,
        SUM(p.price * oi.quantity) AS total_spent
    FROM customers c
    JOIN orders o ON c.customer_id = o.customer_id
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY c.customer_id, c.name
)
SELECT *
FROM customer_revenue
ORDER BY total_spent DESC;
````

---

## 📊 Key Insights

- 💰 A small group of customers contributes a large portion of total revenue
- 📱 Electronics category generates the highest sales
- 📈 Revenue shows a steady month-over-month increase
- ⚠️ Some products generate low revenue and may require attention

---

## 💡Business Recommendations
- Focus retention strategies on high-value customers
- Increase marketing for top-performing product categories
- Review and optimize underperforming products
- Use customer purchase patterns for targeted promotions

---

## 🛠 Tools & Technologies
- SQL (Advanced querying, CTEs, window functions)
- Relational data modeling

---

## 📌 Project Outcome

**This project demonstrates the ability to:**

- Perform complex SQL analysis
- Work with relational databases
- Generate business insights from raw data
- Communicate findings effectively

##👤 Author

Emmanuel Adusu
Data Analyst | SQL | Power BI | Business Intelligence

📧 Email: adusue191@gmail.com

🌐 Portfolio: https://emmanuel-adusu.github.io/
