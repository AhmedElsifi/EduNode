# Authentication, Authorization & Mongoose Relationships

## Introduction

In this section, we build a complete Notes API using:

- MongoDB
- Mongoose
- Express
- bcrypt
- JWT
- Middleware
- Authentication
- Relationships between collections

---

## Project Structure

The project follows a modular architecture that separates responsibilities into different folders.

- Database connection
- Models
- Controllers
- Routes
- Middleware

This structure makes applications easier to maintain, test, and scale as the project grows.

### Folder Structure

```text
Project/
├── db/
│   ├── dbConnection.js
│   └── models/
│       ├── note.model.js
│       └── user.model.js
├── src/
│   ├── middlewares/
│   │   ├── checkId.js
│   │   └── verfiyToken.js
│   └── modules/
│       ├── notes/
│       │   ├── notes.controller.js
│       │   └── notes.routes.js
│       └── users/
│           ├── user.controller.js
│           └── users.routes.js
├── index.js
├── package.json
└── package-lock.json
```

### Responsibilities

#### Database Layer

Contains the database connection and Mongoose models.

```text
db/
├── dbConnection.js
└── models/
```

#### Middleware Layer

Contains reusable logic that runs before controllers, such as authentication and validation.

```text
middlewares/
├── checkId.js
└── verfiyToken.js
```

#### Modules Layer

Each feature has its own folder containing controllers and routes.

```text
modules/
├── users/
└── notes/
```

#### Controllers

Handle business logic and database operations.

Examples:

- Register User
- Login User
- Add Note
- Delete Note

#### Routes

Define API endpoints and connect them to controllers.

Examples:

```js
userRoutes.post("/users/register", addUser);
noteRoutes.post("/note", addNote);
```

#### Application Entry Point

```text
index.js
```

Responsible for:

- Connecting to MongoDB
- Registering routes
- Starting the Express server

---

## MongoDB Connection

Mongoose is used to connect Node.js applications to MongoDB.

```js
mongoose.connect("mongodb://localhost:27017/ITIG3")
```

The connection returns a Promise.

```js
mongoose.connect(DB_URL)
.then(() => console.log("db connected"))
.catch(err => console.log(err))
```

---

## Creating Mongoose Schemas

Schemas define the structure of documents.

```js
const userSchema = new Schema({
    name:String,
    age:Number
})
```

Schemas are then converted into Models.

```js
const User = mongoose.model("User", userSchema)
```

---

## Schema Options

Common schema options:

### timestamps

```js
{
    timestamps:true
}
```

Adds:

- createdAt
- updatedAt

---

### versionKey

```js
{
    versionKey:false
}
```

Removes:

```js
__v
```

from documents.

---

## User Model

```js
const userSchema = new mongoose.Schema({
    name:String,
    age:Number,
    email:String,
    password:String
})
```

Additional fields:

### required

```js
email:{
    required:true
}
```

---

### unique

```js
email:{
    unique:true
}
```

Prevents duplicate emails.

---

### default

```js
isConfirmed:{
    type:Boolean,
    default:false
}
```

---

### enum

```js
role:{
    type:String,
    enum:["admin","user"]
}
```

Restricts values.

---

## Note Model

```js
const noteSchema = new Schema({
    title:String,
    likes:Number,
    private:Boolean
})
```

A note belongs to a user.

---

## Mongoose Relationships

MongoDB supports references between collections.

```js
createdBy:{
    type:mongoose.Types.ObjectId,
    ref:"User"
}
```

This stores the User ID inside the Note.

---

## populate()

Used to retrieve referenced documents.

```js
noteModel.find().populate("createdBy")
```

Without populate:

```js
createdBy:
"65a6..."
```

With populate:

```js
createdBy:{
   name:"Ahmed",
   email:"..."
}
```

---

## CRUD Operations with Mongoose

### Create

```js
insertMany()
```

### Read

```js
find()
findById()
findOne()
```

### Update

```js
findByIdAndUpdate()
```

### Delete

```js
findOneAndDelete()
findByIdAndDelete()
```

---

## Password Hashing with bcrypt

Passwords should never be stored as plain text.

### Hash Password

```js
bcrypt.hashSync(password, 8)
```

Example:

```js
const hashed =
bcrypt.hashSync(req.body.password,8)
```

---

## User Registration

Steps:

1. Check email uniqueness
2. Hash password
3. Save user

```js
let foundedUser =
await userModel.findOne({
    email:req.body.email
})
```

If found:

```js
return res.status(409)
```

Otherwise create user.

---

## User Login

Steps:

1. Find user by email
2. Compare passwords
3. Generate JWT token

```js
bcrypt.compareSync(
    req.body.password,
    foundedUser.password
)
```

---

## JSON Web Tokens (JWT)

JWT is used for authentication.

After login:

```js
jwt.sign()
```

creates a token.

Clients send the token with future requests.

---

## Creating Tokens

```js
jwt.sign(
{
   _id:foundedUser._id,
   role:foundedUser.role
},
"itig3"
)
```

Payload contains:

- user id
- role

Secret key:

```js
"itig3"
```

---

## Verifying Tokens

```js
jwt.verify(
token,
"itig3",
callback
)
```

If token is valid:

```js
req.decoded = decoded
```

User information becomes available in the request.

---

## Authentication Middleware

Middleware runs before controllers.

Example:

```js
const verifyToken = (
req,res,next
)
```

Middleware can:

- verify users
- validate input
- log requests

---

## Custom Middleware

Example:

```js
const isAuth = (
req,res,next
)
```

If conditions pass:

```js
next()
```

Otherwise:

```js
res.send("failed")
```

---

## Route Protection

Protect routes using middleware.

```js
noteRoutes.use(
verifyToken
)
```

All routes below it require a valid JWT.

---

## Request Lifecycle and next()

Middleware passes control using:

```js
next()
```

Flow:

Request

↓

Middleware

↓

Controller

↓

Response

---

## Checking Resource Existence

Reusable middleware:

```js
checkId
```

Purpose:

- find user by ID
- stop request if user doesn't exist

```js
let foundedUser =
await userModel.findById(
req.params.id
)
```

---

## User Routes

### Get Users

```js
GET /users
```

---

### Register

```js
POST /users/register
```

---

### Login

```js
POST /users/login
```

---

### Update User

```js
PUT /users/:id
```

---

### Delete User

```js
DELETE /users/:id
```

---

## Note Routes

Protected using JWT.

### Get Notes

```js
GET /notes
```

---

### Add Note

```js
POST /note
```

---

### Update Note

```js
PUT /note/:id
```

---

### Delete Note

```js
DELETE /note/:id
```

---

## Building a Notes API

Features implemented:

- User Registration
- Login System
- Password Hashing
- JWT Authentication
- Route Protection
- User Notes
- User-Note Relationship
- CRUD Operations
- Middleware Validation

---

## Summary

In this section you learned:

- Mongoose schemas and models
- Schema options
- MongoDB relationships
- ObjectId references
- populate()
- bcrypt hashing
- JWT authentication
- Token verification
- Express middleware
- Route protection
- CRUD operations with Mongoose
- Building a complete Notes API