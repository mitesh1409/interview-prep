# Node.js Interview Questions

## Index

1. [What is Node.js?](./Q-1771583941727.md)
2. [Node.js Vs JavaScript](./Q-1771583999992.md)
3. [How Node.js is a Runtime Environment on the server side? What is V8?](./Q-1771584053509.md)
4. [What is the difference between Runtime Environment and Framework?](./Q-1771584156768.md)
5. [What is the difference between Node.js and Express.js?](./Q-1771584207564.md)
6. [What are the differences between Client-Side (Browser) and Server-Side (Node.js)?](./Q-1771584271092.md)
7. [What are the 7 main features of Node.js?](./Q-1771584340553.md)
8. [Explain Node.js Architecture](./Q-1771584411283.md)
9. [package-lock.json and Versioning NPM Packages](./Q-1771584471031.md)
10. [What is Single Threaded programming?](./Q-1771584523354.md)
11. [What is Synchronous programming?](./Q-1771584592765.md)
12. [What is Multi Threaded programming?](./Q-1771584643035.md)
13. [What is Asynchronous programming? Synchronous vs Asynchronous](./Q-1771584688658.md)
14. [What are Event, Event Handler/Listener, Event Emitter, Event Queue, Event Loop, Event Driven?](./Q-1771584726038.md)
15. [Explain Event Loop in Node.js. How it is different from the Browser's Event Loop?](./Q-1778580799378.md)
16. [What are the main features and advantages of Node.js?](./Q-1771584796045.md)
17. [What are the disadvantages of Node.js? When to use and when not to use Node.js?](./Q-1771584849733.md)
18. [What is npm? What is the purpose of package.json, package-lock.json files & node_modules folder?](./Q-1771584972644.md)
19. [What are Modules in Node.js? Module vs Function](./Q-1771585020564.md)
20. [What is module wrapper function?](./Q-1771585085806.md)
21. [What are different types of modules in Node.js?](./Q-1771585141962.md)
22. [Top 5 built-in modules in Node.js](./Q-1771585192911.md)
23. [Function vs Event](./Q-1771585285562.md)
24. [Order of Execution of a Node.js Application](./Q-1771585333788.md)
25. [Explain `process.nextTick()` in Node.js OR What is the difference between `process.nextTick()`, `setImmediate()` and `setTimeout()`?](./Q-1771585379085.md)
26. [CommonJS Module vs ECMAScript Module in Node.js](./Q-1771585438788.md)
27. [What happens when you call `require()` multiple times for the same module OR import the same module multiple times?](./Q-1778573784076.md)
28. [How does Node.js handles asynchronous operations? OR Callbacks, Promises, async-await in Node.js](./Q-1771585486291.md)
29. [How do you handle errors in asynchronous code? Provide examples.](./Q-1771601217168.md)
30. [Coding Question](./Q-1771601261333.md)
31. [How would you handle multiple asynchronous calls efficiently? OR Promises parallel, sequence & race execution](./Q-1771601292399.md)
32. [Simple HTTP Server using Node.js Only](./Q-1771601388409.md)
33. [Promises in Node.js](./Q-1771601441195.md)
34. [What happens if an unhandled promise rejection occurs? OR The purpose of using global error handlers like `process.on('unhandledRejection')`](./Q-1771601666435.md)
35. [What happens if an uncaught exception occurs? OR The purpose of using global error handlers like `process.on('uncaughtException')`](./Q-1772174646207.md)
36. [Middleware in Express.js](./Q-1772178692962.md)
37. [What are Streams in Node.js?](./Q-1772178705973.md)
38. [How can you secure a Node.js application?](./Q-1777296324889.md)
39. [What is Buffer in Node.js?](./Q-1777296381727.md)
40. [How do you handle large file uploads efficiently in Node.js?](./Q-1777296422940.md)
41. [Scenario: You noticed a sudden increase in latency for your Node.js application hosted on AWS. How would you investigate and resolve this issue?](./Q-1777296475100.md)
42. [How would you implement caching in a Node.js application to improve performance?](./Q-1777296517761.md)
43. [How would you manage environment variables in a Node.js application?](./Q-1777296618988.md)
44. [How would you handle rate limiting in a Node.js API?](./Q-1777296664809.md)
45. [What are worker threads in Node.js, and when would you use them?](./Q-1777296713103.md)
46. [What is clustering in Node.js and when should it be used?](./Q-1777296825038.md)
47. [What is the difference between cluster and worker_threads in Node.js?](./Q-1778571871700.md)
48. [What are child processes? Difference between clusters and child processes.](./Q-1778586424379.md)
49. [How does Node.js handle memory management and garbage collection?](./Q-1777296760934.md)
50. [What is the Microservices Architecture, and how can we use Node.js to build it?](./Q-1777296872515.md)
51. [What are the best practices for optimizing the performance of a Node.js application?](./Q-1777296920432.md)
52. [How do you prevent memory leaks in a Node.js application?](./Q-1777296961824.md)
53. [How would you implement logging in a Node.js application?](./Q-1777296985334.md)
54. [SIGINT, SIGTERM, SIGKILL - What are these signals?](./Q-1774187357682.md)
55. [What will be the output of the following code](./Q-1777904455906.md)
56. [Authentication in Node.js](./Q-1778480759090.md)
57. [How do you design an observability stack in AWS for a large microservices system](./Q-1778480798716.md)
58. [What are commonly used libraries in Node.js?](./Q-1778584189929.md)
59. [Why `await` inside the loop is a bad idea? Is there any other way we can write/execute multiple `await` statements?](./Q-1778588482249.md)
60. [What happens if you call `res.end()` twice in an HTTP server?](./Q-1778590525235.md)
61. [`import.meta.url`](./Q-1782751207664.md)

---

@todo

In case when we have multiple modules grouped together into one big module,  
for example - lets say we have a "finance" module that contains the following modules:  
* loan
* insurance
* savings
* investments
* tax
etc.

How do we organise it?  
How to import/export "finance" module?  
Can we use index.js in such scenario? or is there a way without using index.js?  

---

@todo

https://github.com/mitesh1409/nodejs-master/blob/main/ZTM%20Complete%20Node.js%20Developer%20in%202023/1784541960477.md

---

@todo

https://github.com/mitesh1409/nodejs-master/blob/main/ZTM%20Complete%20Node.js%20Developer%20in%202023/1784549020530.md

---

@todo

* Explore -> Promises/async-await in browsers vs Promises/async-await in Node.js

For Q #24 [Order of Execution of a Node.js Application](./Q-1771585333788.md)

---

Proxy  
Reverse Proxy  

---

5 Coding Interview Tips From A Senior Engineer
https://www.youtube.com/watch?v=oKQcDjxsOvg

---

Solve 30 Backend Questions with me in 2 hrs | Master Backend Interviews
https://www.youtube.com/watch?v=fuFzVlhtybo

---

Backlog  

```javascript
import { createRequire } from 'module';

const require = createRequire(import.meta.url);

// And then we can use require function to import modules which don't support ESM.

```

* What is PM2 and how does it help in managing Node.js applications in production?
[PM2 - Advanced Production Process Manager for Node.js](https://pm2.keymetrics.io/)

---

Node.js - Top 100 Interview Questions and Answers  
https://www.youtube.com/watch?v=Nz-nPR5YJbw  

---

## Basic

## Intermediate

## Advanced
