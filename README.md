# Simple Database Design

The following outlines some simple steps to take a single table of data and split it into a set of relationship tables within a database or schema. 

Note: This is strictly for demonstration and is not perfect. 

![Unstructured Data Set](https://raw.githubusercontent.com/rydevops/SimpleORM/master/Sample%20Data%20Set.png)

## Understanding this dataset

In the image provided above we have a set of employee records that have not been structured for efficiency. In this simply dataset it's easy to locate specific information for an employee. However, as this data increases the following issues will be found:

*  Lots of duplicate fields (cells) which wastes space in your simple database
*  Searching for employees by specific criteria (e.g. By age) becomes more complex
*  Risk of user input issues araise from duplication of fields
*  Filtering becomes more time consuming with larger datasets

Using this dataset we can break it down into sections (tables) removing duplication and making it easier and faster to search. 

## Understanding Primary and Foreign Keys

What is a `Primary Key` and a `Foreign Key` you ask? In simple terms these are fields that allow our tables to be related to each other and allow us to `join` two (or more) tables together when we search our database after we have deconstructed our table above into individual tables. To better understand this concept we need to dig into database tables a bit further. 

In a database you often create many tables each holding specific details. Often these details are based on real-world objects. We then create relationships between this tables (e.g. An employee has 1 home address). In our example, one table could be an Employee table which contains information about our employees and another table could be an Address table which stores house address information for our employees. If we use the concept of an Employee record and an Address record we need to some how relate these records together to know which address belongs to which employee. For example, where does an employee with an employee ID of 1 live? This is where `primary and foreign keys` come to the rescue. 

A `Primary key` is a field within a table (or record) that must be unique and not missing (or null in database terms). This field (or fields; can be more than one) makes each record (or row) within the table unique. Going back to our Employee table we'd have an employee ID which is often unique across employees. In otherwords a single employee will have an employee ID and that employee ID will never be shared with any other employee. 

A `Foreign key` is a field within a table (or record) that may be unique (e.g. in a one-to-one relationship) but doesn't have to be however for the `foreign key` value to exist (in a record/row) it must have a corresponding `Primary key` record listed in another table. If the `primary key` record doesn't exist the relationship is incomplete and often indicates bad data. This `foreign key` allows us to connect our table back to another table by it's `primary key` (most times it's the `primary key` but it can be another field that is unique in the table as well). So for our example say we have a table called Address and in that table we stored a list of addresses for our employees. If we don't have a `foreign key` with a corresponding `primary key` how do we know what Employee record an address belongs to? Whenever we create an employee record with an employee id (i.e. `Primary key`) we also create a record in our address table with a `foreign key` that matches our employee ID from the Employee table. An now when we search an address we can also ask our employee table who lives at the address we found. 

Although the above example is not completely truthful (e.g. more than 1 employee can live at the same address if the business allows it) it is an example of a one-to-one relationship: a single employee lives at a single address and an address can only belong to a single employee. Pay special attention to the wording of that last sentence as it provided the key that it's a one-to-one relationship.  

The diagram below shows a snippet of our database tables constructed using an employee and address table:
![one-to-one relationship](https://raw.githubusercontent.com/rydevops/SimpleORM/master/one-to-one.png)
The single-dash through the line on each side stands for 1 (i.e. 1-to-1). 
