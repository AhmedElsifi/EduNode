# Working with APIs and Asynchronous JavaScript

## Introduction

Modern web applications rarely work with static data. Instead, they communicate with servers and APIs to retrieve, create, update, and delete data.

In this section, we learn how JavaScript communicates with external services using APIs, how asynchronous operations work, and how modern tools such as Fetch API and Async/Await simplify working with remote data.

---

## What is an API?

API stands for **Application Programming Interface**.

An API allows applications to communicate with each other and exchange data.

Examples:

- Weather APIs
- Payment APIs
- Authentication APIs
- Recipe APIs
- Social Media APIs

The section uses the **Forkify API** to retrieve recipe data.

---

## CRUD Operations

Most APIs support CRUD operations.

| Operation | HTTP Method | Purpose              |
| --------- | ----------- | -------------------- |
| Create    | POST        | Add new data         |
| Read      | GET         | Retrieve data        |
| Update    | PUT / PATCH | Modify existing data |
| Delete    | DELETE      | Remove data          |

Examples:

```http
GET /products
```

Retrieve all products.

```http
POST /products
```

Create a new product.

```http
PUT /products/1
```

Update product with ID 1.

```http
DELETE /products/1
```

Delete product with ID 1.

---

## JSON

Most APIs exchange data using JSON.

JSON stands for:

**JavaScript Object Notation**

Example:

```json
{
  "name": "Ahmed",
  "age": 20
}
```

JSON is lightweight, readable, and easy to convert to JavaScript objects.

---

## AJAX

AJAX stands for:

**Asynchronous JavaScript and XML**

AJAX allows JavaScript to communicate with a server without refreshing the page.

Benefits:

- Faster user experience
- Dynamic content updates
- Reduced page reloads

---

## XMLHttpRequest (XHR)

Before Fetch API, developers used XMLHttpRequest to communicate with servers.

Example:

```js
let xml = new XMLHttpRequest();

xml.open("GET", "https://example.com");

xml.send();
```

The request is sent asynchronously.

---

## XMLHttpRequest Ready States

The XMLHttpRequest object passes through several states.

| State | Description             |
| ----- | ----------------------- |
| 0     | Request not initialized |
| 1     | Connection established  |
| 2     | Request received        |
| 3     | Processing request      |
| 4     | Response received       |

Example:

```js
xml.addEventListener("readystatechange", () => {
    if(xml.readyState === 4){
        console.log(xml.response);
    }
});
```

---

## Parsing JSON Responses

API responses usually arrive as JSON strings.

To convert them into JavaScript objects:

```js
let data = JSON.parse(xml.response);
```

After parsing, the data can be displayed in the UI.

---

## Dynamic HTML Generation

JavaScript can generate HTML dynamically using template literals.

Example:

```js
box += `
<div class="card">
    <h5>${product.title}</h5>
</div>
`;
```

The generated HTML can then be inserted into the page:

```js
document.getElementById("dataTable").innerHTML = box;
```

---

## Callback Functions

A callback is a function passed as an argument to another function.

Example:

```js
function test(callback){
    callback();
}
```

Usage:

```js
test(function(){
    console.log("Done");
});
```

---

## Callback Hell

When callbacks become deeply nested, code becomes difficult to read.

Example:

```js
getPizza(function(){
    calcSum(function(){
        test(function(){
            sayHello(function(){

            });
        });
    });
});
```

This problem is known as:

**Callback Hell**

---

## Promises

Promises were introduced to solve callback hell.

A Promise represents a future result.

Example:

```js
let promise = new Promise((resolve, reject) => {

});
```

Promise states:

- Pending
- Fulfilled
- Rejected

---

## Promise Methods

### then()

Runs when the Promise succeeds.

```js
promise.then(() => {
    console.log("Success");
});
```

### catch()

Runs when an error occurs.

```js
promise.catch(error => {
    console.log(error);
});
```

### finally()

Runs regardless of success or failure.

```js
promise.finally(() => {
    console.log("Finished");
});
```

---

## Promise Chaining

Multiple asynchronous operations can be chained together.

```js
promise
.then(() => {})
.then(() => {})
.catch(() => {});
```

This is cleaner than nested callbacks.

---

## Fetch API

Fetch is the modern replacement for XMLHttpRequest.

Example:

```js
fetch("https://example.com");
```

Fetch returns a Promise.

---

## Using Fetch

Example:

```js
fetch("https://example.com")
.then(response => response.json())
.then(data => console.log(data));
```

The response must be converted to JSON before use.

---

## Async and Await

Async/Await makes asynchronous code easier to read.

### async

Marks a function as asynchronous.

```js
async function getData() {

}
```

### await

Waits for a Promise to finish.

```js
let response = await fetch(url);
```

---

## Fetch with Async/Await

Example:

```js
async function getPizza(){

    let response = await fetch(apiUrl);

    let data = await response.json();

    console.log(data);
}
```

This syntax is easier to understand than Promise chains.

---

## Sequential Async Operations

Multiple asynchronous operations can be controlled using await.

```js
async function getAll(){

    await getPizza();

    calcSum();
}
```

The second function runs only after the first operation finishes.

---

## Bootstrap Cards with API Data

The retrieved API data can be displayed using Bootstrap components.

Example:

```html
<div class="card">
    <h5>Pizza Recipe</h5>
</div>
```

Combining APIs with Bootstrap allows developers to build dynamic interfaces quickly.

---

## Summary

In this section, we learned how modern web applications retrieve data from APIs using AJAX concepts. We explored XMLHttpRequest, ready states, JSON parsing, callback functions, callback hell, Promises, Fetch API, Async/Await, and dynamic HTML rendering.

These concepts form the foundation of working with external APIs and are essential for modern frontend development using JavaScript frameworks and libraries.