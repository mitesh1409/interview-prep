# JavaScript Interview Questions & Answers

## 📝 JavaScript Core Concepts

- [X] Study and understand the **Execution Context** in JavaScript.
- [X] Research the **Call Stack** and how it manages function calls.
- [X] Learn about the **Event Loop** and its role in handling asynchronous operations.
- [X] Understand the purpose and function of the **Task Queue** (or Callback Queue).
- [X] Compare and contrast **Microtasks vs Macrotasks** and their priority.
- [X] Master the concept of **Hoisting** for variables and functions, how Hoisting works for var, let, const.
- [X] Study **Scope** in detail: **Block Scope**, **Function Scope**, and **Global Scope**.
- [X] Practice and understand **Closures** and their utility.
- [X] Grasp the concepts of **Prototype** and the **Prototype Chain**.
- [X] Learn the different rules for **`this` binding** (default, implicit, explicit, new).
- [X] Understand the features and implications of using **Strict Mode** (`'use strict'`).
- [X] Investigate how JavaScript handles **Garbage Collection**.

---

## 🚀 Advanced JavaScript Concepts

- [X] Learn the differences and proper use of **`let`** and **`const`**.
- [ ] Understand and implement **arrow functions**.
- [ ] Practice using **template literals** for string interpolation.
- [ ] Master **destructuring** for arrays and objects.
- [ ] Understand and use the **spread/rest** operators.
- [ ] Learn to use **Modules** with the **`import`** and **`export`** syntax.
- [ ] Differentiate between **default vs named exports**.
- [ ] Study asynchronous patterns using **callbacks**.
- [ ] Master **promises** for handling asynchronous operations.
- [ ] Implement and understand **`async`/`await`** for cleaner asynchronous code.
- [ ] Learn basic **error handling** with **`try`/`catch`** blocks.
- [ ] Study how to create and use **custom errors**.
- [ ] Understand the difference between **Deep vs Shallow Copy**.
- [ ] Explore the concept of **Immutability** in JavaScript.
- [ ] Learn to use **`Object.assign`** for shallow copying and merging objects.
- [ ] Understand when to use **`structuredClone`** for deep copying.

---

## Backlog

- [ ] Lexical Scope (merge with #7 above)
- tagged templates (merge with #15 above)
- [ ] Functions as first class citizens in JavaScript
- [ ] Function expression, Function statement
- [ ] Higher order functions
- [ ] Pure functions
- [ ] IIFE
- [ ] Memory Leaks
- Optional chaining
- Nullish coalescing
- Classes
- https://nodejs.org/en/learn/getting-started/how-much-javascript-do-you-need-to-know-to-use-nodejs

---

## #1 Study and understand the **Execution Context** in JavaScript.

### Execution Context

The **Execution Context (EC)** is the abstract concept of an environment where JavaScript code is evaluated and executed. It is the core mechanism that the JavaScript engine uses to manage code execution.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A data structure created by the JavaScript engine to manage the execution of a function, a script, or a module. It defines the environment for the running code. |
| **Why use it?** | To manage **variable storage**, determine the value of the **`this`** keyword, track the **scope chain**, and provide a controlled, isolated environment for code execution. |
| **How does it work?** | When the engine starts executing code, it pushes a new EC onto the **Call Stack**. This EC is created in two phases: **Creation** and **Execution**. |

### Types of Execution Context

There are three main types of Execution Contexts:

1. **Global Execution Context (GEC):** The default context created when the script file first loads. There is only **one** GEC per application. It creates the global object (`window` in browsers, `global` in Node.js) and sets `this` to point to that global object.
2. **Function Execution Context (FEC):** A new context is created every time a function is called. There can be numerous FECs.
3. **Eval Execution Context:** A context created for code executed inside the `eval()` function (used less frequently).

---

### The Two Phases of Context Creation

When an Execution Context is created (either GEC or FEC), it goes through two distinct phases:

#### Phase 1: Creation Phase (Memory Allocation)

In this phase, the engine scans the code *before* execution starts, setting up the necessary components. This is where **Hoisting** occurs.

The EC object is structurally created with three components:

1. **Variable Environment (VE):**
      * Scans the code for **`var`** declarations and **function declarations**.
      * For `var` variables, it allocates memory and initializes them to **`undefined`**.
      * For function declarations, it stores the entire function in memory.
      * It also handles **`let`** and **`const`** declarations, but places them in a separate lexical environment and does *not* initialize them (they remain in the **Temporal Dead Zone**).
2. **Lexical Environment (LE):**
      * Conceptually similar to the VE, but stores `let` and `const` bindings.
      * It also contains the **Scope Chain**, which is the list of parent scopes the current context can access.
3. **`this` Binding:**
      * Determines the value of the `this` keyword (based on the four rules: Default, Implicit, Explicit, New).
      * For the GEC, `this` is bound to the Global Object.
      * For an FEC, `this` is determined by the call site.

#### Phase 2: Execution Phase (Code Running)

In this phase, the engine executes the code line by line.

  * It assigns the actual values to the variables.
  * It executes the function calls.
  * If a function is called, a **new Function Execution Context** is created and pushed onto the **Call Stack**.

---

### The Relationship with the Call Stack

The **Call Stack** is a crucial mechanism that manages the Execution Contexts.

  * It's a **LIFO (Last-In, First-Out)** data structure.
  * When a script starts, the **Global EC** is pushed onto the stack.
  * Whenever a function is called, a **Function EC** is created and pushed onto the top of the stack.
  * When a function finishes executing, its **Function EC** is popped off the stack, and control returns to the context below it.
  * The script finishes when the **Global EC** is popped off the stack.

#### Code Example

```javascript
let a = 10; // GEC scope

function first() { // FEC 1
    let b = 20;
    second();
    console.log('Finished first');
}

function second() { // FEC 2
    let c = 30;
    console.log('Executing second');
}

first(); 
// Call Stack Sequence:
// 1. Script loads: [ GEC ]
// 2. first() called: [ GEC, FEC 1 ]
// 3. second() called: [ GEC, FEC 1, FEC 2 ]
// 4. second() finishes: [ GEC, FEC 1 ] <-- FEC 2 is popped
// 5. first() finishes: [ GEC ] <-- FEC 1 is popped
// 6. Script finishes: [ ] <-- GEC is popped
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Core Function** | The EC is the **runtime environment** that hosts the code currently being executed. |
| **Two Phases** | **1. Creation:** Allocates memory (Variable Environment, Lexical Environment, `this` binding). **2. Execution:** Runs code line by line, assigning values. |
| **Components** | Key components are **Variable Environment** (`var`, function decls), **Lexical Environment** (`let`, `const`, Scope Chain), and **`this` Binding**. |
| **Stack Management** | The **Call Stack** manages the Execution Contexts in a LIFO order (Global EC at the bottom, current function EC at the top). |

---

### 🔗 References

* **MDN Web Docs:** [Scope and Context in JavaScript](https://www.google.com/search?q=https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var%23variable_hoisting) (Related to Execution Context structure)
* **javascript.info:** [Execution context, Scope chain](https://www.google.com/search?q=https://javascript.info/function-object%23execution-context-scope-chain) (Good conceptual overview)
* **YouTube Video:** Search for "JavaScript Execution Context and Call Stack" for visual diagrams.

---

## #2 Research the **Call Stack** and how it manages function calls.

The **Call Stack** is the manager of the Execution Contexts.

### The Call Stack

The **Call Stack** is a vital part of the JavaScript runtime environment. It's an internal mechanism used by the JavaScript engine to keep track of function calls, manage the order of execution, and determine where the program should return to once a function completes.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A **LIFO (Last-In, First-Out)** data structure that manages the sequence of **Execution Contexts** created during the execution of a script. |
| **Why use it?** | To track the current running function, where it was called from, and which function should be executed next. It prevents the program from getting lost in nested function calls. |
| **How does it work?** | When a function is called, its Execution Context is **pushed** onto the stack. When the function returns, its context is **popped** off the stack. |

### Key Mechanics of the Call Stack

#### 1\. LIFO Principle (Last-In, First-Out) 🔄

The Call Stack follows the LIFO principle, much like a stack of plates:

* **Push:** When a function is called, its corresponding Execution Context is placed on top of the stack.
* **Pop:** When a function returns or finishes executing, its Execution Context is removed from the top of the stack.

The engine always executes the function whose context is currently at the **top** of the stack.

#### 2\. Managing Function Calls 📞

1. **Start:** The **Global Execution Context (GEC)** is the first item pushed onto the stack when the script starts.
2. **Calling a function:** A new **Function Execution Context (FEC)** is created and pushed onto the top of the stack.
3. **Nested calls:** If that function calls another function, a new FEC is pushed on top of the first one.
4. **Returning:** Once a function's code finishes, its FEC is popped off the stack, and the engine resumes execution in the context immediately below it.

#### 3\. Stack Overflow 💥

A **Stack Overflow** error occurs when the Call Stack reaches its maximum capacity. This typically happens due to:

* **Infinite Recursion:** A function that calls itself without a proper base (exit) case, causing the engine to continuously push new Execution Contexts onto the stack until it runs out of memory.

**Code Example (Stack Overflow):**

```javascript
function recurse() {
    // There is no exit condition!
    recurse(); 
}

// Calling this function will lead to:
// [ GEC, FEC:recurse, FEC:recurse, FEC:recurse, ... ] 
// until the stack is full.
// recurse(); // Uncommenting this would cause the error
```

### Code Example of Call Stack Flow

```javascript
function b() {
    console.log('Function B is running');
    // Function B finishes and is popped off the stack
}

function a() {
    console.log('Function A is running');
    b(); // B's EC is pushed onto the stack
    console.log('Function A is done');
    // Function A finishes and is popped off the stack
}

a(); // A's EC is pushed onto the stack
// GEC remains until the entire script finishes

// Call Stack Visualization:
// 1. Initial: [ GEC ]
// 2. a() called: [ GEC, a-FEC ]
// 3. b() called: [ GEC, a-FEC, b-FEC ]
// 4. b() returns/finishes: [ GEC, a-FEC ] (b-FEC popped)
// 5. a() returns/finishes: [ GEC ] (a-FEC popped)
// 6. Script ends: [ ] (GEC popped)
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **LIFO Data Structure** | Last-In, First-Out—the last function pushed onto the stack is the first one to be executed and popped off. |
| **Context Manager** | Manages the order of function Execution Contexts. |
| **Execution Flow** | The JavaScript engine always executes the function whose context is currently at the **top** of the stack. |
| **Stack Overflow** | An error caused by too many contexts being pushed onto the stack (e.g., infinite recursion), exceeding the stack's limited memory size. |

---

### 🔗 References

* **MDN Web Docs:** [Concurrency model and Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop) (The Call Stack is the first component explained here).
* **javascript.info:** [Execution context, Scope chain](https://www.google.com/search?q=https://javascript.info/function-object%23execution-context-scope-chain) (Includes a discussion of the stack).
* **YouTube Video:** Search for "JavaScript Call Stack" for visual representations of pushing and popping contexts.

---

## #3 Learn about the **Event Loop** and its role in handling asynchronous operations.

The **Event Loop** is what allows JavaScript to handle asynchronous operations (like network requests or timers) without blocking the main thread, despite being single-threaded.

### The Event Loop

The **Event Loop** is not part of the JavaScript engine itself but is a core part of the browser or Node.js runtime environment. Its primary job is to monitor the **Call Stack** and the **Task Queue** (or Callback Queue) and push tasks from the queue onto the stack when the stack is empty. This non-blocking behavior is crucial for modern web applications.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A constantly running process that checks if the Call Stack is empty and, if so, takes the next waiting **task** from the Task Queue and pushes it onto the stack for execution. |
| **Why use it?** | Because JavaScript is **single-threaded**, the Event Loop prevents **blocking**. It allows time-consuming operations (like fetching data) to be performed asynchronously, so the main thread remains free to handle user interface updates and other critical tasks. |
| **How does it work?** | It facilitates communication between the **Call Stack** (where synchronous code runs) and the **Task Queues** (where asynchronous results wait). |

---

### The Components of the Concurrency Model

Understanding the Event Loop requires knowing the three main components of the JavaScript Runtime:

1. **Call Stack:**
    * Where synchronous code is executed (one function at a time).
    * Must be **empty** for the Event Loop to proceed.
2. **Web APIs (or Node APIs):**
    * Features provided by the environment, not the JS engine itself (e.g., `setTimeout()`, `fetch()`, `DOM events`).
    * When an asynchronous function is called, it's passed to the relevant Web API for processing in the background.
3. **Task Queue (Callback Queue):**
    * Once a Web API finishes its background task (e.g., a timer expires or data is received), the corresponding **callback function** is placed here, waiting to be processed.

### The Event Loop in Action 🎬

The Event Loop executes a very simple, continuous cycle:

1. **Run Sync Code:** The JavaScript engine executes all synchronous code. Function calls are pushed onto and popped off the **Call Stack**.
2. **Delegate Async:** When an asynchronous function (like `setTimeout`) is encountered, the callback is delegated to the **Web APIs** to run in the background. The function call itself is popped off the Call Stack, and the engine moves to the next synchronous instruction.
3. **Wait in Queue:** Once the Web API finishes its job, the asynchronous **callback** is moved from the Web API environment to the **Task Queue**.
4. **The Loop Check (The Magic):** The Event Loop constantly checks:
    * **Is the Call Stack Empty?** (No synchronous code is running).
    * **Is there a waiting callback in the Task Queue?**
5. **Push to Stack:** If the Call Stack is empty, the Event Loop takes the first callback from the Task Queue and **pushes it onto the Call Stack** for execution. This cycle ensures that asynchronous code only runs *after* all synchronous code has finished.

### Code Example Illustrating the Flow

```javascript
console.log('1. Start'); // Synchronous

setTimeout(function callbackA() {
    console.log('3. setTimeout callback'); // Async Macrotask
}, 0); // Timer expires immediately, but must wait in the Task Queue

console.log('2. End'); // Synchronous

// Execution Order and Call Stack Flow:
// 1. console.log('1. Start') runs and logs '1. Start'.
// 2. setTimeout is called. Its callbackA is passed to the Web API Timer.
// 3. console.log('2. End') runs and logs '2. End'.
// 4. Call Stack is now EMPTY.
// 5. The Event Loop checks the Task Queue. It finds callbackA.
// 6. callbackA is PUSHED onto the Call Stack.
// 7. callbackA runs and logs '3. setTimeout callback'.
// 8. callbackA returns and is POPPED off the stack.
// Final Output: 1. Start, 2. End, 3. setTimeout callback (Order is guaranteed)
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Purpose** | Enables **non-blocking** (asynchronous) behavior in single-threaded JavaScript. |
| **Single Thread** | JavaScript runs on one thread; only one task can be on the Call Stack at a time. |
| **Core Rule** | A callback from the Task Queue will **never** be executed until the **Call Stack is completely empty**. |
| **Main Players** | **Call Stack** (synchronous execution), **Web APIs** (asynchronous processing), and the **Task Queue** (waiting area for callbacks). |

---

### 🔗 References

* **MDN Web Docs:** [Concurrency model and Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop) (The definitive resource).
* **javascript.info:** [Event Loop](https://javascript.info/event-loop)
* **YouTube Video:** Search for **"Philip Roberts What the heck is the event loop anyway?"** – this is the most famous and essential talk on the subject.

---

## #4 Understand the purpose and function of the **Task Queue** (or Callback Queue).

### The Task Queue (Callback Queue)

The **Task Queue** (also frequently called the **Callback Queue** or **Message Queue**) is a crucial part of the JavaScript runtime environment. Its main purpose is to hold asynchronous callback functions that are ready to be executed, ensuring they wait their turn until the synchronous code completes.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A queue (FIFO structure) that stores **asynchronous callback functions** that have finished their background processing (via Web APIs) and are ready to be pushed to the Call Stack for execution. |
| **Why use it?** | It acts as a **holding area** to prevent asynchronous callbacks from interrupting the execution of synchronous code, maintaining JavaScript's single-threaded, non-blocking nature. |
| **How does it work?** | The **Event Loop** constantly monitors the Call Stack and the Task Queue. When the Call Stack is empty, the Event Loop takes the **first** callback from the Task Queue and pushes it onto the stack. |

### Key Characteristics and Function

#### 1\. FIFO Structure (First-In, First-Out) ➡️

The Task Queue is a standard queue, meaning the callbacks are processed in the order they were added. The first callback to enter the queue is the first one to be executed once the Call Stack is clear.

#### 2\. Macrotasks vs. Microtasks (Crucial Distinction) ⚖️

While the term "Task Queue" often refers to **Macrotasks**, it's vital to know that the JavaScript concurrency model uses two queues with different priorities:

* **Macrotasks (Tasks):** Handled by the **Task Queue**. These include I/O operations, UI rendering, timers (`setTimeout`, `setInterval`), and `postMessage`.
* **Microtasks:** Handled by the **Microtask Queue** (a separate, higher-priority queue). These include Promises (`.then()`, `.catch()`, `.finally()`) and `queueMicrotask()`.

The **Microtask Queue is always processed entirely before the Event Loop moves to the Macrotask Queue (Task Queue).**

#### 3\. The Role in Non-Blocking I/O ⏳

When you initiate an asynchronous operation like an HTTP request (`fetch`) or a timer (`setTimeout`):

1. The operation is delegated to a **Web API** (or Node API) to run in the background.
2. Once the Web API completes the task, the associated callback function is moved to the **Task Queue** (or Microtask Queue, if it's a Promise).
3. The callback waits in the queue until the Call Stack is empty. This prevents the request from blocking the main thread while it's waiting for data.

### Code Example Highlighting the Queue

This example shows how `setTimeout` callbacks line up in the Task Queue, waiting for their turn.

```javascript
console.log(1); // Sync

setTimeout(() => {
    console.log(3); // Macrotask A
}, 1000); // Enters queue after 1000ms, then waits for stack

setTimeout(() => {
    console.log(4); // Macrotask B
}, 0); // Enters queue immediately (time is 0), but still waits for stack

console.log(2); // Sync

// Call Stack Sequence:
// 1. Logs 1.
// 2. setTimeout(A) is passed to Web API.
// 3. setTimeout(B) is passed to Web API.
// 4. Logs 2.
// 5. Call Stack is EMPTY.
// 6. Web API for (B) finishes, callback (4) moves to Task Queue: [ callbackB(4) ]
// 7. Event Loop pushes callback (4) to Stack. Logs 4. Stack is empty.
// 8. After 1000ms, Web API for (A) finishes, callback (3) moves to Task Queue: [ callbackA(3) ]
// 9. Event Loop pushes callback (3) to Stack. Logs 3. Stack is empty.

// Output: 1, 2, 4, 3 (The 0ms timer still runs after the sync code)
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Data Structure** | A queue (FIFO) that holds **ready-to-run** asynchronous callbacks. |
| **Mechanism** | Where Macrotask callbacks (like timers and I/O) wait after their background process is complete. |
| **Activation** | Callbacks are only executed when the **Event Loop** verifies that the **Call Stack is empty**. |
| **Priority Note** | This queue holds **Macrotasks**, which have **lower priority** than **Microtasks** (Promises), which are processed first. |

---

### 🔗 References

* **MDN Web Docs:** [Concurrency model and Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop) (Contains the definitive diagram and explanation).
* **javascript.info:** [Event Loop](https://javascript.info/event-loop) (Explains Task Queue vs. Microtask Queue).
* **Video:** The Philip Roberts video (mentioned previously) is the best visual aid for this component.

---

## #5 Compare and contrast **Microtasks vs Macrotasks** and their priority.

### Microtasks vs. Macrotasks

JavaScript's asynchronous concurrency model uses two different types of task queues to prioritize execution, ensuring that time-sensitive operations (like promise resolution) run quickly and don't get delayed by longer tasks (like timers or rendering).

### Comparison and Contrast

| Feature | Macrotasks (Tasks) | Microtasks |
| :--- | :--- | :--- |
| **Primary Examples** | **Timers** (`setTimeout`, `setInterval`), **I/O**, **UI Rendering**, `postMessage`. | **Promises** (`.then()`, `.catch()`, `.finally()`), `queueMicrotask()`, **MutationObserver**. |
| **Associated Queue** | **Task Queue** (or Callback Queue) | **Microtask Queue** (a separate, higher-priority queue) |
| **Execution Timing** | Only one Macrotask is processed per **Event Loop cycle**. UI rendering and I/O may happen between Macrotasks. | **All** Microtasks are processed in one go, *after* the current function completes and *before* the next Macrotask begins. |
| **Priority** | **Lower Priority** | **Higher Priority** |

### Priority and Event Loop Flow 🔄

The crucial difference lies in **priority**. The Event Loop strictly follows this processing order:

1. **Run Synchronous Code:** The code on the **Call Stack** executes until it's empty.
2. **Process Microtasks:** The Event Loop checks the **Microtask Queue**. If it's not empty, **ALL** microtasks are executed, one after the other, until the Microtask Queue is completely empty.
    * *(Crucially, a new microtask added during this process will also be executed before moving on).*
3. **Perform Rendering (Browser only):** The browser may choose to update the UI/render changes.
4. **Process Macrotask:** The Event Loop checks the **Task Queue** (Macrotask Queue) and takes **only one** Macrotask to push onto the Call Stack.
5. **Repeat:** The cycle starts again from Step 1.

**Key Takeaway:** The entire **Microtask Queue** is emptied *before* the Event Loop moves to the next **Macrotask**.

### Code Example Illustrating Priority

This classic example shows how a zero-delay `setTimeout` (Macrotask) is delayed by a Promise (Microtask).

```javascript
console.log(1); // Synchronous

setTimeout(() => {
    console.log(4); // Macrotask (waits in Task Queue)
}, 0);

Promise.resolve()
    .then(() => {
        console.log(3); // Microtask (waits in Microtask Queue)
    });

console.log(2); // Synchronous

// Execution Flow:
// 1. Logs 1.
// 2. setTimeout callback is sent to the Web API, then moved to the **Task Queue**.
// 3. Promise.resolve() is called. The .then() callback (3) is moved to the **Microtask Queue**.
// 4. Logs 2.
// 5. Call Stack is EMPTY.
// 6. Event Loop checks Microtasks. It finds (3), executes it, and logs 3.
// 7. Microtask Queue is EMPTY.
// 8. Event Loop checks Macrotasks. It finds (4), executes it, and logs 4.

// Guaranteed Output: 1, 2, 3, 4
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Microtasks** | High-priority tasks (e.g., Promises). The entire queue is emptied *before* proceeding to the Macrotasks or rendering. |
| **Macrotasks** | Lower-priority tasks (e.g., Timers, I/O, UI). Only **one** Macrotask is pulled from its queue per full Event Loop cycle. |
| **Order of Flow** | **Sync Code** $\rightarrow$ **Microtasks (All)** $\rightarrow$ **Render** $\rightarrow$ **Macrotask (One)** $\rightarrow$ **Repeat** |
| **Implication** | Microtasks provide a guarantee of faster execution for operations like promise handling, ensuring data consistency before the next full task or render cycle. |

---

### 🔗 References

* **MDN Web Docs:** [Microtask queue](https://www.google.com/search?q=https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_queue)
* **javascript.info:** [Microtasks and Macrotasks](https://www.google.com/search?q=https://javascript.info/microtasks-macrotasks)

---

## #6 Master the concept of **Hoisting** for variables and functions, how Hoisting works for var, let, const.

hoist = to lift or pull something up, often by using ropes, etc.

### Hoisting

**Hoisting** is JavaScript's default behavior of moving declarations to the top of the current scope (either global or function scope) during the **Creation Phase** of the Execution Context, *before* code execution begins.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A conceptual mechanism where variable and function declarations are processed and put into memory at the beginning of their containing scope. |
| **Why does it happen?** | It's a fundamental part of how the **JavaScript Execution Context** is built. The engine first sets up the memory (Creation Phase) and then runs the code (Execution Phase). |
| **How does it work?** | Only the **declaration** is hoisted, not the **assignment** (initialization). This separation between declaration and assignment is why you can use a variable before you declare it (though it's bad practice). |

---

### Hoisting for Functions vs. Variables

| Type | What is Hoisted? | Initial Value | Implication |
| :--- | :--- | :--- | :--- |
| **Function Declarations** | The **entire function** (name and body). | The function body itself. | You can call a function declaration **before** it appears in the code. |
| **`var` Variables** | Only the **declaration**. | `undefined`. | You can access a `var` variable before its line, but its value will be `undefined`. |
| **`let` / `const` Variables** | Only the **declaration**. | Uninitialized (They enter the **Temporal Dead Zone**). | Accessing them before the declaration throws a `ReferenceError`. |

---

### Hoisting: `var` (Full Hoisting of Declaration)

For `var`, the declaration is hoisted to the top of the function or global scope, but the assignment remains in place.

#### Code Example (`var`)

```javascript
console.log(car); // Output: undefined
// Why? The declaration `var car;` was hoisted, and its value defaults to 'undefined'.

var car = 'Tesla';

function driving() {
    console.log(speed); // Output: undefined (Function-scoped var is hoisted to the top of driving FEC)
    var speed = 80;
}
driving();

// What the engine conceptually sees:
/*
var car; // Hoisted
// GEC starts execution...
console.log(car); 
car = 'Tesla'; 
// ...
*/
```

---

### Hoisting: `let` and `const` (The Temporal Dead Zone)

`let` and `const` declarations are also hoisted, but they are handled differently by the Lexical Environment. They are not initialized and are said to be in the **Temporal Dead Zone (TDZ)** from the start of the scope until the actual declaration is executed.

Attempting to access a `let` or `const` variable within the TDZ results in a **`ReferenceError`**—not `undefined`.

#### Code Example (`let` and `const`)

```javascript
// console.log(bike); // ReferenceError: Cannot access 'bike' before initialization
let bike = 'Mountain';

// console.log(price); // ReferenceError: Cannot access 'price' before initialization
const price = 500;

function mechanics() {
    let part = 'Wheel';
    console.log(part); // Output: Wheel
    // console.log(tool); // ReferenceError: tool is in TDZ
    let tool = 'Wrench';
}
mechanics();

// Key point: TDZ lasts from the start of the scope until the declaration line.
```

---

### Hoisting: Function Declarations (Full Function Hoisting)

Function declarations are entirely hoisted, meaning the function name and its body are available in memory from the start of the scope.

#### Code Example (Function Declarations)

```javascript
// This works perfectly because the entire function is hoisted.
hello(); // Output: Hello from a function declaration!

function hello() {
    console.log('Hello from a function declaration!');
}

// Function expressions (which are assigned to a variable) only hoist the variable part.
// greet(); // TypeError: greet is not a function (since 'greet' is hoisted as 'undefined')
var greet = function() {
    console.log('Hello from a function expression!');
};
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Hoisting Rule** | **Declarations** (not assignments) are moved to the top of the scope before execution. |
| **`var` Hoisting** | Hoisted and initialized with **`undefined`**. Avoids a crash but leads to unpredictable behavior. |
| **`let`/`const` Hoisting** | Hoisted but **uninitialized** (in the **Temporal Dead Zone / TDZ**). Accessing them throws a `ReferenceError`, promoting safer code. |
| **Function Hoisting** | **Function declarations** are fully hoisted. **Function expressions** only hoist the variable (`var`) and are initially `undefined`. |
| **Best Practice** | Always declare variables (`let`/`const`) and functions *before* you use them to avoid relying on Hoisting, which makes code less readable and prone to error. |

---

### 🔗 References

* **MDN Web Docs:** [Hoisting - JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
* **javascript.info:** [Variable Scopes, Closure](https://www.google.com/search?q=https://javascript.info/var-let-const%23hoisting) (Specifically addresses the difference between `var`, `let`, and `const`).
* **YouTube Video:** Search for "JavaScript Hoisting explained var let const" for visual aids on TDZ.

---

## #7 Study **Scope** in detail: **Block Scope**, **Function Scope**, **Global Scope** and **Scope Chain**.

### Scope

**Scope** in JavaScript determines the accessibility of variables, functions, and objects in different parts of your code. It's the set of rules that governs where and how a variable can be looked up.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | The current context of execution, defining which variables and expressions are "visible" and accessible at a given point in the code. |
| **Why use it?** | **Security:** Variables defined inside a scope (e.g., a function) are protected from external access, preventing naming collisions and bugs. **Memory Management:** Variables can be garbage collected when they fall out of scope. |
| **How does it work?** | The JavaScript engine creates a **Lexical Environment** for each scope, establishing a hierarchical **Scope Chain** that determines lookup rules. |

---

### The Three Types of Scope

#### 1\. Global Scope 🌐

**What:** The outermost scope, accessible from anywhere in the JavaScript program. Variables declared outside of any function or block live here.

**Implication:**

* In a browser environment, the Global Scope is associated with the **`window`** object.
* Variables declared with `var` in the Global Scope become properties of the Global Object.
* It's best practice to minimize the use of Global Scope to prevent naming conflicts and memory clutter.

**Code Example:**

```javascript
// Global Scope
const globalMessage = "Hello World"; // Available everywhere

function checkGlobal() {
    console.log(globalMessage); // Accessing the global variable
}

console.log(globalMessage); // Output: Hello World
checkGlobal();              // Output: Hello World
```

---

#### 2\. Function Scope (Local Scope) 🧱

**What:** Variables declared inside a function are only accessible within that function and any nested functions inside it.

**Implication:**

* This is the traditional form of local scope in pre-ES6 JavaScript, primarily associated with the **`var`** keyword.
* Function scope provides data privacy; variables inside a function are invisible to the outside world.

**Code Example:**

```javascript
function calculateSum(a, b) {
    // Function Scope for `result` and `message`
    var result = a + b; 
    let message = "The sum is: ";
    console.log(message + result); // Accessible here
}

calculateSum(5, 10);
// console.log(result); // ReferenceError: result is not defined (outside of function scope)
```

---

#### 3\. Block Scope (ES6+) 📦

**What:** Variables declared with **`let`** and **`const`** inside a pair of curly braces (`{}`)—such as in `if` statements, `for` loops, or just standalone blocks—are confined to that block.

**Implication:**

* This solved many of the issues caused by `var` leaking out of blocks (like `for` loops).
* `let` and `const` enable true block-level scoping, making code cleaner and more predictable.

**Code Example:**

```javascript
if (true) {
    // Block Scope for `x` and `y`
    let x = 10; 
    const y = 20; 
    var z = 30; // 'var' is NOT block-scoped! It leaks out.
    console.log(x); // Output: 10
}

// console.log(x); // ReferenceError: x is not defined (outside of block scope)

console.log(z); // Output: 30 (DANGER: var ignores block scope)
```

### The Scope Chain (The Lookup Mechanism)

The **Scope Chain** is the hierarchy of scopes that the JavaScript engine uses to look up variables.

* When the engine needs to resolve a variable, it starts by checking the **current scope**.
* If the variable is not found there, it moves up to the **immediate parent scope**.
* This process continues layer by layer until it reaches the **Global Scope**.
* If the variable is still not found in the Global Scope, a **`ReferenceError`** is thrown (unless running in non-strict mode where it might implicitly create a global variable).

**Code Example (Scope Chain):**

```javascript
const app = 'MyApp'; // Global Scope

function outerFunction() {
    const outerVar = 'Outer'; // Scope 1: Function Scope of outerFunction

    function innerFunction() {
        const innerVar = 'Inner'; // Scope 2: Function Scope of innerFunction
        
        // Lookup `app`: Checks Scope 2 -> Checks Scope 1 -> Finds in Global Scope
        console.log(app); 
        
        // Lookup `outerVar`: Checks Scope 2 -> Finds in Scope 1
        console.log(outerVar);
        
        // Lookup `innerVar`: Finds in Scope 2
        console.log(innerVar);
    }
    innerFunction();
}
outerFunction();
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Scope Definition** | The region of the code where a variable is accessible. |
| **`var`** | Is **Function-scoped** or **Global-scoped**. **Ignores** Block scope (`if`, `for` loops), leading to leakage. |
| **`let` and `const`** | Are **Block-scoped**. Recommended for most variable declarations as they contain variables safely within the nearest `{}` block. |
| **Scope Chain** | The hierarchical path the JS engine follows to look up a variable, moving outward from the current scope to the Global Scope. |

---

### 🔗 References

* **MDN Web Docs:** [Scope - JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/Scope)
* **javascript.info:** [Variable Scope, Closure](https://www.google.com/search?q=https://javascript.info/var-let-const%23scope-summary)
* **YouTube Video:** Search for "JavaScript Scope and Scope Chain" for visual diagrams.

---

## #8 Practice and understand **Closures** and their utility.

### Closures

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

## #10 Learn the different rules for **`this` binding** (default, implicit, explicit, new).

### `this` Binding Rules

The value of the **`this`** keyword in JavaScript is **dynamic**; it's determined not by where a function is *declared*, but by *how* it is **called** (its invocation context).

| Rule | Invocation Pattern | How `this` is Set | What `this` Refers To |
| :--- | :--- | :--- | :--- |
| **1. Default** | Standalone function call (`myFunc()`) | Falls back to the global object. | The **Global Object** (`window` in a browser, `global` in Node.js). **`undefined`** in Strict Mode. |
| **2. Implicit** | Function called as a method (`obj.method()`) | The object that "owns" the method at the call site. | The **Object** preceding the dot (`.`). |
| **3. Explicit** | Using `call()`, `apply()`, or `bind()` | Explicitly set by the developer. | The **Object** passed as the first argument. |
| **4. `new`** | Function called with `new` (`new Constructor()`) | A new object is created and set as `this`. | The **Newly Created Object** instance. |

---

### Rule 1: Default Binding (The Global Fallback)

**What:** This is the fallback rule when none of the other rules apply. The function is called directly without any object context.

**How:** In non-strict mode, JavaScript defaults `this` to the **Global Object** (e.g., `window`).

**Why:** To ensure `this` always points to *something* useful, even in the absence of an explicit owner. However, this is a common source of bugs and is mitigated by Strict Mode.

#### Code Example

```javascript
'use strict'; // Activate Strict Mode for better behavior

function showThis() {
    // In Strict Mode, 'this' is 'undefined' here.
    // Without 'use strict', 'this' would be the Global Object (window).
    console.log(`showThis(): this is`, this);
}

showThis(); 
// Output: showThis(): this is undefined 
// (or the global object if 'use strict' is removed)

const obj = { method: showThis };
obj.method();
// Output: showThis(): this is { method: [Function: showThis] } (Implicit Binding takes priority)
```

---

### Rule 2: Implicit Binding (The Object Owner)

**What:** When a function is called as a property (a method) of an object.

**How:** The object immediately to the left of the final dot (`.`) in the method call becomes the value of `this`.

**Why:** This allows methods to access and modify the data (properties) of the specific object they belong to.

#### Code Example

```javascript
const user = {
    name: 'Alice',
    greet: function() {
        console.log(`greet(): Hello, my name is ${this.name}`);
    }
};

const dog = {
    name: 'Buddy',
    bark: user.greet // Borrowing the function
};

user.greet(); // Call site: user.greet()
// Output: greet(): Hello, my name is Alice (this is 'user')

dog.bark(); // Call site: dog.bark()
// Output: greet(): Hello, my name is Buddy (this is 'dog')
```

---

### Rule 3: Explicit Binding (The Forced Context)

**What:** Manually forcing the value of `this` using one of the function prototype methods: **`call()`**, **`apply()`**, or **`bind()`**.

**How:** The first argument passed to these methods becomes the explicit value for `this`.

* **`call(thisArg, arg1, arg2, ...)`:** Executes the function immediately with the specified `this` and arguments passed individually.
* **`apply(thisArg, [argsArray])`:** Executes the function immediately with the specified `this` and arguments passed as an array.
* **`bind(thisArg)`:** Returns a *new function* permanently bound to the specified `this`. It does *not* execute the original function immediately.

**Why:** Useful for function borrowing, functional programming patterns, and ensuring callbacks have the correct `this` context.

#### Code Example

```javascript
function introduce(job, city) {
    console.log(`${this.name} is a ${job} from ${city}.`);
}

const person = { name: 'Bob' };
const another = { name: 'Charlie' };

// Using call() - immediate execution, arguments passed individually
introduce.call(person, 'Developer', 'London');
// Output: Bob is a Developer from London. (this is 'person')

// Using apply() - immediate execution, arguments passed as an array
introduce.apply(another, ['Designer', 'Paris']);
// Output: Charlie is a Designer from Paris. (this is 'another')

// Using bind() - creates a new, bound function
const boundIntroduce = introduce.bind(person, 'Manager', 'New York');
boundIntroduce(); // Execute the bound function later
// Output: Bob is a Manager from New York. (this is 'person' permanently)
```

---

### Rule 4: `new` Binding (The Constructor)

**What:** When a function is invoked with the `new` operator, transforming it into a constructor.

**How:** The `new` keyword executes a specific sequence of steps:

1. It creates a brand new, empty object.
2. It sets the prototype of the new object.
3. It sets **the newly created object** as the `this` context for the function call.
4. It returns the new object (unless the function returns a non-primitive value).

**Why:** This is the traditional way to create instances of a function's "class" in pre-ES6 JavaScript.

#### Code Example

```javascript
function Car(make, model) {
    // 1. 'this' is a new, empty object {}
    this.make = make; // 2. 'this' is used to set properties on the new object
    this.model = model;
    this.year = 2024;
    // 3. The new object is implicitly returned
}

const civic = new Car('Honda', 'Civic');
console.log(`new Binding:`, civic);
// Output: new Binding: Car { make: 'Honda', model: 'Civic', year: 2024 } 
// (this inside Car() was the new 'civic' object)
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **`this` is Dynamic** | The value of `this` is determined at the **call site** (how the function is executed), not the definition site. |
| **Order of Precedence** | The binding rules have a precedence: **`new`** \> **Explicit** \> **Implicit** \> **Default**. |
| **Strict Mode Effect** | In **Strict Mode (`'use strict'`)**, the Default Binding rule makes `this` be `undefined`, which helps prevent accidental access/modification of the Global Object. |
| **Arrow Functions** | **Arrow functions** ignore all four rules and instead adopt the `this` value from their immediately surrounding **lexical scope** (where they are defined). |

---

### 🔗 References

* **MDN Web Docs:** [this - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
* **javascript.info:** [The "new Function()" syntax](https://javascript.info/new-function) (Covers `new` binding and constructors)
* **YouTube Video:** Searching for "JavaScript this binding explained" will yield many great visual explanations of the call site and rules. Look for videos that specifically mention the four rules (Default, Implicit, Explicit, New).

---

## #11 Understand the features and implications of using **Strict Mode** (`'use strict'`).

Strict Mode = Modern Mode
Strict Mode X Sloppy Mode (this is not an official term)

**Strict Mode** is key for writing safer and more reliable JavaScript.

### Strict Mode (`'use strict'`)

**Strict Mode** is a way to opt-in to a restricted version of JavaScript. It was introduced in ECMAScript 5 (ES5) to eliminate silent errors, fix mistakes that make it difficult for JavaScript engines to perform optimizations, and prohibit some syntax likely to be defined in future ES versions.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A directive (`'use strict';`) that enables a safer, stricter subset of JavaScript by changing how code is executed. |
| **Why use it?** | **1. Catches common coding mistakes** as exceptions (e.g., trying to use an undeclared variable) instead of failing silently. **2. Fixes mistakes** that made JavaScript language difficult to optimize. **3. Prohibits** confusing and poorly designed "bad parts" of the language. |
| **How to use it?** | By placing the string literal `'use strict';` at the very beginning of a script or the beginning of a function body. |

### Key Features and Implications (What it Changes)

Strict Mode primarily works by transforming bad practices into explicit errors.

#### 1\. Eliminates Global Variable Leaks 🚫

In non-strict mode, assigning to an undeclared variable implicitly creates a global variable. Strict Mode makes this an error.

**Code Example:**

```javascript
// Non-Strict Mode (without 'use strict'):
function messyFunction() {
    message = "Hello"; // 'message' becomes a global variable
}
messyFunction();
console.log(message); // Output: Hello (No error)

// Strict Mode:
function cleanFunction() {
    'use strict';
    try {
        errorMsg = "Oops"; // ReferenceError: errorMsg is not defined
    } catch (e) {
        console.log("Strict Mode Error:", e.name);
    }
}
cleanFunction();
// Output: Strict Mode Error: ReferenceError 
```

#### 2\. Disables Default `this` Binding 🎯

As you learned earlier, in non-strict mode, when a function is called without an explicit owner (`simpleFunction()`), `this` defaults to the Global Object (`window`/`global`).

In Strict Mode, **`this` remains `undefined`** in such standalone function calls, preventing accidental modifications to the global scope.

**Code Example:**

```javascript
'use strict';

function logThis() {
    console.log('logThis(): this is', this);
}

logThis(); // Default Binding in Strict Mode
// Output: logThis(): this is undefined 
```

#### 3\. Prevents Deleting Variables, Functions, and Arguments 🗑️

Trying to delete simple variable names, functions, or function arguments will throw a `SyntaxError` or `TypeError`.

**Code Example:**

```javascript
'use strict';
let x = 10;

// This will throw a TypeError in strict mode
try {
    delete x;
} catch (e) {
    console.log("Strict Mode Error:", e.name, "— cannot delete variable.");
}
// Output: Strict Mode Error: TypeError — cannot delete variable.
```

#### 4\. Prohibits Duplicate Parameter Names 👯

Strict Mode makes it a `SyntaxError` to have two or more parameters with the same name in a function declaration.

**Code Example:**

```javascript
// This throws a SyntaxError in strict mode
/*
function sum(a, b, a) { // SyntaxError
    'use strict'; 
    // ...
}
*/
```

#### 5\. Restricts `eval` Usage 🤫

Strict Mode prevents variables declared inside an `eval()` call from creating new variables in the surrounding scope. This improves security and predictability.

---

### How to Apply Strict Mode

You can apply Strict Mode globally or locally:

1. **Entire Script (Global Strict Mode):** Put `'use strict';` at the very top of your JavaScript file.

    ```javascript
    'use strict';

    // All code in this file is now executed in strict mode.
    function foo() { /* ... */ }
    // ...
    ```

2. **Function Level (Local Strict Mode):** Put `'use strict';` at the top of a specific function body. This is useful for migrating code gradually.

    ```javascript
    // This code is not in strict mode
    function safeFunction() {
        'use strict';
        // Only the code inside this function is in strict mode.
    }
    // This code is not in strict mode
    ```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Directive** | Enabled by the string literal `'use strict';` at the start of a script or function. |
| **Safety First** | It turns many confusing, silent failures into explicit **runtime errors** (e.g., undeclared variables). |
| **Global `this`** | Prevents `this` from defaulting to the Global Object in function calls, making it **`undefined`** instead. |
| **Best Practice** | It's highly recommended to write all new JavaScript code in Strict Mode for better code quality, debuggability, and performance. |

---

### 🔗 References

* **MDN Web Docs:** [Strict mode - JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)
* **javascript.info:** [Strict Mode](https://javascript.info/strict-mode)

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

## #13 Learn the differences and proper use of **`let`**, **`const`** and **`var`**.

### `var`, `let`, and `const`

The introduction of `let` and `const` in ES6 (ECMAScript 2015) addressed many of the common issues and ambiguities associated with `var`, fundamentally changing how variables are scoped and used in JavaScript.

| Feature | `var` (Legacy) | `let` (Modern) | `const` (Modern) |
| :--- | :--- | :--- | :--- |
| **Scope** | **Function-scoped** or Global-scoped. Ignores block scope (`if`, `for` loops). | **Block-scoped**. Confined to the nearest pair of curly braces (`{}`). | **Block-scoped**. Confined to the nearest pair of curly braces (`{}`). |
| **Hoisting** | Hoisted to the top of its scope and initialized with **`undefined`**. | Hoisted, but remains **uninitialized** (in the **Temporal Dead Zone / TDZ**) until the declaration is reached. | Hoisted, but remains **uninitialized** (in the **Temporal Dead Zone / TDZ**) until the declaration is reached. |
| **Re-declaration** | **Allowed** in the same scope without error. | **Not Allowed** in the same scope. Throws a `SyntaxError`. | **Not Allowed** in the same scope. Throws a `SyntaxError`. |
| **Re-assignment** | **Allowed** (can change the value later). | **Allowed** (can change the value later). | **Not Allowed** (must be initialized on declaration and cannot change the value). |

---

### Key Differences and Implications

#### 1\. Scoping Rule: Function vs. Block

| Keyword | Example | Implication |
| :--- | :--- | :--- |
| **`var`** | ` javascript for (var i = 0; i < 1; i++) {} console.log(i); // Output: 1 (leaks outside loop)  ` | `var` is **function-scoped**. The loop body is a block, but `i` leaks out and is accessible globally or throughout the function. |
| **`let`/`const`** | ` javascript for (let j = 0; j < 1; j++) {} console.log(j); // ReferenceError: j is not defined (contained)  ` | `let` and `const` are **block-scoped**. The variable `j` is confined to the loop block, preventing accidental access or collisions. |

#### 2\. Hoisting and the Temporal Dead Zone (TDZ)

| Keyword | Example | Implication |
| :--- | :--- | :--- |
| **`var`** | ` javascript console.log(a); // undefined var a = 10;  ` | Since `var a` is hoisted and initialized to `undefined`, the code executes without crashing, but its output is often unexpected. |
| **`let`/`const`** | ` javascript console.log(b); // ReferenceError: b is not defined let b = 10;  ` | `let/const` is hoisted but uninitialized. Attempting to access it results in a **ReferenceError**, which is a better signal to the developer that they used the variable before declaring it. This period of inaccessibility is the **TDZ**. |

#### 3\. Mutability (`let` vs. `const`)

| Keyword | Example | Implication |
| :--- | :--- | :--- |
| **`let`** | ` javascript let name = 'Bob'; name = 'Alice'; // Allowed  ` | Use `let` for variables whose value is expected to change (e.g., counters in a loop, re-assigned variables). |
| **`const`** | ` javascript const PI = 3.14; PI = 3.141; // TypeError: Assignment to constant variable.  ` | Use `const` for variables whose value should **never** change. This does **not** mean the value is immutable, only that the variable **binding** cannot be re-assigned. |

**Important `const` Note:**
For non-primitive values (objects or arrays), `const` prevents re-assignment of the variable itself, but it **does not** prevent mutation of the object/array contents.

```javascript
const user = { name: 'Bob' };
user.name = 'Alice'; // ALLOWED (mutating the object content)
// user = { name: 'Charlie' }; // NOT ALLOWED (re-assigning the variable)
```

---

### Best Practices (The Modern Standard) 🌟

1. **Avoid `var`:** In all new code, completely discontinue the use of `var`. It is legacy and causes scope confusion, making code harder to maintain and debug.
2. **Prioritize `const`:** Start with `const` for every variable declaration. If you find that you must re-assign the variable later, and only then, switch to `let`. This makes your code more declarative and minimizes unexpected changes.
3. **Use `let` Sparingly:** Reserve `let` for things like loop counters, or values that genuinely need to change over the life cycle of a function (e.g., a total score).
4. **Block Scope Awareness:** Utilize the block scope of `let` and `const` to keep variables contained locally, improving code isolation and preventing accidental global pollution.

---

### 🔗 References

* **MDN Web Docs:** [`var` - JavaScript](https://www.google.com/search?q=%5Bhttps://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var%5D\(https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var\)), [`let` - JavaScript](https://www.google.com/search?q=%5Bhttps://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let%5D\(https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let\)), [`const` - JavaScript](https://www.google.com/search?q=%5Bhttps://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const%5D\(https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const\))
* **javascript.info:** [var, let, const difference](https://www.google.com/search?q=https://javascript.info/var-let-const)

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

---

## #15 Practice using **template literals** for string interpolation.

### Template Literals

**Template literals** (or template strings) are a modern way to create strings in JavaScript, offering enhanced features like **string interpolation** and **multi-line strings**. They are defined using the **backtick** (`` ` ``) character, instead of single (``'``) or double (``"``) quotes.

### What, Why, and How

| Aspect | Details |
| :--- | :--- |
| **What is it?** | A string literal delimited by **backticks** (`` ` `` `` ` ``) that allows for embedded expressions and multi-line text. |
| **Why use it?** | **Readability:** Makes dynamic strings with variables much cleaner. **Functionality:** Easily supports multi-line strings and advanced features like tagged templates. |
| **How to use it?** | Use **backticks** (`` ` `` `` ` ``) to enclose the string, and embed JavaScript expressions inside a **placeholder** using the dollar sign and curly braces: $\texttt{\$\{expression\}}$ |

---

### 1\. String Interpolation (The Main Use Case) 💉

**String interpolation** is the process of embedding variables or expressions directly within a string without breaking the string or using the concatenation operator ($\texttt{+}$).

#### Code Example (Comparison)

| Traditional Concatenation (Pre-ES6) | Template Literal Interpolation (ES6+) |
| :--- | :--- |
| ` javascript const name = 'Bob'; const age = 30; console.log("Hello, my name is " + name + " and I am " + age + " years old.");  ` | `` javascript const name = 'Bob'; const age = 30; console.log(`Hello, my name is ${name} and I am ${age} years old.`);  `` |

#### Embedding Expressions

The placeholder $\texttt{\$\{}$ $\texttt{\}}$ can contain **any valid JavaScript expression**, not just simple variables.

```javascript
const price = 10;
const quantity = 3;

// Embedding an arithmetic expression
const message = `Total: $${price * quantity}`;
console.log(message); // Output: Total: $30

// Embedding a function call or ternary operator
const age = 25;
const isAdult = age >= 18 ? 'an adult' : 'a minor';
console.log(`The user is ${isAdult}.`); 
```

### 2\. Multi-line Strings 📝

Template literals automatically respect line breaks inserted into the code, making it easy to create multi-line strings without using the escape sequence `\n`.

#### Code Example

| Traditional String (Requires `\n`) | Template Literal (Natural Line Breaks) |
| :--- | :--- |
| ` javascript const text = "Line 1\nLine 2\nLine 3"; console.log(text);  ` | `` javascript const text = `Line 1 Line 2 Line 3`; console.log(text);  `` |

### 3\. Nesting Template Literals 🥚

Template literals can be nested inside their own expression placeholders, useful for generating dynamic HTML or complex strings.

```javascript
const items = ['Apple', 'Banana'];
const html = `<ul> ${items.map(item => `<li>${item}</li>`).join('')} </ul>`;
console.log(html); 
// Output: <ul> <li>Apple</li><li>Banana</li> </ul>
```

---

### 💡 Summary / Key Takeaways

| Key Concept | Description |
| :--- | :--- |
| **Delimiter** | Uses the **backtick** (`` ` ``) character. |
| **Interpolation** | Variables and expressions are embedded using the **placeholder** $\texttt{\$\{expression\}}$ |
| **Multi-line** | Supports multi-line strings without needing the `\n` escape sequence. |
| **Best Practice** | Use template literals as the default way to construct strings in modern JavaScript. |

---

### 🔗 References

* **MDN Web Docs:** [Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
* **javascript.info:** [Template literals](https://www.google.com/search?q=https://javascript.info/string-methods%23backticks)

---

- [ ] Master **destructuring** for arrays and objects.
- [ ] Understand and use the **spread/rest** operators.
- [ ] Learn to use **Modules** with the **`import`** and **`export`** syntax.
- [ ] Differentiate between **default vs named exports**.
