# SQL_Data_Analysis_PROJECT

## 1.	Project overview
The project is about the pizza sales unit with database containing the information about the orders, order details and the types of pizzas available in the sales menu.
We intend to use SQL to make queries on the database to answer the related questions concernig the sales and making changes into the sales by analyzing the outputs of the queries. The dataset conatains all data about sales, price of every kind of pizza and the user details who has made the order. 

## 2.	Data sources
The data has been taken from the folder "PIZZA HUT" which contains the tables that have the "orders" with order_id and #order details with all the details of the order. 
The pizzas table have the information about the kind of pizzas available and the details about the category and types of pizzas.
The pizza_types tavle have the informaton about the pizza type under every category and works as the reference for the orders.

## 3.	Tools used and what for
We are using SQL(Structured Query Language) to query the database and find insights from the tables. The queries are made according to the need and asak of the question to be analysed.
We used various JOINS, SUBQUERIES and window functions(RANK (), OVER) to solve the questions. The basic data handling is also done with the help of SELECT, FROM, WHERE, GROUP BY, ORDER BY.

```
SQL
SELECT 
    ROUND(AVG(quantity), 0)
FROM
    (SELECT 
        order_date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY order_date) AS quantity_ordered;
```

## 4.	EDA (with questions)
The exploration of the Dataset includes variuos questions that we intend to solve that are attched in the file "QUESTION.TXT".
Some of the exploration question looks like:

### 1. Identify the most common pizza size ordered.
--> This uses the basic style of SQL query to identify the most common pizza size ordered from the pizza table and pizza_type table. 

### 2. Determine the top 3 most ordered pizza types based on revenue
--> The top 3 most ordered pizzas are queried by joining two tables that are pizza and pizza_type that is joined y the pizza__type_id and the revenue is taken from the orders table by joining it with the pizzas table on the common column of pizza_id.

```
SQL
    SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```  

### 3. Analyze the cumulative revenue generated over time.

```
SQL
select order_date, 
sum(revenue) over (order by order_date) as cumulative_revenue
from 
(select orders.order_date, 
sum(order_details.quantity * pizzas.price) as revenue
from order_details join pizzas
on order_details.pizza_id=pizzas.pizza_id
join orders
on orders.order_id=order_details.order_id
group by orders.order_date) as sales;
```

## 5.	Limitations(zero/null values, outliers)
Cannot handle very large datasets for which we might need to use some other way to analyse like Excel that can handle very large datasets.  




