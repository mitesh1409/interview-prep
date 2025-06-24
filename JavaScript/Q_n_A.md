## Question-1: Log your name and favorite hobby to the console.

```JavaScript
console.log('Mitesh', 'I love to build things!');
```

---

## Question-2: Perform and log the result of 45 * 2 - 10.

```JavaScript
console.log(45 * 2 - 10);
```

---

## Question-3.1: Use console.log to display the current year.

```JavaScript
console.log(new Date().getFullYear());
```

## Question-3.2: Use console.log to display the current date and time.

```JavaScript
console.log(new Date());
```

## Question-3.3: Use console.log to display the current date and time in US timezone.

```JavaScript
console.log(new Date().toLocaleString('en-US', { timeZone: 'America/New_York' }));
```

## Question-3.4: Use console.log to display the current date and time in UK timezone.

```JavaScript
console.log(new Date().toLocaleString('en-GB', { timeZone: 'Europe/London' }));
```

---

## Question-4: Create two variables - firstName and lastName, concatenate them, and log the full name.

```JavaScript
const firstName = 'Mitesh';
const lastName = 'Prajapati';

console.log(`${firstName} ${lastName}`);
```

---

## Question-5: Track the value of a variable - log its value, change it, and log it again.

```JavaScript
let money = 1000;
console.log('money', money);
money += 500;
console.log('money', money);

// console.log('salary', salary); // Uncaught ReferenceError: Cannot access 'salary' before initialization
// let salary;
// salary = 50000;
// console.log('salary', salary);
```

---

## Question-6: Use console.error to log an error message.

```JavaScript
console.error('This is an error message!');
```

---

## Question-7: Log the square of the number 12 to the console.

```JavaScript
const number = 12;
console.log(`The square of ${number} is ${number ** 2}`); // Exponentiation operator **
console.log(`The square of ${number} is ${number * number}`);
console.log(`The square of ${number} is ${Math.pow(number, 2)}`);
```

---

## Question-8: Log the type of the variable holding the value true.

```JavaScript
const isTrue = true;
console.log(`The type of isTrue is ${typeof isTrue}`);
```

---

## Question-9: Create a variable holding your age and log whether you are an adult or not (age > 18 means you are an adult).

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

```JavaScript
console.log(100 / 0); // Infinity
console.log(1000);
console.log('typeof (100 / 0)', typeof (100 / 0)); // number
console.log('typeof 1000', typeof 1000); // number
```

---

## Question-11: Declare a variable using let and log its value.

```JavaScript
let username;
console.log('username', username);
```

---

## Question-12: Create a constant to store the value of pi and log it.

```JavaScript
const PI = Math.PI
console.log('PI', PI);
```

---

## Question-13: Reassign a value to a variable declared with let and log the new value.

```JavaScript
username = 'mitesh_prajapati';
console.log('username', username);

username = 'john_wick';
console.log('username', username);
```

---

## Question-14: Check the type of null, undefined, NaN and log it.

```JavaScript
console.log('Type of null:', typeof null);
console.log('Type of undefined:', typeof undefined);
console.log('Type of NaN:', typeof NaN);
```

---

## Question-15: Create a variable with a number as a string and log its type.

```JavaScript
const fifty = '50';
console.log('Type of fifty:', typeof fifty);
```

---

## Question-16: Use typeof to check the type of a boolean variable and log it.

```JavaScript
let isWeekend = false;
console.log('Type of isWeekend:', typeof isWeekend);
```

---

## Question-17: Create three variables of types string, number, and boolean, and log their values and types.

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

```JavaScript
let uninitializedVar;
console.log('uninitializedVar:', uninitializedVar, 'Type:', typeof uninitializedVar);
```

---

## Question-19: NaN === NaN, what is the result? Log it.

```JavaScript
console.log('NaN === NaN:', NaN === NaN); // false, because NaN is not equal to anything, including itself
```

---

## Question-20: Create a variable with undefined value and log its type.

```JavaScript
let undefinedVar = undefined;
console.log('undefinedVar:', undefinedVar, 'Type:', typeof undefinedVar);
```

---

## Question-21: Use const to create an array. Try reassigning the array and observe the behavior.

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


## Reference

- [Sheryians Coding School | 50 JavaScript Interview Questions Solved in 1 Hour](https://www.youtube.com/watch?v=qTszFuibDEg)



