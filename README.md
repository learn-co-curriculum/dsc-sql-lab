
# More Practice With SQL Queries - Lab

## Introduction

In this lesson, we'll run through some practice questions to refresh your knowledge of SQL Queries!

## Objectives

You will be able to:

- Use `GROUP BY` statements in SQL to apply aggregate functions like: `COUNT`, `MAX`, `MIN`, and `SUM`
- Decide and perform whichever type of join is best for retrieving desired data
- Use the `HAVING` clause to compare different aggregates
- Write subqueries to decompose complex queries

## Getting Started

As in previous labs, we'll make use of the `sqlite3` library as well as Pandas. By combining them, we'll be able to write queries as Python strings, and make sure that the results are always returned as a Pandas DataFrame. 

We'll start by loading both libraries and connecting to the database we'll be using for this lab, `data.sqlite`. You may remember this database from a previous lab. As a refresher, here's the ERD diagram for this database: 

<img src='images/Database-Schema.png'>

In the cell below:

* Import the necessary libraries `pandas` and `sqlite3` 
* Establish a connection to the database `data.sqlite` 
* Get the `cursor` from the connection and store it in the variable `c` 


```python

```


```python
# __SOLUTION__ 
import pandas as pd
import sqlite3

conn = sqlite3.Connection('data.sqlite')
c = conn.cursor()
```

## Basic Queries

Now, let's review basic SQL queries. In the cell below:

Write a query that gets the first name, last name, phone number, address, and credit limit for all customers in California with a credit limit greater than 25000.00. 


```python
# For the first query, the boilerplate for getting 
# the query into a DataFrame has been provided for you
c.execute(""" """)
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df
```


```python
# __SOLUTION__ 
c.execute("""SELECT contactFirstName, contactLastName,
                    phone, addressLine1,
                    creditLimit
                    FROM customers 
                    WHERE state == 'CA' 
                          and creditLimit > 25000;
          """)
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
print(len(df))
df
```

    10





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
      <td>210500.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Julie</td>
      <td>Murphy</td>
      <td>6505555787</td>
      <td>5557 North Pendale Street</td>
      <td>64600.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Juri</td>
      <td>Hashimoto</td>
      <td>6505556809</td>
      <td>9408 Furth Circle</td>
      <td>84600.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Julie</td>
      <td>Young</td>
      <td>6265557265</td>
      <td>78934 Hillside Dr.</td>
      <td>90700.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Valarie</td>
      <td>Thompson</td>
      <td>7605558146</td>
      <td>361 Furth Circle</td>
      <td>105000.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Julie</td>
      <td>Brown</td>
      <td>6505551386</td>
      <td>7734 Strong St.</td>
      <td>105000.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Brian</td>
      <td>Chandler</td>
      <td>2155554369</td>
      <td>6047 Douglas Av.</td>
      <td>57700.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Sue</td>
      <td>Frick</td>
      <td>4085553659</td>
      <td>3086 Ingle Ln.</td>
      <td>77600.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Steve</td>
      <td>Thompson</td>
      <td>3105553722</td>
      <td>3675 Furth Circle</td>
      <td>55400.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Sue</td>
      <td>Taylor</td>
      <td>4155554312</td>
      <td>2793 Furth Circle</td>
      <td>60300.0</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/expected-output-1.png'>

## Aggregate Functions and GROUP BY

Next, write a query that gets the average credit limit per state.


```python

```


```python
# __SOLUTION__ 
c.execute("""SELECT state, AVG(creditLimit) from customers GROUP BY state""")
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df
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
      <th>AVG(creditLimit)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td></td>
      <td>61839.726027</td>
    </tr>
    <tr>
      <th>1</th>
      <td>BC</td>
      <td>89950.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>CA</td>
      <td>83854.545455</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CT</td>
      <td>57350.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Co. Cork</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Isle of Wight</td>
      <td>93900.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>MA</td>
      <td>70755.555556</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NH</td>
      <td>114200.000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NJ</td>
      <td>43000.000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>NSW</td>
      <td>100550.000000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NV</td>
      <td>71800.000000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>NY</td>
      <td>89966.666667</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Osaka</td>
      <td>81200.000000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>PA</td>
      <td>84766.666667</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Pretoria</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Queensland</td>
      <td>51600.000000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Québec</td>
      <td>48700.000000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Tokyo</td>
      <td>94400.000000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Victoria</td>
      <td>88800.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/expected-output-2.png'>

## JOINs

Now, write a query that uses JOIN statements to get the customer name, customer number, order number, status, and quantity ordered. Print only the head of this DataFrame. 


```python

```


```python
# __SOLUTION__ 
c.execute("""SELECT c.customerName, c.customerNumber, o.orderNumber, o.status, od.quantityOrdered FROM Customers c JOIN Orders o 
ON c.customerNumber = o.customerNumber JOIN OrderDetails od ON od.orderNumber = o.orderNumber""")
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df.head()
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
      <th>orderNumber</th>
      <th>status</th>
      <th>quantityOrdered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Online Diecast Creations Co.</td>
      <td>363</td>
      <td>10100</td>
      <td>Shipped</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Online Diecast Creations Co.</td>
      <td>363</td>
      <td>10100</td>
      <td>Shipped</td>
      <td>30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Online Diecast Creations Co.</td>
      <td>363</td>
      <td>10100</td>
      <td>Shipped</td>
      <td>49</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Online Diecast Creations Co.</td>
      <td>363</td>
      <td>10100</td>
      <td>Shipped</td>
      <td>50</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Blauer See Auto, Co.</td>
      <td>128</td>
      <td>10101</td>
      <td>Shipped</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/joins.png'>

## HAVING and ORDER BY

Now, return the customerName, customerNumber, productName, productCode, and total number ordered for any product a customer has bought 10 or more of cumulatively. Sort the rows in descending order by the quantity ordered. 

**_Hint_**: For this one, you'll need to make use of HAVING, GROUP BY, and ORDER BY -- make sure you get the order of them correct!


```python

```


```python
# __SOLUTION__ 
c.execute("""SELECT c.customerName, c.customerNumber, p.productName,
                    p.productCode, sum(od.quantityOrdered) as TotalOrdered
                    FROM Customers c JOIN Orders o 
                                     ON c.customerNumber = o.customerNumber
                                     JOIN OrderDetails od
                                     ON od.orderNumber = o.orderNumber
                                     JOIN Products p
                                     USING(productCode)
             GROUP BY c.customerNumber, productCode
             HAVING SUM(od.quantityOrdered) >= 10 
             ORDER BY TotalOrdered DESC"""
         )
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df.head()
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
      <th>TotalOrdered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1992 Ferrari 360 Spider red</td>
      <td>S18_3232</td>
      <td>308</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1958 Chevy Corvette Limited Edition</td>
      <td>S24_2840</td>
      <td>245</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1970 Dodge Coronet</td>
      <td>S24_1444</td>
      <td>197</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>1957 Chevy Pickup</td>
      <td>S12_4473</td>
      <td>183</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Euro+ Shopping Channel</td>
      <td>141</td>
      <td>2002 Chevy Corvette</td>
      <td>S24_3432</td>
      <td>174</td>
    </tr>
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/having_order.png'>

## Subqueries

Finally, get the first name, last name, employee number, and office code for employees from offices with less than 5 employees. Print the first five rows of this DataFrame. 


```python

```


```python
# __SOLUTION__ 
c.execute("""select lastName, firstName, employeeNumber, officeCode
                    FROM employees
                    WHERE officeCode IN (SELECT officeCode 
                                                FROM offices 
                                                JOIN employees
                                                USING(officeCode)
                                                GROUP BY 1
                                                HAVING COUNT(employeeNumber) < 5
                                         );
          """
         )
df = pd.DataFrame(c.fetchall())
df.columns = [x[0] for x in c.description]
df.head()
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
  </tbody>
</table>
</div>



#### Expected Output

<img src='images/expected-output-5.png'>

# Summary

In this lesson, we reviewed all the major concepts and keywords associated with SQL queries!
