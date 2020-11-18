# Database
***
### Relational database
A relational database, also called Relational Database Management System (RDBMS) or SQL database, **stores data in tables and rows** also referred to as records. A relational database works by **linking information from multiple tables through the use of “keys.”** A key is a unique identifier which can be assigned to a row of data contained within a table. 
One significant **advantage** to using an RDBMS is “referential integrity.” 
**Referential integrity** preserves data integrity through **“constraints.”** The three rules that referential integrity enforces are:

1.    A foreign key must have a corresponding primary key. (“No orphans” rule.)
2.    When a record in a primary table is deleted, all related records referencing the primary key must also be deleted, which is typically accomplished by using cascade delete.
3.    If the primary key for a record changes, all corresponding records in other tables using the primary key as a foreign key must also be modified.  This can be accomplished by using a cascade update.

**Querying** the data in a relational database management system is done by using **Structured Querying Language (SQL)**, which is a robust language designed for managing the data housed in a relational database.

SQL has the capabilities to **create, retrieve, update and delete records** and heavily relies on this **primary/foreign key relationship** to identify related data across multiple tables. 

### Non-Relational Database
non-relational database uses a storage model optimized for specific requirements of the type of data being stored. There are four popular non-relational types: document data store, column-oriented database, key-value store and graph database.  Often combinations of these types are used for a single application.
**document data store:** stores string fields and object data values, fields are exposed for querying, do not require all document to maintain identical structure, which provides great flexibility
**column-oriented database:** A columnar data store organizes data into columns, which is conceptually similar to the relational database. The true advantage of a column-family database is in its **denormalized** approach to **structuring sparse data**
**key-value**: stored in key-value pair, simple
**document store**: They don’t assume a particular document structure specified with a schema. The document store is designed to store everyday documents as is, and they allow for complicated querying.
**graph database**:  It’s designed to efficiently store relations between entities. When data is greatly interconnected, such as purchasing and manufacturing systems or referencing catalogs, graph databases are a good solution.

Instead of the Structure Query Language (SQL) used by relational databases, **the NoSQL database uses Object-relational-mapping (ORM)**.  The concept of ORM is the ability to write queries using your preferred programming language.  Some of the more popular ORMs are Java, Javascript, .NET and PHP.

### Summary
##### Advantage of relational database:
- Work with structured data
- limitless indexing flexibility, fast querying time
- Relationships in the system have constraints, which promotes a high level of data integrity
- They provide the ability to write complex SQL queries for data analysis and reporting

##### Advantage of non-relational database:
- They have the ability to store large amounts of data with little structure.
- They provide scalability and flexibility to meet changing business requirements.
- They have the ability to capture all types of data “Big Data” including unstructured data.
- They are best for Rapid Application Development. NoSQL is the best selection for flexible data storage with little to no structure limitations.
