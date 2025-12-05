### 1. Is JavaScript statically typed or dynamically typed?

JavaScript is dynamically typed, meaning a variable’s type is determined at runtime and can change during execution.

let value = 10; // number
value = "Hello"; // string
value = true; // boolean

There is no need to specify data type explicitly.

### 2. How to build a scalable JavaScript application?

To build a scalable JavaScript application:

Use modular architecture (ES Modules – import/export)

Follow design patterns (MVC, MVVM, Singleton, Factory)

Use state management (Redux, Context API, Zustand)

Apply code splitting & lazy loading

Use proper folder structure (/services, /utils, /components)

Write reusable & testable code

Use Web Workers for heavy tasks

Implement caching & memoization

Best practices example structure:

src/
├── components/
├── services/
├── utils/
├── hooks/
└── pages/

### 3. Difference between var, let, and const

Feature var let const
Scope Function Block Block
Hoisting Yes (undefined) Yes (TDZ) Yes (TDZ)
Reassign ✅ Yes ✅ Yes ❌ No
Redeclare ✅ Yes ❌ No ❌ No
let name = "John";
name = "David"; // ✅ Works

const age = 20;
age = 25; // ❌ Error

### 4. What is Hoisting?

Hoisting is JavaScript's behavior of moving variable and function declarations to the top of their scope before execution.

console.log(a); // undefined
var a = 10;

// Behind the scenes:
var a;
console.log(a);
a = 10;

But with let and const, TDZ (Temporal Dead Zone) applies:

console.log(b); // ❌ ReferenceError
let b = 5;

### 5. What is the Event Loop?

The Event Loop handles asynchronous operations in JavaScript.

Key parts:

Call Stack

Web APIs

Callback Queue

Event Loop

Example:

console.log("Start");

setTimeout(() => {
console.log("Running");
}, 0);

console.log("End");

✅ Output:

Start
End
Running

Even with 0ms, setTimeout runs after the synchronous code finishes.

### 6. Difference between async and await

async makes a function return a Promise

await pauses execution until the Promise resolves

async function getData() {
const response = await fetch(url);
const data = await response.json();
return data;
}

✅ Makes async code look synchronous
✅ Improves readability

### 7. What is a Callback Function?

A callback is a function passed as an argument to another function and executed later.

function greet(name, callback) {
console.log("Hello " + name);
callback();
}

function sayBye() {
console.log("Bye!");
}

greet("Gautam", sayBye);

Used in:

setTimeout()

API calls

Event listeners

### 8. What is a Closure?

A closure is when a function remembers and accesses variables from its outer scope even after the outer function has finished execution.

function counter() {
let count = 0;
return function () {
count++;
return count;
};
}

const increment = counter();
increment(); // 1
increment(); // 2

Very important for:

Data privacy

Encapsulation

Function factories

### 9. What is a Promise?

A Promise represents the result of an async operation that may complete in the future.

States:

Pending

Fulfilled

Rejected

const promise = new Promise((resolve, reject) => {
setTimeout(() => {
resolve("Success!");
}, 2000);
});

promise.then(data => console.log(data));

### 10. Synchronous vs Asynchronous JavaScript

    Synchronous Asynchronous
    Blocks execution Non-blocking
    Runs line by line Runs in background
    Slow for large tasks Efficient
    // Synchronous
    console.log("1");
    console.log("2");

// Asynchronous
setTimeout(() => {
console.log("3");
}, 1000);

### 11. What is this in JavaScript?

this refers to the object calling the function.

const user = {
name: "Gautam",
getName() {
return this.name;
}
}
console.log(user.getName()); // Gautam

But in arrow functions, this refers to outer scope.

### 12. Difference between == and ===

    == ===
    Loose equality Strict equality
    Converts type No type conversion
    Slow Faster & recommended
    5 == "5" // true
    5 === "5" // false

✅ Always use ===

### 13. What is Event Delegation?

Event Delegation uses a parent element to handle events from child elements.

document.querySelector("#list").addEventListener("click", e => {
if(e.target.tagName === "LI"){
console.log(e.target.innerText);
}
});

✅ Reduces memory
✅ Optimizes performance

### 14. What is the Prototype in JavaScript?

Every JS object has a hidden [[Prototype]] that it inherits methods from.

function Person(name){
this.name = name
}

Person.prototype.sayHello = function(){
return "Hello " + this.name
}

const p1 = new Person("Gautam");
p1.sayHello();

### 15. What is Debouncing?

Limits how often a function executes.

function debounce(fn, delay){
let timer;
return function(){
clearTimeout(timer)
timer = setTimeout(fn, delay)
}
}

✅ Used in search inputs

### 16. What is Throttling?

Limits function call execution to once per time interval.

✅ Used in scrolling, resizing

### 17. Memory Leak in JavaScript

Occurs when unused memory is not released.

Reason:

Unused variables

DOM references

Closures

Event listeners

Solution:

Remove listeners

Clean references

element.removeEventListener(...)

### 18. What is IIFE (Immediately Invoked Function)?

    (function(){
    console.log("Runs immediately")
    })();

✅ Avoids global scope pollution

### 19. What is Babel/Webpack?

Babel → Converts ES6+ to ES5

Webpack → Bundler for JS files

Used in React/Vue/Angular projects.
