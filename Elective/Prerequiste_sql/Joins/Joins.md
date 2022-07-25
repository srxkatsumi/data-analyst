# Data Analyst

JOINs are useful for allowing us to pull data from multiple tables. This is both simple and powerful all at the same time.<br />

With the addition of the JOIN statement to our toolkit, we will also be adding the ON statement.<br />

#### Primary and Foreign Keys

primary keys - are unique for every row in a table. These are generally the first column in our database (like you saw with the id column for every table in the Parch & Posey database). <br />

foreign keys - are the primary key appearing in another table, which allows the rows to be non-unique. <br />


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

![alt text](https://video.udacity-data.com/topher/2017/October/59e946e7_erd/erd.png)

    SELECT *
    FROM web_events
    JOIN accounts
    ON web_events.account_id = accounts.id
    JOIN orders
    ON accounts.id = orders.account_id

#### Aliases for Columns in Resulting Table

While aliasing tables is the most common use case. It can also be used to alias the columns selected to have the resulting table reflect a more readable name.<br />

    Select t1.column1 aliasname, t2.column2 aliasname2
    FROM tablename AS t1
    JOIN tablename2 AS t2

###### Questions
- Provide a table for all web_events associated with account name of Walmart. There should be three columns. Be sure to include the primary_poc, time of the event, and the channel for each event. Additionally, you might choose to add a fourth column to assure only Walmart events were chosen.

        SELECT 
            a.primary_poc, 
            w.occurred_at, w.channel, 
            a.name
        FROM web_events w
        JOIN accounts a
        ON w.account_id = a.id
        WHERE a.name = 'Walmart';

- Provide a table that provides the region for each sales_rep along with their associated accounts. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

        SELECT 
            r.name region, 
            s.name rep, 
            a.name account
        FROM sales_reps s
        JOIN region r
        ON s.region_id = r.id
        JOIN accounts a
        ON a.sales_rep_id = s.id
        ORDER BY a.name;

- Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. Your final table should have 3 columns: region name, account name, and unit price. A few accounts have 0 for total, so I divided by (total + 0.01) to assure not dividing by zero.

        SELECT 
            r.name region, 
            a.name account,
            o.total_amt_usd/(o.total + 0.01) unit_price
        FROM region r
        JOIN sales_reps s
        ON s.region_id = r.id
        JOIN accounts a
        ON a.sales_rep_id = s.id
        JOIN orders o
        ON o.account_id = a.id;
        
   ![alt text](https://i.stack.imgur.com/VQ5XP.png)
<br />
Imagem tirada do <a href ="https://stackoverflow.com/questions/5706437/whats-the-difference-between-inner-join-left-join-right-join-and-full-join"> stack overflow </a> 25/07/22- 09:26 AM <br />

Quiz 
1 - Provide a table that provides the region for each sales_rep along with their associated accounts. This time only for the Midwest region. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

    SELECT r.name region, s.name rep, a.name account
    FROM sales_reps s
    JOIN region r
    ON s.region_id = r.id
    JOIN accounts a
    ON a.sales_rep_id = s.id
    WHERE r.name = 'Midwest'
    ORDER BY a.name;

2 - Provide a table that provides the region for each sales_rep along with their associated accounts. This time only for accounts where the sales rep has a first name starting with S and in the Midwest region. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

    SELECT r.name region, s.name rep, a.name account
    FROM sales_reps s
    JOIN region r
    ON s.region_id = r.id
    JOIN accounts a
    ON a.sales_rep_id = s.id
    WHERE r.name = 'Midwest' AND s.name LIKE 'S%'
    ORDER BY a.name;

3 - Provide a table that provides the region for each sales_rep along with their associated accounts. This time only for accounts where the sales rep has a last name starting with K and in the Midwest region. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

    SELECT r.name region, s.name rep, a.name account
    FROM sales_reps s
    JOIN region r
    ON s.region_id = r.id
    JOIN accounts a
    ON a.sales_rep_id = s.id
    WHERE r.name = 'Midwest' AND s.name LIKE '% K%'
    ORDER BY a.name;

4 - Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. However, you should only provide the results if the standard order quantity exceeds 100. Your final table should have 3 columns: region name, account name, and unit price.

    SELECT r.name region, a.name account, o.total_amt_usd/(o.total + 0.01) unit_price
    FROM region r
    JOIN sales_reps s
    ON s.region_id = r.id
    JOIN accounts a
    ON a.sales_rep_id = s.id
    JOIN orders o
    ON o.account_id = a.id
    WHERE o.standard_qty > 100;

5 - Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. However, you should only provide the results if the standard order quantity exceeds 100 and the poster order quantity exceeds 50. Your final table should have 3 columns: region name, account name, and unit price. Sort for the smallest unit price first.

    SELECT r.name region, a.name account, o.total_amt_usd/(o.total + 0.01) unit_price
    FROM region r
    JOIN sales_reps s
    ON s.region_id = r.id
    JOIN accounts a
    ON a.sales_rep_id = s.id
    JOIN orders o
    ON o.account_id = a.id
    WHERE o.standard_qty > 100 AND o.poster_qty > 50
    ORDER BY unit_price;

6 - Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. However, you should only provide the results if the standard order quantity exceeds 100 and the poster order quantity exceeds 50. Your final table should have 3 columns: region name, account name, and unit price. Sort for the largest unit price first.

    SELECT r.name region, a.name account, o.total_amt_usd/(o.total + 0.01) unit_price
    FROM region r
    JOIN sales_reps s
    ON s.region_id = r.id
    JOIN accounts a
    ON a.sales_rep_id = s.id
    JOIN orders o
    ON o.account_id = a.id
    WHERE o.standard_qty > 100 AND o.poster_qty > 50
    ORDER BY unit_price DESC;

7 - What are the different channels used by account id 1001? Your final table should have only 2 columns: account name and the different channels. You can try SELECT DISTINCT to narrow down the results to only the unique values.

    SELECT DISTINCT a.name, w.channel
    FROM accounts a
    JOIN web_events w
    ON a.id = w.account_id
    WHERE a.id = '1001';
    Find all the orders that occurred in 2015. Your final table should have 4 columns: occurred_at, account name, order total, and order total_amt_usd.
    SELECT o.occurred_at, a.name, o.total, o.total_amt_usd
    FROM accounts a
    JOIN orders o
    ON o.account_id = a.id
    WHERE o.occurred_at BETWEEN '01-01-2015' AND '01-01-2016'
    ORDER BY o.occurred_at DESC;
    
