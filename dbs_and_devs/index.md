#  Databases and Developers

SELECT * FROM exciting_knowledge;
------


First off

## Ask questions

### at any time!
------


## Who am I?

# Kyle Tolle

[kyletolle.com](http://kyletolle.com) |
[@kyletolle](https://twitter.com/kyletolle)

Software Engineer on Sabbatical
___
Member of the software industry for 8 years.
------


### Not a database expert

but I want to

## share

some of what

## I've learned
------


## Slides for this talk

### Presentation:

[https://kyletolle.github.io/dbs_and_devs_talk](https://kyletolle.github.io/dbs_and_devs_talk)

### Code:

[https://github.com/kyletolle/dbs_and_devs_talk](https://github.com/kyletolle/dbs_and_devs_talk)
------


# Past Jobs

- tools supporting satellite simulation
- ICBM command and control system
- geospatial data collection
- mobile wallet applications
___
mobile wallet includes things like order-ahead, loyalty, coupons for large restaurant chains.
These jobs are diverse but they all had something in common: using databases.
------


# Overview

- ...
- ...
- Fill this out more after the talk has taken shape
------


Let's start at

## The Very Beginning
___
Sometimes we hop into the middle of things, and it's good to think back on the beginning and understand the context in which things came about.
------


### Humans are

## knowledge-hunters

&

## data-gatherers
___
We take in the world as it is and can see it how it will be later.
We see a line on the ground and know a deer walks here often.
Then we wait there, because we know we will see the deer in the future.
------


## We like to

- Collect data
- Store it
- Process it
___
We gather information from the world around us and do a lot with it.
------


## Software crunches

# data
------


## Data needs

# a home
___
In order to do something useful with software, we need data.
We need to store that data somewhere.
That somewhere is usually databases.
------


## a.k.a.

# A Data Store
------


### Data's earliest home

# The Flat File
------


`cat students.csv`

{:.text}
    Id, Name, Grade
    1,  Ted,  B
    2,  Stan, A-
    3,  Fred, C++
    4,  Ned,  5%
------


## Easy peasy

Some things don't need to be complicated
------


### Flat Files

can be used for

## Data Interchange
___
Passing data between systems
------


## Common flat file types

- CSV
- HTML
- XML
- JSON
- ini
- conf
___
Flat files allow us to pass data between systems.
They allow us to store simple information when anything more complex would be overkill.
------


Then, things grow

## Complicated
------


### Want to

- know more
- ask questions
- give access
------


## Use the data for

- Web application
- Report generation
- Analysis performation
------


Fortunately for us,

some smart folks

## invented the

# database
------


Popular DBs according to Devs

![Popular DBs chart](./popular_dbs.png)

<small><https://insights.stackoverflow.com/survey/2017#technology-databases></small>
------


Many common DBs use the

## Relational Model

![Relational Model Concepts](./relational_model_concepts.png)*

<small>*: By <a href="//commons.wikimedia.org/wiki/User:AutumnSnow" title="User:AutumnSnow">User:AutumnSnow</a> - <span class="int-own-work" lang="en">Own work</span>, <a href="http://creativecommons.org/licenses/by-sa/3.0/" title="Creative Commons Attribution-Share Alike 3.0">CC BY-SA 3.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=1313684">Link</a><small>
------


In other words, we have a

# Table

with

## Rows & Columns
------

# SQL is a <abbr title="domain-specific language">DSL</abbr>


for managing data in a

## Relational DB
------


Remember our student data?
------


{:.text}
    Id, Name, Grade
    1,  Ted,  B
    2,  Stan, A-
    3,  Fred, C++
    4,  Ned,  5%
------


## Put a database on it!

![Put a bird on it](./put_a_bird_on_it.jpg)
___
Portlandia!
------


`students`

| -  +   -  +   -   |
| id | name | grade |
| -  +   -  +   -   |
| 1  | Ted  | B     |
| 2  | Stan | A-    |
| 3  | Fred | C++   |
| 4  | Ned  | 5%    |
| -  +   -  +   -   |
------


The `table` has a `schema`.

Each `row` is a `record`.

Each `column` is an `attribute`.
------


Let's drop into

# Postgres
------


# Set up Postgres

{:.sh}
    brew install postgres
    brew services start postgresql
    createdb dbs_and_devs_talk
    psql dbs_and_devs_talk
------


# Create a Table

    CREATE TABLE students(
      id SERIAL PRIMARY KEY,
      name TEXT NOT NULL,
      grade TEXT NOT NULL
    );
------


# Insert Data

    INSERT INTO students (name, grade) values ('Ted', 'B');
    INSERT INTO students (name, grade) values ('Stan', 'A-');
    INSERT INTO students (name, grade) values ('Fred', 'C++');
    INSERT INTO students (name, grade) values ('Ned', '5%');
------


## View Data

`SELECT * FROM students;`

{:.text}
     id | name | grade
    ====+======+=======
      1 | Ted  | B
      2 | Stan | A-
      3 | Fred | C++
      4 | Ned  | 5%
    (4 rows)
------

## Create a Relationship

    CREATE TABLE backpacks(
      id SERIAL PRIMARY KEY,
      color TEXT NOT NULL,
      student_id INT references students(id)
    );
------

# Insert Data

    INSERT INTO backpacks (color, student_id) values('blue', 1);
    INSERT INTO backpacks (color, student_id) values('red', 2);
    INSERT INTO backpacks (color, student_id) values('grey', 3);
    INSERT INTO backpacks (color, student_id) values('green', 4);
    INSERT INTO backpacks (color, student_id) values('clear', 1);
------

## View Data

{:.text}
     id | color | student_id
    ====+=======+============
      1 | blue  |          1
      2 | red   |          2
      3 | grey  |          3
      4 | green |          4
      5 | clear |          1
    (5 rows)
------

## Backback colors for a Student

    SELECT color FROM backpacks WHERE student_id = 1;

{:.text}
     color
    =======
     blue
     clear
    (2 rows)
------

## Backpacks for all students

    SELECT s.name, b.color FROM students as s
      JOIN backpacks as b ON s.id = b.student_id
      ORDER BY s.name;

{:.text}
     name | color
    ======+=======
     Fred | grey
     Ned  | green
     Stan | red
     Ted  | blue
     Ted  | clear
    (5 rows)
------

## Quit Postgres

`\q`
------


Do stuff

## the hard way

for a while
------

![I dare you not to use your tools](no_tools.jpg)
------


### Allows you to understand

- the benefits of your tools
- the limits of your tools
- how to work around those limits
------


Plus, learn to

## channel your anger

![Angry Anakin](./angry_anakin.jpg)
------


# Useful Tools

- [pgAdmin 4](https://www.pgadmin.org/download/pgadmin-4-macos/)
- [Postico](https://eggerapps.at/postico/)
- ORMs
___
Great for managing the database without typing everything yourself.
------


# ORM

Object-Relational Mapping
------


Maps

## database records

to

## object instances
___
Inflates the DB data into an instantiated class
------


# Ruby on Rails

follows the

## [Active Record pattern](https://www.martinfowler.com/eaaCatalog/activeRecord.html)
------


# Install Ruby

{:.sh}
    brew install rbenv
    rbenv install 2.4.2
    rbenv global 2.4.2
------


## Ensure [SQLite3](https://www.sqlite.org/) is installed

`sqlite3 --version`
------


# Install Rails

    gem install rails
    rails --version

<br />

<small>We expect to see `Rails 5.1.4`</small>
------


# Create Rails app

    rails new blog
    cd blog
------


# Create Posts

{:.sh}
    rails generate scaffold Post title:string body:text
------


    class CreatePosts < ActiveRecord::Migration[5.1]
      def change
        create_table :posts do |t|
          t.string :title
          t.text :body

          t.timestamps
        end
      end
    end
------


# Run db migration

{:.sh}
    rails db:migrate

------


## Enter Ruby console

{:.sh}
    rails console
------


## Active Record is a DSL

We can modify SQL data with it
------


# Create a new post

{:.ruby}
    Post.create title: 'First!', body: 'Viral to the max.'
------


The console shows us

## SQL queries

for various

## Active Record methods
------


## Output for create

{:.text}
    (0.1ms)  begin transaction
    SQL (0.4ms)  INSERT INTO "posts"
      ("title", "body", "created_at", "updated_at")
      VALUES (?, ?, ?, ?)
      [
        ["title", "First!"],
        ["body", "Viral to the max."],
        ["created_at", "2017-11-29 01:37:50.231847"],
        ["updated_at", "2017-11-29 01:37:50.231847"]
      ]
    (0.7ms)  commit transaction
    => #<Post id: 1, title: "First!",
         body: "Viral to the max.",
         created_at: "2017-11-29 01:37:50",
         updated_at: "2017-11-29 01:37:50">
------


## Let's break it down

- Transactions
- Queries
- Duration
- Conventions
------


# [Transactions](https://en.wikipedia.org/wiki/Database_transaction)

{:.text}
    (0.1ms)  begin transaction
    ...
    (0.7ms)  commit transaction
------


Multiple queries either

## all happen

or

## none happen
------


An error in a query causes a

## Rollback
------


Provides protection from

## Errors

causing

## Inconsistent Data
------


Important when

## modifying many records

in

## one logical change
___
Like transferring money from one bank account to another.
One must be credited and the other must be debited at the same time.
Either both get updated or neither do.
------


# Queries

{:.text}
    SQL (0.4ms)  INSERT INTO "posts"
      ("title", "body", "created_at", "updated_at")
      VALUES (?, ?, ?, ?)
      [
        ["title", "First!"],
        ["body", "Viral to the max."],
        ["created_at", "2017-11-29 01:37:50.231847"],
        ["updated_at", "2017-11-29 01:37:50.231847"]
      ]
------


## Dynamically generated

Active Record analyzes the table and

generates the correct query
------


# Duration

{:.text}
    SQL (0.4ms)  INSERT INTO "posts" ...
------


Active Record measures

## how long

each query takes to run
------


Can help

## profile queries

and find slow ones
------


# Conventions
------


## Naming

  Table name (`posts`) is pluralized

  Class name (`Post`) is singular
------

Handles

## complex cases

like

### `people` & `Person`
------


## Timestamps
------

Notice the

### `updated_at` & `created_at`

columns?
------


Active Record automatically tracks

## creation & modification

times of each record
------


# Other Examples
------

# Viewing

`Post.last`

{:.text}
    SELECT  "posts".* FROM "posts"
      ORDER BY "posts"."id" DESC LIMIT ?  [["LIMIT", 1]]
    => #<Post id: 1,
              title: "First!",
              body: "Viral to the max.",
              created_at: "2017-11-29 01:37:50",
              updated_at: "2017-11-29 01:37:50">
------


# Counting

`Post.count`

{:.text}
    SELECT COUNT(*) FROM "posts"
    => 1
------


# Destroying

`Post.destroy_all`

{:.text}
    SELECT "posts".* FROM "posts"
    DELETE FROM "posts" WHERE "posts"."id" = ?  [["id", 1]]
------


## Just a sample

This is not an exhaustive list
------


# Exit Rails Console

`exit`
------


## Add a Column to Post

{:.sh}
    rails g migration AddCurrentSongToPosts current_song:string

{:.ruby}
    class AddCurrentSongToPosts < ActiveRecord::Migration[5.1]
      def change
        add_column :posts, :current_song, :string
      end
    end
------


Migrations support

## rolling back schema
------


## Create a Relationship

{:.sh}
    rails g model like post:references author:references

{:.ruby}
    class CreateLikes < ActiveRecord::Migration[5.1]
      def change
        create_table :likes do |t|
          t.references :post, foreign_key: true
          t.references :author, foreign_key: true

          t.timestamps
        end
      end
    end

- Adds indexes for `author_id`, `post_id`
------


## Complex Relationship

An author can publish many posts, and

a post can have many authors
------


## Complex Relationship, cont.

{:.sh}
    rails g model author name:string email:string

{:.ruby}
    class CreateAuthors < ActiveRecord::Migration[5.1]
      def change
        create_table :authors do |t|
          t.string :name
          t.string :email

          t.timestamps
        end
      end
    end
------


## Complex Relationship, cont.

{:.sh}
    rails g migration CreateJoinTableAuthorsPosts author post

{:.ruby}
    class CreateJoinTableAuthorsPosts < ActiveRecord::Migration[5.1]
      def change
        create_join_table :authors, :posts do |t|
          # t.index [:author_id, :post_id]
          # t.index [:post_id, :author_id]
        end
      end
    end
------


## Even Complexerer

Sometimes the join tables are more complex
------


- Give the concept a name, like `Publications`
- Treat it like a model itself
- Consider needing additional data, like a
  - `primary_author` boolean attribute
  - `royalty_earned` money attribute
- Use Timestamps to know when changes happen
------


Quick detour into

## Database Theory
------


# [ACID](https://en.wikipedia.org/wiki/ACID)

A single DB can provide

- Atomicity
- Consistency
- Isolation
- Durability
------


# [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem)

In the event of a network failure,

a distributed DB must choose between

- consistency
- availability
------


# [BASE](https://en.wikipedia.org/wiki/Eventual_consistency)

Some distributed DBs prioritize availability.

Eventually, all the data will be consistent.


- Basically Available
- Soft state
- Eventual consistency
------


# Other Topics
------


### ORMs

support multiple databases through

### DB Adapters
___
The same DSL works for MySQL, Postgres, etc.
------


### ORMS

generate queries on the fly, so

are slower than hard-coded queries
___
Dynamically generating the query is slower than having one hardcoded, but it's a tradeoff. Hardcoding a query can make your application more fragile to changes in the future, whereas the dynamic queries will update themselves as things change and the ORM picks up those changes. A hardcoded query might refer to a column that was renamed, for instance.
------


### ORMs

inflate objects with

data from the DB, which

### isn't ideal for bulk work
___
Can be slow when you want to quickly loop over a single attribute of all records.
Especially when the object is complex and has special serialization rules.
------


## Alternate SQL Library

like [sequel](https://github.com/jeremyevans/sequel) for Ruby

### might be more performant
___
It does not inflate data the same way.
Working closer to the metal.
------


## N+1 Query Explosions

ORMS may lazily fetch data, resulting in

1,000+1 queries for

1,000 posts by 1 author
------


# [Primary Key](https://en.wikipedia.org/wiki/Relational_database#Primary_key)

Key (like `id`) that [uniquely](https://en.wikipedia.org/wiki/Unique_key) identifies this record

No two records have the same primary key
------


# [Foreign Key](https://en.wikipedia.org/wiki/Foreign_key)

Key (like `post_id`) that uniquely identifies

some other row in a different table or in this table
------


### Why reference a row in the same table?

To allow parent-child relationships
------


# Indexes

Can speed up lookups, but

require extra space on disk and

slow down writes
___
Can make looking up a user by their email fast.
Every change to a users email means updating the index.
------


# Sanitization
------


## Never trust user input

[SQL injection](https://en.wikipedia.org/wiki/SQL_injection) can cause your DB to be hacked

Some ORMs may sanitize some data by default
------


## Responsibility falls on you

### to ensure data is sanitized

before entering your system
------


# Permissions

Some users might only need read-only access

Review DB permissions to reduce risk of accidental

### deletion or modification
------


# [Normalization](https://en.wikipedia.org/wiki/Database_normalization)

Removing duplication across tables
------


## Normal Forms

- [1NF](https://en.wikipedia.org/wiki/First_normal_form)
- [2NF](https://en.wikipedia.org/wiki/Second_normal_form)
- [3NF](https://en.wikipedia.org/wiki/Third_normal_form)
  - "Nothing but the key"
------


# [Constraints](https://en.wikipedia.org/wiki/Data_integrity#Databases)

- primary key
- foreign key
- not null
- unique
___
Helps the DBMS enforce data integrity
------


# Migrations

Changing the database schema as your needs change
------


# Migrations, cont.

- Migrating the database by hand is tricky.
- Need to be consistent.
- Need to do it across many machines (many developers)
- Need to do it differently in different environments
  - PRD has multiple DBs whereas DEV has one local DB.
------


# Migrations, cont.

- Take databases offline
  - Can make changes that would break your application

- Keep databases online
  - Have to make sure old version of app works with new DB schema
  - Might have to make multiple deployments.
------


# Migrations, cont.

For example, to rename a column:

  - Deploy code that duplicates a column with a new name
  - Deploy code that references the new column
  - Deploy code that deletes the old column
------


# [Data integrity](https://en.wikipedia.org/wiki/Data_integrity#Databases)

Application-level integrity vs Database-level integrity

  - Database can enforce referential integrity (city exists when creating a weather report)
  - Application can enforce higher-level things (Only an admin user can modify this record)
------


# Locks

Adding a non null column to a large table

can lock the table

and prevent reads or writes
------


# Locks, cont.

To prevent a long table lock

- Add a column that allows null values
  - Table is only locked for a short time
- Update each row to have a default value
  - This will lock each row for a short time
- Update the column to not allow null values
  - Since each column already has
------


## Scaling

- Vertical
  - Faster CPUs
  - More Memory
  - More Disk Space
  - [Caching](https://en.wikipedia.org/wiki/Database_caching)
  - [Partitioning](https://en.wikipedia.org/wiki/Partition_(database)) large tables  data across nodes
- Horizontal - More servers
  - Load balancing
  - Create a pool of nodes
  - Add more nodes to the pool
  - [Sharding](https://en.wikipedia.org/wiki/Shard_(database_architecture))
------


## Fault tolerance

If one node goes down,

another can still serve requests
------

# Recovery

Make regular backups of databases
------


# Recovery, cont.

Have a restore process

### Try out the restore process

to make sure it works!
------


# NoSQL DBs

- [MongoDB](https://www.mongodb.com/) is a document-based store
- [Redis](https://redis.io/) is a key-value store
- [Cassandra](http://cassandra.apache.org/) is a column-based store
- [Neo4j](https://neo4j.com/) is a graph-based store
------


## Other topics to consider

- Data Types
- Views
- Triggers
- Full text search
- Security
------


## Other topics to consider, cont.

- Stored Procedures
- Reports/Analytics
- Spatial extensions
- Self-hosted vs Cloud
------


## Links

- [Flat files](https://en.wikipedia.org/wiki/Flat_file_database)
- [Relational model](https://en.wikipedia.org/wiki/Relational_model)
- [Relational database](https://en.wikipedia.org/wiki/Relational_database)
- [SQL](https://en.wikipedia.org/wiki/SQL)
- [Tables](https://en.wikipedia.org/wiki/Table_(database))
- [Fields](https://en.wikipedia.org/wiki/Field_(computer_science))
- [Active Record pattern](https://en.wikipedia.org/wiki/Active_record_pattern)
- [Views](https://en.wikipedia.org/wiki/View_(SQL))
- [Transactions](https://en.wikipedia.org/wiki/Database_transaction)
- [Commits](https://en.wikipedia.org/wiki/Commit_(data_management))
------


## Links, cont.

- [Rollback](https://en.wikipedia.org/wiki/Rollback_(data_management)#SQL)
- [Distributed data store](https://en.wikipedia.org/wiki/Distributed_data_store)
- [Normalization overview](http://searchsqlserver.techtarget.com/definition/normalization)
- [Rails ActiveRecord Basics](http://guides.rubyonrails.org/active_record_basics.html)
- [Use the Index, Luke](http://use-the-index-luke.com/)
- [Python ORMs](https://www.pythoncentral.io/sqlalchemy-vs-orms/)
- [Khan Academy SQL Basics](https://www.khanacademy.org/computing/computer-programming/sql/sql-basics/v/welcome-to-sql)
- [101 things I wish I knew...](https://thomaslarock.com/2015/06/101-things-i-wish-you-knew-about-sql-server/)
------


# Thanks!

