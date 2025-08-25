# SQL-PRACTICE
SQL queries, arithmetic function exercise

```sql
create database LuxDev;


--creating a schema 
create schema luxsql;
set search_path to luxsql;
--creating customer tables 
create table customers(
customer_id serial primary key,
First_Name Varchar(50)not null,
Last_Name Varchar(50)not null,
Email varchar(100)unique not null,
Phone_number char(13)
);
--Insert data into customer table
INSERT INTO customers (first_name, last_name, email, phone_number) 
values
('John', 'Doe', 'john.doe@example.com', '+254712345678'),
('Jane', 'Smith', 'jane.smith@example.com', '+254798765432'), 
('Paul', 'Otieno', 'paul.otieno@example.com', '+254701234567'), 
('Mary', 'Okello', 'mary.okello@example.com', '+254711223344');

select *
from customers;

--Creating Books Table 
CREATE TABLE books (
book_id SERIAL PRIMARY KEY, 
title VARCHAR(150) NOT NULL, 
author VARCHAR(100), 
price NUMERIC(8, 2) NOT NULL, 
published_date DATE );

--Insert data into books
INSERT INTO books (title, author, price, published_date) 
values
('Understanding SQL', 'David Kimani', 1500.00, '2023-01-15'),
('Advanced PostgreSQL', 'Grace Achieng', 2500.00, '2023-02-20'),
('Learning Python', 'James Mwangi', 3000.00, '2022-11-10'), 
('Data Analytics Basics', 'Susan Njeri', 2200.00, '2023-03-05');

select *
from books;

--Creating Orders Table 
CREATE TABLE orders (
order_id SERIAL PRIMARY KEY, 
customer_id INT REFERENCES customers(customer_id), 
book_id INT REFERENCES books(book_id), 
order_date DATE DEFAULT CURRENT_DATE );

--Insert Orders 
INSERT INTO orders (customer_id, book_id, order_date)
VALUES 
(1, 3, '2023-04-01'), 
(2, 1, '2023-04-02'), 
(3, 2, '2023-04-03'), 
(4, 4, '2023-04-04'), 
(1, 2, '2023-04-05');

select *
from orders;

--Table manipulation
--Adding  a column
alter table customers
add column city varchar(100);

alter table orders
add column quantity int default 1;

--updating exeisting data
update customers 
set city = 'Nairobi'
where customer_id=1;

--Updating multiple rows with case statement 
update customers
set city=case customer_id
when 1 then 'Nairobi'
when 2 then 'Mombasa'
when 3 then 'Kisumu'
when 4 then 'Nakuru'
else city
end;

--Updating orders with specific quantity
update orders
set quantity= case order_id
when 1 then 2
when 2 then 1
when 3 then 3
when 4 then 2
when 5 then 1
else quantity
end;
select * from orders;

--Deleting columns in SQL
alter table books
drop column published_date;

--Deleting a row
delete from orders
where order_id=3;

--Deleting multiple rows 
delete from orders
where order_id in (6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30);

--inserting data into a table
insert into customers(first_name, last_name, email, phone_number, city)
values
('Ali', 'Hassan','ali.hassan@gmail.com','+254123456789','Nairobi');

--Inserting multiple rows 
insert into books (title, author, price)
values 
('Intro to SQL','Antony Kimani',1200.00),
('Data science 101','Sera Mwangi',2200.00);

--Deleting multiple rows
delete from books
where book_id in(11,12);

--Common SQL keywords 
--SELECT--Retreive Data
--List Book Titles with authors
select book_id, title, author
from books;

--list all orders with customer_id and order_id
select customer_id,order_id
from orders;

--WHERE- Filter Data 
--list first_name, last_name, city where city = 'Nairobi'
select first_name, last_name, city 
from customers 
where city ='Nairobi';

--Find orders where order quantity was greater than 1 
select order_id, order_date
from orders o 
where quantity > 1;

--Find orders placed by customer with customer_id 1 
select *
from orders 
where customer_id = 1;

--ORDER BY- Sort data--
--List (title and price) books by price from lowest to highiest
select title, price
from books b
order by price asc;

--List all orders by date (latest first)
select *
from orders o 
order by order_date desc;

--GROUP BY- Group and summarize data
--count how many books each author has 
select author,count(*) as total_books
from books b 
group by author;

--Count number of customers per city
select city, count (*) as total_customers
from customers c
group by city;

--Having filter after grouping
--show authors with more than 1 book
select author, count(*) as total_books
from books b
group by author 
having count (*)>1;

--show customers with more than 1 order 
select customer_id  , count (*) as total_orders
from orders
group by customer_id
having count (*)>1;

--LIMIT- Restrict number of results 
--Show the top 2 most expensive books 
select title, price
from books 
order by price  desc
limit 2;

--Shop the cheapest book
select title, price
from books
order by price asc
limit 1;

--AGGREGATE FUNCTIONS 
--COUNT()- Count the number of rows that match a condition
--count total customers 
select count (*) as total_customers
from customers;

--Count customers in Kisumu
select count (*) as Kisumu_customers
from customers c
where city='Kisumu';

--how many books are in the books table 
select count(*)
from books b;

--sum aggregate function
--sum for numeric column
--total quantity for all orders
select sum(quantity) as totalquantities from orders;

--Total quantity of books ordered for book id 2 
select sum (quantity) as quantities from orders o 
where book_id=2;

--Average - mean of values in a numeric column
select avg(price) as averageprice from books;

--Average quantity of books ordered for customers id-3;
select avg(quantity) as averagebooksordered from orders
where customer_id=3;
select * from orders;

--Highiest quantity ordered
select max(quantity) as maxquantity
from orders o;

--Latest order date in the order table
select max(order_date) as latestorderdate
from orders;

--minimum value
--Earliest orderdate in orders table
select min(order_date) as earliestorderdate
from orders;

--SQL OPERATORS
--Comparison operators 
--equals- select rows where column matches exact value
--Find books with price =2,500
select * from books where price=2500;

--Get books titled learning python
select * from books
where title= 'Learning Python';

--not equals- (<>,!=,)
--where column value doesn't match
--List customers not in Eldoret 
select * from customers c 
where city !='Eldoret';

--Find books not by Grace Achieng
select * from books b 
where author != 'Grace Achieng';

--> select rows where column value is > the given value
--list customers with customer id greater than 2
select * from customers
where customer_id > 2;

--Get orders placed after 2023-04-04
select * from orders
where order_date > '2023-04-04';

--< - Select rows where column value is less thann the given value
--show orders with quantity < 2
select * from orders
where quantity < 2;

--List books with price under 2000
select * from books 
where price < 2000;

--(>=) select rows where column value is >= the given value 
--Orders with quantity >=2
select * from orders o 
where quantity >= 2;

--Show orders placed on or after '2023-04-03'.
select * from orders
where order_date >= '2023-04-03';

--(<=) Select rows where column value is <= the given value
--Find books priced >= 2200
select * from books 
where price <=2200;

--Get customers with customer_id <=3
select * from customers
where customer_id <= 3;

--Between - select rows and columns between 2 values 
--Select books priced between 2000 & 3000
select * from books b
where price between 2000 and 3000;

--Get orders placed between '2023-04-01' and '2023-04-03'.
select * from orders
where order_date between '2023-04-01' and '2023-04-03';

--Show orders with quantity between 1 and 2.
select * from orders
where quantity between 1 and 2;

--List customers with customer_id between 1 and 2.
select * from customers 
where customer_id between 1 and 2;

--LIKE (Patter Matching )- to match text data %
-- Find cutomers with First_Name starting with 'J'
select * from customers 
where first_name like 'J%';

--FIND BOOKS with titles containing 'Advanced'
select * from books 
where title like '%Advanced%';

-- Authors with Last_Name starting wih 'Achieng'
select * from books b
where author like '%Achieng';

--Find books with title containing 'Python
select * from books b
where title like '%Python%';

--List customers with email ending with 'gmail.com'
select * from customers c 
where email like '%gmail.com';

--Show books with title starting with 'U'
select * from books
where title like 'U%';

--Get customers with first name containing 'a'
select * from customers 
where first_name like '%a%';

--Find books with title containing 'Analytics'
select * from books b 
where title like '%Analytics%';

--IN - TO FILTER records by matching any value in a list
-- Find customers in Kisumu or Nairobi
select * from customers c 
where city in ('Kisumu','Nairobi');

--Books by James Mwangi or Susan Njeri
select * from books
where author in ('James Mwangi','Susan Njeri');

--Logical Operators - combine multiple conditions inside where clause 
-- AND operator - combines 2 or more conditions 
--Find orders with quantity >= 2 and Customer_Id = 1
select * from orders o 
where quantity>=2 and customer_id=1;

--List books priced>2000 and authored by James Mwangi
select * from books 
where price>2000 and author='James Mwangi';

--OR- combines 2 or more conditions, the row is included if any condition is TRUE
--Find Customers from Nairobi or Kisumu
select * from customers 
where city = 'Nairobi' or city='Kisumu';

--Find orders with Quantity = 3 or Customer_id =4
select * from orders 
where quantity=3 or customer_id=4;

--List customers from 'Kisumu' or 'Mombasa'. 
select * from customers
where city = 'Kisumu' or city ='Mombasa';

--Show books priced over 2500 or authored by 'Grace Achieng'. 
select * from books 
where price > 2500 or author='Grace Achieng';

--Find orders with quantity = 1 or order date = '2023-04-02'. 
select * from orders
where quantity=1 or order_date ='2023-04-02';

--NOT- reverses the result of a condition
--List customers not in Kisumu
select * from customers 
where not CITY = 'Kisumu';

--List books not authored by 'David Kimani'
select * from books
where not author = 'David Kimani';

--1. List books not priced at 1500.
select * from books
where not price=1500;

-- Find customers not from 'Nairobi'. 
select * from customers
where not city='Nairobi';

--Show orders not with quantity = 2
select * from orders 
where not quantity=2;

--SQL ARITHMETIC OPERATORS
--Addition
--Add 200 to every book price
select title,price,price+200 as newprice
from books;

--Add 10 to each order_quantity
select order_id,quantity,quantity+10 as newquantity
from orders;

--Add 2 numbers directly to a query e.g. 87 + 169
select 87+169 as total;

--SUBTRACTION 
--Reduce book prices by 150
select title,price,price-150 as newprice
from books;

--Subtract 500-275
select 500-275 as subtraction;

--MULTIPLICATION
--Double the book prices 
select title,price,price*2 as newprice
from books;

--Multiply 15*75
select 15*75 as total;

--DIVISION 
--Divide quantity by 4
select order_id,quantity,quantity/4 as dividedquantity
from orders;

--MODULUS (%)
--Find remainder of books price divided by 3 
select title,price,price%3 as remainder
from books;

--(//)- on Python
--Set Operators - Union, Union all, Intersect, Except
--Windows Functions 

```



