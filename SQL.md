
# Read: 01 - Sql Basics
<hr>

# summary of Sql basic

<hr>

## Introduction to SQL

What is SQL?

SQL, or Structured Query Language, is a language designed to allow both technical and non-technical users query, manipulate, and transform data from a relational database. And due to its simplicity, SQL databases provide safe and scalable storage for millions of websites and mobile applications.

# SELECT queries 101 

<hr>

To retrieve data from a SQL database, we need to write SELECT statements, which are often colloquially refered to as queries.

such as :

`SELECT column, another_column, …
FROM mytable;`

If we want to retrieve absolutely all the columns of data from a table, we can then use the asterisk (*) shorthand in place of listing all the column names individually.

such as :

`SELECT * 
FROM mytable;`


<hr>

# Queries with constraints 

<hr>

In order to filter certain results from being returned, we need to use a WHERE clause in the query. The clause is applied to each row of data by checking specific column values to determine whether it should be included in the results or not.

such as :

`SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;`
    
    
    
  
