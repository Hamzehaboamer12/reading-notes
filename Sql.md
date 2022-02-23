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

![Alt text](where%20oper.png)


<hr>

# Filtering and sorting Query results


<hr>

## Ordering results 

To help with this, SQL provides a way to sort your results by a given column in ascending or descending order using the ORDER BY clause.

such as :

`SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
`

## Limiting results to a subset 

The LIMIT will reduce the number of rows to return, and the optional OFFSET will specify where to begin counting the number rows from.

such as :

`SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;`

* DISTINCT keyword will blindly remove duplicate rows 

<hr>

# Multi-table queries with JOINs 

<hr>

## Database normalization 

Database normalization is useful because it minimizes duplicate data in any single table, and allows for data in the database to grow independently of each other (ie. Types of car engines can grow independent of each type of car). As a trade-off, queries get slightly more complex since they have to be able to find data from different parts of the database, and performance issues can arise when working with many large tables.

## Multi-table queries with JOINs

Tables that share information about a single entity need to have a primary key that identifies that entity uniquely across the database. One common primary key type is an auto-incrementing integer (because they are space efficient), but it can also be a string, hashed value, so long as it is unique.

Using the JOIN clause in a query, we can combine row data across two separate tables using this unique key. The first of the joins that we will introduce is the INNER JOIN.

such as :

`SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table
ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;`

* the INNER JOIN is written simply as a JOIN. These two are equivalent


<hr>

# Inserting rows 

<hr>

What is a Schema?

We previously described a table in a database as a two-dimensional set of rows and columns, with the columns being the properties and the rows being instances of the entity in the table. In SQL, the database schema is what describes the structure of each table, and the datatypes that each column of the table can contain.

Inserting new data
When inserting data into a database, we need to use an INSERT statement, which declares which table to write into, the columns of data that we are filling, and one or more rows of data to insert

such as :

`INSERT INTO mytable
VALUES (value_or_expr, another_value_or_expr, …),
(value_or_expr_2, another_value_or_expr_2, …),
…;
`


In some cases, if you have incomplete data and the table contains columns that support default values, you can insert rows with only the columns of data you have by specifying them explicitly.


`INSERT INTO mytable
(column, another_column, …)
VALUES (value_or_expr, another_value_or_expr, …),
(value_or_expr_2, another_value_or_expr_2, …),
…;`

<hr>

# Updating rows

<hr>

In addition to adding new data, a common task is to update existing data, which can be done using an UPDATE statement

such as :

`UPDATE mytable
SET column = value_or_expr,
other_column = another_value_or_expr,
…
WHERE condition;`


<hr>

# Deleting rows

<hr>

When you need to delete data from a table in the database, you can use a DELETE statement, which describes the table to act on, and the rows of the table to delete through the WHERE clause.


`DELETE FROM mytable
WHERE condition;`

<hr>

# Creating tables
  
<hr>

When you have new entities and relationships to store in your database, you can create a new database table using the CREATE TABLE statement.


`CREATE TABLE IF NOT EXISTS mytable (
column DataType TableConstraint DEFAULT default_value,
another_column DataType TableConstraint DEFAULT default_value,
…
);`


## Table data types

![Alt text](datatype%20.png)


## Table constraints 

![Alt text](table%20const.png)


<hr>


# Altering tables 

<hr>

As your data changes over time, SQL provides a way for you to update your corresponding tables and database schemas by using the ALTER TABLE statement to add, remove, or modify columns and table constraints.


## Adding columns 

such as :

`ALTER TABLE mytable
ADD column DataType OptionalTableConstraint
DEFAULT default_value;`

## Removing columns 

such as :

`ALTER TABLE mytable
DROP column_to_be_deleted;`

## Renaming the table

such as :

`ALTER TABLE mytable
RENAME TO new_table_name;`

<hr>

# Dropping tables 

<hr>

some rare cases, you may want to remove an entire table including all of its data and metadata, and to do so, you can use the DROP TABLE statement, which differs from the DELETE statement in that it also removes the table schema from the database entirely.

such as :

`DROP TABLE IF EXISTS mytable;`




    
   