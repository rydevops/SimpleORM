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

## One-to-One and One-to-Many Relationships

When creating tables from a dataset we need to consider the different types of relationships between these tables. For this discussion we will talk about `one-to-one relationships` and `one-to-many relationships`. There is a third relationship called a `many-to-many relationship` which is a bit more complex and will have its own section. 

In our example above we create a `one-to-one relationship` by saying a single employee has at most one address and one address has at most one employee. This is often referred to as a business constraint and in this case the business is essentially stating that they will never have an two employees with the same address. When this type of relationship is created often the `primary key` and `foreign key` exists and are both marked as `unique non-null keys`. If we try to insert a second record with a `foreign key` that already exists in our application/database we will be deny the transaction as this breaks the relationship. 

What if our business rules change and now we can have more than one employee with the same house address (e.g. A brother and a sister now work for our business)? We now need to create a new relationship called a `one-to-many` relationship. In this new relationship which table has the many records and which table as the single record? This is often up for debate depending on the table however in our case the decision is simple. To figure out which table has the many side (or has more than one record) ask yourself which table should I create a record in such that I don't duplicate my data? For example, assuming we think our Address table should be the many side what would that look like? In the table below we created two address records referencing the two employees who live at the same address. Do we have duplicate data in either table now? Yes we do. The Address table is storing the same address information twice and the only field that is unique is the address id. In this case, our assumption is incorrect. If we were to read this relationship from left-to-right we are saying one employee is associated with one address (based on `primary/foreign keys`) and so this is still a one-to-one relationship. So instead of adding the employee_id to the address table we will add the address_id to the employee record instead. In the image below (second figure) we can now see that the only thing duplicated is the address_id in the employee table and this is exactly what we want. In this case we have created a many-to-one relationship (which is the same thing as a one-to-many just read in reverse order). If we read this from right-to-left now we can say an address can have one or more employees associated with it (i.e. one-to-many relationship)

![incorrect-one-to-many-relationship](https://raw.githubusercontent.com/rydevops/SimpleORM/master/incorrect-one-to-many.png)

*incorrect one-to-many relationship*

![correct-one-to-many-relationship](https://raw.githubusercontent.com/rydevops/SimpleORM/master/correct-one-to-many.png)

*correct one-to-many relationship*

## Many to Many Relationships

