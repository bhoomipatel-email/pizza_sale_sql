🍕 Pizza Sales Analysis using SQL
📌 Project Title - Pizza Sales Performance & Revenue Analysis using SQL
🧾 Project Overview

This project analyzes pizza sales data using SQL to derive meaningful business insights related to revenue, customer demand, product performance, and operational trends.
The analysis is designed to simulate real-world business intelligence and decision-making scenarios, making it suitable for Data Analyst, Business Analyst, Risk Analyst, and Consulting roles.

The project progresses from basic SQL queries to advanced analytical techniques, including window functions and cumulative metrics.

🎯 Project Objectives

The primary objectives of this project are to:

* Analyze overall sales performance and revenue generation
* Identify top-performing pizzas and categories
* Understand customer ordering behavior and demand patterns
* Perform time-based and category-wise trend analysis
* Demonstrate strong SQL querying, data aggregation, and analytical skills
* Build a portfolio-ready project aligned with industry expectations

🔍 Key SQL Analysis Performed

🔹 Basic Analysis

* Total number of orders
* Total revenue generated
* Highest priced pizza
* Most common pizza size ordered
* Top 5 pizzas by quantity ordered

🔹 Intermediate Analysis

* Total quantity ordered per pizza
* Hour-wise order distribution
* Category-wise pizza distribution
* Average pizzas ordered per day
* Top 3 pizzas by revenue

🔹 Advanced Analysis

* Revenue contribution percentage by pizza type
* Cumulative revenue over time
* Top 3 pizzas by revenue within each category using **window functions


🔹 BASIC SQL QUERIES

 1️⃣ Retrieve the total number of orders placed

```sql
SELECT COUNT(order_id) AS total_orders
FROM orders;
```

---
2️⃣ Calculate the total revenue generated from pizza sales

```sql
SELECT 
    SUM(order_details.quantity * pizzas.price) AS total_revenue
FROM order_details
JOIN pizzas 
    ON order_details.pizza_id = pizzas.pizza_id;
```


---

3️⃣ Identify the highest-priced pizza

```sql
SELECT pizza_types.name, pizzas.price
FROM pizzas
JOIN pizza_types 
    ON pizzas.pizza_type_id = pizza_types.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
```

---

4️⃣ Identify the most common pizza size ordered

```sql
SELECT pizzas.size, COUNT(*) AS total_orders
FROM pizzas
JOIN order_details 
    ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY total_orders DESC
LIMIT 1;
```

---

5️⃣ List the top 5 most ordered pizza types along with quantities

```sql
SELECT 
    pizza_types.name,
    SUM(order_details.quantity) AS quantity_ordered
FROM pizza_types
JOIN pizzas 
    ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details 
    ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity_ordered DESC
LIMIT 5;
```

---

🔹 INTERMEDIATE SQL QUERIES

 6️⃣ Total quantity ordered for each pizza type

```sql
SELECT 
    pizza_types.name,
    SUM(order_details.quantity) AS total_quantity
FROM pizza_types
JOIN pizzas 
    ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details 
    ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizza_types.name;
```

---

7️⃣ Distribution of orders by hour of the day

```sql
SELECT 
    HOUR(time) AS order_hour,
    COUNT(order_id) AS total_orders
FROM orders
GROUP BY order_hour
ORDER BY order_hour;
```

---

8️⃣ Category-wise distribution of pizzas

```sql
SELECT 
    pizza_types.category,
    COUNT(*) AS total_pizzas
FROM pizza_types
GROUP BY pizza_types.category;
```

---

9️⃣ Average number of pizzas ordered per day

```sql
SELECT 
    orders.date,
    AVG(order_details.quantity) AS avg_pizzas_per_day
FROM orders
JOIN order_details 
    ON orders.order_id = order_details.order_id
GROUP BY orders.date;
```

---

🔟 Top 3 pizza types based on revenue

```sql
SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM pizza_types
JOIN pizzas 
    ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details 
    ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```

---

🔹 ADVANCED SQL QUERIES

 1️⃣1️⃣ Percentage contribution of each pizza type to total revenue

```sql
SELECT 
    pizza_types.name,
    ROUND(
        SUM(order_details.quantity * pizzas.price) /
        (SELECT SUM(order_details.quantity * pizzas.price)
         FROM order_details
         JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id) * 100, 2
    ) AS revenue_percentage
FROM pizza_types
JOIN pizzas 
    ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details 
    ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue_percentage DESC;
```

---

 1️⃣2️⃣ Cumulative revenue generated over time

```sql
SELECT 
    orders.date,
    SUM(order_details.quantity * pizzas.price) AS daily_revenue,
    SUM(SUM(order_details.quantity * pizzas.price)) 
        OVER (ORDER BY orders.date) AS cumulative_revenue
FROM orders
JOIN order_details 
    ON orders.order_id = order_details.order_id
JOIN pizzas 
    ON order_details.pizza_id = pizzas.pizza_id
GROUP BY orders.date;
```

---

1️⃣3️⃣ Top 3 pizzas by revenue for each category

```sql
SELECT name, revenue
FROM (
    SELECT 
        pizza_types.category,
        pizza_types.name,
        SUM(order_details.quantity * pizzas.price) AS revenue,
        RANK() OVER (
            PARTITION BY pizza_types.category 
            ORDER BY SUM(order_details.quantity * pizzas.price) DESC
        ) AS rank_position
    FROM pizza_types
    JOIN pizzas 
        ON pizza_types.pizza_type_id = pizzas.pizza_type_id
    JOIN order_details 
        ON pizzas.pizza_id = order_details.pizza_id
    GROUP BY pizza_types.category, pizza_types.name
) ranked_pizzas
WHERE rank_position <= 3;
```

















































































📊 Key Findings & Insights

 🍕 A small set of pizzas contributes a significant portion of total revenue , indicating strong best-sellers.
 ⏰ Peak ordering hours occur during lunch and dinner times, reflecting customer behavior patterns.
 📈 Revenue is not evenly distributed across categories, with certain categories outperforming others.
 💰 Top 3 pizzas per category drive the majority of category-level revenue.
 📅 Cumulative revenue analysis highlights consistent growth over time, useful for forecasting.

 🛠️ Skills Demonstrated

* SQL Joins & Aggregations
* Subqueries & Nested Queries
* Window Functions (`RANK()`, `OVER()`)
* Revenue & Trend Analysis
* Business-Oriented Data Interpretation
* MySQL / PostgreSQL compatible SQL


📌 Conclusion

This project demonstrates how **SQL can be used as a powerful analytical tool to convert raw transactional data into actionable business insights.
It reflects real-world analytical thinking, strong query optimization, and a clear understanding of business impact—making it a solid portfolio project for analytics and consulting roles.
