
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

## Reference

- [Sheryians Coding School | 50 JavaScript Interview Questions Solved in 1 Hour](https://www.youtube.com/watch?v=qTszFuibDEg)



