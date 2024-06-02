create database pizzahut;
create table orders(
order_id int not null,
order_date date not null,
order_time time not null,
primary key(order_id));

create table order_details(
order_details_id int not null,
order_id int not null,
pizza_id text not null,
quantity int not null,
primary key(order_details_id));
select * from orders;
select * from order_details;
select * from pizza_types;
select * from pizzas;

-- Retrieve the total number of orders placed. 
select count(*) as Total_orders from orders;

-- Calculate the total revenue generated from pizza sales.
select sum(quantity*price) as amt from pizzas p right join order_details d on p.pizza_id=d.pizza_id;
-- with cte 
with cte_amt as(
select (quantity*price) as amt from pizzas p right join order_details d on p.pizza_id=d.pizza_id)
select round(sum(amt),2) as total_revenue from cte_amt;

-- Identify the highest-priced pizza.
select name, price as highest_price from pizzas p join pizza_types t on p.pizza_type_id = t.pizza_type_id 
order by price desc limit 1;

-- Identify the most common pizza size ordered.
select size, count(order_details_id) as order_count from pizzas p join order_details d on p.pizza_id=d.pizza_id group by size order by count(size) desc limit 1;

-- List the top 5 most ordered pizza types along with their quantities.
select name, sum(quantity) from pizza_types T 
join pizzas p on p.pizza_type_id=t.pizza_type_id 
join order_details D on p.pizza_id=d.pizza_id 
group by name order by sum(quantity) desc limit 5;

-- Join the necessary tables to find the total quantity of each pizza category ordered.
select distinct(category), sum(quantity) from pizza_types t 
join pizzas p on p.pizza_type_id=t.pizza_type_id 
join order_details D on p.pizza_id=d.pizza_id group by category order by count(order_id) desc;

-- Determine the distribution of orders by hour of the day.
select hour(order_time) hour,count(order_id) order_count from orders group by hour(order_time);

-- Join relevant tables to find the category-wise distribution of pizzas.
select category, count(name) Type from pizza_types group by category;

-- Group the orders by date and calculate the average number of pizzas ordered per day.
with cte_avg as(
select order_date, sum(quantity) as QUANTITY from orders o join order_details d on o.order_id = d.order_id group by order_date)
select ROUND(AVG (QUANTITY),0) AVG_QUANTITY  from cte_avg;


-- Determine the top 3 most ordered pizza types based on revenue.
select name, sum(quantity*price) as amt from pizzas p 
join order_details d on p.pizza_id=d.pizza_id
join pizza_types t on t.pizza_type_id= p.pizza_type_id group by name order by amt desc limit 3;

-- Calculate the percentage contribution of each pizza type to total revenue.
SELECT category, ROUND(((SUM(quantity * price) / 
    (SELECT SUM(quantity * price) FROM pizzas p JOIN order_details d ON p.pizza_id = d.pizza_id)
)) * 100,2) AS rev FROM pizzas p
JOIN order_details d ON p.pizza_id = d.pizza_id
JOIN pizza_types t ON t.pizza_type_id = p.pizza_type_id
GROUP BY category;

-- Analyze the cumulative revenue generated over time.
with cte_cumm_rev as (
 SELECT order_date, sum(quantity * price) revenue FROM pizzas p JOIN order_details d ON p.pizza_id = d.pizza_id join orders o on o.order_id= d.order_id group by order_date)
 select order_date, sum(revenue) over (order by order_date) from cte_cumm_rev;
 
--  Determine the top 3 most ordered pizza types based on revenue for each pizza category.
with cte as (with cte_rev as (select category,name, sum(quantity*price) as rev from pizzas p 
join order_details d on p.pizza_id=d.pizza_id
join pizza_types t on t.pizza_type_id= p.pizza_type_id group by category, name)
select category,name, rev, rank() over(partition by category order by rev desc) rnk from cte_rev)
select name, rev from cte where rnk<=3;









