# Learn MongoDB

Resource: [Complete MongoDB Tutorial by Ninja Net](https://youtu.be/ExcRbA7fy_A)

## Table of contents

    - [Learn MongoDB](#learn-mongodb)
      - [Table of contents](#table-of-contents)
      - [What is MongoDB](#what-is-mongodb)
        - [SQL Database](#sql-database)
        - [NoSQL Database](#nosql-database)

## What is MongoDB

MongoDB is a database, something we used to store data in whether that be user data for a e-commerce website or blog data for a blog site. Besides that, MongoDB is what's known as a **NoSQL** database, meaning that we don't use **SQL commands** to interact with it. One such SQL database which we might probably heard of is **MySQL**.

### SQL database

SQL databases are **relational databases**. They are made up of table where each table is used to store a specific data like *users*, *blog posts*, etc. In each **row** in a table is essentially a user record or a blog post record. In each **column** of the table is a property on that particular record like a *username*, *DOB*, *email*, and etc.

In some cases, these data tables can somehow be related to one another. For example, we might have:

1. **Author** table which store the data about authors.
2. **Book** table, which stores data about books.

These two table stores different type of data but they could be related to each other, which means we can form a **relationship** between the tables. For example, one author might have many books. So the relationship between **Author** to **Book** is a **One-to-many** relationship.

To query data from the SQL databases, we use SQL commands which looks something like this:

> You don't have to understand the command, this is just to show case how does a SQL command looks like

    ```sql
    SELECT * FROM authors
    -- query all records from the author table
    ```

### NoSQL Database

NoSQL database are VERY DIFFERENT from SQL database. Instead of dealing with tables with rows and colunms, it uses what's known as **collections** and **documents**. These collections in a NoSQL database would store a specific type of data records like users, blog posts, and etc. In NoSQL terminology, these records inside the collections are called **documents**, and they look very much like **JSON objects**, with *key-value* pairs.

There are few benefits over using tables, rows and columns.

1. It's much easier to work with something like JavaScript because documents in MongoDB are very much like JSON or JavaScript objects.
2. It allows us to store nested documents within a document. For example, in the book collection could have an author property and it can be a document in itself which has a first name property, last name property and etc.

        ```json
        { // this is a document
            "title": "Name of the Wind",
            "rating": 9,
            "pages": 400,
            "author": { // this is another document
                "first_name": "John",
                "last_name": "Doe"
            }
        }
        ```

## Installing MongoDB

Installation link: [here](https://www.mongodb.com/try/download/community-kubernetes-operator)

There are two ways to use MongoDB:

1. Use MongoDB locally (Will focus more on this)
2. Use MongoDB as a service (Will revisit in the future)

To try MongoDB locally, head to [here](https://www.mongodb.com/try/download/community-kubernetes-operator). Download the **MongoDB Community Server** edition. Also, choose the version you like (i'm using 6.0.4), your platform and the package type. Once you downloaded it, just go through the installation process.

For the setup type, go for the **Complete** and ensure that you **install MongoDB as a service** (make sure this option is checked). Also, **install MongoDB Compass** (make sure this option is checked as well), this is the GUI version of MongoDB.

After you have installed MongoDB locally, there's another tool (will be used most of the time) that we need to install, which is the **MongoDB shell**. Again, head to the link above and head to the **Tools** section and download **MongoDB Shell**.

To verify our installation, open up your terminal and run:

    ```shell
    mongosh
    ```

If installed correctly, we will go into the interative Mongo shell where we can run MongoDB commands.

## Collections & Documents

As we mentioned earlier, MongoDB stores data inside collections and a database can have as many collections as you like for different types of data. For example a database can have collection (equivalent terminology to tables in SQL database) of users, collection of blog posts and etc. In users collection, would store user data or user documents.

A document represent the individual records in a specific collection. The way that the data is structured inside a document looks very much like a JSON or JavaScript object, with key and value pairs. Below is an example of a blog post document.

    ```json
    {
        "title": "My first blog post",
        "author": "Yoshi",
        "tags": ["video games", "reviews"],
        "upvotes": 20,
        "body": "Lorem ipsum..."
    }
    ```

So when we fetch the data from a collection, JSON objects are ultimately what we get back. Documents can have properties whose values themselves can also be documents or arrays of documents (nested document).

    ```json
    {
        "title": "My first blog post",
        "author": {
            "name": "Yoshi",
            "email": "yoshi2023@gmail.com",
            "role": "Game reviewer"
        },
        "tags": ["video games", "reviews"],
        "upvotes": 20,
        "body": "Lorem ipsum..."
    }
    ```

## MongoDB Shell

Some commands that you need to know:

    ```shell
    show dbs # show all the database we have
    use database-name # switch to the database you want to work on
    db # check the current database i'm in
    show collections # list all the collections of the current database im in
    var name = "yoshi" # create a variable, name is the variable name
    [var_name] # print the value of the variable
    name = "mario" # modify the value of the variable
    help # list all the possible commands
    exit # exit shell
    ```

One thing that you need to know is that MongoDB doesn't care if the database exists or not. If you do `use test` to switch to a unexisted database called `test`, it will still switch to it. But if we try to create a collection or documents inside this unexisted database, MongoDB will then create that database for us.

### Adding new documents

In order to create a new document, we need to first reference the collection that we want to put the new document to.

> You don't have to have a existed collection to create a document. If the collection does not existed, MongoDB will then create that collection for you.

    ```shell
    # db will reference to the current databse
    # books will reference to the books collection
    # insertOne(): a method to create one new document
    db.books.insertOne({title: "The Color of Magic", author: "Terry Pratchett", pages: 300, rating: 7, genres: ["fantasy", "magic"]})

    # if the operation is successful, you will see something like
    # {
    #   acknowledged: true,
    #   insertedId: ObjectId("random_id")
    # }

    # insert more than one document
    db.books.insertMany([{title: "The Light Fantastic", author: "Terry Pratchett", pages: 250, rating: 6, genres: ["fantasy"]}, {title: "Dune", author: "Frank Herbert", pages: 500, rating: 10, genres: ["fantasy", "sci-fi", "dystopian"]}])
    ```

### Finding documents

A lot of times, you will want to fetch documents from a certain collection so you can do something about it. For example, when making an API, I may need to expose an endpoint that returns a list of books to whoever's calling the API. Inside the handler function, we need to communicate with MongoDB to fetch all of the book documents from the book collection. So, how to achieve that? Use the `find` method!

    ```shell
    db.books.find() # if we didn't pass anything to the method, it will show the first 20 document.

    # to iterate through the next 20 books, use
    it # iterate

    # the object inside is the filter options
    db.books.find({author: "Terry Pratchett"})

    # more filter option
    db.books.find({author: "Terry Pratchett", rating: 7})

    # 2nd args of find() specifies which fields we want to get back from each document. 1 means include that
    # noted that it will always get back with the _id even though you didn't specify
    db.books.find({author: "Brandon Simpson"}, {title: 1, author: 1})

    # how about not using the first argument?
    # will get all the books but only with the specified fields
    db.books.find({}, {title: 1, author: 1})

    # find one book based on something
    db.books.findOne({_id: ObjectId("63cfc9a7dca1e5cc1f083f44")})
    ```

### Finding documents

### Sorting & limiting data

### Nested documents

### Operators & complex queries

### Using `$in` & `$nin`

### Querying arrays

### Deleting documents

### Updating documents

### MongoDB Drivers

### Connecting to MongoDB

### Cursors & Fetching Data

### Finding single documents

### Handling POST Requests

### Handling DELETE Requests

### Handling PATCH Requests

### Pagination

### Indexes

### MongoDB Atlas
