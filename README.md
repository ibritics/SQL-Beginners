<h1 align="center"> SQL for Beginners Tutorials </h1> 
<h3> This is the repository for the tutorials on SQL, you can find a cheatsheet related to the content on tutorials to help you accelerate your learning </h3>
<p align="left">
  <a href="https://www.youtube.com/watch?v=3-dDTMiZ4SI&list=PLuxrVWOO03T1IC-TlJQTjwEO8yHzqtMqn&index=1" target="_blank" rel="noreferrer">
    <img src="https://img.youtube.com/vi/3-dDTMiZ4SI/maxresdefault.jpg" alt="mysql" width="1080">
  </a>
</p>

<h3> <b>Example Dataset</b> </h3>
<h3> Consider a table named <b>"orders"</b> with the following columns:  </h3> 
order_id (integer) <br>
customer_id (integer)  <br> 
order_date (date)  <br>
total_amount (decimal)  <br>
<h3>And a table named <b>"customers"</b> with the following columns: </h3>
customer_id (integer)  <br>
first_name (string)  <br>
last_name (string)  <br>
email (string)  <br><br>

SELECT

```
SELECT * FROM customers; -- select all columns from the "customers" table 
SELECT first_name, last_name FROM customers; -- select only the "first_name" and "last_name" columns from the "customers" table 
```
WHERE
``` 
SELECT * FROM orders WHERE order_date = '2022-01-01'; -- select rows from the "orders" table where the "order_date" column has a value of '2022-01-01' 
SELECT * FROM orders WHERE total_amount > 100.00; -- select rows from the "orders" table where the "total_amount" column is greater than 100.00 
```
ORDER BY 
```
SELECT * FROM orders ORDER BY order_date DESC; -- sort results from the "orders" table by the "order_date" column in descending order
```
GROUP BY
```
SELECT customer_id, COUNT(*) FROM orders GROUP BY customer_id; -- group results from the "orders" table by the "customer_id" column and count the occurrences 
```
JOIN
```
SELECT customers.first_name, customers.last_name, orders.order_date, orders.total_amount 
FROM customers 
JOIN orders 
ON customers.customer_id = orders.customer_id; -- join the "customers" and "orders" tables based on the "customer_id" column and select specific columns from both tables 
```
INSERT
```
INSERT INTO customers (first_name, last_name, email) VALUES ('John', 'Doe', 'johndoe@example.com'); -- insert a new row into the "customers" table with specific values for the "first_name", "last_name", and "email" columns
```
UPDATE 
```
UPDATE orders SET total_amount = 200.00 WHERE order_id = 100; -- update the "total_amount" column in the "orders" table to 200.00 where the "order_id" column has a value of 100
```
DELETE 
```
DELETE FROM customers WHERE customer_id = 50; -- delete a row from the "customers" table where the "customer_id" column has a value of 50 
```
CREATE TABLE 
```
CREATE TABLE products (product_id integer, product_name string, price decimal); -- create a new "products" table with the specified columns and data types
```
ALTER TABLE 
```
ALTER TABLE orders ADD order_status string; -- add a new column named "order_status" to the "orders" table with a data type of "string" 
ALTER TABLE customers MODIFY email VARCHAR(100); -- modify the data type of the "email" column in the "customers" table to "VARCHAR(100)"
```
DROP TABLE 
```
DROP TABLE orders; -- delete the entire "orders" table and all its contents
```
