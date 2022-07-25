# Data Analyst

JOINs are useful for allowing us to pull data from multiple tables. This is both simple and powerful all at the same time.<br />

With the addition of the JOIN statement to our toolkit, we will also be adding the ON statement.<br />

Let's say I have the following tables: <br />
Accounts; <br />
orders; <br />
region;<br />
sales_reps;<br />
web_events.<br />

To join orders with accounts would be done as follows;<br />
Select the orders table and then join the id of orders and accounts<br />

    SELECT orders.*
    FROM orders
    JOIN accounts
    ON orders.account_id = accounts.id;
    
- The table name is always before the period.
- The column you want from that table is always after the period.


For example, if we want to pull only the account name and the dates in which that account placed an order, but none of the other columns, we can do this with the following query:<br />

    SELECT accounts.name, orders.occurred_at
    FROM orders
    JOIN accounts
    ON orders.account_id = accounts.id;


### Quiz questions 
- pulling all the data from the accounts table, and all the data from the orders table.

      SELECT 
        orders.*,  
        accounts.*
      FROM orders
      JOIN accounts
      ON orders.account_id = accounts.id; 
      
- pulling standard_qty, gloss_qty, and poster_qty from the orders table, and the website and the primary_poc from the accounts table.
      SELECT 
          orders.standard_qty,
          orders.gloss_qty,
          orders.poster_qty,
          accounts.website,
          accounts.primary_poc  
      FROM orders
      JOIN accounts
      ON orders.account_id = accounts.id; 


### Entity Relationship Diagrams

![alt text]([http://url/to/img.png](https://video.udacity-data.com/topher/2017/October/59e946e7_erd/erd.png))
