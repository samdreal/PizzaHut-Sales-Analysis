# PizzaHut-Sales-Analysis
## Introduction:
Pizza Hut Sales Analysis: A SQL Case Study
In the highly competitive world of fast food, understanding sales patterns and customer preferences is crucial for maintaining a competitive edge. This project focuses on analyzing the sales data of Pizza Hut, one of the leading pizza chains globally, using SQL. By leveraging the power of SQL, we aim to extract meaningful insights from the sales data, which can help in making informed business decisions.

## Project Overview:
The primary objective of this project is to perform a comprehensive analysis of Pizza Hut's sales data. Through this analysis, we will address several case study questions that revolve around key business metrics and trends. This includes identifying top-selling products, understanding customer behavior, and evaluating sales performance across different regions and time periods.

## ER Diagram
![Pizza Hut ER Diagram](https://github.com/samdreal/PizzaHut-Sales-Analysis/blob/main/pizza_hut_er_diagram_with_all_connections.png?raw=true)


## Case Study Questions & Solutions
1.Retrieve the total number of orders placed.

`select count(order_id) as total_orders from orders`

## Answer
<img width="119" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/f4fc49ec-d54f-463b-80a1-ae58bedab230">

2.Calculate the total revenue generated from pizza sales.

```
select round(sum(pizzas.price*order_details.quantity),0)as total_revenue 
from pizzas
join order_details
on pizzas.pizza_id=order_details.pizza_id
```
## Answer
<img width="109" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/4d99c9e6-0672-4503-bae8-24ada9858764">

3.Identify the highest-priced pizza.
```
select pizza_types1.name,pizzas.price 
from pizza_types1
join pizzas
on pizza_types1.pizza_type_id=pizzas.pizza_type
order by pizzas.price desc limit 1;
```
## Answer
<img width="182" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/5d025b48-d7e0-47b9-bcf2-d9a840059b07">

4.Identify the most common pizza size ordered.
```
select pizzas.size,count(order_details.order_details_id) as order_count
from pizzas
join order_details
on pizzas.pizza_id=order_details.pizza_id
group by pizzas.size
order by order_count desc
```
## Answer
<img width="179" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/da4bfb28-554d-4bf7-90b1-1e656567bdb9">

5.List the top 5 most ordered pizza types along with their quantities.
```
select pizza_types1.name,
sum(order_details.quantity) as quantity
from pizza_types1 join pizzas
on pizza_types1.pizza_type_id=pizzas.pizza_type
join order_details
on order_details.pizza_id=pizzas.pizza_id
group by pizza_types1.name 
order by quantity desc
limit 5
```
## Answer
<img width="232" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/0c73cfea-e261-4c34-a374-40ea3fcaaabe">

6.Join the necessary tables to find the total quantity of each pizza category ordered.
```
select pizza_types1.category,sum(order_details.quantity) as quantity
from pizza_types1
join pizzas
on pizza_types1.pizza_type_id=pizzas.pizza_type
join order_details
on order_details.pizza_id=pizzas.pizza_id
group by pizza_types1.category
order by quantity desc;
```
## Answer
<img width="172" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/3119861d-f2b0-4363-8889-4ea81557d476">

7.Determine the distribution of orders by hour of the day.
```
select extract(hour from time),count(order_id) as order_count
from orders 
group by extract(hour from time)
order by order_count desc
```
## Answer
<img width="169" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/08451563-3fa2-4d1d-9569-ff77116104a7">

8.Join relevant tables to find the category-wise distribution of pizzas.
```
select category,count(name) as pizza_count
from pizza_types1
group by category
```
## Answer
<img width="168" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/7cf07c2e-b70f-4a18-8811-8b2424b64a46">

9.Group the orders by date and calculate the average number of pizzas ordered per day.
```
select round(avg(quantity),0) as avg_no_pizzas
from
(select orders.date,sum(order_details.quantity) as quantity
from orders
join order_details
on orders.order_id=order_details.order_id
group by orders.date)
```
##Answer
<img width="131" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/74acb813-a607-4222-8859-2d3178db252a">

10.Determine the top 3 most ordered pizza types based on revenue.
```
select pizza_types1.name,
sum(order_details.quantity*pizzas.price) as revenue
from pizza_types1
join pizzas 
on pizza_types1.pizza_type_id=pizzas.pizza_type 
join order_details
on order_details.pizza_id=pizzas.pizza_id
group by pizza_types1.name 
order by revenue desc
limit 3;
```

## Answer
<img width="224" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/7ee7df4d-0412-4fd8-93c3-9ef14fa914d4">

11.Calculate the percentage contribution of each pizza type to total revenue.
```
select pizza_types1.category,
round(sum (order_details.quantity*pizzas.price)/
(select round(sum(order_details.quantity*pizzas.price),2) as total_sales
from
order_details
join pizzas
on order_details.pizza_id=pizzas.pizza_id)*100,2) as revenue
from pizza_types1 join pizzas
on pizza_types1.pizza_type_id=pizzas.pizza_type
join order_details
on order_details.pizza_id=pizzas.pizza_id
group by pizza_types1.category
order by revenue desc;
```
## Answer
<img width="156" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/34fdb3b7-7cfe-4dd9-a8f0-5eedfc76b2f6">

12.Analyze the cumulative revenue generated over time.
```
select date,
sum(revenue) over (order by date) as cum_revnue
from
(select orders.date,
sum(order_details.quantity*pizzas.price) as revenue
from order_details join pizzas
on order_details.pizza_id=pizzas.pizza_id
join orders
on orders.order_id=order_details.order_id
group by orders.date) as sales

```
## Answer
<img width="257" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/aa2d9146-a2cc-47d5-a407-b29ead9f2024">

13.Determine the top 3 most ordered pizza types based on revenue for each pizza category.
```
select name,revenue
from
(select category,name,revenue,
 rank() over(partition by category order by revenue desc) as rn 
from
 (select pizza_types1.category,pizza_types1.name,
  sum((order_details.quantity)*pizzas.price) as revenue
  from pizza_types1 join pizzas
  on pizza_types1.pizza_type_id=pizzas.pizza_type
  join order_details
  on order_details.pizza_id=pizzas.pizza_id
  group by pizza_types1.category,pizza_types1.name) as a ) as b
where rn<=3;
```
## Answer
<img width="224" alt="image" src="https://github.com/samdreal/PizzaHut-Sales-Analysis/assets/68226718/352353d4-5c3b-45db-b532-da43e9d40f5a">





