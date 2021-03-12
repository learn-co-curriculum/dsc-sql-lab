# SQL - Cumulative Lab

## Introduction

In this lesson, we'll run through some practice questions to reinforce your knowledge of SQL queries.

## Objectives

You will be able to:

- Practice interpreting "word problems" and translating them into SQL queries
- Practice deciding and performing whichever type of `JOIN` is best for retrieving desired data
- Practice using `GROUP BY` statements in SQL to apply aggregate functions like `COUNT`, `MAX`, `MIN`, and `SUM`
- Practice using the `HAVING` clause to compare different aggregates
- Practice writing subqueries to decompose complex queries

## Your Task: Querying a Customer Database

![shelves filled with colorful model cars](images/model_cars.jpg)

Photo by <a href="https://unsplash.com/@bright?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Karen Vardazaryan</a> on <a href="/s/photos/model-car?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

### Business Understanding

Your employer makes miniature models of products such as classic cars, motorcycles, and planes. They want you to pull several reports on different segments of their past customers, in order to better understand past sales as well as determine which customers will receive promotional material.

### Data Understanding

You may remember this database from a previous lab. As a refresher, here's the ERD diagram for this database:

<img src='images/Database-Schema.png'>

The queries you are asked to write will become more complex over the course of the lab.

## Getting Started

As in previous labs, we'll make use of the `sqlite3` library as well as `pandas`. By combining them, we'll be able to write queries as Python strings, then display the results in a conveniently-formatted table.

***Note:*** Throughout this lesson, the only thing you will need to change is the content of the strings containing SQL queries. You do NOT need to modify any of the code relating to `pandas`; this is just to help make the output more readable.

In the cell below, we:

* Import the necessary libraries, `pandas` and `sqlite3`
* Establish a connection to the database `data.sqlite`, called `conn`


```python
import sqlite3
import pandas as pd

conn = sqlite3.Connection("data.sqlite")
```

The basic structure of a query in this lab is:

* Write the SQL query inside of the Python string
* Use `pd.read_sql` to display the results of the query in a formatted table

For example, if we wanted to select a list of all product lines from the company, that would look like this:


```python
q0 = """
SELECT productline
FROM productlines
;
"""

pd.read_sql(q0, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>productLine</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Classic Cars</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Motorcycles</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Planes</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ships</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Trains</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Trucks and Buses</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Vintage Cars</td>
    </tr>
  </tbody>
</table>
</div>



From now on, you will replace `None` within these Python strings with the actual SQL query code.

## Part 1: Basic Queries

First, let's review some basic SQL queries, which do not require any joining, aggregation, or subqueries.

### Query 1: Customers with Credit Over 25,000 in California
Write a query that gets the contact first name, contact last name, phone number, address line 1, and credit limit for all customers in California with a credit limit greater than 25000.00.

(California means that the `state` value is `'CA'`.)

#### Expected Output

<img src='images/expected_output_q1.png'>


```python
q1 = """
SELECT
    contactFirstName,
    contactLastName,
    phone,
    addressLine1,
    creditLimit
FROM customers
WHERE
    state == 'CA'
    AND creditLimit > 25000
;
"""

q1_result = pd.read_sql(q1, conn)
q1_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>contactFirstName</th>
      <th>contactLastName</th>
      <th>phone</th>
      <th>addressLine1</th>
      <th>creditLimit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Susan</td>
      <td>Nelson</td>
      <td>4155551450</td>
      <td>5677 Strong St.</td>
      <td>210500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Julie</td>
      <td>Murphy</td>
      <td>6505555787</td>
      <td>5557 North Pendale Street</td>
      <td>64600</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Juri</td>
      <td>Hashimoto</td>
      <td>6505556809</td>
      <td>9408 Furth Circle</td>
      <td>84600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Julie</td>
      <td>Young</td>
      <td>6265557265</td>
      <td>78934 Hillside Dr.</td>
      <td>90700</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Valarie</td>
      <td>Thompson</td>
      <td>7605558146</td>
      <td>361 Furth Circle</td>
      <td>105000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Julie</td>
      <td>Brown</td>
      <td>6505551386</td>
      <td>7734 Strong St.</td>
      <td>105000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Brian</td>
      <td>Chandler</td>
      <td>2155554369</td>
      <td>6047 Douglas Av.</td>
      <td>57700</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Sue</td>
      <td>Frick</td>
      <td>4085553659</td>
      <td>3086 Ingle Ln.</td>
      <td>77600</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Steve</td>
      <td>Thompson</td>
      <td>3105553722</td>
      <td>3675 Furth Circle</td>
      <td>55400</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Sue</td>
      <td>Taylor</td>
      <td>4155554312</td>
      <td>2793 Furth Circle</td>
      <td>60300</td>
    </tr>
  </tbody>
</table>
</div>



The following code checks that your result is correct:


```python

# Testing which columns are returned
assert list(q1_result.columns) == ['contactFirstName', 'contactLastName', 'phone', 'addressLine1', 'creditLimit']

# Testing how many rows are returned
assert len(q1_result) == 10

# Testing the values in the first result
assert list(q1_result.iloc[0]) == ['Susan', 'Nelson', '4155551450', '5677 Strong St.', 210500]
```

### Query 2: Customers Outside of the USA with "Collect" in Their Name

Write a query that gets the customer name, state, and country, for all customers outside of the USA with `"Collect"` as part of their customer name.

We are looking for customers with names like `"Australian Collectors, Co."` or `"BG&E Collectables"`, where `country` is not `"USA"`.

#### Expected Output

<img src='images/expected_output_q2.png'>


```python
q2 = """
SELECT
    customerName,
    state,
    country
FROM
    customers
WHERE
    customerName LIKE '%Collect%'
    AND country != 'USA'
;
"""

q2_result = pd.read_sql(q2, conn)
q2_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerName</th>
      <th>state</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Australian Collectors, Co.</td>
      <td>Victoria</td>
      <td>Australia</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Clover Collections, Co.</td>
      <td>None</td>
      <td>Ireland</td>
    </tr>
    <tr>
      <th>2</th>
      <td>UK Collectables, Ltd.</td>
      <td>None</td>
      <td>UK</td>
    </tr>
    <tr>
      <th>3</th>
      <td>King Kong Collectables, Co.</td>
      <td>None</td>
      <td>Hong Kong</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Heintze Collectables</td>
      <td>None</td>
      <td>Denmark</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Royal Canadian Collectables, Ltd.</td>
      <td>BC</td>
      <td>Canada</td>
    </tr>
    <tr>
      <th>6</th>
      <td>BG&amp;E Collectables</td>
      <td>None</td>
      <td>Switzerland</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Reims Collectables</td>
      <td>None</td>
      <td>France</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Precious Collectables</td>
      <td>None</td>
      <td>Switzerland</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Salzburg Collectables</td>
      <td>None</td>
      <td>Austria</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Tokyo Collectables, Ltd</td>
      <td>Tokyo</td>
      <td>Japan</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Stuttgart Collectable Exchange</td>
      <td>None</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Bavarian Collectables Imports, Co.</td>
      <td>None</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Australian Collectables, Ltd</td>
      <td>Victoria</td>
      <td>Australia</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Kremlin Collectables, Co.</td>
      <td>None</td>
      <td>Russia</td>
    </tr>
  </tbody>
</table>
</div>



The following code checks that your result is correct:


```python

# Testing which columns are returned
assert list(q2_result.columns) == ['customerName', 'state', 'country']

# Testing how many rows are returned
assert len(q2_result) == 15

# Testing the values in the first result
assert list(q2_result.iloc[0]) == ['Australian Collectors, Co.', 'Victoria', 'Australia']
```

### Query 3: Customers without Null States

Write a query that gets the full address (line 1, line 2, city, state, postal code, country) for all customers where the `state` field is not null.

Here we'll only display the first 10 results.

#### Expected Output

<img src='images/expected_output_q3.png'>


```python
q3 = """
SELECT
    addressLine1,
    addressLine2,
    city,
    state,
    postalCode,
    country
FROM customers
WHERE state IS NOT NULL
;
"""

q3_result = pd.read_sql(q3, conn)
q3_result.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>addressLine1</th>
      <th>addressLine2</th>
      <th>city</th>
      <th>state</th>
      <th>postalCode</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8489 Strong St.</td>
      <td></td>
      <td>Las Vegas</td>
      <td>NV</td>
      <td>83030</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>1</th>
      <td>636 St Kilda Road</td>
      <td>Level 3</td>
      <td>Melbourne</td>
      <td>Victoria</td>
      <td>3004</td>
      <td>Australia</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5677 Strong St.</td>
      <td></td>
      <td>San Rafael</td>
      <td>CA</td>
      <td>97562</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5557 North Pendale Street</td>
      <td></td>
      <td>San Francisco</td>
      <td>CA</td>
      <td>94217</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>4</th>
      <td>897 Long Airport Avenue</td>
      <td></td>
      <td>NYC</td>
      <td>NY</td>
      <td>10022</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>5</th>
      <td>4092 Furth Circle</td>
      <td>Suite 400</td>
      <td>NYC</td>
      <td>NY</td>
      <td>10022</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7586 Pompton St.</td>
      <td></td>
      <td>Allentown</td>
      <td>PA</td>
      <td>70267</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>7</th>
      <td>9408 Furth Circle</td>
      <td></td>
      <td>Burlingame</td>
      <td>CA</td>
      <td>94217</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>8</th>
      <td>149 Spinnaker Dr.</td>
      <td>Suite 101</td>
      <td>New Haven</td>
      <td>CT</td>
      <td>97823</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4658 Baden Av.</td>
      <td></td>
      <td>Cambridge</td>
      <td>MA</td>
      <td>51247</td>
      <td>USA</td>
    </tr>
  </tbody>
</table>
</div>



The following code checks that your result is correct:


```python

# Testing which columns are returned
assert list(q3_result.columns) == ['addressLine1', 'addressLine2', 'city', 'state', 'postalCode', 'country']

# Testing how many rows are returned
assert len(q3_result) == 49

# Testing the values in the first result
assert list(q3_result.iloc[0]) == ['8489 Strong St.', '', 'Las Vegas', 'NV', '83030', 'USA']
```

You have now completed all of the basic queries!

## Part 2: Aggregate and Join Queries

### Query 4: Average Credit Limit by State in USA

Write a query that gets the average credit limit per state in the USA.

The two fields selected should be `state` and `average_credit_limit`, which is the average of the `creditLimit` field for that state.

#### Expected Output

<img src='images/expected_output_q4.png'>


```python
q4 = """
SELECT
    state,
    AVG(creditLimit) AS average_credit_limit
FROM customers
WHERE country = 'USA'
GROUP BY state
;
"""

q4_result = pd.read_sql(q4, conn)
q4_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state</th>
      <th>average_credit_limit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CA</td>
      <td>83854.545455</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CT</td>
      <td>57350.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MA</td>
      <td>70755.555556</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NH</td>
      <td>114200.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NJ</td>
      <td>43000.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NV</td>
      <td>71800.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NY</td>
      <td>89966.666667</td>
    </tr>
    <tr>
      <th>7</th>
      <td>PA</td>
      <td>84766.666667</td>
    </tr>
  </tbody>
</table>
</div>



The following code checks that your result is correct:


```python

# Testing which columns are returned
assert list(q4_result.columns) == ['state', 'average_credit_limit']

# Testing how many rows are returned
assert len(q4_result) == 8

# Testing the values in the first result
first_result_list = list(q4_result.iloc[0])
assert first_result_list[0] == 'CA' 
assert round(first_result_list[1], 3) == round(83854.54545454546, 3)
```

### Query 5: Joining Customers and Orders

Write a query that uses `JOIN` statements to get the customer name, order number, and status for all orders. Refer to the ERD above to understand which tables contain these pieces of information, and the relationship between these tables.

We will only display the first 15 results.

#### Expected Output

<img src='images/expected_output_q5.png'>


```python
q5 = """
SELECT
    c.customerName,
    o.orderNumber,
    o.status
FROM
    customers c
    JOIN orders o
        ON c.customerNumber = o.customerNumber
;
"""
q5_result = pd.read_sql(q5, conn)
q5_result.head(15)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerName</th>
      <th>orderNumber</th>
      <th>status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Atelier graphique</td>
      <td>10123</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Atelier graphique</td>
      <td>10298</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Atelier graphique</td>
      <td>10345</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Signal Gift Stores</td>
      <td>10124</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Signal Gift Stores</td>
      <td>10278</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Signal Gift Stores</td>
      <td>10346</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Australian Collectors, Co.</td>
      <td>10120</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Australian Collectors, Co.</td>
      <td>10125</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Australian Collectors, Co.</td>
      <td>10223</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Australian Collectors, Co.</td>
      <td>10342</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Australian Collectors, Co.</td>
      <td>10347</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>11</th>
      <td>La Rochelle Gifts</td>
      <td>10275</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>12</th>
      <td>La Rochelle Gifts</td>
      <td>10315</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>13</th>
      <td>La Rochelle Gifts</td>
      <td>10375</td>
      <td>Shipped</td>
    </tr>
    <tr>
      <th>14</th>
      <td>La Rochelle Gifts</td>
      <td>10425</td>
      <td>In Process</td>
    </tr>
  </tbody>
</table>
</div>



The following code checks that your result is correct:


```python

# Testing which columns are returned
assert list(q5_result.columns) == ['customerName', 'orderNumber', 'status']

# Testing how many rows are returned
assert len(q5_result) == 326

# Testing the values in the first result
assert list(q5_result.iloc[0]) == ['Atelier graphique', 10123, 'Shipped']
```

### Query 6: Total Payments

Write a query that uses `JOIN` statements to get top 10 customers in terms of total payment amount. Find the customer name, customer number, and sum of all payments made. The results should be ordered by the sum of payments made, starting from the highest value.

The three columns selected should be `customerName`, `customerNumber` and `total_payment_amount`.

#### Expected Output

<img src='images/expected_output_q6.png'>


```python
q6 = """
SELECT
    c.customerName,
    c.customerNumber,
    SUM(p.amount) AS total_payment_amount
FROM
    customers c
    JOIN payments p
        ON c.customerNumber = p.customerNumber
GROUP BY c.customerName
ORDER BY total_payment_amount DESC
LIMIT 10
;
"""
q6_result = pd.read_sql(q6, conn)
q6_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerName</th>
      <th>customerNumber</th>
      <th>total_payment_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>715738.98</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mini Gifts Distributors Ltd.</td>
      <td>124</td>
      <td>584188.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Australian Collectors, Co.</td>
      <td>114</td>
      <td>180585.07</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Muscle Machine Inc</td>
      <td>151</td>
      <td>177913.95</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Dragon Souveniers, Ltd.</td>
      <td>148</td>
      <td>156251.03</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Down Under Souveniers, Inc</td>
      <td>323</td>
      <td>154622.08</td>
    </tr>
    <tr>
      <th>6</th>
      <td>AV Stores, Co.</td>
      <td>187</td>
      <td>148410.09</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Anna's Decorations, Ltd</td>
      <td>276</td>
      <td>137034.22</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Corporate Gift Ideas Co.</td>
      <td>321</td>
      <td>132340.78</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Saveley &amp; Henriot, Co.</td>
      <td>146</td>
      <td>130305.35</td>
    </tr>
  </tbody>
</table>
</div>



The following code checks that your result is correct:


```python

# Testing which columns are returned
assert list(q6_result.columns) == ['customerName', 'customerNumber', 'total_payment_amount']

# Testing how many rows are returned
assert len(q6_result) == 10

# Testing the values in the first result
assert list(q6_result.iloc[0]) == ['Euro+ Shopping Channel', 141, 715738.98]
```

### Query 7: Products that Have Been Purchased 10 or More Times

Write a query that, for each customer, finds all of the products that they have purchased 10 or more times cumulatively. For each record, return  the customer name, customer number, product name, product code, and total number ordered. Sort the rows in descending order by the quantity ordered.

The five columns selected should be `customerName`, `customerNumber`, `productName`, `productCode`, and `total_ordered`, where `total_ordered` is the sum of all quantities of that product ordered by that customer.

**_Hint_**: For this one, you'll need to make use of `HAVING`, `GROUP BY`, and `ORDER BY` — make sure you get the order of them correct!

#### Expected Output

<img src='images/expected_output_q7.png'>


```python

q7 = """
SELECT
    c.customerName,
    c.customerNumber,
    p.productName,
    p.productCode,
    SUM(od.quantityOrdered) AS total_ordered
FROM
    customers c
        JOIN orders o
            ON c.customerNumber = o.customerNumber
        JOIN orderdetails od
            ON od.orderNumber = o.orderNumber
        JOIN products p
            ON p.productCode = od.productCode
GROUP BY c.customerNumber, od.productCode
HAVING SUM(od.quantityOrdered) >= 10
ORDER BY total_ordered
;
"""

# Notes on "HAVING SUM(od.quantityOrdered) >= 10"
# 1. this needs to be HAVING rather than WHERE because it is
#    the result of GROUP BY then an aggregate (SUM)
# 2. this cannot be SUM(total_ordered) because of the internal
#    execution order of SQL. you can't use an alias (the name
#    after AS) in a WHERE or HAVING clause; you have to use
#    the original name (e.g. SUM(od.quantityOrdered)), or you
#    can use a subquery, something like:
"""
SELECT * FROM (
    SELECT
        c.customerName,
        c.customerNumber,
        p.productName,
        p.productCode,
        SUM(od.quantityOrdered) AS total_ordered
    FROM
        customers c
            JOIN orders o
                ON c.customerNumber = o.customerNumber
            JOIN orderdetails od
                ON od.orderNumber = o.orderNumber
            JOIN products p
                ON p.productCode = od.productCode
    GROUP BY c.customerNumber, od.productCode
)
WHERE total_ordered >= 10
ORDER BY total_ordered
;
"""

q7_result = pd.read_sql(q7, conn)
q7_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerName</th>
      <th>customerNumber</th>
      <th>productName</th>
      <th>productCode</th>
      <th>total_ordered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Petit Auto</td>
      <td>314</td>
      <td>1913 Ford Model T Speedster</td>
      <td>S18_2949</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Extreme Desk Decorations, Ltd</td>
      <td>412</td>
      <td>1961 Chevrolet Impala</td>
      <td>S24_4620</td>
      <td>10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>La Rochelle Gifts</td>
      <td>119</td>
      <td>1954 Greyhound Scenicruiser</td>
      <td>S32_2509</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tekni Collectables Inc.</td>
      <td>328</td>
      <td>American Airlines: B767-300</td>
      <td>S700_1691</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The Sharp Gifts Warehouse</td>
      <td>450</td>
      <td>1969 Chevrolet Camaro Z28</td>
      <td>S24_3191</td>
      <td>13</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2526</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>2002 Chevy Corvette</td>
      <td>S24_3432</td>
      <td>174</td>
    </tr>
    <tr>
      <th>2527</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1957 Chevy Pickup</td>
      <td>S12_4473</td>
      <td>183</td>
    </tr>
    <tr>
      <th>2528</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1970 Dodge Coronet</td>
      <td>S24_1444</td>
      <td>197</td>
    </tr>
    <tr>
      <th>2529</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1958 Chevy Corvette Limited Edition</td>
      <td>S24_2840</td>
      <td>245</td>
    </tr>
    <tr>
      <th>2530</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1992 Ferrari 360 Spider red</td>
      <td>S18_3232</td>
      <td>308</td>
    </tr>
  </tbody>
</table>
<p>2531 rows × 5 columns</p>
</div>



The following code checks that your result is correct:


```python

# Testing which columns are returned
assert list(q7_result.columns) == ['customerName', 'customerNumber', 'productName', 'productCode', 'total_ordered']

# Testing how many rows are returned
assert len(q7_result) == 2531

# Testing the values in the first result
assert list(q7_result.iloc[0]) == ['Petit Auto', 314, '1913 Ford Model T Speedster', 'S18_2949', 10]
```

### Query 8: Employees in Offices with Fewer than Five Employees

Finally, get the first name, last name, employee number, and office code for employees from offices with fewer than 5 employees.

***Hint:*** Use a subquery to find the relevant offices.

#### Expected Output

<img src='images/expected_output_q8.png'>


```python
q8 = """
SELECT
    lastName,
    firstName,
    employeeNumber,
    officeCode
FROM employees
WHERE officeCode IN (
    SELECT o.officeCode
    FROM offices o
        JOIN employees e
            ON o.officeCode = e.officeCode
    GROUP BY o.officeCode
    HAVING COUNT(e.employeeNumber) < 5
);
"""
q8_result = pd.read_sql(q8, conn)
q8_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>lastName</th>
      <th>firstName</th>
      <th>employeeNumber</th>
      <th>officeCode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Patterson</td>
      <td>William</td>
      <td>1088</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Firrelli</td>
      <td>Julie</td>
      <td>1188</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Patterson</td>
      <td>Steve</td>
      <td>1216</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tseng</td>
      <td>Foon Yue</td>
      <td>1286</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Vanauf</td>
      <td>George</td>
      <td>1323</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Bott</td>
      <td>Larry</td>
      <td>1501</td>
      <td>7</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Jones</td>
      <td>Barry</td>
      <td>1504</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Fixter</td>
      <td>Andy</td>
      <td>1611</td>
      <td>6</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Marsh</td>
      <td>Peter</td>
      <td>1612</td>
      <td>6</td>
    </tr>
    <tr>
      <th>9</th>
      <td>King</td>
      <td>Tom</td>
      <td>1619</td>
      <td>6</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Nishi</td>
      <td>Mami</td>
      <td>1621</td>
      <td>5</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Kato</td>
      <td>Yoshimi</td>
      <td>1625</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



The following code checks that your result is correct:


```python

# Testing which columns are returned
assert list(q8_result.columns) == ['lastName', 'firstName', 'employeeNumber', 'officeCode']

# Testing how many rows are returned
assert len(q8_result) == 12

# Testing the values in the first result
assert list(q8_result.iloc[0]) == ['Patterson', 'William', 1088, 6]
```

Now that we are finished writing queries, close the connection to the database:


```python
conn.close()
```

## Summary

In this lesson, we produced several data queries for a model car company, mainly focused around its customer data. Along the way, we reviewed many of the major concepts and keywords associated with SQL `SELECT` queries: `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `JOIN`, `SUM`, `COUNT`, and `AVG`.
