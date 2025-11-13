# Backlog

Next  
25

8. Practice and understand **Closures** and their utility.
9. Grasp the concepts of **Prototype** and the **Prototype Chain**.
12. Investigate how JavaScript handles **Garbage Collection**.

14. Understand and implement **arrow functions**.
20. Study asynchronous patterns using **callbacks**.
21. Master **promises** for handling asynchronous operations.
22. Implement and understand **`async`/`await`** for cleaner asynchronous code.
23. Learn basic **error handling** with **`try`/`catch`** blocks.
24. Study how to create and use **custom errors**.
X 25. Understand the difference between **Deep vs Shallow Copy**.
26. Explore the concept of **Immutability** in JavaScript.
27. Learn to use **`Object.assign`** for shallow copying and merging objects.
28. Understand when to use **`structuredClone`** for deep copying.

32. Functions as first class citizens in JavaScript
33. Function expression, Function statement, Function Declaration
34. Higher order functions
35. Pure functions
36. IIFE
37. Memory Leaks
38. Optional chaining
39. Nullish coalescing
40. Classes (Take it from Learn Angular book)
42. Currying
43. Memoization
44. Custom Hooks

- Find second highest number from the given list of numbers.
- Find second lowest number from the given list of numbers.
- == vs ===
- Coding question - https://www.youtube.com/shorts/8EkdjS8IY0Q
- Coding question - https://www.youtube.com/shorts/wDuJUiJfv4k


## #8 Practice and understand **Closures** and their utility.

### Closures

Closure = Function + Lexical Environment
A **Closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). In simpler terms, a closure gives you access to an outer function's scope from an inner function, even after the outer function has finished executing.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A function that remembers and accesses variables from its parent's scope, even when executed outside that parent's scope. |
| **Why does it happen?** | Because functions in JavaScript are **lexically scoped**—they are defined where they are written, not where they are executed. The inner function maintains a persistent reference (a 'closure') to the parent's Lexical Environment. |
| **How to create it?** | A closure is automatically created whenever you define a function inside another function and then expose that inner function (usually by returning it or passing it as a callback). |

### Code Example: The Core Concept

```javascript
function createCounter() {
    let count = 0; // The private variable, part of the closure's environment

    // The returned function forms the closure
    return function increment() {
        count = count + 1;
        console.log(count);
    };
}

const counterA = createCounter(); // Outer function runs and finishes
counterA(); // Output: 1 (The inner function still remembers 'count')
counterA(); // Output: 2

const counterB = createCounter(); // Creates a completely separate closure/environment
counterB(); // Output: 1 (Starts a new 'count' at 0)
```

**Explanation:** When `createCounter()` finishes, its Execution Context is normally destroyed. However, because the returned `increment` function still needs access to the `count` variable, the JavaScript engine preserves the lexical environment containing `count=0`. This preserved environment is the **closure**.

---

### Real-Life Use Cases of Closures

Closures are fundamental to many common JavaScript patterns:

### Use Case 1: Data Privacy and Encapsulation (Module Pattern) 🛡️

Closures are the traditional way to achieve true **private variables** in JavaScript by hiding data inside the outer function's scope.

```javascript
// A simple module that encapsulates a private state
const userManager = (function() {
    let _activeUser = 'Guest'; // Private variable (prefixed with _ convention)

    return {
        login: function(name) {
            _activeUser = name;
            console.log(`${_activeUser} logged in.`);
        },
        getUsername: function() {
            return _activeUser; // Closure accesses the private _activeUser
        }
    };
})();

userManager.login('Alice'); 
console.log('Current User:', userManager.getUsername()); // Output: Current User: Alice
// console.log(userManager._activeUser); // undefined - cannot access the private variable
```

### Use Case 2: Function Currying and Partial Application 🥕

Currying is the process of transforming a function that takes multiple arguments into a sequence of functions, each taking a single argument. Closures maintain the state of the arguments between the calls.

```javascript
// Function that returns a function that returns a function (currying)
function multiply(a) {
    return function(b) {
        return function(c) {
            return a * b * c; // Closures remember 'a' and 'b'
        };
    };
}

const multiplyByTen = multiply(10); 
const tenTimesFive = multiplyByTen(5);
console.log(tenTimesFive(2)); // Output: 100 (10 * 5 * 2)
```

---

### Modern Relevance and Alternatives

### Is using Closure still a good practice?

**Yes, absolutely.** Closures are not just a quirk; they are a fundamental part of JavaScript's design.

1. **Immutability/Functional Programming:** Closures are essential for writing pure functions and managing state in functional patterns.
2. **React Hooks:** Custom Hooks in React heavily rely on closures to maintain state variables (`useState`) and reference values (`useRef`) across re-renders.
3. **Performance:** While there is a slight memory overhead to maintain the lexical environment, modern JS engines are highly optimized to handle closures efficiently.

### Is there any alternative of closure?

The need for data encapsulation and private state management that closures traditionally solved can now be addressed using newer ES features:

1. **ES6 Classes and `#` Private Fields (The Modern Alternative):**
    ES2022 introduced native support for truly private class fields using the `#` prefix. This is the official, object-oriented alternative for data hiding within classes.

    ```javascript
    class Car {
        #speed = 0; // Truly private field

        accelerate() {
            this.#speed += 10; // Only accessible inside the class
            console.log(`Speed: ${this.#speed}`);
        }
    }
    const myCar = new Car();
    myCar.accelerate(); // Output: Speed: 10
    // console.log(myCar.#speed); // SyntaxError: Private field must be declared in an enclosing class
    ```

2. **ES6 Modules (For Encapsulation):**
    Variables declared at the top level of an ES module are **module-scoped** and are not globally accessible. Only values that are explicitly `export`ed are public, providing strong encapsulation without needing a closure pattern (like the IIFE Module Pattern).

**Conclusion:** Closures are still mandatory for functional patterns (like currying, memoization, and custom hooks). For object-oriented data privacy, ES6 Classes with private `#` fields are the modern, cleaner alternative.

---

### 🔗 References

* **MDN Web Docs:** [Closures - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
* **javascript.info:** [Closures](https://javascript.info/closure)
* **YouTube Video:** Search for "JavaScript Closures Explained" for a visual deep dive.

---

## #9 Grasp the concepts of **Prototype** and the **Prototype Chain**.

**Prototypes** and the **Prototype Chain** are foundational to object-oriented programming in JavaScript.

### Prototypes and the Prototype Chain

JavaScript uses a **prototype-based inheritance model**, meaning objects inherit properties and methods from other objects. **Prototypes** are the mechanism by which this inheritance is achieved.

### What is a Prototype?

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A plain object (the prototype object) that another object uses as a blueprint for inheriting properties and methods. |
| **How to Access it?** | Every object in JavaScript has an internal property, accessible via `Object.getPrototypeOf(obj)` or, historically and less commonly, the non-standard `obj.__proto__`. |
| **Function Role** | Every function (including constructor functions) automatically gets a special property called **`prototype`** (not to be confused with the internal `__proto__` link). Objects created via `new FunctionName()` automatically get linked to `FunctionName.prototype`. |

### Code Example: Creating and Inheriting

```javascript
// 1. Define a Constructor Function
function Vehicle(make) {
    this.make = make;
}

// 2. Add a method to the constructor's prototype
Vehicle.prototype.startEngine = function() {
    console.log(`The ${this.make} engine is starting.`);
};

// 3. Create a new object instance
const car = new Vehicle('Honda');

// The instance 'car' does not have 'startEngine' directly, but inherits it
car.startEngine(); // Output: The Honda engine is starting.

// Accessing the internal prototype link
console.log(Object.getPrototypeOf(car) === Vehicle.prototype); // Output: true
```

---

### The Prototype Chain: The Inheritance Link

The **Prototype Chain** is the hierarchy of links between objects that determines the inheritance path.

### How the Chain Works

1. When you try to access a property or method on an object (e.g., `car.make` or `car.startEngine()`), the JavaScript engine first checks if the property exists **directly** on the object itself.
2. If it doesn't find the property there, it follows the internal **`[[Prototype]]` link** (the `__proto__` link) to the object's prototype.
3. It checks the prototype object for the property.
4. If it still isn't found, it follows the prototype's own `[[Prototype]]` link up the chain.
5. This continues until it reaches the end of the chain, which is always **`Object.prototype`**.
6. The prototype of `Object.prototype` is **`null`**, which terminates the chain. If the property is not found anywhere, the result is `undefined`.

### Code Example: Traversing the Chain

```javascript
// Function Constructor
function Animal(name) {
    this.name = name;
}
Animal.prototype.speak = function() { return `${this.name} makes a sound.`; };

// Inheriting Constructor (ES5 style)
function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
}
Dog.prototype = Object.create(Animal.prototype); // 1. Link Dog's prototype to Animal's prototype

const rex = new Dog('Rex', 'Shepherd');

// Prototype Chain Lookup for rex.toString:
// 1. Is toString on 'rex'? No.
// 2. Follow __proto__ to Dog.prototype. Is toString here? No.
// 3. Follow Dog.prototype's __proto__ to Animal.prototype. Is toString here? No.
// 4. Follow Animal.prototype's __proto__ to Object.prototype. Is toString here? YES!
console.log(rex.toString()); // Inherits from Object.prototype
```

**Chain:** `rex` $\rightarrow$ `Dog.prototype` $\rightarrow$ `Animal.prototype` $\rightarrow$ `Object.prototype` $\rightarrow$ `null`

### Relationship with Classes (ES6)

ES6 **Classes** are primarily **syntactical sugar** over the prototype-based inheritance model. The `extends` and `super` keywords simply automate the complex linkage necessary to set up the prototype chain correctly.

**ES6 Class Example:**

```javascript
class Animal {
    // ...
}
class Dog extends Animal { // Internally sets Dog.prototype to inherit from Animal.prototype
    // ...
}
```

The underlying mechanism is still the prototype chain.

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Prototype** | An object serving as a template; an instance object is linked to it via the internal `[[Prototype]]` property. |
| **Prototype Chain** | The sequence of linked objects that the JS engine traverses to look up properties and methods, providing inheritance. |
| **Lookup Rule** | Properties are searched directly on the object, then up the chain, until `Object.prototype` (and then `null`) is reached. |
| **ES6 Classes** | They are syntactic sugar; they use the **Prototype Chain** under the hood to manage inheritance. |

---

### 🔗 References

* **MDN Web Docs:** [Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
* **javascript.info:** [Prototypal inheritance](https://javascript.info/prototype-inheritance)
* **YouTube Video:** Search for "JavaScript Prototypes and Prototype Chain" for visual diagrams.

---

## #12 Investigate how JavaScript handles **Garbage Collection**.

### Garbage Collection

**Garbage Collection** is the process of automatically reclaiming memory that is no longer being used by a program. Since JavaScript is a high-level language, developers don't manually allocate or deallocate memory; the JavaScript engine (like V8 in Chrome/Node.js) handles this automatically through its garbage collector.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | An automated background process that tracks memory allocations and deallocates (frees) the memory occupied by objects and values that are no longer **reachable** (in use). |
| **Why use it?** | To prevent **memory leaks** and simplify development by relieving the programmer from manual memory management, which is often error-prone. |
| **How does it work?** | Modern collectors use complex algorithms, primarily based on the concept of **reachability**, to identify and clean up unused memory. |

### The Core Principle: Reachability

The fundamental principle of JavaScript garbage collection is **reachability**. A piece of memory is considered "in use" if it is **reachable** from the program's root (Global Object or the current Execution Context).

**Roots** include:

* The currently executing function/variables.
* The global object (`window` or `global`).
* The values currently on the call stack.

Any object that can be reached from a root is considered **reachable** (alive). If an object can no longer be reached, it is considered **unreachable** (garbage) and subject to collection.

### Modern Algorithm: Mark-and-Sweep

The most common algorithm used by modern JS engines is called **Mark-and-Sweep**:

1. **Marking Phase:** The collector starts from the **roots** and traverses all references, marking every object it can reach as "active" or "alive."
2. **Sweeping Phase:** The collector scans the heap (the memory area where objects live) and sweeps away (deallocates) all memory occupied by objects that were **not** marked as active.

This method is efficient because it only focuses on reaching objects from the roots, rather than tracking references between every single object.

### Generational Collection (Optimization)

To improve performance, modern engines use a Generational Collector:

* **Young Generation (Nursery):** A smaller area for new and short-lived objects. Objects here are subject to frequent, quick checks (minor GC). Most objects die young.
* **Old Generation (Main Heap):** A larger area for long-lived objects (those that survive multiple minor GC checks). This area is checked less frequently but more thoroughly (major GC).

This optimization dramatically reduces the pause time the GC takes, ensuring a smoother application experience.

---

### Memory Leaks and Garbage Collection

### Relationship to Memory Leaks

**Yes, memory leaks in JavaScript are directly related to Garbage Collection (GC).**

A **memory leak** occurs when memory that is no longer needed by the application is somehow *still considered reachable* by the garbage collector. Because the GC thinks the memory is still needed, it never collects it, leading to wasted memory and performance degradation over time.

### Common Causes of Memory Leaks (GC Failures)

1. **Accidental Global Variables:** In non-strict mode, forgetting to use `var`/`let`/`const` creates a property on the global object, keeping it alive indefinitely.
2. **Unremoved Event Listeners:** Attaching listeners to DOM elements but failing to remove them when the element is destroyed. The closure containing the listener often holds a reference to the element or surrounding scope.
3. **Forgotten Timers or Callbacks:** Using `setInterval` or `setTimeout` and failing to call `clearInterval` or `clearTimeout` when they are no longer needed. The callback and its scope remain alive.
4. **Out-of-Scope Closures:** A closure that is retained longer than expected can keep large objects in its scope chain alive, even if those objects are no longer used by the program.

---

### 🔗 References

* **MDN Web Docs:** [Memory Management - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)
* **Google Developers (V8):** [A tour of V8: Garbage Collection](https://www.google.com/search?q=https://v8.dev/blog/garbage-collection) (Highly technical but excellent insight).
* **YouTube Video:** Search for "JavaScript Garbage Collection Mark and Sweep" for visual explanations.

---

## #14 Understand and implement **Arrow Functions**. **Arrow Functions** Vs **Regular Functions**, when to use what?

### Arrow Functions vs. Regular Functions

Arrow functions (introduced in ES6) are a compact alternative to traditional function expressions, but they are not just syntactic sugar; their main difference lies in how they handle `this`, arguments, and constructors.

### The Differences

| Feature | Arrow Function (`=>`) | Regular Function (`function`) |
| :--- | :--- | :--- |
| **Syntax** | Concise; single-expression body allows implicit `return`. | Verbose; requires `return` keyword for output. |
| **`this` Binding** | **Lexically scoped.** Inherits `this` from the surrounding parent scope (the place where it's defined). **Cannot** be bound dynamically. | **Dynamically scoped.** `this` is determined by *how* the function is called (Default, Implicit, Explicit, or `new` binding). |
| **`arguments` Object** | **No** `arguments` object. Must use the **Rest Parameter** (`...args`). | Has its own local `arguments` object, containing all passed arguments. |
| **As a Constructor** | **Cannot** be used with the `new` keyword. Throws a `TypeError`. | **Can** be used as a constructor function to create new objects. |
| **Method Usage** | Generally **not suitable** for object methods due to lack of dynamic `this`. | **Suitable** for object methods. |

---

### 1\. `this` Binding: The Crucial Distinction

This is the single most important difference. Arrow functions do not create their own `this` context.

#### Regular Function (`function`): Dynamic `this`

The value of `this` is determined when the function is **called**.

```javascript
const user = {
    name: 'Alice',
    logRegular: function() {
        // 'this' is bound to the 'user' object (Implicit Binding)
        console.log('Regular:', this.name);
    }
};
user.logRegular(); // Output: Regular: Alice
```

#### Arrow Function (`=>`): Lexical `this`

The value of `this` is determined when the function is **defined** (from the surrounding code block).

```javascript
const user = {
    name: 'Alice',
    logArrow: () => {
        // 'this' is inherited from the surrounding scope, which is usually the Global/Window object.
        console.log('Arrow:', this.name); 
    }
};
user.logArrow(); // Output: Arrow: undefined (or empty string, because 'this' is the global object, which lacks a 'name' property)
```

**Key Use Case:** The power of lexical `this` is best seen inside callbacks, where you want to retain the `this` of the outer function:

```javascript
function Timer() {
    this.seconds = 0; // 'this' refers to the Timer instance

    setInterval(() => {
        // Arrow function inherits 'this' from the outer Timer function/scope
        this.seconds++;
        // If this was a regular function, 'this' inside would be the Global object.
    }, 1000);
}
// new Timer().seconds will update correctly because of the arrow function
```

---

### 2\. Syntax and Implicit Return

Arrow functions offer concise syntax, especially for single-expression returns.

| Regular Function | Arrow Function |
| :--- | :--- |
| ` javascript const add = function(a, b) { return a + b; };  ` | ` javascript const add = (a, b) => a + b;  ` |
| ` javascript const square = function(x) { return x * x; };  ` | ` javascript const square = x => x * x; // Single argument, no parentheses needed  ` |

**Note on returning objects:** To implicitly return an object literal, you must wrap it in parentheses: `const getObj = () => ({ key: 'value' });`

---

### When to Use What

| Scenario | Recommendation | Why? |
| :--- | :--- | :--- |
| **Object Methods** | **Regular Function** | Need the dynamic, implicit `this` binding to refer to the object itself. |
| **Constructors** | **Regular Function** | Arrow functions cannot be used with `new`. |
| **Global Functions** | **Regular Function** | For functions exposed to the global scope or used across modules. |
| **Array Methods** (`map`, `filter`, `reduce`) | **Arrow Function** | Conciseness and clean syntax make them ideal for quick, simple callbacks. |
| **Callbacks (especially Timers/Events)** | **Arrow Function** | Guarantees that `this` retains the context of the surrounding scope, avoiding the common need for binding (`.bind(this)`) or storing `this` in a local variable (`that = this`). |

---

### 🔗 References

* **MDN Web Docs:** [Arrow function expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
* **javascript.info:** [Arrow functions revisited](https://javascript.info/arrow-functions-basics)
