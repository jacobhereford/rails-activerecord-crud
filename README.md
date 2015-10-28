![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

# An introduction to ActiveRecord

## Instructions

Fork, clone, and bundle install

## Objectives

By the end of this lesson, students should be able to:

- Create a database table using Ruby on Rails
- Insert a row or rows into a database table using ActiveRecord
- Retrieve a row or rows from a database table using ActiveRecord
- Update a row or rows in a database table using ActiveRecord
- Delete a row or rows from a database table using ActiveRecord

## Prerequisites

- A working [Rails](http://rubyonrails.org/download/) installation.
- [An introduction to relational databases](https://github.com/ga-wdi-boston/sql-crud)
- Required reading: http://guides.rubyonrails.org/active_record_basics.html

## Introduction

**[ActiveRecord](http://api.rubyonrails.org/files/activerecord/README_rdoc.html)** _(see also [Active record pattern](http://en.wikipedia.org/wiki/Active_record_pattern))_ is the mechanism Rails uses to store and retrieve data from an RDBMS.  This, and similar mechanism, are referred to as an [Object-relational mapping](http://en.wikipedia.org/wiki/Object-relational_mapping) or ORM.

Why is this important?

Because the vast majority of enterprise data is stored in relational databases, but objects are often used to manipulate that data in applications. And, although **[NoSQL](http://en.wikipedia.org/wiki/NoSQL)** databases _(e.g Mongo DB)_ have been growing in popularity, their ability to support distributed data to enhance performance makes achieving **[ACID](http://en.wikipedia.org/wiki/ACID)** transactions a significant challenge.

**[Mongo DB Is Web Scale](https://www.youtube.com/watch?v=b2F-DItXtZs)**

The point of the video is not to state a preference for one technology over another, but to make clear that the need should drive the technology choice, not the hype.

ActiveRecord makes it easy for us to store and retrieve rows from a database table.

What about more complicated data?

ActiveRecord makes it easy to create objects that reference other objects (using tables that reference other tables) which allows arbitrary nesting of objects.  This is something we'll be looking at more closely later.

We'll be using [PostgreSQL](http://www.postgresql.org/) as the [RDBMS](http://en.wikipedia.org/wiki/Relational_database_management_system) backing ActiveRecord.

The Rails App we'll be using was created with the command `rails-api new --skip-sprockets --skip-spring --skip-javascript --skip-turbolinks --skip-test-unit --database=postgresql.`.

We'll use `rails-crud_development` as the database to hold our tables and **[rails dbconsole](http://guides.rubyonrails.org/command_line.html#rails-dbconsole)** _(alias `rails db`)_ to interact with it with SQL.  By default, each Rails App is created to potentially use one of three databases, `<rails app name>_development`, `<rails app name>_test`, and `<rails app name>_production`.  We'll use **[rails console](http://guides.rubyonrails.org/command_line.html#rails-console)** _(alias `rails c`)_ to interactively use Models and **[rails runner](http://guides.rubyonrails.org/command_line.html#rails-runner)** _(alias `rails r`)_ to invoke any scripts we write.

## Create the database

### Code along

```bash
$ rails db
psql: FATAL:  database "rails-crud_development" does not exist
$
```

As we can see, `rails db` runs `psql`.  If the Rails app had been configured for a different database server, `rails db` would have started a different command line client.

As before, we need to create the database.  We'll use the command line application **[rake](http://guides.rubyonrails.org/command_line.html#rake)** which Rails uses to manage changes to the structure of the database (among other things).

```bash
$ rake db:create
```

Rake is a task runner and `rake -T ` provides a brief description of the tasks it's configured to run from the current `Rakefile`.  Let's have a look at the `db` tasks.

```bash
$ rake -T db
```

## Creating tables and ActiveRecord object to manipulate them

### Demonstration

We'll store and manipulate information about people.

To generate the code necessary to create a table and the code to manipulate data stored in that table, we use `rails generate model` _(alias `rails g model`)_.  If you run `rails g model` without any arguments, Rails tells you what you can do.

```bash
$ rails g model Person surname:string
      invoke  active_record
      create    db/migrate/<datetime>_create_people.rb
      create    app/models/person.rb
$
```

Let's look at the files created.  The model created inherits from **[ActiveRecord::Base](http://api.rubyonrails.org/classes/ActiveRecord/Base.html)**.  The migration from **[ActiveRecord::Migration](http://api.rubyonrails.org/classes/ActiveRecord/Migration.html)** (see also http://guides.rubyonrails.org/active_record_migrations.html).

The table defined by the migration, and needed by the model, isn't created until we run `rake db:migrate`.

### Code along

Together let's create a table and model for cities

### Practice

Create a table and model for pets, then people

---

## Insert a row

To insert a row we can create a script that uses `<Model>`.**[new](http://api.rubyonrails.org/classes/ActiveRecord/Core.html#method-c-new)** combined with `<Model>`.[save](http://api.rubyonrails.org/classes/ActiveRecord/Persistence.html#method-i-save), or `<Model>`.**[create](http://api.rubyonrails.org/classes/ActiveRecord/Persistence/ClassMethods.html#method-i-create)**.

### Demonstration

Let's add one person then see how we can add people in bulk.

Adding records in bulk recommends a transaction to wrap the call to `create!`.  We'll set this up as a rake task to make it easier execute.

### Code along

Together let's do the same for cities.

### Practice

First pets, then people

---

## Find a row

By id, `<Model>`.**[find](http://api.rubyonrails.org/classes/ActiveRecord/FinderMethods.html#method-i-find)**; by criteria, `<Model>`.**[find_by](http://api.rubyonrails.org/classes/ActiveRecord/FinderMethods.html#method-i-find_by)**.  Let's also look at, http://api.rubyonrails.org/classes/ActiveRecord/QueryMethods.html and http://guides.rubyonrails.org/active_record_querying.html.  The [where](http://api.rubyonrails.org/classes/ActiveRecord/QueryMethods.html#method-i-where) method provides more complex criteria.

### Demonstration

Let's explore how we can retrieve ActiveRecord models for people.

### Code along

Together let's do the same for cities.

### Practice

First pets, then people

---

## Modify a table

To modify an existing table we'll run `rails generate migration` _(alias `rails g migration`)_ followed by `rake db:migrate`.


### Demonstration

Let's see how we change the table underlying the ActiveRecord models for people.  Note that in many circumstances we don't need to change the model class when we change the table.

```bash
$ rails g migration RemoveHeightFromPerson height:integer
```

### Code along

Together let's change the type for longitude and latitude for cities.

### Practice

Add the column `weight` to pets then remove the column `height` from people.
---

## Update a row

After retrieving an object, we can update it using `<model>`.**[update](http://api.rubyonrails.org/classes/ActiveRecord/Persistence.html#method-i-update)**.  Or we can set the models attributes then call `<model>.save`.

### Demonstration

Let's update some people's weight using each of those methods.

### Code along

Together let's do the same for some cities' population.

### Practice

Update weight, first for pets, then people.

---

## Delete a row

To delete a row we'll want to use `<model>`.**[destroy](http://api.rubyonrails.org/classes/ActiveRecord/Persistence.html#method-i-destroy)** or `<Model>`.**[destroy_all](http://api.rubyonrails.org/classes/ActiveRecord/Relation.html#method-i-destroy_all)**.  These methods are especially useful when dealing with [Active Record Associations](http://guides.rubyonrails.org/association_basics.html) which we'll cover in more detail later.

### Demonstration

We'll remove a few people.

### Code along

Let's remove the cities that don't have a region.

### Practice

Remove pets born before 1996 then people taller than 6 feet.

## Assessment
