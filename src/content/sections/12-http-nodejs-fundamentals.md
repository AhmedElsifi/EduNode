# Section 12 — HTTP and Node.js Fundamentals

## HTTP and Communication

A **protocol** defines how two systems communicate and exchange data.

In web development, the frontend and backend commonly communicate using HTTP-based protocols.

Common protocols include:

* `HTTP` — standard protocol for web communication.
* `HTTPS` — encrypted version of HTTP.
* `FTP` — used for file transfer.
* `SMTP` — used for sending emails.
* `WebSocket` — enables persistent, two-way communication between client and server.

> **Key Takeaway:** HTTP is the protocol commonly used for communication between a web client and a server, using requests and responses.

## HTTP Request Methods

HTTP methods describe the action the client wants the server to perform.

| Method   | Purpose                        |
| -------- | ------------------------------ |
| `GET`    | Retrieve data                  |
| `POST`   | Create or submit data          |
| `PUT`    | Replace existing data          |
| `PATCH`  | Partially update existing data |
| `DELETE` | Delete data                    |

For example:

```http
GET /users
POST /users/signup
DELETE /users

```

## HTTP Status Codes

HTTP status codes tell the client how the server handled a request.

| Range     | Meaning            |
| --------- | ------------------ |
| `200–299` | Successful request |
| `300–399` | Redirection        |
| `400–499` | Client error       |
| `500–599` | Server error       |

For example:

* `200 OK` — request succeeded.
* `404 Not Found` — requested resource or endpoint does not exist.
* `500 Internal Server Error` — server encountered an error.

A useful website for visualizing HTTP status codes is `http.cat`.

## What Is Node.js?

**Node.js** is a JavaScript runtime environment that allows JavaScript to run outside the browser.

This makes it possible to use JavaScript for server-side development.

With Node.js, you can build:

* HTTP servers
* REST APIs
* Backend applications
* CLI applications
* File-processing tools

> **Key Takeaway:** Node.js is not a programming language. It is a runtime environment that executes JavaScript outside the browser.

## Node.js Modules

Node.js applications can use different types of modules.

### Built-in Modules

These modules are included with Node.js and do not need to be installed.

Examples:

* `http` — create HTTP servers.
* `fs` — work with the file system.
* `os` — interact with operating-system information.

```js
const http = require("http");
const fs = require("fs");
const os = require("os");

```

### Custom Modules

Modules created by you or within your own project.

They can be shared between files using module exports/imports.

### Third-Party Modules

Packages created by other developers and installed through `npm`.

```bash
npm install package-name

```

They can then be imported into your application.

## Creating an HTTP Server

Node.js provides the built-in `http` module for creating HTTP servers.

```js
const http = require("http");

const server = http.createServer((req, res) => {
    res.end("Hello from server");
});

server.listen(3000, () => {
    console.log("Server running");
});

```

### `http.createServer()`

`http.createServer()` creates an HTTP server.

Its callback receives two important objects:

* `req` — contains information about the incoming request.
* `res` — used to send a response back to the client.

```js
http.createServer((req, res) => {
    // Handle request
});

```

### `server.listen()`

`server.listen()` starts the server on a specific port.

```js
server.listen(3000);

```

The application can then be accessed at:

```text
http://localhost:3000

```

## Sending JSON Responses

When returning JSON data, the response should specify the appropriate content type.

```js
res.setHeader("Content-Type", "application/json");

```

JavaScript objects cannot be sent directly as an HTTP response, so they need to be converted into a JSON string.

```js
res.end(JSON.stringify(users));

```

Example:

```js
const users = [
    {
        id: 1,
        name: "Ahmed"
    },
    {
        id: 2,
        name: "Yara"
    }
];

res.setHeader("Content-Type", "application/json");
res.end(JSON.stringify(users));

```

The client receives:

```json
[
    {
        "id": 1,
        "name": "Ahmed"
    },
    {
        "id": 2,
        "name": "Yara"
    }
]

```

> **Warning:** `res.end()` expects the response body as data that can be sent over HTTP. Use `JSON.stringify()` when sending JavaScript objects or arrays as JSON.

## Routing with Node.js HTTP

Without a framework such as Express, routes can be handled manually by checking:

* `req.url`
* `req.method`

Example:

```js
if (req.url === "/users" && req.method === "GET") {
    // Get users
}

```

A simple API can therefore have multiple endpoints:

```text
GET     /users
POST    /users/signup
DELETE  /users

```

## Handling GET Requests

A `GET` request can return the current users.

```js
if (req.url === "/users" && req.method === "GET") {
    res.setHeader("Content-Type", "application/json");
    res.end(JSON.stringify(users));
}

```

The server checks both the URL and HTTP method to determine which operation should run.

## Handling POST Request Data

When a client sends data in a `POST` request, Node.js receives the request body as a stream.

The `data` event can be used to receive incoming chunks:

```js
req.on("data", (chunk) => {
    // Process incoming data
});

```

If the client sends JSON, the chunk can be converted into a JavaScript object:

```js
const newUser = JSON.parse(chunk);

```

The new user can then be added to the in-memory array:

```js
users.push(newUser);

```

### Example Signup Endpoint

```js
if (req.url === "/users/signup" && req.method === "POST") {
    req.on("data", (chunk) => {
        const newUser = JSON.parse(chunk);

        const exists = users.find(
            (user) => user.email === newUser.email
        );

        if (exists) {
            return res.end("User already exists, please login");
        }

        newUser.id = users[users.length - 1].id + 1;

        users.push(newUser);

        res.end("User added successfully");
    });
}

```

The flow is:

1. Check that the request is `POST /users/signup`.
2. Read the request body.
3. Parse the JSON using `JSON.parse()`.
4. Check whether the email already exists.
5. Generate an ID.
6. Add the user to the `users` array.
7. Send a response.

## Searching Arrays with `find()`

The `Array.prototype.find()` method searches for the first element matching a condition.

```js
const exists = users.find(
    (user) => user.email === newUser.email
);

```

If a matching user exists, `find()` returns that user object.

If no match exists, it returns `undefined`.

This makes it useful for checking whether an email is already registered.

## Deleting Data with `filter()`

The `filter()` method creates a new array containing only elements that satisfy a condition.

The delete endpoint uses it to remove a user by ID:

```js
users = users.filter((user) => user.id !== id);

```

For example, if the ID is `2`, every user except the user with `id === 2` remains in the array.

> **Key Takeaway:** `find()` is useful when you need to retrieve a matching element, while `filter()` is useful when you need to create an array excluding or selecting elements.

## Handling DELETE Request Data

The same request-body mechanism can be used for a `DELETE` request.

```js
if (req.url === "/users" && req.method === "DELETE") {
    req.on("data", (chunk) => {
        const id = JSON.parse(chunk).id;

        users = users.filter((user) => user.id !== id);

        res.end("Delete done");
    });
}

```

The client could send:

```json
{
    "id": 2
}

```

The server extracts the ID and removes the corresponding user.

## Handling Unknown Endpoints

A final `else` can handle requests that do not match any defined route:

```js
else {
    res.end("Not found endpoint");
}

```

A production API would normally return an appropriate HTTP status such as `404`.

## Important Request/Response Concepts

The basic flow of an HTTP API is:

```text
Client
  │
  │ HTTP Request
  │ method + URL + body
  ▼
Node.js Server
  │
  │ route handling
  │ business logic
  ▼
Response
  │
  │ status + headers + body
  ▼
Client

```

For example:

```text
POST /users/signup
        │
        ▼
Parse JSON body
        │
        ▼
Check email
        │
        ▼
Create user
        │
        ▼
Send response

```

> **Warning:** The example stores users in a JavaScript array, so all data is lost when the Node.js process restarts. Real applications use persistent storage such as MongoDB or another database.

## Summary

* HTTP is a protocol used for communication between clients and servers.
* Common HTTP methods include `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`.
* HTTP status codes are grouped into `2xx` success, `3xx` redirection, `4xx` client errors, and `5xx` server errors.
* Node.js allows JavaScript to run on the server.
* Node.js provides built-in modules such as `http`, `fs`, and `os`.
* `http.createServer()` creates an HTTP server.
* `req` represents the incoming request, while `res` is used to send the response.
* `req.url` and `req.method` can be used to implement basic routing.
* `JSON.stringify()` converts JavaScript data into JSON for an HTTP response.
* `JSON.parse()` converts incoming JSON into a JavaScript value.
* `req.on("data", ...)` can be used to read request-body data.
* `find()` searches for a matching element, while `filter()` creates a new filtered array.
* A real backend should use persistent database storage instead of an in-memory array.