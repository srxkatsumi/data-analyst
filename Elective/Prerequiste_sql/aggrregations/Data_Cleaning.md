# Data Cleaning

- Clean and re-structure messy data.
- Convert columns to different data types.
- Tricks for manipulating NULLs.

New functions
- Left
- Right
- Length

### LEFT & RIGHT Quizzes
- In the accounts table, there is a column holding the website for each company. The last three digits specify what type of web address they are using. A list of extensions (and pricing) is provided here. Pull these extensions and provide how many of each website type exist in the accounts table.

      SELECT RIGHT(website, 3) AS domain, COUNT(*) num_companies
      FROM accounts
      GROUP BY 1
      ORDER BY 2 DESC;

- There is much debate about how much the name (or even the first letter of a company name) matters. Use the accounts table to pull the first letter of each company name to see the distribution of company names that begin with each letter (or number).

      SELECT LEFT(UPPER(name), 1) AS first_letter, COUNT(*) num_companies
      FROM accounts
      GROUP BY 1
      ORDER BY 2 DESC;

- Use the accounts table and a CASE statement to create two groups: one group of company names that start with a number and a second group of those company names that start with a letter. What proportion of company names start with a letter?

      SELECT SUM(num) nums, SUM(letter) letters
      FROM (SELECT name, CASE WHEN LEFT(UPPER(name), 1) IN ('0','1','2','3','4','5','6','7','8','9') 
                             THEN 1 ELSE 0 END AS num, 
               CASE WHEN LEFT(UPPER(name), 1) IN ('0','1','2','3','4','5','6','7','8','9') 
                             THEN 0 ELSE 1 END AS letter
            FROM accounts) t1;

- Consider vowels as a, e, i, o, and u. What proportion of company names start with a vowel, and what percent start with anything else?

      SELECT SUM(vowels) vowels, SUM(other) other
      FROM (SELECT name, CASE WHEN LEFT(UPPER(name), 1) IN ('A','E','I','O','U') 
                              THEN 1 ELSE 0 END AS vowels, 
                CASE WHEN LEFT(UPPER(name), 1) IN ('A','E','I','O','U') 
                             THEN 0 ELSE 1 END AS other
               FROM accounts) t1;



New functions: 
- Position
- Strpos
- Substr
- Lower
- Upper

### Quizzes POSITION & STRPOS
- Use the accounts table to create first and last name columns that hold the first and last names for the primary_poc.

      SELECT LEFT(primary_poc, STRPOS(primary_poc, ' ') -1 ) first_name, 
      RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name
      FROM accounts;

- Now see if you can do the same thing for every rep name in the sales_reps table. Again provide first and last name columns.

      SELECT LEFT(name, STRPOS(name, ' ') -1 ) first_name, 
             RIGHT(name, LENGTH(name) - STRPOS(name, ' ')) last_name
      FROM sales_reps;


New:
- Concat
- Piping ||

### Quiz Concat

- Each company in the accounts table wants to create an email address for each primary_poc. The email address should be the first name of the primary_poc . last name primary_poc @ company name .com.


      WITH t1 AS (
       SELECT LEFT(primary_poc,     STRPOS(primary_poc, ' ') -1 ) first_name,  RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name, name
       FROM accounts)
      SELECT first_name, last_name, CONCAT(first_name, '.', last_name, '@', name, '.com')
      FROM t1;

- You may have noticed that in the previous solution some of the company names include spaces, which will certainly not work in an email address. See if you can create an email address that will work by removing all of the spaces in the account name, but otherwise your solution should be just as in question 1. Some helpful documentation is here.

      WITH t1 AS (
       SELECT LEFT(primary_poc,     STRPOS(primary_poc, ' ') -1 ) first_name,  RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name, name
       FROM accounts)
      SELECT first_name, last_name, CONCAT(first_name, '.', last_name, '@', REPLACE(name, ' ', ''), '.com')
      FROM  t1;

- We would also like to create an initial password, which they will change after their first log in. The first password will be the first letter of the primary_poc's first name (lowercase), then the last letter of their first name (lowercase), the first letter of their last name (lowercase), the last letter of their last name (lowercase), the number of letters in their first name, the number of letters in their last name, and then the name of the company they are working with, all capitalized with no spaces.

      WITH t1 AS (
       SELECT LEFT(primary_poc,     STRPOS(primary_poc, ' ') -1 ) first_name,  RIGHT(primary_poc, LENGTH(primary_poc) - STRPOS(primary_poc, ' ')) last_name, name
       FROM accounts)
      SELECT first_name, last_name, CONCAT(first_name, '.', last_name, '@', name, '.com'), LEFT(LOWER(first_name), 1) || RIGHT(LOWER(first_name), 1) || LEFT(LOWER(last_name), 1) || RIGHT(LOWER(last_name), 1) || LENGTH(first_name) || LENGTH(last_name) || REPLACE(UPPER(name), ' ', '')
      FROM t1;



Functionality for working with dates 

- TO_DATE
- CAST
- Casting with ::

### Quiz Cast

    SELECT *
    FROM sf_crime_data
    LIMIT 10;

    SELECT date orig_date, (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) || '-' || SUBSTR(date, 4, 2)) new_date
    FROM sf_crime_data;

    SELECT date orig_date, (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) || '-' || SUBSTR(date, 4, 2))::DATE new_date
    FROM sf_crime_data;


New: 
- Coalesce

### Quiz Coalesce

- Run the query entered below in the SQL workspace to notice the row with missing data

      SELECT *
      FROM accounts a
      LEFT JOIN orders o
      ON a.id = o.account_id
      WHERE o.total IS NULL; 
        
- Use COALESCE to fill the accounts.id column with the account.id for the NULL value for the table in 1.
       
      SELECT COALESCE(o.id, a.id) filled_id, a.name, a.website, a.lat, a.long, a.primary_poc, a.sales_rep_id, o.*
      FROM accounts a
      LEFT JOIN orders o
      ON a.id = o.account_id
      WHERE o.total IS NULL;

- Use COALESCE to fill in the orders.account_id column with the account.id for the NULL value for the table in 1

      SELECT COALESCE(o.id, a.id) filled_id, a.name, a.website, a.lat, a.long, a.primary_poc, a.sales_rep_id, COALESCE(o.account_id, a.id) account_id, o.occurred_at, o.standard_qty, o.gloss_qty, o.poster_qty, o.total, o.standard_amt_usd, o.gloss_amt_usd, o.poster_amt_usd, o.total_amt_usd
      FROM accounts a
      LEFT JOIN orders o
      ON a.id = o.account_id
      WHERE o.total IS NULL;

- Use CIAKESCE ti fill in each of the qty and usd columns with 0 for the table in 1

      SELECT COALESCE(o.id, a.id) filled_id, a.name, a.website, a.lat, a.long, a.primary_poc, a.sales_rep_id, COALESCE(o.account_id, a.id) account_id, o.occurred_at, COALESCE(o.standard_qty, 0) standard_qty, COALESCE(o.gloss_qty,0) gloss_qty, COALESCE(o.poster_qty,0) poster_qty, COALESCE(o.total,0) total, COALESCE(o.standard_amt_usd,0) standard_amt_usd, COALESCE(o.gloss_amt_usd,0) gloss_amt_usd, COALESCE(o.poster_amt_usd,0) poster_amt_usd, COALESCE(o.total_amt_usd,0) total_amt_usd
      FROM accounts a
      LEFT JOIN orders o
      ON a.id = o.account_id
      WHERE o.total IS NULL;
- Run the query in 1 with the WHERE removed and count the number of id.

      SELECT COUNT(*)
      FROM accounts a
      LEFT JOIN orders o
      ON a.id = o.account_id;

- Tun the query in 5, but with the COALESCE function used in question 2 through 4.

      SELECT COALESCE(o.id, a.id) filled_id, a.name, a.website, a.lat, a.long, a.primary_poc, a.sales_rep_id, COALESCE(o.account_id, a.id) account_id, o.occurred_at, COALESCE(o.standard_qty, 0) standard_qty, COALESCE(o.gloss_qty,0) gloss_qty, COALESCE(o.poster_qty,0) poster_qty, COALESCE(o.total,0) total, COALESCE(o.standard_amt_usd,0) standard_amt_usd, COALESCE(o.gloss_amt_usd,0) gloss_amt_usd, COALESCE(o.poster_amt_usd,0) poster_amt_usd, COALESCE(o.total_amt_usd,0) total_amt_usd
      FROM accounts a
      LEFT JOIN orders o
      ON a.id = o.account_id;
