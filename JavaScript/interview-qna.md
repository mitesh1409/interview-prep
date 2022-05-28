# # JavaScript Interview Qs & As

Index
1. Hoisting
2. Implicit and Explicit Binding OR this Binding
3. Implement Caching/Memoize Function
4. Output Based Question on Event Loop
5. Infinite Currying


[Frontend Interview Experience (Cars24) - Javascript and React JS Interview Questions](https://www.youtube.com/watch?v=vxggZffOqek&t=7s)


#### #1 Hoisting

Meaning of "Hoist"
raise (something) by means of ropes and pulleys.

[Hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

JavaScript Hoisting refers to the process whereby the interpreter appears to move the declaration of functions, variables or classes to the top of their scope, prior to execution of the code.

Hoisting allows functions to be safely used in code before they are declared.

Variable and class declarations are also hoisted, so they too can be referenced before they are declared. Note that **doing so can lead to unexpected errors, and is not generally recommended**.

```
function def() {
    console.log('d', d); // Allows to access: d undefined

    var d = 10;
}

def();

function ghi() {
    console.log('g', g); // Allows to access: g undefined
    console.log('h', h); // Gives an error: Cannot access 'h' before initialization
    console.log('i', i); // Gives an error: Cannot access 'i' before initialization

    var g = 5;
    let h = 10;
    const i = 15;
}

ghi();
```

Variables declared with `const` & `let` are still hoisted in their "Temporal Dead Zone", but we can't access them like variables declared with `var`.

Use the debugger and check the "Scope" inside browser console.


#### #2 Implicit and Explicit Binding OR this Binding

this is bound differently in normal functions and arrow functions.

**this binding in normal function**
"this" is bound to the current object.
In case of normal functions, "this" in the parent scope is overridden by "this" in the current scope.

**this binding in arrow function**
In case of Arrow functions, "this" is not overridden.
It uses the same "this" as available in its parent's scope.

```
var objectOne = {
    firstName: 'Peter',

    // Normal function.
    // "this" is bound to the current object.
    // In case of normal functions, "this" in the parent scope is overridden by "this" in the current scope.
    display: function() {
        console.log('this', this);
        console.log('firstName', this.firstName);
    }

    // Arrow function.
    // In case of Arrow functions, "this" is not overridden.
    // It uses the same "this" as available in its parent's scope.
    // display function's parent is objectOne variable.
    // Since objectOne is defined in the global scope, "this" is bound to
    // the Window object in the global scope.
    // display: () => {
    //     console.log('this', this);
    //     console.log('firstName', this.firstName);
    // }
};

var objectTwo = {
    firstName: 'Stephen'
};

objectOne.display();
objectOne.display.call(objectTwo);
```


#### #3 Implement Caching/Memoize Function

We can have some function in our application that do some heavy or time consuming operations.
In this case it is a good idea to cache/memoize the results of this function.
So subsequent calls to this function won't take longer to finish, they will return cached result immediately.

```
// Some function that is doing some costly or time consuming operations.
const badMultiply = (num1, num2) => {
    for (let i = 0; i < 1000000000; i++) {}

    return num1 * num2;
}

console.time('badMultiply-1');
console.log('badMultiply', badMultiply(2000.99, 9000.11));
console.timeEnd('badMultiply-1');

console.time('badMultiply-2');
console.log('badMultiply', badMultiply(2000.99, 9000.11));
console.timeEnd('badMultiply-2');

console.time('badMultiply-3');
console.log('badMultiply', badMultiply(2000.99, 9000.11));
console.timeEnd('badMultiply-3');


// Memoize/Cache the result of a function within a given context.
// It saves function results for the given set of arguments.
function memoize(fn, context) {
    // Variable to save result of the function.
    const result = {};

    // Return a function.
    return function (...args) {
        let keyArgs = JSON.stringify(args);

        if (!result[keyArgs]) {
            result[keyArgs] = fn.call(context || this, ...args);
        }

        return result[keyArgs];
    }
}

// We memoize/cache a function only once, it returns a function that we store in a variable.
// After this, whenever we need we call this function via this variable.
const multiplyMemoize = memoize(badMultiply, this);

console.time('multiplyMemoize-1');
console.log('multiplyMemoize', multiplyMemoize(2000.99, 9000.11));
console.timeEnd('multiplyMemoize-1');

console.time('multiplyMemoize-2');
console.log('multiplyMemoize', multiplyMemoize(2000.99, 9000.11));
console.timeEnd('multiplyMemoize-2');

console.time('multiplyMemoize-3');
console.log('multiplyMemoize', multiplyMemoize(2000.99, 9000.11));
console.timeEnd('multiplyMemoize-3');
```


#### #4 Output Based Question on Event Loop

```
console.log("a");

setTimeout(() => console.log("setTimeout"), 0);

Promise
    .resolve(() => console.log("Promise"))
    .then((res) => res());

console.log("b");
```

`setTimeout` is not the part of the JavaScript, it is the part of Web APIs/Browsers.
It runs after the complete code in our JavaScript file has ran successfully, even if the value of timeout argument is 0.
This will go inside "Task Queue".

`Promise` runs at the end as well.
`Promise` goes inside "Microtask Queue" or "Priority Queue".

"Microtask Queue" or "Priority Queue" gets priority over the "Task Queue".

So the `Promise` gets priority over `setTimeout`.

The output will be as follows:
```
a
b
Promise
setTimeout
```


Backlog

#1 Temporal Deadzone
#2 how setTimeout() works?
#3 How Promise works?
#4 Event Loop


#### #5 Infinite Currying


