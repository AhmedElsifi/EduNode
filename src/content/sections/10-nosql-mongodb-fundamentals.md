# 1. NoSQL

## What is NoSQL?

NoSQL (Not Only SQL) is a category of database management systems designed to handle large volumes of data, high scalability, and flexible data structures.

### Characteristics

* Non-relational
* Schema-free or flexible schema
* Horizontally scalable
* Distributed architecture
* Open source
* Handles structured, semi-structured, and unstructured data
* Follows BASE consistency model more often than ACID

### Advantages

* High performance
* Easy scalability
* Flexible data models
* Suitable for cloud computing
* Handles huge amounts of data

### Limitations

* No standard query language
* Limited query capabilities compared to SQL
* Some systems do not fully support ACID transactions
* Data integrity can be more difficult to maintain

## NoSQL Types

### Key-Value Databases

Store data as key-value pairs.

Examples:

* Redis
* Riak
* Amazon SimpleDB

### Document Databases

Store data as JSON-like documents.

Examples:

* MongoDB
* CouchDB

### Column-Oriented Databases

Store data by columns instead of rows.

Examples:

* Cassandra
* HBase

### Graph Databases

Store relationships using nodes and edges.

Examples:

* Neo4j
* Infinite Graph

---

## ACID vs BASE

### ACID

* Atomicity
* Consistency
* Isolation
* Durability

### BASE

* Basic Availability
* Soft State
* Eventual Consistency

---

# 2. MongoDB

## What is MongoDB?

MongoDB is a cross-platform, document-oriented NoSQL database that stores data in BSON (Binary JSON) format.

### Features

* Document-oriented storage
* Schema-free collections
* High scalability
* Rich querying capabilities
* Indexing support
* Replication and failover
* Aggregation support

---

## MongoDB Structure

### Database

Container that holds collections.

### Collection

Equivalent to a SQL table.

### Document

Equivalent to a SQL row.

Example:

```json
{
  "name": "Ahmed",
  "age": 20,
  "city": "Cairo"
}
```

---

## BSON

BSON stands for Binary JSON.

Advantages:

* Faster scanning
* Better storage efficiency
* Supports more data types than JSON

---

## MongoDB vs SQL

| SQL              | MongoDB            |
| ---------------- | ------------------ |
| Database         | Database           |
| Table            | Collection         |
| Row              | Document           |
| Column           | Field              |
| Schema Required  | Schema Optional    |
| Vertical Scaling | Horizontal Scaling |

---

# 3. MongoDB Shell Commands

## Show Databases

### Syntax

```javascript
show dbs
```

### Example

```javascript
show dbs
```

Displays all databases.

---

## Switch/Create Database

### Syntax

```javascript
use databaseName
```

### Example

```javascript
use SchoolDB
```

Switches to SchoolDB or creates it if it doesn't exist.

---

## Create Collection

### Syntax

```javascript
db.createCollection("CollectionName")
```

### Example

```javascript
db.createCollection("Students")
```

Creates a collection called Students.

---

## Delete Collection

### Syntax

```javascript
db.CollectionName.drop()
```

### Example

```javascript
db.Students.drop()
```

Deletes the Students collection.

---

## Delete Database

### Syntax

```javascript
db.dropDatabase()
```

### Example

```javascript
db.dropDatabase()
```

Deletes the current database.

---

# Inserting Documents

## insertOne()

### Syntax

```javascript
db.CollectionName.insertOne(document)
```

### Example

```javascript
db.Students.insertOne({
  name: "Ahmed",
  age: 20
})
```

Inserts one document.

---

## insertMany()

### Syntax

```javascript
db.CollectionName.insertMany([documents])
```

### Example

```javascript
db.Students.insertMany([
  { name: "Ahmed", age: 20 },
  { name: "Ali", age: 22 }
])
```

Inserts multiple documents.

---

# Reading Documents

## find()

### Syntax

```javascript
db.CollectionName.find()
```

### Example

```javascript
db.Students.find()
```

Returns all documents.

---

## findOne()

### Syntax

```javascript
db.CollectionName.findOne()
```

### Example

```javascript
db.Students.findOne()
```

Returns the first matching document.

---

## find() with Filter

### Syntax

```javascript
db.CollectionName.find(filter)
```

### Example

```javascript
db.Students.find({ age: 20 })
```

Returns documents where age is 20.

---

# Comparison Operators

## Greater Than ($gt)

### Syntax

```javascript
db.Collection.find({
  field: { $gt: value }
})
```

### Example

```javascript
db.Products.find({
  price: { $gt: 100 }
})
```

---

## Less Than ($lt)

### Example

```javascript
db.Products.find({
  price: { $lt: 100 }
})
```

---

## Greater Than or Equal ($gte)

### Example

```javascript
db.Products.find({
  price: { $gte: 100 }
})
```

---

## Less Than or Equal ($lte)

### Example

```javascript
db.Products.find({
  price: { $lte: 100 }
})
```

---

# Logical Operators

## $and

### Syntax

```javascript
db.Collection.find({
  $and: [
    condition1,
    condition2
  ]
})
```

### Example

```javascript
db.Posts.find({
  $and: [
    { likes: { $gt: 250 } },
    { img: { $exists: true } }
  ]
})
```

---

## $or

### Example

```javascript
db.Posts.find({
  $or: [
    { likes: { $gt: 250 } },
    { img: { $exists: true } }
  ]
})
```

---

# Array Operators

## $all

### Example

```javascript
db.Posts.find({
  tags: {
    $all: ["math", "science"]
  }
})
```

Document must contain both tags.

---

## $in

### Example

```javascript
db.Posts.find({
  tags: {
    $in: ["math", "science"]
  }
})
```

Contains at least one tag.

---

## $nin

### Example

```javascript
db.Posts.find({
  tags: {
    $nin: ["math", "science"]
  }
})
```

Contains neither tag.

---

## $size

### Example

```javascript
db.Posts.find({
  tags: {
    $size: 2
  }
})
```

Array length equals 2.

---

# Projection

## Syntax

```javascript
db.Collection.find(
  filter,
  projection
)
```

### Example

```javascript
db.Students.find(
  {},
  {
    name: 1,
    age: 1,
    _id: 0
  }
)
```

Displays only name and age.

---

# Updating Documents

## updateOne()

### Syntax

```javascript
db.Collection.updateOne(
  condition,
  update
)
```

### Example

```javascript
db.Posts.updateOne(
  { id: 1 },
  {
    $set: {
      likes: 250
    }
  }
)
```

Updates the first matching document.

---

## updateMany()

### Syntax

```javascript
db.Collection.updateMany(
  condition,
  update
)
```

### Example

```javascript
db.Posts.updateMany(
  { category: "Tech" },
  {
    $set: {
      featured: true
    }
  }
)
```

Updates all matching documents.

---

## $set

### Example

```javascript
db.Users.updateOne(
  { id: 1 },
  {
    $set: {
      age: 21
    }
  }
)
```

Creates or updates a field.

---

## $unset

### Example

```javascript
db.Users.updateOne(
  { id: 1 },
  {
    $unset: {
      age: ""
    }
  }
)
```

Removes a field.

---

## $inc

### Example

```javascript
db.Posts.updateOne(
  { id: 1 },
  {
    $inc: {
      likes: 1
    }
  }
)
```

Increments a numeric value.

---

## $push

### Example

```javascript
db.Users.updateOne(
  { id: 1 },
  {
    $push: {
      skills: "Node.js"
    }
  }
)
```

Adds an item to an array.

---

## $pull

### Example

```javascript
db.Users.updateOne(
  { id: 1 },
  {
    $pull: {
      skills: "PHP"
    }
  }
)
```

Removes an item from an array.

---

## $addToSet

### Example

```javascript
db.Users.updateOne(
  { id: 1 },
  {
    $addToSet: {
      skills: "MongoDB"
    }
  }
)
```

Adds value only if it doesn't already exist.

---

# Deleting Documents

## deleteOne()

### Syntax

```javascript
db.Collection.deleteOne(condition)
```

### Example

```javascript
db.Users.deleteOne({
  id: 1
})
```

Deletes the first matching document.

---

## deleteMany()

### Syntax

```javascript
db.Users.deleteMany(condition)
```

### Example

```javascript
db.Users.deleteMany({
  active: false
})
```

Deletes all matching documents.

---

# Other Useful Commands

## findOneAndUpdate()

### Example

```javascript
db.Users.findOneAndUpdate(
  { id: 1 },
  {
    $set: {
      age: 25
    }
  }
)
```

Finds a document and updates it in one operation.

---

## Index Creation

### Example

```javascript
db.Users.createIndex({
  email: 1
})
```

Creates an index for faster searches.

---

## Aggregation

### Example

```javascript
db.Orders.aggregate([
  {
    $group: {
      _id: "$category",
      total: {
        $sum: "$price"
      }
    }
  }
])
```

Groups data and performs calculations.

---

### Summary

By the end of this section you should understand:

* NoSQL fundamentals and types
* ACID vs BASE
* MongoDB architecture (Database → Collection → Document)
* BSON
* CRUD operations in MongoDB
* Query operators
* Array operators
* Projection
* Update operators
* Aggregation basics
* Indexes
* MongoDB Shell workflow (`show dbs → use db → create collection → CRUD`)
