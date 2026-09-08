# Modern JavaScript (ES6+) Fundamentals

## Introduction

In this section, we move beyond basic JavaScript and explore the features introduced in ES6 (ECMAScript 2015) and later versions. These features make code cleaner, easier to maintain, and more powerful.

We will learn about variable declarations, template literals, default parameters, the spread operator, arrow functions, destructuring, and modern array methods commonly used in real-world applications.

---

## Variable Declarations

### var

The traditional way of declaring variables.

Features:
- Can be redeclared.
- Can be reassigned.
- Function scoped.
- Supports hoisting.

```js
var name = "Ahmed";
var name = "Mohamed";
````

### let

Introduced in ES6.

Features:

* Cannot be redeclared in the same scope.
* Can be reassigned.
* Block scoped.

```js
let age = 20;
age = 21;
```

### const

Used for values that should not be reassigned.

Features:

* Cannot be redeclared.
* Cannot be reassigned.
* Block scoped.

```js
const country = "Egypt";
```

---

## Hoisting

Hoisting is JavaScript's behavior of moving declarations to the top of their scope before execution.

### Variable Hoisting

```js
console.log(x);

var x = 10;
```

### Function Hoisting

```js
sayHello();

function sayHello() {
    console.log("Hello");
}
```

Function declarations are hoisted, while function expressions are not.

---

## Template Literals

Template literals allow embedding variables directly inside strings.

```js
let name = "Ahmed";
let age = 20;

console.log(`My name is ${name} and I am ${age}`);
```

Benefits:

* Cleaner syntax.
* Supports multiline strings.
* Supports variable interpolation.

---

## Default Parameters

Default values can be assigned to function parameters.

```js
function printData(name = "Ahmed", age = 20) {
    console.log(name, age);
}
```

If no value is passed, the default value is used.

---

## Spread Operator

The spread operator (`...`) expands arrays or objects.

### Copying Arrays

```js
let arr1 = [1, 2, 3];
let arr2 = [...arr1];
```

### Merging Arrays

```js
let arr1 = [1, 2];
let arr2 = [3, 4];

let result = [...arr1, ...arr2];
```

### Passing Arguments

```js
let nums = [10, 20, 30, 40];

sum(...nums);
```

---

## Arrow Functions

Arrow functions provide a shorter syntax for writing functions.

### Traditional Function

```js
function sayHello(name) {
    return "Hello " + name;
}
```

### Arrow Function

```js
const sayHello = name => `Hello ${name}`;
```

Advantages:

* Shorter syntax.
* Commonly used with array methods.
* Frequently used in modern frameworks.

---

## Destructuring

Destructuring extracts values from objects and arrays into variables.

### Object Destructuring

```js
const person = {
    name: "Ahmed",
    age: 26
};

const { name, age } = person;
```

### Array Destructuring

```js
const numbers = [10, 20, 30];

const [x, y, z] = numbers;
```

Benefits:

* Cleaner code.
* Easier access to data.

---

## Modern Array Methods

Modern JavaScript provides powerful methods for working with arrays.

### filter()

Returns all elements that satisfy a condition.

```js
let numbers = [10,20,30,40,50,60];

let result = numbers.filter(num => num > 50);
```

Output:

```js
[60]
```

---

### find()

Returns the first matching element.

```js
let result = numbers.find(num => num > 50);
```

Output:

```js
60
```

---

### map()

Creates a new array by transforming each element.

```js
let result = numbers.map(num => num * 2);
```

Output:

```js
[20,40,60,80,100,120]
```

---

### forEach()

Executes a function for every element.

```js
numbers.forEach(num => {
    console.log(num);
});
```

Used when performing actions without creating a new array.

---

## Additional Array Methods

The following methods were mentioned for self-study:

### some()

Checks whether at least one element matches a condition.

```js
numbers.some(num => num > 50);
```

### reduce()

Reduces an array to a single value.

```js
numbers.reduce((sum, num) => sum + num, 0);
```

---

## String Methods

JavaScript provides built-in methods for working with strings.

Examples:

```js
productName.toLowerCase();

productName.includes("phone");

productName.slice(0, 3);
```

These methods are commonly used in search functionality and text processing.
