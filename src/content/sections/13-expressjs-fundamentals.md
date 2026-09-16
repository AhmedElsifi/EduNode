# Section 13 — Express.js Fundamentals

## What Is Express.js?

Express.js is a lightweight web framework for Node.js used to build web servers and APIs.

Node.js provides the low-level `http` module, but building an API directly with it requires manually checking things such as:

* `req.url`
* `req.method`
* Request body data
* Routing conditions
* Response headers

Express.js provides a simpler abstraction over Node's HTTP functionality.

For example, a route with native Node.js might require:

```js
if (req.url === "/users" && req.method === "GET") {
    // Handle request
}

```

With Express.js:

```js
app.get("/users", (req, res) => {
    // Handle request
});

```

> **Key Takeaway:** Express.js does not replace Node.js. Express.js is a framework that runs on top of Node.js and simplifies server and API development.

## Installing Express.js

Create a Node.js project first:

```bash
npm init -y

```

Then install Express:

```bash
npm install express

```

This installs Express as a project dependency and adds it to `package.json`.

After installation, Express can be imported:

```js
import express from "express";

```

> **Warning:** If you are using `import` syntax in a Node.js project, make sure your project is configured for ES modules, for example by adding `"type": "module"` to `package.json`.

## Creating an Express Application

The basic Express setup is:

```js
import express from "express";

const app = express();

```

`express()` creates an Express application instance.

The `app` object provides methods for:

* Defining routes
* Handling HTTP methods
* Adding middleware
* Starting the server

## Starting the Server

Use `app.listen()` to start the Express server:

```js
app.listen(3000, () => {
    console.log("server running");
});

```

The server is now available at:

```text
http://localhost:3000

```

## Defining Routes

Express provides methods corresponding to HTTP request methods.

```js
app.get("/", (req, res) => {
    res.json({ message: "hello from get" });
});

```

The route consists of:

* `app.get()` — handles GET requests.
* `"/"` — the route path.
* `req` — incoming request.
* `res` — outgoing response.

Other common methods include:

```js
app.post("/users", handler);
app.put("/users/:id", handler);
app.patch("/users/:id", handler);
app.delete("/users/:id", handler);

```

## Sending JSON Responses

Express provides `res.json()` for sending JSON responses.

```js
res.json({
    message: "all users",
    users
});

```

Unlike the native Node.js `http` module, you do not need to manually call `JSON.stringify()`.

Express handles the JSON conversion and appropriate response headers.

## Express Middleware

Middleware is a function that runs during the request-response cycle.

Express provides middleware for common tasks such as parsing JSON request bodies.

### `express.json()`

When a client sends JSON data in the request body, use `express.json()` to parse it.

```js
app.post("/users/register", express.json(), (req, res) => {
    console.log(req.body);
});

```

After the middleware processes the request, the parsed JSON is available through:

```js
req.body

```

For example, if the client sends:

```json
{
    "email": "ahmed@example.com",
    "password": "password1",
    "name": "Ahmed",
    "age": 20
}

```

You can access:

* `req.body.email`
* `req.body.password`
* `req.body.name`
* `req.body.age`

> **Key Takeaway:** `express.json()` parses incoming JSON request bodies and makes the resulting JavaScript object available through `req.body`.

## Getting All Users

A `GET /users` endpoint can return all users:

```js
app.get("/users", (req, res) => {
    res.json({
        message: "all users",
        users
    });
});

```

The response contains both a message and the `users` array.

## Registering a User

A registration endpoint can accept user data through the request body.

```js
app.post("/users/register", express.json(), (req, res) => {
    const exists = users.find(
        (user) => user.email === req.body.email
    );

    if (exists) {
        return res.json({
            message: "user already exists, please login"
        });
    }

    req.body.id = users[users.length - 1].id + 1;

    users.push(req.body);

    res.status(201).json({
        message: "registered successfully",
        user: req.body
    });
});

```

The flow is:

1. Parse the JSON body.
2. Check whether the email already exists.
3. Generate an ID.
4. Add the new user.
5. Return the created user.

### `res.status()`

Express allows you to set the HTTP status code using `res.status()`.

```js
res.status(201).json({
    message: "registered successfully",
    user: req.body
});

```

`201 Created` indicates that a new resource was successfully created.

## User Login

A login endpoint can search for a user using their email.

```js
app.post("/users/login", express.json(), (req, res) => {
    const exists = users.find(
        (user) => user.email === req.body.email
    );

    if (exists) {
        if (exists.password === req.body.password) {
            return res.json({
                message: "login successfully",
                user: exists
            });
        }

        return res.json({
            message: "password is incorrect"
        });
    }

    res.json({
        message: "user not found, please register"
    });
});

```

The endpoint:

1. Searches for the email.
2. If the user exists, compares the password.
3. Returns a successful response if both match.
4. Otherwise, returns an appropriate message.

> **Warning:** This example compares passwords directly and stores them as plain text. Real applications should never store plain-text passwords; passwords should be securely hashed.

## Route Parameters

Express supports dynamic values in URLs using route parameters.

```js
app.delete("/users/:id", (req, res) => {
    // ...
});

```

Here, `:id` is a route parameter.

A request such as:

```text
DELETE /users/3

```

makes the ID available through:

```js
req.params.id

```

However, route parameters are strings by default.

```js
const id = Number(req.params.id);

```

Converting the value to a number allows strict comparison with numeric IDs:

```js
user.id === id

```

> **Key Takeaway:** `req.params` contains values captured from dynamic parts of the URL, while `req.body` contains data sent inside the request body.

## Deleting a User

A user can be deleted using a route parameter:

```js
app.delete("/users/:id", express.json(), (req, res) => {
    const id = Number(req.params.id);

    const exists = users.find((user) => user.id === id);

    if (!exists) {
        return res.json({
            message: "can't find user with this id",
            users
        });
    }

    users = users.filter((user) => user.id !== id);

    res.json({
        message: "user deleted successfully"
    });
});

```

The process is:

1. Read the ID from `req.params`.
2. Convert it to a number.
3. Check whether the user exists.
4. Use `filter()` to remove the user.
5. Send a response.

## Updating a User

A PUT request can be used to update a user.

```js
app.put("/users/:id", express.json(), (req, res) => {
    const id = Number(req.params.id);

    const foundedUser = users.find(
        (user) => user.id === id
    );

    const updatedUser = {
        ...foundedUser,
        ...req.body
    };

    res.json({
        message: "user updated successfully",
        user: updatedUser
    });
});

```

### Object Spread for Updates

The spread operator can combine the existing user with the new properties:

```js
const updatedUser = {
    ...foundedUser,
    ...req.body
};

```

Properties in `req.body` override properties with the same names from the existing user.

For example:

```js
const user = {
    id: 1,
    name: "Ahmed",
    age: 20
};

const updates = {
    name: "Ali"
};

const updatedUser = {
    ...user,
    ...updates
};

```

Result:

```json
{
    "id": 1,
    "name": "Ali",
    "age": 20
}

```

> **Warning:** In the provided implementation, `updatedUser = {...foundedUser, ...req.body}` only creates a new object and returns it; it does not replace the original object inside the `users` array. To actually persist the update in the array, the array element must be replaced or mutated.

## Updating the Array Element

One approach is to find the user's index:

```js
const index = users.findIndex((user) => user.id === id);

if (index === -1) {
    return res.status(404).json({
        message: "user not found"
    });
}

users[index] = {
    ...users[index],
    ...req.body
};

res.json({
    message: "user updated successfully",
    user: users[index]
});

```

`findIndex()` returns the position of the matching user in the array.

If no user matches, it returns `-1`.

## Express Request and Response Flow

An Express API follows a simple flow:

```text
Client
   │
   │ HTTP Request
   ▼
Express Route
   │
   ├── Middleware
   │      └── express.json()
   │
   ├── Read request data
   │      ├── req.body
   │      └── req.params
   │
   ├── Perform operation
   │      ├── find()
   │      ├── filter()
   │      └── update/create
   │
   ▼
HTTP Response
   │
   └── res.json()

```

## Express vs Native Node.js HTTP

Express simplifies many tasks that would otherwise require manual handling with Node's `http` module.

| Task | Native Node.js | Express |
| --- | --- | --- |
| Create server | `http.createServer()` | `express()` + `app.listen()` |
| Routing | Manually check `req.url` and `req.method` | `app.get()`, `app.post()`, etc. |
| JSON response | `JSON.stringify()` + `res.end()` | `res.json()` |
| Request body | Manually process request streams | `express.json()` |
| Route parameters | Manual URL parsing | `req.params` |
| Status codes | `res.statusCode` | `res.status()` |

> **Key Takeaway:** Express makes Node.js API development easier by providing routing, middleware, request parsing, and response helpers on top of Node's HTTP capabilities.

## Summary

* Express.js is a web framework built on top of Node.js.
* Install it with `npm install express`.
* `express()` creates an Express application.
* `app.listen()` starts the server.
* Express provides route methods such as `app.get()`, `app.post()`, `app.put()`, and `app.delete()`.
* `res.json()` sends JSON responses without manually using `JSON.stringify()`.
* `express.json()` parses JSON request bodies and makes them available through `req.body`.
* `req.params` contains dynamic values from route parameters such as `/users/:id`.
* `res.status()` allows you to specify the HTTP status code.
* `find()` can locate a user, while `filter()` can remove users from an array.
* Object spread (`...`) is useful for combining existing data with updates.
* Express simplifies API development compared with using Node's native `http` module directly.