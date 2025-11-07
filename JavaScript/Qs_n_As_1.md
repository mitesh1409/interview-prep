# JavaScript Interview Questions & Answers

## Source - [Sheryians Coding School | 50 JavaScript Interview Questions Solved in 1 Hour](https://www.youtube.com/watch?v=qTszFuibDEg)

---

## Question-1: Log your name and favorite hobby to the console.

Answer

```JavaScript
console.log('Mitesh', 'I love to build things!');
```

---

## Question-2: Perform and log the result of 45 * 2 - 10.

Answer

```JavaScript
console.log(45 * 2 - 10);
```

---

## Question-3.1: Use console.log to display the current year.

Answer

```JavaScript
console.log(new Date().getFullYear());
```

## Question-3.2: Use console.log to display the current date and time.

Answer

```JavaScript
console.log(new Date());
```

## Question-3.3: Use console.log to display the current date and time in US timezone.

Answer

```JavaScript
console.log(new Date().toLocaleString('en-US', { timeZone: 'America/New_York' }));
```

## Question-3.4: Use console.log to display the current date and time in UK timezone.

Answer

```JavaScript
console.log(new Date().toLocaleString('en-GB', { timeZone: 'Europe/London' }));
```

---

## Question-4: Create two variables - firstName and lastName, concatenate them, and log the full name.

Answer

```JavaScript
const firstName = 'Mitesh';
const lastName = 'Prajapati';

console.log(`${firstName} ${lastName}`);
```

---

## Question-5: Track the value of a variable - log its value, change it, and log it again.

Answer

```JavaScript
// Initial balance.
let money = 1000;
console.log('money', money);

// Earned 500.
money += 500;
console.log('money', money);

// console.log('salary', salary); // Uncaught ReferenceError: Cannot access 'salary' before initialization
// let salary;
// salary = 50000;
// console.log('salary', salary);
```

---

## Question-6: Use console.error to log an error message.

Answer

```JavaScript
console.error('This is an error message!');
```

---

## Question-7: Log the square of the number 12 to the console.

Answer

```JavaScript
const number = 12;
console.log(`The square of ${number} is ${number ** 2}`); // Exponentiation operator **
console.log(`The square of ${number} is ${number * number}`);
console.log(`The square of ${number} is ${Math.pow(number, 2)}`);
```

---

## Question-8: Log the type of the variable holding the value true.

Answer

```JavaScript
const isTrue = true;
console.log(`The type of isTrue is ${typeof isTrue}`);
```

---

## Question-9: Create a variable holding your age and log whether you are an adult or not (age > 18 means you are an adult).

Answer

```JavaScript
const age = 39;

if (age > 18) {
  console.log('You are an adult.');
} else {
  console.log('You are not an adult.');
}
```

---

## Question-10: Log the result of 100 / 0 and observe the output.

Answer

```JavaScript
console.log(100 / 0); // Infinity
console.log(1000);
console.log('typeof (100 / 0)', typeof (100 / 0)); // number
console.log('typeof 1000', typeof 1000); // number
```

---

## Question-11: Declare a variable using let and log its value.

Answer

```JavaScript
let nickName;
console.log('nickName', nickName);
```

---

## Question-12: Create a constant to store the value of pi and log it.

Answer

```JavaScript
const PI = Math.PI;
console.log('PI', PI);
console.log(typeof PI); // number
```

---

## Question-13: Reassign a value to a variable declared with let and log the new value.

Answer

```JavaScript
let username = 'mitesh_prajapati';
console.log('username', username);

username = 'john_wick';
console.log('username', username);
```

---

## Question-14: Check the type of null, undefined, NaN and log it.

Answer

```JavaScript
console.log('Type of null:', typeof null); // Type of null: object

console.log('Type of undefined:', typeof undefined); // Type of undefined: undefined

console.log('Type of NaN:', typeof NaN); // Type of NaN: number
```

---

## Question-15: Create a variable with a number as a string and log its type.

Answer

```JavaScript
const fifty = '50';
console.log('Type of fifty:', typeof fifty); // Type of fifty: string
```

---

## Question-16: Use typeof to check the type of a boolean variable and log it.

Answer

```JavaScript
let isWeekend = false;
console.log('Type of isWeekend:', typeof isWeekend); // Type of isWeekend: boolean
```

---

## Question-17: Create three variables of types string, number, and boolean, and log their values and types.

Answer

```JavaScript
let songTitle = 'Shape of You';
const ADULT_AGE = 18;
let isWeekday = true;

console.log('songTitle:', songTitle, 'Type:', typeof songTitle);
console.log('ADULT_AGE:', ADULT_AGE, 'Type:', typeof ADULT_AGE);
console.log('isWeekday:', isWeekday, 'Type:', typeof isWeekday);
```

---

## Question-18: Declare a variable without initializing it and log its type.

Answer

```JavaScript
let uninitializedVar;
console.log('uninitializedVar:', uninitializedVar, 'Type:', typeof uninitializedVar);
```

---

## Question-19: NaN === NaN, what is the result? Log it.

Answer

```JavaScript
console.log('NaN === NaN:', NaN === NaN); // false, because NaN is not equal to anything, including itself
```

---

## Question-20: Create a variable with undefined value and log its type.

Answer

```JavaScript
let undefinedVar = undefined;
console.log('undefinedVar:', undefinedVar, 'Type:', typeof undefinedVar);
```

---

## Question-21: Use const to create an array. Try reassigning the array and observe the behavior.

Answer

```JavaScript
const fruits = ['apple', 'banana', 'cherry'];
console.log('fruits:', fruits);

// fruits = ['orange', 'grape']; // Uncaught TypeError: Assignment to constant variable.
// console.log('fruits:', fruits);

fruits.push('orange'); // This is allowed, as we are modifying the contents of the array, not reassigning it.
console.log('fruits after push:', fruits);

fruits.pop(); // This is also allowed, as we are modifying the contents of the array.
console.log('fruits after pop:', fruits);
```

---

## Question-22: Write a for loop to print numbers from 1 to 50.

Answer

```JavaScript
for (let i = 1; i <= 50; i++) {
  console.log(i);
}
```

---

## Question-23: Use a while loop to sum numbers from 1 to 100 and log the result.

Answer

```JavaScript
let sum = 0;
let j = 1;
while (j <= 100) {
  sum += j;
  j++;
}
console.log('Sum from 1 to 100:', sum);
```

---

## Question-24: Create a for...of loop to log each character of a string.

Answer

```JavaScript
const str = "JavaScript is Awesome!";
for (const char of str) {
  console.log(char);
}
```

---

## Question-25: Write a for loop that skips even numbers and logs only odd numbers from 1 to 20.

Answer

```JavaScript
for (let num = 1; num <= 20; num++) {
  if (num % 2 !== 0) {
    console.log(num); // Logs odd numbers only
  }
}
```

---

## Question-26: Use a do...while loop to log numbers from 10 to 1 in reverse order.

Answer

```JavaScript
let number = 10;
do {
  console.log(number);
  number--;
} while (number >= 1);
```

---

## Question-27: Create a for loop to calculate the factorial of a number (e.g., 5) and log the result.

Answer

```JavaScript
// const num = 5;
// let factorial = 1;
// for (let i = 2; i <= num; i++) {
//   factorial *= i;
// }
// console.log(`Factorial of ${num} is ${factorial}`);

function factorial(n) {
  if (n === 0 || n === 1) {
    return 1;
  }

  let result = 1;
  for (let i = 2; i <= n; i++) {
    result *= i;
  }
  return result;
}

const num = 5;
console.log(`Factorial of ${num} is ${factorial(num)}`);
```

---

## Question-28: Write a nested loop to print a 3 X 3 grid of numbers in a matrix format.

Answer

```JavaScript
// let aNumber = 1;
// for (let row = 1; row <= 3; row++) {
//   for (let col = 1; col <= 3; col++) {
//     console.log('row:', row, 'col:', col, 'number:', aNumber);
//     aNumber++;
//   }
// }

// for (let row = 1; row <= 3; row++) {
//   let rowOutput = '';
//   for (let col = 1; col <= 3; col++) {
//     rowOutput += `${row * col} `;
//   }
//   console.log(rowOutput.trim());
// }

// This code prints a 3x3 grid of numbers starting from 1 to 9 in a matrix format.
let aNumber = 1;
for (let row = 1; row <= 3; row++) {
  let $rowOutput = '';
  for (let col = 1; col <= 3; col++) {
    $rowOutput += `${aNumber} `;
    aNumber++;
  }
  console.log($rowOutput);
}
```

---

## Question-29: Write a for loop to reverse an array of numbers and log the reversed array.

Answer

```JavaScript
// Way 1: Using a for loop to reverse an array, keeping the original array intact.
const oneToTen = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const tenToOne = [];

for (let i = oneToTen.length-1; i >= 0; i--) {
  tenToOne.push(oneToTen[i]);
}

console.log('Original array:', oneToTen);
console.log('Reversed array:', tenToOne);

// Way 2: Using the reverse() method to reverse an array in place.
const oneToTwenty = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20];
const reversedArray = oneToTwenty.reverse();
console.log('Original array:', oneToTwenty);
console.log('Reversed array:', reversedArray);

// Way 3: Using the spread operator + reverse() to create a new reversed array.
const oneToFive = [1, 2, 3, 4, 5];
const reversedArray2 = [...oneToFive].reverse();
console.log('Original array:', oneToFive);
console.log('Reversed array:', reversedArray2);

// Way 4: Reversing an original array without using any array methods.
const oneToTwentyFive = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25];

console.log('Original array:', oneToTwentyFive);

for (let i = 1; i <= Math.floor(oneToTwentyFive.length / 2); i++) {
  const temp = oneToTwentyFive[i - 1];
  oneToTwentyFive[i - 1] = oneToTwentyFive[oneToTwentyFive.length - i];
  oneToTwentyFive[oneToTwentyFive.length - i] = temp;
}

console.log('Reversed array:', oneToTwentyFive);
```

---

## Question-30: Write a while loop that logs numbers from 1 to 100 divisible by 5.

Answer

```JavaScript
let i = 1;
while (i <= 100) {
  if (i % 5 === 0) {
    console.log(i);
  }
  i++;
}

// OR

let num = 5;
while (num <= 100) {
  console.log(num);
  num += 5;
}
```

---

## Question-31: Use a for...in loop to iterate over the properties of an object and log each key-value pair.

```JavaScript
const superHero = {
  realName: 'Peter Parker',
  heroName: 'Spider-Man',
  age: 20,
  powers: ['Web-slinging', 'Spider-sense', 'Wall-crawling'],
  from: 'New York City'
}

for (const key in superHero) {
  console.log(`${key}: ${superHero[key]}`);
}
```

---

## Source - [Top 30 JavaScript Interview Questions 2025 | JavaScript Interview Questions & Answers | Intellipaat](https://www.youtube.com/watch?v=MX48mv73jf8)

Questions - 52 to 81

## Question-52: What is JavaScript and where is it commonly used?

JavaScript is a high level programming language.

It can be used on both sides of the application - front-end & back-end.
Front-end = Browser
Back-end = Server

On the client side JavaScript runs inside the Browser.
On the server side JavaScript runs inside the Node.js.

Both Browser & Node.js provides JavaScript Runtime Environment to execute JavaScript.

Examples:
What JavaScript can do on Front-end = Browser?
1. Form validation before submission.
2. Calculating order total based on number of items and quantity on a cart page.
3. Show/hide elements based on user interaction.
etc.

What JavaScript can do on Back-end = Server?
1. Creating an HTTP server to handle client requests.
2. CLI script to execute tasks/jobs in the background.
3. Writing APIs.
etc.

---

## Question-53: What are template literals in JavaScript?

Template literals are a modern JavaScript feature,
allows us to embed variables and expressions directly into a string.
Writing multi-line strings is easier.
It looks more cleaner & readable compared to traditional strings.

Key Features of Template Literals
- String interpolation - Embed variables and expressions directly in strings
- Multi-line strings - Create strings spanning multiple lines without escape characters
- Tagged templates - Process template literals with a function for advanced string manipulation

Benefits of Template Literals
- Readability - Code is cleaner and intention is clearer
- Maintainability - Easier to update strings with embedded values
- Flexibility - Can include any valid JavaScript expression inside ${}
- Multi-line support - No need for escape characters or concatenation for line breaks

Template literals are particularly useful when building dynamic HTML content, creating complex string formats, or when working with data that needs to be displayed in a structured way.

Example
```JavaScript
const name = "Peter Parker";
const age = 20;
const aka = 'Spiderman';
const homeTown = 'New York';

// String concatenation/without template literals
var message = "Hello, I am " + name + ".\n" + 
              "I am from " + homeTown + ".\n" +
              "I am " + age + " years old and I am the " + aka + " in the Avengers team.\n" + 
              "I have " + (2023 - (age - 5)) + " years of experience.";

console.log(message);

// Using template literals (with backticks)
var message2 = `Hello, I am ${name}.
I am from ${homeTown}.
I am ${age} years old and I am the ${aka} in the Avengers team.
I have ${(2023 - (age - 5))} years of experience.`;

console.log(message2);
```

Tagged templates
Tagged templates allow you to process template literals with a custom function, giving you complete control over how the template is transformed into the final string.

How Tagged Template is processed?
Step-1: Embedded variables/expressions are evaluated
Step-2: Strings & Values are then passed to the custom function defined for the tagged template (function name = tag name).
This function then prepares the final string as per the custom implementation logic and returns it.
Step-3: Final string is then used in place of the Tagged Template.

Here are the examples:

```JavaScript

// Example #1
// Highlight dynamic values in the string which are set through variables/expressions.

// Define a tag function
function highlight(strings, ...values) {
  // strings contains the literal text segments
  // values contains the evaluated expressions
  
  let result = '';
  
  // Interweave strings and values with custom formatting
  strings.forEach((string, i) => {
    result += string;
    if (i < values.length) {
      // Wrap each interpolated value in HTML <strong> tags
      result += `<strong>${values[i]}</strong>`;
    }
  });
  
  return result;
}

// User information
const name = "Mitesh Prajapati";
const role = "Developer";
const experience = 10;

// Using the tagged template
const profile = highlight`Name: ${name}, Role: ${role}, Years of experience: ${experience}`;

console.log(profile);
// Output: "Name: <strong>Alex</strong>, Role: <strong>Developer</strong>, Years of experience: <strong>5</strong>"

// Example #2
// Internationalization of the dynamic content set through variables/expressions.
const amount = 1250.33;
const date = new Date('2025-03-15');

// Tagged template for internationalization
const userMessage = i18n`Your payment of ${amount} is due on ${date}`;
// Could output: "Your payment of $1,250.33 is due on Mar 15, 2025" (US)
// Or: "Your payment of 1.250,33 € is due on 15.03.2025" (Germany)

function i18n(strings, ...values) {
  // implementation of the i18n function
}
```

---

## Question-54: What is Hoisting? Give an example.




---

## Reference

- [Sheryians Coding School | 50 JavaScript Interview Questions Solved in 1 Hour](https://www.youtube.com/watch?v=qTszFuibDEg)

---

## Backlog

<script type="module" ?

1️⃣ Closures – Functions that remember the scope they were created in
2️⃣ Hoisting – Declarations are lifted, initializations are not
3️⃣ Event Loop – JS is single-threaded but non-blocking, thanks to callbacks
4️⃣ Promises – Handling async with cleaner control flow
5️⃣ Async/Await – Write async code like it's synchronous
6️⃣ Arrow Functions – Short syntax, no own this
7️⃣ Destructuring – Cleanly extract values from arrays/objects
8️⃣ Spread & Rest – Expand or gather elements with ...
9️⃣ Prototypes – JS’s inheritance model
🔟 This keyword – Depends on invocation context
1️⃣1️⃣ Classes – ES6 syntax sugar over prototype inheritance
1️⃣2️⃣ Modules – Organize and reuse code with import/export
1️⃣3️⃣ Map & Filter – Array transformation and selection
1️⃣4️⃣ Reduce – Accumulate values, compute sums, etc.
1️⃣5️⃣ setTimeout / setInterval – Delayed & repeated executions
1️⃣6️⃣ Template Literals – ${...} makes string interpolation easier
1️⃣7️⃣ Type Coercion – JS auto-converts types (sometimes unexpectedly)
1️⃣8️⃣ Truthy & Falsy – Know what evaluates to true or false
1️⃣9️⃣ Debounce & Throttle – Control function execution in real-time events
2️⃣0️⃣ Currying – Break functions into smaller, reusable pieces

