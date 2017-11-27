# An Intro to Databases for Developers

With Exciting Knowledge, Of Course!
------


# Who am I?

## Kyle Tolle

[kyletolle.com](http://kyletolle.com) |
[@kyletolle](https://twitter.com/kyletolle)

Software Engineer on Sabbatical

Member of the industry for 8 years.
------

I've worked on tools supporting satellite simulation, ICBM command and control, geospatial data collection, and mobile wallet applications (order-ahead, loyalty, coupons) for large restaurant chains.


# First off

Ask questions at any time!
------


## Slides for this talk

### Presentation:

[https://kyletolle.github.io/databases_and_developers](https://kyletolle.github.io/databases_and_developers)

### Code:

[https://github.com/kyletolle/databases_and_developers](https://github.com/kyletolle/databases_and_developers)
------


# Overview

------

- Why should you know it?
- You'll use SQL during your career.
- It's used for setting up applications, analytics, etc
- Applications will likely start out with SQL and then portions will migrate to more complex setups as they grow and require it.

------

First, a little background

------

Simple databases

Flat files

https://en.wikipedia.org/wiki/Flat_file_database

------


------

- Databases
  - Relational
    - Schemas
      - Tables
        - https://en.wikipedia.org/wiki/Table_(database)
        - Columns
        - Rows

      - Fields
      - Indexes
      - Relationships
    - Queries
  - Database management system
    - https://en.wikipedia.org/wiki/Database
    - > A general-purpose DBMS allows the definition, creation, querying, update, and administration of databases.
---
  - CAP Theorem
    - https://en.wikipedia.org/wiki/CAP_theorem
    - When you use network partitioning, you must choose between consistency and availability.
    - https://en.wikipedia.org/wiki/Distributed_data_store
    - Information is stored on more than one node, using replication
  - ACID (Atomicity, Consistency, Isolation, Durability)
    - https://en.wikipedia.org/wiki/ACID
  - BASE (Basically Available, Soft state, Eventual consistency)
    - https://en.wikipedia.org/wiki/Eventual_consistency
  - CRUD
    - Create, Read, Update, Delete
  - Transactions
    - https://en.wikipedia.org/wiki/Commit_(data_management)
    - Example of a transaction
    - `BEGIN WORK` - Marks the beginning of a transaction
    - Issue one or more SQL commands
      - Issue `UPDATE` command to modify a row
      - Issue `UPDATE` command to modify another row
    - `COMMIT` - Marks the success and the end of a transaction
  - Rollbacks
    - If an error happens in the middle of one of those SQL commands, a `ROLLBACK` is issues, which reverts all those changes.
    - In a transaction, either all the commands pass and are applied or they all fail and nothing happens.
    - https://en.wikipedia.org/wiki/Rollback_(data_management)#SQL
    - `BEGIN WORK` - Marks the beginning of a transaction
    - Issue one or more SQL commands
      - Issue `UPDATE` command to modify a row
      - Issue `UPDATE` command to modify another row - But this one fails
    - `ROLLBACK` - Marks the failure and the end of the transaction, reverts the changes
  - Savepoints
    - Allow you to roll back a portion of a transaction.
    - Allow implementing complex error recovery in database application
  - Constraints
  - Unique key - A column or set of columns which are guaranteed to be unique in the table
    - https://en.wikipedia.org/wiki/Unique_key
    - Primary key is the key other entity (row in another table) can use to reference this entity.
    - A simple table will have a primary key that is an automatically-increasing integer called `id`
    - No two rows (entities) will have the same `id`
  - Foreign key
    - https://en.wikipedia.org/wiki/Foreign_key
    - Uniquely identifies a row in another table or in this table
    - Why might you want to reference another row in the same table?
    - To allow parent-child relationships.
  - Types of databases
    - NoSQL (Mongo)
    - SQL (MySQL)
    - Key-Value (Redis)
    - Centralized?
      - https://en.wikipedia.org/wiki/Centralized_database
      - Good data integrity
      - Bottlenecks with high traffic
    - Distributed (Mongo)
    - https://en.wikipedia.org/wiki/Centralized_database#Centralised_databases_vs._Distributed_databases
    - Peer to Peer (BitTorrent)
    - Sqlite
    - Flat-file
      - https://en.wikipedia.org/wiki/Flat_file_database
      - CSV is an example
      - Plain text
      - Binary
  - Brands of SQL DBs
    - Oracle, MySQL, Postgres
- Basic Database Design
- How to work with Databases
- It's helpful to do things by hand first.
- Then you understand the benefits that software tools give you.
- Software to interact with databases
  - Rails
    - Rails can create DBs for you when creating a new project.
    - Can customize things
    - DB adapters
  - ORM
    - Object Relational Mapping.
    - Rails follows the Active record pattern

    - https://en.wikipedia.org/wiki/Active_record_pattern
    - https://www.martinfowler.com/eaaCatalog/activeRecord.html
    - > An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data.
    - Martin Fowler
    - In fact, the library that handles this stuff is called `ActiveRecord`
    - http://guides.rubyonrails.org/active_record_basics.html
    - Abstract the SQL behind a domain-specific language (DSL)
    - Make working with the database easier
    - Dynamically reads the tables and their columns and creates SQL queries on the fly
    - The ORM inflates the data from the DB into an object and its attributes
    - This can take a decent bit of time for a complex object with a lot of attributes
    - Some complex or advanced things aren't always possible, so knowing the SQL you need to write can help you get the job done.
    - Understanding the limits of your tools
    - Still need to understand what the query you want should look like
    - Helps you avoid N+1 query explosions
    - The performance might not be what you need for processing data in bulk
    - Dynamically generating the query is slower than having one hardcoded, but it's a tradeoff. Hardcoding a query can make your application more fragile to changes in the future, whereas the dynamic queries will update themselves as things change and the ORM picks up those changes. A hardcoded query might refer to a column that was renamed, for instance.
    - This is qhere something like Sequel is nice, because it gives you the raw data
  - It is worth doing things the "hard way" first.
    - Without the ORM
    - Without Hadoop
    - Without the ML library
    - You can learn the basics
    - Learn what the libraries solve for you
    - Also learn the limitations of the libraries
    - Know how to get around the limitations when you need to.
  - Other gems like Sequel
  - Database migrations at the application level
    - Migrating the database is tricky.
    - Need to be consistent.
    - Need to do it across many machines (many developers)
    - Need to do it differently in different environments
      - PRD has multiple DBs whereas DEV has one local DB.
- Your Data
  - Indexes
    - Pros and Cons
    - Take up space
    - Increase write times
    - Make reads fast
  - Duplication of data
  - Data integrity
  - Application-level integrity vs Database-level integrity
  - Permissions on tables
  - Normalizing
    - http://searchsqlserver.techtarget.com/definition/normalization
    - https://en.wikipedia.org/wiki/Database_normalization
    - https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form
- Security
  - Data sanitization
  - Some is by default, but other things the dev must keep in mind, especially when creating queries on their own.
  - SQL injection
  - How ORMs can help with this
- Other
  - Stored Procedures
  - Putting created_at and updated_at columns on a table.
    - This can be automatically managed by Rails
    - Helps you know when things were updated last, which is really helpful when diagnosing bugs
  - Join tables
  - Database locks
    - Row level
    - Table level
  - Adding colums
    - Adding a column to a large table can lock the entire table for a long time
    - This can cause timeouts in an application
    - Multi-step migrations can help
    - Add a column that is allowed to be null.
    - The table-lock is over quickly
    - Then go through and populate all the rows in the table.
    - Row locks help keep the table from being unresponsive and are released often
    - Then update the table to not allow null values.
  - Triggers
  - Views
  - Data anlytics
    - Generating reports from the data
    - Running custom queries to answer questions from clients
- Scaling
  - Nodes
  - Replication
  - Master/Slave
  - Partitions
  - Fault tolerance
  - Backups
  - Self-hosted vs cloud



------

## So I'm here

to talk about databases
------


## But first

we need some background
------


## Let's start with

what we know
------

Formats:
- CSV
- XML
- JSON
- Binary Files

- Some of these are data-interchange formats. Getting data from one system into another.

- REST is one pattern of implementing a web service
- The above formats can be used to send the data
- https://en.wikipedia.org/wiki/Representational_state_transfer

## Downsides
------


## #1

### ...
------


### How long until

------


## #2

### ...
------


## #3

### ...
------


## #4

### ...
------


## Okay

### we see the disadvantages
------


## But so what?

### We already know how to use these formats

- It exists
- It works
------


## Let's come back to what's important
------


### ...

------

## The Benefits
------


## #1

### ...
------


## #2

### ...
------


## #3

### ...
------

## Let's see a small example

### Show us ...

Start small &amp; build momentum
------


## ...

    some code here

------


### The Idea

- ...
- ...

------


## Database Recap

- Communicate events between your server and a client's using HTTP
- Webhooks accompany an API to enable powerful integrations
- Only real requirement is a web server
- You can do lots of cool stuff with them
------


## Other things to think about

- ...
- ...
------


## What's Next?

- Where can you learn more?
  - Khan Academy?
------


## Links to Code

- ...

------


# Thanks!

