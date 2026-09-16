# Section 14 — MongoDB & Mongoose Integration

## Synchronous vs Asynchronous JavaScript

JavaScript is a **single-threaded**, **synchronous** language by default, executing code line by line.

To handle time-consuming tasks—such as network calls, database queries, and file I/O—without blocking the main thread, JavaScript relies on **asynchronous execution**.

* **Synchronous:** Blocks execution until the current task completes.
* **Asynchronous:** Offloads non-blocking tasks and processes their results via callbacks, Promises, or `async/await`.

## Node.js Architecture & Libuv

Node.js executes asynchronous, non-blocking I/O using **Libuv**, a C library that handles the event loop and thread pool.

```text
JavaScript Code
      │
      ▼
V8 Engine (Call Stack)
      │
      ▼
    Libuv
  ├── Event Loop
  └── Thread Pool (File I/O, Cryptography, Database Calls)

```

Libuv abstracts OS-level asynchronous operations and allows Node.js to perform heavy I/O operations without stopping the execution of other JavaScript tasks.

## Security Fundamentals: Encryption vs Decryption vs Hashing

Understanding data protection mechanisms is essential when managing user sensitive information:

| Term | Mechanism | Reversibility | Common Use Case |
| --- | --- | --- | --- |
| **Encryption** | Transforms plaintext into ciphertext using an encryption key | Reversible (with private/secret key) | Data in transit (HTTPS), sensitive storage |
| **Decryption** | Converts ciphertext back to original plaintext using a key | Reversible | Retrieving encrypted messages |
| **Hashing** | Transforms input into a fixed-length cryptographic signature | **Irreversible** (One-way) | Storing passwords safely |

> **Key Takeaway:** Passwords should **never** be stored as plaintext or encrypted—they must always be **hashed** using one-way cryptographic algorithms (e.g., bcrypt).

### Salting and Cost Factors (Rounds)

To protect hashed passwords against precomputed dictionary attacks (Rainbow Tables), hashing libraries use **Salting** and **Cost Factors**:

* **Salt:** Random bytes added to input before hashing to guarantee unique outputs for identical passwords.
* **Cost Factor (Rounds):** Determines the computational time required to compute the hash (e.g., `4` or `10` salt rounds). Higher rounds slow down brute-force attacks.

## What is an ODM? (Object Document Mapper)

An **ODM (Object Document Mapper)** translates between JavaScript code objects and MongoDB document representations.

* **MongoDB:** Stores data as flexible BSON (Binary JSON) documents inside collections.
* **Mongoose:** A popular ODM for Node.js that provides schema validation, business logic hooks, and query abstractions over native MongoDB drivers.

```text
Node.js Application (JS Objects)
            │
            ▼
     Mongoose (ODM)
            │
            ▼
 Native MongoDB Driver
            │
            ▼
  MongoDB Server (BSON)

```

## Connecting Mongoose to MongoDB

Use `mongoose.connect()` to establish a connection to your database cluster:

```js
import mongoose from "mongoose";

mongoose
  .connect("mongodb://localhost:27017/ITIG3")
  .then(() => console.log("db connected"))
  .catch((err) => console.log("db error", err));

```

## Defining Schemas and Models

Mongoose uses **Schemas** to define document structure and **Models** to interface with collections.

### 1. Schema Definition

A schema maps out the fields, types, and validation rules for documents inside a collection:

```js
const userSchema = new mongoose.Schema({
  name: String,
  age: Number,
  email: String
});

```

### 2. Creating a Model

A model acts as a constructor wrapper built from the schema. It provides the API interface for querying and mutating MongoDB documents:

```js
const userModel = mongoose.model("User", userSchema);

```

> **Note:** Mongoose automatically pluralizes `"User"` to target the `"users"` collection in MongoDB.

## Implementing Express Endpoints with Mongoose

### Fetching Documents (`GET /users`)

Use `await userModel.find()` to retrieve all documents from the collection asynchronously:

```js
app.get("/users", async (req, res) => {
  const users = await userModel.find();
  res.json({ message: "all users", users });
});

```

### Creating Documents (`POST /users`)

When creating a new record, query first to prevent duplicate entries, then save the document:

```js
app.post("/users", express.json(), async (req, res) => {
  const newUser = req.body;
  
  const exists = await userModel.findOne({ email: newUser.email });
  if (exists) return res.json({ message: "user already exists" });

  await userModel.create(newUser);
  res.json({ message: "added successfully" });
});

```

> **Warning:** In the provided code snippet, `userModel.insertMany(newUser)` was called asynchronously without `await`, and passing a single object instead of an array to `insertMany` is non-standard. For single document creation, `userModel.create()` or `new userModel().save()` is preferred.

## Code Summary & Refactored Reference

Below is the complete, cleaned-up implementation incorporating proper `async/await` handling:

```js
import express from "express";
import mongoose from "mongoose";

const app = express();

mongoose
  .connect("mongodb://localhost:27017/ITIG3")
  .then(() => console.log("db connected"))
  .catch((err) => console.log("db error", err));

const userSchema = new mongoose.Schema({
  name: String,
  age: Number,
  email: String
});

const userModel = mongoose.model("User", userSchema);

app.get("/users", async (req, res) => {
  const users = await userModel.find();
  res.json({ message: "all users", users });
});

app.post("/users", express.json(), async (req, res) => {
  const newUser = req.body;
  const exists = await userModel.findOne({ email: newUser.email });

  if (exists) return res.json({ message: "user already exists" });

  await userModel.create(newUser);
  res.json({ message: "added successfully" });
});

app.get("/", (req, res) => res.send("Hello World!"));

app.listen(3000, () => console.log("app listening on port 3000!"));

```

## Summary

* **Asynchronous Node:** Node offloads asynchronous I/O tasks to Libuv, avoiding main thread blockage.
* **Security Practices:** Encrypt for 2-way data flow; Hash with salts and cost factors for non-reversible password storage.
* **ODMs:** Mongoose maps JavaScript objects to MongoDB documents with schemas and models.
* **Data Flow:** Define a `Schema` -> Compile a `Model` -> Query asynchronously with `find()`, `findOne()`, and `create()`.

---
