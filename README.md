/*Date created: August 10th, 2023
created by: Abhiraj Singh Dadiyan
description: the code here contains my answers/solutions to ten different questions from Brandon Southern's Test Your SQL Skills course */

/*Q1 Write a query to report customer ids and product names and return the order by the product name alphabetically
two tables are products and customers, products consist of product key and product name where product_key is foriegn key, 
customers consist of customer_id and product_key there is no primary key, it may contain duplicates, customer id is not null, product key is primary key*/
```
SELECT
	 c.customer_id
 	,p.product_name 
FROM
	 customers c
INNER JOIN 	
  	products p
ON
	c.product_key = p.product_key;
```
/*Q2 To be done*/

/*Q:3 Product_key number 2 is being replaced by a new product with a product_key of 4. 
But this is only happening for customer_id number 3. Write a SQL statement to update the appropriate records in the table.
Return the results of the table after you've updated your records. Order the results by customer_id (from least to greatest) 
and then by product_key (from least to greatest) */
```
UPDATE
	customers_for_update
SET
	product_key = 4
WHERE
	product_key = 2
AND
  	customer_id =3;
   
SELECT
	  customer_id
   ,product_key
FROM
	customers_for_update
ORDER BY
	customer_id, product_key;
```

/* Q4  Write a SQL query that returns all of the customer_sales records where the purchase_amount is greater
than the average purchase_amount (across all purchases).
Return the customer_name, the purchase_amount, and the average purchase_amount (across all records). */
```
SELECT
    cs.customer_name, 
    cs.purchase_amount,
    (SELECT AVG(purchase_amount) FROM customer_sales) AS average_purchase_amount
FROM 
    customer_sales AS cs
WHERE 
    cs.purchase_amount > (SELECT AVG(purchase_amount) FROM customer_sales);
```

/*Q5 The column name "product_key" has been confusing some users. The business thinks
that this field should be called "product_id" instead. Write a SQL statement to update the table to update the field name to be "product_id" */
```
ALTER TABLE
  customers_for_col_name_change 
        RENAME COLUMN product_key TO product_id;
SELECT
	*
FROM
	customers_for_col_name_change;
```

/* Q6  Write a SQL query to show the customer_id along with the name of the product that they purchased.
 All customers should be shown, even if a matching product can't be found. If a product can't be found, 
 you should output the product_name as, "unknown-product" Return the result order by the customer_id (least to greatest)
 and then by product_name (alphabetically) */
```
SELECT 
    c.customer_id
  ,COALESCE(p.product_name, 'unknown-product') AS    product_name
FROM 
    customers c
LEFT JOIN 
    products p 
ON 
	c.product_key = p.product_key
ORDER BY 
    c.customer_id, 
    COALESCE(p.product_name, 'unknown-product');
```

/* Q7 Write a SQL query to show the all customer_ids and all product_names. 
 If a product_name exists but was never purchased, it should still be displayed in the results. 
 Return the result order by the customer_id (least to greatest) and then by product_name (alphabetically) */
```
SELECT 
    DISTINCT c.customer_id,
    p.product_name
FROM 
    products p
LEFT JOIN 
    customers c 
ON 
	p.product_key = c.product_key
ORDER BY 
    c.customer_id, 
    p.product_name;
```

/* Q8 Write a SQL query to show all of the employee_names along with the employee's manager's name. 
If the employee doesn't have a manager, leave the manager's name as "null"
Return the result order by the employee's name (alphabetically), then by the manager's name (alphabetically) */
```
SELECT
    e1.employee_name AS "Employee Name",
    COALESCE(e2.employee_name, 'null') AS "Manager's Name"
FROM
    employees e1
LEFT JOIN
    employees e2 ON e1.manager_id = e2.employee_id
ORDER BY
    e1.employee_name ASC,
    COALESCE(e2.employee_name, 'null') ASC;
```

/*Q9 Write a SQL query to returned the min, max, average, and total sales across all customers in the customer_sales table.
 Your results should be limited to 2 decimal places */
```  
SELECT
   ROUND(MIN(purchase_amount),2)
  ,ROUND(MAX(purchase_amount),2)
  ,ROUND(AVG(purchase_amount),2)
  ,ROUND(SUM(purchase_amount),2)
FROM
	customer_Sales
```

/*Q10 Write a SQL query to returned the each customer_name from the customer_sales table.
When the customer_name is either "brad" or "rick", your result set should display their name in all capitalized letters. */
```
SELECT 
    CASE 
        WHEN LOWER(customer_name) = 'brad' THEN UPPER(customer_name)
        WHEN LOWER(customer_name) = 'rick' THEN UPPER(customer_name)
        ELSE customer_name 
    END AS customer_name
FROM 
    customer_sales;
```
