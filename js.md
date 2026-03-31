# JavaScript Interview Questions & Answers

### Complete Theory + Code Guide for Interview Preparation

---

## 📌 Table of Contents

1. [General JavaScript](#general-javascript)
2. [Array & Object Questions](#array--object-questions)
3. [Advanced JavaScript](#advanced-javascript)
4. [DOM & Browser APIs](#dom--browser-apis)
5. [Bonus Questions](#bonus-questions)

---

## General JavaScript

---

### 1. What are the differences between `let`, `const`, and `var`?

In JavaScript, `var`, `let`, and `const` are all used to declare variables, but they behave very differently in terms of **scope**, **hoisting**, **re-declaration**, and **re-assignment**.

**`var`** is the oldest way to declare variables and is **function-scoped**, meaning it is accessible anywhere within the function it is declared in. If declared outside a function, it becomes a global variable. One of the biggest problems with `var` is that it is **hoisted to the top of its scope** and initialized with `undefined`, which can lead to unexpected behavior. It also allows **re-declaration** within the same scope, which can cause bugs in large codebases.

**`let`** was introduced in ES6 (2015) and is **block-scoped**, meaning it is only accessible within the `{}` block where it is declared (e.g., inside an `if` block or `for` loop). It is also hoisted, but unlike `var`, it is NOT initialized — accessing it before declaration results in a **ReferenceError** due to the **Temporal Dead Zone (TDZ)**. `let` allows re-assignment but does NOT allow re-declaration in the same scope.

**`const`** is also block-scoped like `let`, but it does NOT allow re-assignment after the initial value is set. It must be **initialized at the time of declaration**. However, `const` does not make objects or arrays immutable — you can still modify the properties of an object or elements of an array declared with `const`. It just prevents you from reassigning the variable to a completely new value.

| Feature         | `var`           | `let`           | `const`         |
| --------------- | --------------- | --------------- | --------------- |
| Scope           | Function-scoped | Block-scoped    | Block-scoped    |
| Hoisting        | Yes (undefined) | Yes (TDZ error) | Yes (TDZ error) |
| Re-declaration  | Allowed         | Not allowed     | Not allowed     |
| Re-assignment   | Allowed         | Allowed         | Not allowed     |
| Must initialize | No              | No              | Yes             |

```js
// var — function scoped
function test() {
  var x = 10;
  if (true) {
    var x = 20; // same variable!
    console.log(x); // 20
  }
  console.log(x); // 20 (changed!)
}

// let — block scoped
function test2() {
  let y = 10;
  if (true) {
    let y = 20; // different variable
    console.log(y); // 20
  }
  console.log(y); // 10 (unchanged)
}

// const — block scoped, no reassignment
const obj = { name: "Alice" };
obj.name = "Bob"; // ✅ Allowed (mutating property)
// obj = {};      // ❌ TypeError (reassignment not allowed)
```

> **Best Practice:** Always use `const` by default. Use `let` when you need to reassign a variable. Avoid `var` in modern JavaScript.

---

### 2. What is the difference between `null` and `undefined`?

Both `null` and `undefined` represent the **absence of a value** in JavaScript, but they are used in different contexts and have different meanings.

**`undefined`** means a variable has been **declared but has not yet been assigned a value**. JavaScript itself assigns `undefined` automatically in several situations — when a variable is declared but not initialized, when a function doesn't explicitly return a value, or when you try to access an object property that doesn't exist. It is essentially JavaScript's way of saying "this thing exists, but has no value yet."

**`null`** is an **intentional, explicit assignment** that represents "no value" or "empty value." It is used by developers when they want to deliberately indicate that a variable currently holds no object or value. For example, if you want to reset a variable or indicate that an API returned no data, you would set it to `null`. Unlike `undefined`, `null` does not occur naturally — a programmer has to set it manually.

An important quirk is that `typeof null` returns `"object"`, which is a **well-known bug in JavaScript** that has been kept for backward compatibility reasons. In reality, `null` is its own primitive type.

```js
// undefined — automatically assigned
let a;
console.log(a); // undefined
console.log(typeof a); // "undefined"

function greet() {}
console.log(greet()); // undefined (no return value)

const user = {};
console.log(user.name); // undefined (property doesn't exist)

// null — manually assigned
let data = null;
console.log(data); // null
console.log(typeof null); // "object" (historical JS bug)

// Comparison
console.log(null == undefined); // true  (loose — both represent "nothing")
console.log(null === undefined); // false (strict — different types)
console.log(null == 0); // false
console.log(undefined == 0); // false
```

> **When to use which:** Use `null` when you intentionally want to clear or empty a value. Let JavaScript use `undefined` naturally — don't manually assign it yourself.

---

### 3. What is the difference between `==` and `===`?

JavaScript has two types of equality operators, and understanding the difference is critical to avoiding hard-to-find bugs.

**`==` (Loose Equality / Abstract Equality)** compares two values **after performing type coercion**. This means JavaScript will try to convert both values to a common type before comparing them. For example, comparing a number and a string: JavaScript will convert the string to a number first, then compare. This can lead to surprising results.

**`===` (Strict Equality / Identity Equality)** compares both the **value AND the type** without any type conversion. If the types are different, it immediately returns `false` without trying to coerce anything.

In most cases, `===` is the preferred and safer choice because it is predictable and avoids the confusing behavior caused by type coercion.

```js
// == with type coercion
console.log(0 == "0"); // true  (string "0" → number 0)
console.log(0 == false); // true  (false → 0)
console.log("" == false); // true  (both → 0)
console.log(null == undefined); // true  (special rule)
console.log(1 == "1"); // true

// === without type coercion
console.log(0 === "0"); // false (number vs string)
console.log(0 === false); // false (number vs boolean)
console.log(null === undefined); // false (null vs undefined)
console.log(1 === "1"); // false

// NaN is never equal to itself
console.log(NaN == NaN); // false
console.log(NaN === NaN); // false
// Use Number.isNaN() to check for NaN
console.log(Number.isNaN(NaN)); // true
```

> **Best Practice:** Always use `===` unless you have a specific reason to use `==`. The only common use case for `==` is checking if something is `null or undefined` at the same time: `value == null`.

---

### 4. What is type coercion in JavaScript?

Type coercion is the **automatic or implicit conversion of a value from one data type to another** by the JavaScript engine. Since JavaScript is a dynamically typed language, it tries to make operations work even when the data types don't match — sometimes in surprising or unintended ways.

There are two types of coercion:

**Implicit coercion** happens automatically when you use operators like `+`, `-`, `==`, or logical operators with mixed types. The JavaScript engine decides what type to convert to.

**Explicit coercion** (also called type casting) is when the developer manually converts a value from one type to another using built-in functions like `Number()`, `String()`, `Boolean()`, `parseInt()`, `parseFloat()`, etc.

The most confusing coercions involve the `+` operator. When one of the operands is a **string**, `+` performs **string concatenation** instead of addition. But with `-`, `*`, `/`, JavaScript always tries to convert to a **number**.

```js
// Implicit coercion — the JS engine does it automatically

// + with string → concatenation
"5" + 2; // "52" (number 2 becomes "2")
"5" + true; // "5true"
"5" + null; // "5null"
"5" + undefined; // "5undefined"

// -, *, / → always numeric
"5" - 2; // 3   (string "5" → number 5)
"5" * 2; // 10
"10" / "2"; // 5
true + 1; // 2   (true → 1)
false + 1; // 1   (false → 0)
null + 1; // 1   (null → 0)
undefined + 1; // NaN (undefined → NaN)

// Boolean coercion (truthy/falsy)
Boolean(0); // false
Boolean(""); // false
Boolean(null); // false
Boolean(undefined); // false
Boolean(NaN); // false
Boolean("hello"); // true
Boolean(42); // true
Boolean([]); // true (empty array is truthy!)
Boolean({}); // true (empty object is truthy!)

// Explicit coercion — developer manually converts
Number("42"); // 42
Number("3.14"); // 3.14
Number(""); // 0
Number("abc"); // NaN
Number(true); // 1
Number(false); // 0
Number(null); // 0
Number(undefined); // NaN

String(42); // "42"
String(true); // "true"
String(null); // "null"

parseInt("42px"); // 42 (stops at non-numeric character)
parseFloat("3.14"); // 3.14
```

> **Key Takeaway:** Type coercion is one of the most misunderstood features of JavaScript. Understanding it helps you write safer code and avoid common bugs, especially when comparing values or performing arithmetic.

---

### 5. What are JavaScript closures? Provide an example.

A **closure** is one of the most important and powerful concepts in JavaScript. A closure is formed when an **inner function has access to the variables and scope of its outer (enclosing) function**, even after the outer function has finished executing and returned.

In other words, a closure allows a function to "remember" the environment in which it was created. When a function is defined inside another function, the inner function retains a reference to the variables in the outer function's scope. This reference persists even after the outer function has completed, which is what makes closures so powerful.

**How closures work internally:** Every function in JavaScript keeps a reference to its **lexical environment** — the scope where it was written. When you call an inner function later, JavaScript looks up the scope chain and finds the variables from the outer function.

**Common use cases of closures:**

- **Data encapsulation / private variables** — hiding state that shouldn't be accessed from outside
- **Factory functions** — creating customized functions
- **Memoization** — caching results of function calls
- **Event handlers and callbacks** — preserving context
- **Module pattern** — creating modules with private and public members

```js
// Basic closure example
function outerFunction() {
  let count = 0; // This variable is "closed over"

  function innerFunction() {
    count++;
    console.log("Count:", count);
  }

  return innerFunction;
}

const counter = outerFunction(); // outerFunction has returned, but count lives on
counter(); // Count: 1
counter(); // Count: 2
counter(); // Count: 3

// Each call to outerFunction creates its own closure
const counter2 = outerFunction();
counter2(); // Count: 1 (independent from counter)

// Real-world use case: Private variables
function createBankAccount(initialBalance) {
  let balance = initialBalance; // private — not accessible from outside

  return {
    deposit(amount) {
      balance += amount;
      console.log(`Deposited ${amount}. Balance: ${balance}`);
    },
    withdraw(amount) {
      if (amount > balance) {
        console.log("Insufficient funds");
      } else {
        balance -= amount;
        console.log(`Withdrew ${amount}. Balance: ${balance}`);
      }
    },
    getBalance() {
      return balance;
    },
  };
}

const account = createBankAccount(1000);
account.deposit(500); // Deposited 500. Balance: 1500
account.withdraw(200); // Withdrew 200. Balance: 1300
console.log(account.getBalance()); // 1300
// console.log(account.balance); // undefined — balance is private!

// Factory function using closures
function multiplier(factor) {
  return function (number) {
    return number * factor;
  };
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

> **Important:** Closures don't copy the variable's value — they hold a **live reference** to it. So if the variable changes, the closure will see the updated value.

---

### 6. What is the event loop in JavaScript?

The **event loop** is the mechanism that allows JavaScript to perform **non-blocking, asynchronous operations** despite being a **single-threaded** language. This is one of the most critical concepts to understand about how JavaScript works under the hood.

JavaScript can only do one thing at a time because it has a single **call stack**. However, it needs to handle things like network requests, timers, user interactions, etc., without freezing the entire program. The event loop makes this possible.

**The key components are:**

1. **Call Stack** — This is where JavaScript executes code. When a function is called, it is pushed onto the stack. When it returns, it is popped off. JavaScript executes one stack frame at a time.

2. **Web APIs (Browser APIs)** — When JavaScript encounters asynchronous operations like `setTimeout`, `fetch`, DOM events, etc., it hands them off to the browser's Web APIs. These run outside the call stack in the background.

3. **Callback Queue (Macrotask Queue)** — Once a Web API completes its work (e.g., a timer fires), it places the callback function into the Callback Queue. This includes: `setTimeout`, `setInterval`, `setImmediate`, I/O callbacks.

4. **Microtask Queue** — This is a higher-priority queue that holds callbacks from: Promises (`.then`, `.catch`), `async/await`, `queueMicrotask()`, and `MutationObserver`. Microtasks are processed **before** macrotasks.

5. **Event Loop** — The event loop continuously monitors the call stack. When the call stack is **empty**, it first drains the entire **microtask queue**, then picks the first item from the **macrotask queue** and pushes it to the call stack.

```js
console.log("1 - Start"); // Synchronous

setTimeout(() => {
  console.log("2 - setTimeout"); // Macrotask
}, 0);

Promise.resolve().then(() => {
  console.log("3 - Promise .then"); // Microtask
});

queueMicrotask(() => {
  console.log("4 - queueMicrotask"); // Microtask
});

console.log("5 - End"); // Synchronous

// Output Order:
// 1 - Start
// 5 - End
// 3 - Promise .then    ← microtasks run before macrotasks
// 4 - queueMicrotask   ← microtask
// 2 - setTimeout       ← macrotask (even with 0ms delay!)
```

**Visual flow:**

```
Call Stack → Empty?
              ├── Yes → Drain Microtask Queue first
              │         └── Then take ONE item from Macrotask Queue
              └── No  → Keep executing current stack
```

> **Why this matters in interviews:** Understanding the event loop explains why `setTimeout(fn, 0)` doesn't run immediately, why Promises are faster than `setTimeout`, and how JavaScript can be non-blocking without multiple threads.

---

### 7. What are Promises, and how do they work?

A **Promise** is a JavaScript object that represents the **eventual completion or failure of an asynchronous operation** and its resulting value. It is a cleaner alternative to callbacks and avoids "callback hell."

Before Promises, async code was handled using nested callbacks, which made code difficult to read and maintain. Promises provide a structured, chainable way to handle async results.

**A Promise has three possible states:**

- **`pending`** — The initial state. The async operation has not yet completed.
- **`fulfilled`** — The operation completed successfully and the Promise has a result value.
- **`rejected`** — The operation failed and the Promise has a reason (error).

Once a Promise moves from `pending` to either `fulfilled` or `rejected`, it is **settled** and **cannot change state again**.

**Promise chaining** allows you to chain `.then()` calls — each `.then()` receives the result of the previous one. `.catch()` handles any rejection in the chain, and `.finally()` runs regardless of success or failure.

**Static Promise methods:**

- `Promise.all()` — waits for ALL promises to resolve; rejects if any one rejects
- `Promise.allSettled()` — waits for ALL promises to settle (resolve or reject); never rejects
- `Promise.race()` — resolves/rejects as soon as the FIRST promise settles
- `Promise.any()` — resolves as soon as the FIRST promise fulfills; rejects only if ALL reject

```js
// Creating a Promise
const fetchUserData = new Promise((resolve, reject) => {
  const success = true;

  setTimeout(() => {
    if (success) {
      resolve({ id: 1, name: "Alice", age: 25 }); // Fulfilled
    } else {
      reject(new Error("Failed to fetch user data")); // Rejected
    }
  }, 1000);
});

// Consuming a Promise
fetchUserData
  .then((user) => {
    console.log("User fetched:", user.name); // "Alice"
    return user.age; // Pass value to next .then()
  })
  .then((age) => {
    console.log("Age:", age); // 25
  })
  .catch((error) => {
    console.error("Error:", error.message); // Handles any rejection in the chain
  })
  .finally(() => {
    console.log("Operation complete"); // Always runs
  });

// Real-world fetch example
function getPost(id) {
  return fetch(`https://jsonplaceholder.typicode.com/posts/${id}`).then(
    (response) => {
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return response.json();
    },
  );
}

getPost(1)
  .then((post) => console.log(post.title))
  .catch((err) => console.error(err));

// Promise.all — all must succeed
Promise.all([
  fetch("https://api.example.com/users"),
  fetch("https://api.example.com/posts"),
  fetch("https://api.example.com/comments"),
])
  .then(([users, posts, comments]) => {
    console.log("All data loaded");
  })
  .catch((err) => console.error("One of them failed:", err));

// Promise.allSettled — get results of all, even if some fail
Promise.allSettled([
  Promise.resolve("success"),
  Promise.reject("failure"),
  Promise.resolve("another success"),
]).then((results) => {
  results.forEach((result) => {
    if (result.status === "fulfilled") {
      console.log("Value:", result.value);
    } else {
      console.log("Reason:", result.reason);
    }
  });
});
```

---

### 8. How does `async/await` work in JavaScript?

`async/await` is **syntactic sugar built on top of Promises**, introduced in ES2017 (ES8). It allows you to write asynchronous code that **looks and reads like synchronous code**, making it much easier to understand, debug, and maintain compared to raw Promise chains.

**`async` keyword:** When you put `async` before a function declaration, it does two things:

1. The function always returns a **Promise** (even if you return a plain value, it wraps it in a resolved Promise).
2. It enables the use of `await` inside the function.

**`await` keyword:** `await` can only be used inside an `async` function. It **pauses the execution of the async function** and waits for the Promise to resolve or reject. While waiting, the JavaScript engine is free to run other code (it's non-blocking). Once the Promise settles, execution resumes from where it paused.

**Error handling with async/await:** Instead of using `.catch()`, we use the standard `try...catch` block, which makes error handling cleaner and more consistent with synchronous code.

**Running multiple async operations in parallel:** If you `await` multiple independent Promises sequentially, it's slow because each waits for the previous one to finish. Use `Promise.all()` with `await` to run them in parallel.

```js
// Basic async/await
async function getUserData(userId) {
  try {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/users/${userId}`,
    );

    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status}`);
    }

    const user = await response.json(); // Waits for JSON parsing
    console.log("User:", user.name);
    return user;
  } catch (error) {
    console.error("Failed to get user:", error.message);
    throw error; // Re-throw if needed
  } finally {
    console.log("Request finished");
  }
}

getUserData(1);

// Sequential vs Parallel — Important Performance Difference!

// ❌ Sequential (slow — 2 seconds total if each takes 1 second)
async function sequential() {
  const user = await fetchUser(1); // Wait 1 second
  const posts = await fetchPosts(1); // Wait another 1 second
  // Total: 2 seconds
}

// ✅ Parallel (fast — 1 second total, runs simultaneously)
async function parallel() {
  const [user, posts] = await Promise.all([fetchUser(1), fetchPosts(1)]);
  // Total: 1 second
}

// async function always returns a Promise
async function greet() {
  return "Hello!";
}
greet().then((msg) => console.log(msg)); // "Hello!"

// await on a non-Promise just returns the value
async function example() {
  const result = await 42; // Works fine
  console.log(result); // 42
}
```

---

### 9. What is the difference between synchronous and asynchronous programming?

**Synchronous programming** means code is executed **line by line, one at a time**. Each line waits for the previous one to complete before executing. This is simple and predictable, but if any operation takes a long time (like reading a large file or making a network request), the entire program is **blocked** and cannot do anything else during that time. This is called **blocking behavior**.

**Asynchronous programming** allows the program to **start a long-running operation and continue executing other code** without waiting for the operation to complete. When the async operation finishes, it notifies the program through callbacks, Promises, or events. This is called **non-blocking behavior**, and it keeps applications responsive.

In JavaScript, the browser or Node.js runtime handles async operations in the background (via Web APIs or libuv). When the operation finishes, the result is placed in the event queue, and the event loop picks it up when the call stack is empty.

**Real-world analogy:**

- **Synchronous** = Going to a restaurant, placing your order, and standing at the counter waiting for your food before doing anything else.
- **Asynchronous** = Placing your order, sitting at your table, doing other things, and the waiter brings your food when it's ready.

```js
// Synchronous — blocks execution
console.log("Start");

function heavyTask() {
  // Imagine this takes 5 seconds
  for (let i = 0; i < 1_000_000_000; i++) {} // Blocks thread!
  return "Done";
}

const result = heavyTask(); // UI freezes here for 5 seconds
console.log(result);
console.log("End"); // Only runs after heavyTask finishes

// Output: Start → (5 second freeze) → Done → End

// Asynchronous — non-blocking
console.log("Start");

setTimeout(() => {
  console.log("Async operation done"); // Runs later, after 2 seconds
}, 2000);

console.log("End"); // Runs immediately, doesn't wait for setTimeout

// Output: Start → End → (2 seconds later) → Async operation done

// Async with fetch — real example
console.log("Fetching data...");

fetch("https://api.example.com/data")
  .then((res) => res.json())
  .then((data) => console.log("Data received:", data)); // Runs when ready

console.log("This runs immediately, doesn't wait for fetch!");
```

| Feature     | Synchronous             | Asynchronous                         |
| ----------- | ----------------------- | ------------------------------------ |
| Execution   | Line by line, blocking  | Non-blocking, continues without wait |
| Use case    | Simple logic, CPU tasks | Network requests, file I/O, timers   |
| Performance | Blocks entire thread    | Keeps UI/server responsive           |
| Code style  | Straightforward         | Callbacks, Promises, async/await     |

---

### 10. What are JavaScript callbacks?

A **callback** is simply a **function that is passed as an argument to another function** and is **executed (called back) at a later time** — either synchronously or asynchronously.

Callbacks are one of the foundational patterns in JavaScript for handling asynchronous operations. They were the primary way to deal with async code before Promises and async/await were introduced.

**Synchronous callbacks** are executed immediately within the function they are passed to. For example, `Array.prototype.forEach`, `map`, `filter` all use synchronous callbacks.

**Asynchronous callbacks** are executed later, after some async operation completes — like `setTimeout`, reading a file, or making an HTTP request.

**The problem with callbacks — "Callback Hell":** When you need to perform multiple async operations where each one depends on the result of the previous one, you end up with deeply nested callbacks. This pattern is called "Callback Hell" or the "Pyramid of Doom." It makes code hard to read, maintain, and debug. This is one of the main reasons Promises and async/await were introduced.

```js
// Synchronous callback
function greet(name, callback) {
  console.log("Hello, " + name);
  callback(); // Called immediately
}

greet("Alice", function () {
  console.log("Callback executed!");
});
// Hello, Alice
// Callback executed!

// Asynchronous callback
function fetchData(url, onSuccess, onError) {
  setTimeout(() => {
    if (url) {
      onSuccess({ data: "Some data from " + url });
    } else {
      onError(new Error("URL is required"));
    }
  }, 1000);
}

fetchData(
  "https://api.example.com",
  (result) => console.log("Success:", result.data),
  (error) => console.error("Error:", error.message),
);

// ❌ Callback Hell (Pyramid of Doom)
login(user, function (err, loggedInUser) {
  if (err) return handleError(err);
  getUserProfile(loggedInUser.id, function (err, profile) {
    if (err) return handleError(err);
    getProfilePosts(profile.id, function (err, posts) {
      if (err) return handleError(err);
      displayPosts(posts, function (err) {
        if (err) return handleError(err);
        console.log("Done!"); // Deeply nested!
      });
    });
  });
});

// ✅ Same logic using async/await (much cleaner)
async function loadUserPosts(user) {
  try {
    const loggedInUser = await login(user);
    const profile = await getUserProfile(loggedInUser.id);
    const posts = await getProfilePosts(profile.id);
    await displayPosts(posts);
    console.log("Done!");
  } catch (err) {
    handleError(err);
  }
}
```

---

### 11. What is hoisting in JavaScript?

**Hoisting** is JavaScript's default behavior of **moving variable and function declarations to the top of their containing scope** during the **compilation phase**, before the code is actually executed.

It's important to understand that only the **declaration** is hoisted, not the **initialization** or assignment. So the variable exists in memory before the code runs, but its value is only set when execution reaches that line.

**Hoisting behavior differs by declaration type:**

- **`var` declarations** are hoisted and initialized with `undefined`. This is why you can use a `var` variable before its declaration without getting an error — but you'll get `undefined` instead of the actual value.

- **Function declarations** are fully hoisted — both the name AND the body. This means you can call a function declaration anywhere in its scope, even before the line where it's defined.

- **`let` and `const` declarations** are also hoisted, but they are NOT initialized. They exist in a **Temporal Dead Zone (TDZ)** from the start of the block until the declaration is reached. Accessing them before declaration throws a `ReferenceError`.

- **Function expressions and arrow functions** assigned to `var` are hoisted as `undefined` (since `var` is hoisted), and those assigned to `let`/`const` are in the TDZ.

```js
// var hoisting
console.log(a); // undefined (not an error!)
var a = 5;
console.log(a); // 5

// What JS actually sees internally:
// var a; ← hoisted to top
// console.log(a); // undefined
// a = 5;
// console.log(a); // 5

// Function declaration hoisting — fully hoisted
greet(); // ✅ Works! Prints "Hello"

function greet() {
  console.log("Hello");
}

// let and const — Temporal Dead Zone
console.log(b); // ❌ ReferenceError: Cannot access 'b' before initialization
let b = 10;

console.log(c); // ❌ ReferenceError
const c = 20;

// Function expression — NOT hoisted as a function
sayHi(); // ❌ TypeError: sayHi is not a function

var sayHi = function () {
  console.log("Hi!");
};
// var sayHi is hoisted as undefined, so sayHi() throws TypeError

// Class declarations — also have TDZ
const obj = new MyClass(); // ❌ ReferenceError
class MyClass {
  constructor() {
    this.name = "test";
  }
}
```

> **Key insight:** Hoisting is why it's considered best practice to always declare variables at the top of their scope and to use `const`/`let` instead of `var` — it makes the code behavior more predictable.

---

### 12. What is the difference between `call`, `apply`, and `bind`?

`call`, `apply`, and `bind` are three methods available on every JavaScript function. They are all used to **explicitly set the value of `this`** inside a function. The difference lies in **when the function is invoked** and **how arguments are passed**.

**`call(thisArg, arg1, arg2, ...)`** — Invokes the function **immediately** with a specified `this` value and arguments passed **individually (comma-separated)**.

**`apply(thisArg, [arg1, arg2, ...])`** — Invokes the function **immediately** with a specified `this` value and arguments passed as an **array**. This is useful when you already have arguments in an array.

**`bind(thisArg, arg1, arg2, ...)`** — Does NOT invoke the function immediately. Instead, it **returns a new function** with `this` permanently bound to the specified value. You can call the returned function later. It also supports **partial application** — pre-filling some arguments.

A useful mnemonic: **call = comma-separated, apply = array, bind = binding for later**.

```js
function introduce(city, country) {
  console.log(`I'm ${this.name} from ${city}, ${country}`);
}

const person1 = { name: "Alice" };
const person2 = { name: "Bob" };

// call — invoke immediately, args comma-separated
introduce.call(person1, "Mumbai", "India");
// "I'm Alice from Mumbai, India"

introduce.call(person2, "London", "UK");
// "I'm Bob from London, UK"

// apply — invoke immediately, args as array
introduce.apply(person1, ["Mumbai", "India"]);
// "I'm Alice from Mumbai, India"

// Practical use case for apply — Math.max with an array
const numbers = [3, 1, 4, 1, 5, 9, 2, 6];
const max = Math.max.apply(null, numbers); // 9
// Modern equivalent: Math.max(...numbers)

// bind — returns new function for later use
const introduceAlice = introduce.bind(person1, "Mumbai");
introduceAlice("India"); // "I'm Alice from Mumbai, India"
introduceAlice("USA"); // "I'm Alice from Mumbai, USA"

// Real-world use case: fixing 'this' in event handlers
class Timer {
  constructor() {
    this.seconds = 0;
  }

  start() {
    // Without bind, 'this' inside the callback would be wrong
    setInterval(this.tick.bind(this), 1000);
  }

  tick() {
    this.seconds++;
    console.log(this.seconds);
  }
}

const timer = new Timer();
timer.start(); // 1, 2, 3, ... every second

// Partial application with bind
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2); // Pre-fill first argument
const triple = multiply.bind(null, 3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

| Method  | Calls immediately | Argument style  | Returns         |
| ------- | ----------------- | --------------- | --------------- |
| `call`  | ✅ Yes            | Comma-separated | Function result |
| `apply` | ✅ Yes            | Array           | Function result |
| `bind`  | ❌ No             | Comma-separated | New function    |

---

### 13. What is the difference between deep copy and shallow copy?

When you copy an object or array in JavaScript, there are two levels of copying: **shallow** and **deep**.

**Shallow copy** creates a new object, but only copies the **top-level properties**. If any of those properties are themselves objects or arrays (reference types), the shallow copy does **not duplicate them** — instead it copies the **reference** (memory address) to the same nested object. So both the original and the copy point to the same nested objects. Modifying a nested property in one will affect the other.

**Deep copy** creates a completely new, **independent copy of an object**, including all nested objects and arrays, at every level. There is no shared reference between the original and the copy. Modifying any level of the deep copy does not affect the original.

**Methods to create copies:**

- Shallow: Spread operator `{...obj}`, `Object.assign({}, obj)`, `[...arr]`, `Array.from(arr)`
- Deep: `JSON.parse(JSON.stringify(obj))` (simple but has limitations), `structuredClone(obj)` (modern and recommended), `_.cloneDeep(obj)` (Lodash)

**Limitations of JSON method:** It cannot handle functions, `undefined`, `Symbol`, `Date` objects (converts to string), `RegExp`, `Map`, `Set`, circular references, etc.

```js
// ---- SHALLOW COPY ----
const original = {
  name: "Alice",
  address: {
    city: "Mumbai",
    zip: "400001",
  },
  hobbies: ["reading", "coding"],
};

// Spread operator — shallow copy
const shallow = { ...original };

// Top-level: independent
shallow.name = "Bob";
console.log(original.name); // "Alice" ✅ Not affected

// Nested objects: SHARED reference
shallow.address.city = "Delhi";
console.log(original.address.city); // "Delhi" ❌ Original changed!

shallow.hobbies.push("gaming");
console.log(original.hobbies); // ["reading", "coding", "gaming"] ❌ Original changed!

// ---- DEEP COPY ----
const original2 = {
  name: "Alice",
  address: { city: "Mumbai" },
  hobbies: ["reading", "coding"],
};

// Method 1: JSON.parse + JSON.stringify (simple, but limited)
const deepJson = JSON.parse(JSON.stringify(original2));
deepJson.address.city = "Delhi";
console.log(original2.address.city); // "Mumbai" ✅ Not affected!

// Method 2: structuredClone (modern, recommended — Node.js 17+, all modern browsers)
const deepClone = structuredClone(original2);
deepClone.address.city = "Pune";
console.log(original2.address.city); // "Mumbai" ✅ Still safe!

// structuredClone handles more types
const withDate = { date: new Date(), set: new Set([1, 2, 3]) };
const cloned = structuredClone(withDate);
console.log(cloned.date instanceof Date); // true ✅
console.log(cloned.set instanceof Set); // true ✅

// JSON method fails with functions and undefined
const withFn = { greet: function () {}, value: undefined };
const jsonCopy = JSON.parse(JSON.stringify(withFn));
console.log(jsonCopy.greet); // undefined — function lost!
console.log(jsonCopy.value); // property completely removed!
```

---

### 14. How does JavaScript handle memory management and garbage collection?

JavaScript uses **automatic memory management**, meaning developers don't need to manually allocate or free memory (unlike C/C++). The JavaScript engine handles this through a process called **garbage collection (GC)**.

**Memory lifecycle in JavaScript:**

1. **Allocation** — Memory is automatically allocated when you create variables, objects, arrays, functions, etc.
2. **Usage** — You read and write to that memory during program execution.
3. **Release** — When memory is no longer needed (no more references to it), the GC frees it.

**The Mark-and-Sweep Algorithm** is the primary garbage collection algorithm used by modern JavaScript engines (like V8 used in Chrome and Node.js):

1. **Mark phase:** The GC starts from "roots" (global variables, call stack, current function scope) and marks all objects that are **reachable** — i.e., can be accessed through any chain of references.
2. **Sweep phase:** All unmarked objects (unreachable from roots) are considered garbage and their memory is freed.

**Memory Leaks** happen when objects are no longer needed by the application but are still referenced, preventing the GC from freeing them. Common causes:

- **Global variables** — accidentally creating global variables stores them forever
- **Event listeners not removed** — listeners hold references to DOM elements and their closures
- **Closures** — inadvertently keeping large objects alive inside a closure
- **setInterval** — if not cleared, its callback (and anything it references) stays in memory
- **Detached DOM nodes** — removed from DOM but still referenced in JS

```js
// How memory is allocated
let num = 42; // Allocated on stack
let str = "hello"; // String allocated on heap
let obj = { name: "Alice" }; // Object allocated on heap
let arr = [1, 2, 3]; // Array allocated on heap

// Unreachable = can be garbage collected
let a = { value: 1 };
let b = a; // Both a and b point to the same object

a = null; // a no longer references the object
// The object is STILL alive because b references it

b = null; // Now no references exist → eligible for GC ✅

// ❌ Memory Leak: Global variable
function createLeak() {
  leakedData = new Array(1000000).fill("leak"); // No var/let/const = global!
}
createLeak(); // leakedData is now a global, never collected

// ❌ Memory Leak: Event listener not removed
const button = document.querySelector("#btn");
const bigData = new Array(100000).fill("data");

function handleClick() {
  console.log(bigData.length); // Closure keeps bigData alive
}

button.addEventListener("click", handleClick);
// If button is removed from DOM but listener not removed:
// bigData stays in memory!

// ✅ Fix: Remove listener when no longer needed
button.removeEventListener("click", handleClick);

// ❌ Memory Leak: setInterval not cleared
const interval = setInterval(() => {
  const data = fetchNewData(); // Keeps running forever
}, 1000);

// ✅ Fix: Clear when done
clearInterval(interval);

// WeakMap/WeakSet — GC-friendly references
const cache = new WeakMap(); // Keys are weakly held
let element = document.querySelector("#myDiv");
cache.set(element, { data: "some info" });

element = null; // When element is no longer referenced,
// the WeakMap entry is automatically removed from memory ✅
```

> **Key Insight:** While JavaScript's GC is automatic, being aware of memory leaks and cleaning up after yourself (removing event listeners, clearing intervals, nullifying large objects) is essential for building performant, long-running applications.

---

### 15. What is the difference between function declaration and function expression?

In JavaScript, there are multiple ways to define a function, and the two most fundamental are **function declarations** and **function expressions**. They look similar but behave differently, especially around hoisting and scope.

**Function Declaration:** A named function defined using the `function` keyword at the statement level. Function declarations are **fully hoisted** — both the name and the function body are moved to the top of the scope during compilation, which means you can call a function declaration **before** it appears in the code.

**Function Expression:** A function that is assigned to a variable, object property, or passed as an argument. Function expressions are **not hoisted** in the same way — only the variable declaration is hoisted (if `var`), not the function body. If you use `let` or `const`, it's in the TDZ.

**Arrow Functions** (introduced in ES6) are a special form of function expression with a shorter syntax. They do NOT have their own `this`, `arguments`, or `prototype`, and cannot be used as constructors.

**Named vs Anonymous function expressions:** Function expressions can be named or anonymous. A named function expression has the advantage that the name appears in stack traces, making debugging easier.

```js
// ---- FUNCTION DECLARATION ----
// Can be called BEFORE it's defined (hoisting)
console.log(add(2, 3)); // ✅ 5 — works due to hoisting!

function add(a, b) {
  return a + b;
}

console.log(add(4, 5)); // ✅ 9

// ---- FUNCTION EXPRESSION ----
// Cannot be called before the line where it's assigned

// console.log(subtract(5, 2)); // ❌ TypeError: subtract is not a function
// (var is hoisted as undefined, not as a function)

var subtract = function (a, b) {
  return a - b;
};

console.log(subtract(5, 2)); // ✅ 3 — after the assignment

// With let/const
// console.log(multiply(2, 3)); // ❌ ReferenceError (TDZ)

const multiply = function (a, b) {
  return a * b;
};

// ---- NAMED FUNCTION EXPRESSION ----
const factorial = function computeFactorial(n) {
  if (n <= 1) return 1;
  return n * computeFactorial(n - 1); // Can reference itself by name
};

console.log(factorial(5)); // 120
// computeFactorial(5); // ❌ ReferenceError — name only exists inside the function

// ---- ARROW FUNCTIONS ----
const greet = (name) => `Hello, ${name}!`;
const square = (n) => n * n; // Single param, no parentheses needed
const getObject = () => ({ id: 1 }); // Wrap object in () to avoid confusion with block

// Arrow functions have no own 'this'
class Button {
  constructor() {
    this.clickCount = 0;
  }

  setupHandler() {
    // ✅ Arrow function inherits 'this' from setupHandler
    document.addEventListener("click", () => {
      this.clickCount++; // 'this' correctly refers to Button instance
    });

    // ❌ Regular function — 'this' would be 'document' or undefined
    document.addEventListener("click", function () {
      // this.clickCount++; // 'this' is wrong here!
    });
  }
}

// Summary of differences
// Arrow functions CANNOT be used as constructors
// const Obj = () => {};
// new Obj(); // ❌ TypeError: Obj is not a constructor

// Arrow functions have no 'arguments' object
function regular() {
  console.log(arguments); // [1, 2, 3]
}
const arrow = () => {
  // console.log(arguments); // ❌ ReferenceError
};
regular(1, 2, 3);
```

---

## Array & Object Questions

---

### 16. What is destructuring in JavaScript?

**Destructuring** is a JavaScript syntax feature (introduced in ES6) that allows you to **unpack values from arrays or properties from objects into separate variables** in a clean, concise way. Instead of accessing each element or property one by one, you can extract multiple values in a single statement.

Destructuring does not modify the original array or object — it just creates new variables with copies of the values.

There are two types of destructuring: **array destructuring** and **object destructuring**.

**Array destructuring** uses the position of elements to assign values. You use square brackets `[]` on the left side of the assignment.

**Object destructuring** uses the property names (keys) to assign values. You use curly braces `{}` on the left side. The variable name must match the property name unless you rename it.

**Advanced features:**

- **Default values** — assign a default if the value is `undefined`
- **Renaming** — use a different variable name than the property key
- **Rest operator** — collect remaining elements/properties into a new array/object
- **Nested destructuring** — destructure deeply nested structures
- **Function parameters** — destructure directly in function signatures (very common in React)

```js
// ---- ARRAY DESTRUCTURING ----
const colors = ["red", "green", "blue", "yellow"];

const [first, second, third] = colors;
console.log(first); // "red"
console.log(second); // "green"
console.log(third); // "blue"

// Skip elements using commas
const [, , thirdColor] = colors;
console.log(thirdColor); // "blue"

// Rest operator
const [head, ...rest] = colors;
console.log(head); // "red"
console.log(rest); // ["green", "blue", "yellow"]

// Default values
const [a = 10, b = 20, c = 30] = [1, 2];
console.log(a, b, c); // 1, 2, 30 (c uses default)

// Swapping variables without temp variable
let x = 1,
  y = 2;
[x, y] = [y, x];
console.log(x, y); // 2, 1

// ---- OBJECT DESTRUCTURING ----
const user = {
  name: "Alice",
  age: 25,
  email: "alice@example.com",
  address: {
    city: "Mumbai",
    country: "India",
  },
};

// Basic destructuring
const { name, age, email } = user;
console.log(name, age, email); // Alice 25 alice@example.com

// Renaming variables
const { name: fullName, age: userAge } = user;
console.log(fullName, userAge); // Alice 25

// Default values
const { phone = "Not provided", name: uName } = user;
console.log(phone); // "Not provided"
console.log(uName); // "Alice"

// Rest operator for objects
const { name: n, ...rest2 } = user;
console.log(n); // "Alice"
console.log(rest2); // { age: 25, email: ..., address: ... }

// Nested destructuring
const {
  address: { city, country },
} = user;
console.log(city, country); // Mumbai India

// ---- DESTRUCTURING IN FUNCTION PARAMETERS ----
// Very common in React props!
function displayUser({ name, age, email = "N/A" }) {
  console.log(`${name}, Age: ${age}, Email: ${email}`);
}

displayUser(user); // Alice, Age: 25, Email: alice@example.com

// ---- PRACTICAL EXAMPLES ----
// Swapping with array destructuring
let p = "hello",
  q = "world";
[p, q] = [q, p];
console.log(p, q); // world hello

// Returning multiple values from a function
function getMinMax(arr) {
  return [Math.min(...arr), Math.max(...arr)];
}
const [min, max] = getMinMax([3, 1, 4, 1, 5, 9]);
console.log(min, max); // 1 9
```

---

### 17. How do you merge two arrays in JavaScript?

```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const arr3 = [7, 8, 9];

// 1. Spread operator (most readable, ES6+)
const merged = [...arr1, ...arr2, ...arr3];
console.log(merged); // [1, 2, 3, 4, 5, 6, 7, 8, 9]

// 2. concat() — classic approach
const merged2 = arr1.concat(arr2, arr3);
console.log(merged2); // [1, 2, 3, 4, 5, 6, 7, 8, 9]

// 3. Array.from with Set (merge + remove duplicates)
const a = [1, 2, 3, 2];
const b = [2, 3, 4, 5];
const uniqueMerged = [...new Set([...a, ...b])];
console.log(uniqueMerged); // [1, 2, 3, 4, 5]

// 4. push with spread (mutates arr1)
arr1.push(...arr2);
```

---

### 18. How do you remove duplicates from an array?

```js
const arr = [1, 2, 2, 3, 4, 4, 5, 1];

// 1. Using Set — most concise (ES6+)
const unique = [...new Set(arr)];
console.log(unique); // [1, 2, 3, 4, 5]

// 2. Using filter with indexOf
const unique2 = arr.filter((val, index) => arr.indexOf(val) === index);

// 3. Using reduce
const unique3 = arr.reduce((acc, val) => {
  if (!acc.includes(val)) acc.push(val);
  return acc;
}, []);

// 4. For objects — filter by a property
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 1, name: "Alice (duplicate)" },
];

const uniqueUsers = arr.filter(
  (user, index, self) => index === self.findIndex((u) => u.id === user.id),
);
```

---

### 19. What is the difference between `map()`, `filter()`, and `reduce()`?

These are the three most important **higher-order array methods** in JavaScript. They are all **pure methods** — they don't modify the original array and always return a new value.

**`map()`** — Transforms every element in the array and returns a **new array of the same length**. Use it when you want to convert each element to something else.

**`filter()`** — Tests each element against a condition and returns a **new array containing only the elements that pass the test**. The returned array may be shorter than the original.

**`reduce()`** — The most powerful and versatile. It iterates through the array and **accumulates a single output value** (which can be a number, string, object, or even another array). It takes a callback and an initial accumulator value.

```js
const products = [
  { name: "Apple", price: 30, category: "Fruit" },
  { name: "Banana", price: 10, category: "Fruit" },
  { name: "Milk", price: 50, category: "Dairy" },
  { name: "Cheese", price: 200, category: "Dairy" },
  { name: "Bread", price: 40, category: "Bakery" },
];

// map() — transform: get just names
const names = products.map((p) => p.name);
console.log(names); // ["Apple", "Banana", "Milk", "Cheese", "Bread"]

// map() — transform: apply 10% discount
const discounted = products.map((p) => ({
  ...p,
  price: p.price * 0.9,
}));

// filter() — keep only fruits
const fruits = products.filter((p) => p.category === "Fruit");
console.log(fruits); // [{Apple}, {Banana}]

// filter() — keep only products under ₹50
const affordable = products.filter((p) => p.price < 50);

// reduce() — total price of all products
const totalPrice = products.reduce((sum, p) => sum + p.price, 0);
console.log(totalPrice); // 330

// reduce() — group by category
const grouped = products.reduce((acc, p) => {
  if (!acc[p.category]) acc[p.category] = [];
  acc[p.category].push(p.name);
  return acc;
}, {});
// { Fruit: ["Apple", "Banana"], Dairy: ["Milk", "Cheese"], Bakery: ["Bread"] }

// reduce() — flatten an array
const nested = [
  [1, 2],
  [3, 4],
  [5, 6],
];
const flat = nested.reduce((acc, arr) => acc.concat(arr), []);
// [1, 2, 3, 4, 5, 6]

// Chaining them together — find total price of fruits only
const fruitTotal = products
  .filter((p) => p.category === "Fruit") // [Apple, Banana]
  .map((p) => p.price) // [30, 10]
  .reduce((sum, price) => sum + price, 0); // 40

console.log(fruitTotal); // 40
```

| Method     | Purpose                            | Returns          | Changes length |
| ---------- | ---------------------------------- | ---------------- | -------------- |
| `map()`    | Transform each element             | New array        | No (same)      |
| `filter()` | Select elements matching condition | New array        | Yes (smaller)  |
| `reduce()` | Accumulate to a single value       | Any single value | N/A            |

---

### 20. How do you sort an array of objects?

```js
const users = [
  { name: "Charlie", age: 30, salary: 50000 },
  { name: "Alice", age: 25, salary: 70000 },
  { name: "Bob", age: 28, salary: 60000 },
  { name: "Diana", age: 25, salary: 55000 },
];

// Sort by age ascending (smallest first)
users.sort((a, b) => a.age - b.age);

// Sort by age descending (largest first)
users.sort((a, b) => b.age - a.age);

// Sort by name alphabetically
users.sort((a, b) => a.name.localeCompare(b.name));

// Sort by name reverse alphabetically
users.sort((a, b) => b.name.localeCompare(a.name));

// Multi-level sort: by age, then by salary if same age
users.sort((a, b) => {
  if (a.age !== b.age) return a.age - b.age; // Primary: age
  return a.salary - b.salary; // Secondary: salary
});

// Note: sort() mutates the original array!
// To avoid mutation:
const sorted = [...users].sort((a, b) => a.age - b.age);
```

---

### 21. What is the difference between `forEach` and `map`?

Both `forEach` and `map` iterate over every element in an array, but they serve very different purposes and should not be used interchangeably.

**`forEach`** is designed for **side effects** — doing something for each element (logging, updating DOM, pushing to an external array). It always returns `undefined` and cannot be chained. It doesn't produce a new array.

**`map`** is designed for **transformation** — creating a new array where each element is the result of applying a function to the corresponding element of the original. It always returns a new array of the same length and can be chained with other array methods.

```js
const numbers = [1, 2, 3, 4, 5];

// forEach — side effects, returns undefined
const result1 = numbers.forEach((n) => n * 2);
console.log(result1); // undefined ❌

// Common forEach use cases
numbers.forEach((n) => console.log(n)); // Logging
numbers.forEach((n) => someExternalArray.push(n)); // Side effect

// map — transformation, returns new array
const doubled = numbers.map((n) => n * 2);
console.log(doubled); // [2, 4, 6, 8, 10] ✅

// map can be chained
const result = numbers
  .map((n) => n * 2)
  .filter((n) => n > 4)
  .reduce((sum, n) => sum + n, 0);
// forEach cannot be chained

// forEach cannot break out of the loop
// Use for...of or some() / every() for early exit
for (const n of numbers) {
  if (n === 3) break; // ✅ Can break
}

// Use .some() to "break" from a map-like operation
numbers.some((n) => {
  if (n === 3) return true; // stops iteration
  console.log(n);
});
```

---

### 22. How do you flatten a nested array?

```js
const nested = [1, [2, 3], [4, [5, [6]]]];

// flat() — specify depth (default is 1)
nested.flat(); // [1, 2, 3, 4, [5, [6]]]
nested.flat(2); // [1, 2, 3, 4, 5, [6]]
nested.flat(Infinity); // [1, 2, 3, 4, 5, 6] ✅ (completely flattened)

// flatMap() — map + flat(1) in one step
const sentences = ["Hello World", "Foo Bar"];
const words = sentences.flatMap((s) => s.split(" "));
console.log(words); // ["Hello", "World", "Foo", "Bar"]

// Manual recursive flatten with reduce
function deepFlatten(arr) {
  return arr.reduce(
    (acc, val) =>
      Array.isArray(val) ? acc.concat(deepFlatten(val)) : acc.concat(val),
    [],
  );
}
console.log(deepFlatten(nested)); // [1, 2, 3, 4, 5, 6]
```

---

### 23. How do you check if an object is empty?

```js
const emptyObj = {};
const nonEmptyObj = { name: "Alice" };

// Method 1: Object.keys() — most common ✅
Object.keys(emptyObj).length === 0; // true
Object.keys(nonEmptyObj).length === 0; // false

// Method 2: JSON.stringify
JSON.stringify(emptyObj) === "{}"; // true

// Method 3: Object.entries()
Object.entries(emptyObj).length === 0; // true

// Method 4: for...in loop
function isEmpty(obj) {
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) return false;
  }
  return true;
}

// Edge cases to be aware of
const withSymbol = { [Symbol("key")]: "value" };
Object.keys(withSymbol).length === 0; // true — Symbol keys not included!
// Use Object.getOwnPropertySymbols() for Symbol keys
```

---

### 24. How do you convert an object to an array?

```js
const obj = { a: 1, b: 2, c: 3 };

// Keys only
Object.keys(obj); // ["a", "b", "c"]

// Values only
Object.values(obj); // [1, 2, 3]

// Key-value pairs
Object.entries(obj); // [["a", 1], ["b", 2], ["c", 3]]

// Convert back from entries
const backToObj = Object.fromEntries([
  ["a", 1],
  ["b", 2],
]);
// { a: 1, b: 2 }

// Practical: transform object values
const prices = { apple: 30, banana: 10, mango: 50 };
const doubled = Object.fromEntries(
  Object.entries(prices).map(([key, val]) => [key, val * 2]),
);
// { apple: 60, banana: 20, mango: 100 }
```

---

### 25. What is the difference between `Object.freeze()` and `Object.seal()`?

```js
// Object.freeze() — completely immutable
const frozen = Object.freeze({ a: 1, b: { c: 2 } });
frozen.a = 99; // ❌ Silently ignored (strict mode throws TypeError)
frozen.d = 4; // ❌ Cannot add
delete frozen.a; // ❌ Cannot delete
console.log(frozen.a); // 1 — unchanged

// Note: freeze is SHALLOW — nested objects can still change!
frozen.b.c = 99; // ✅ This WORKS (shallow freeze)
console.log(frozen.b.c); // 99

// Deep freeze (recursive)
function deepFreeze(obj) {
  Object.keys(obj).forEach((key) => {
    if (typeof obj[key] === "object" && obj[key] !== null) {
      deepFreeze(obj[key]);
    }
  });
  return Object.freeze(obj);
}

// Object.seal() — no add/delete, but can modify existing
const sealed = Object.seal({ x: 1, y: 2 });
sealed.x = 99; // ✅ Can modify existing
sealed.z = 3; // ❌ Cannot add new
delete sealed.x; // ❌ Cannot delete
console.log(sealed.x); // 99

// Checking state
Object.isFrozen(frozen); // true
Object.isSealed(sealed); // true
```

---

### 26. How do you merge two objects?

```js
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 }; // 'b' exists in both

// 1. Spread operator (most common, ES6+)
// Last object wins for duplicate keys
const merged = { ...obj1, ...obj2 };
console.log(merged); // { a: 1, b: 3, c: 4 }

// 2. Object.assign()
const merged2 = Object.assign({}, obj1, obj2);
// { a: 1, b: 3, c: 4 }

// Mutates first argument! So always pass empty object first
Object.assign(obj1, obj2); // Mutates obj1 ❌

// 3. For DEEP merge, spread only does shallow
const user = { name: "Alice", prefs: { theme: "dark" } };
const updates = { prefs: { lang: "en" } };

const shallowMerge = { ...user, ...updates };
// prefs: { lang: "en" } — theme is LOST!

// Deep merge manually or with libraries (lodash _.merge)
function deepMerge(target, source) {
  const result = { ...target };
  for (const key in source) {
    if (
      source[key] &&
      typeof source[key] === "object" &&
      !Array.isArray(source[key])
    ) {
      result[key] = deepMerge(target[key] || {}, source[key]);
    } else {
      result[key] = source[key];
    }
  }
  return result;
}
```

---

### 27. What is the difference between `Object.keys()` and `Object.values()`?

```js
const student = {
  name: "Alice",
  age: 20,
  grade: "A",
};

// Object.keys() — returns array of own enumerable property NAMES
Object.keys(student); // ["name", "age", "grade"]

// Object.values() — returns array of own enumerable property VALUES
Object.values(student); // ["Alice", 20, "A"]

// Object.entries() — returns array of [key, value] pairs
Object.entries(student); // [["name","Alice"], ["age",20], ["grade","A"]]

// Practical use cases
// Sum all values in an object
const scores = { math: 90, science: 85, english: 92 };
const total = Object.values(scores).reduce((sum, score) => sum + score, 0); // 267

// Convert object to sorted entries
const sorted = Object.entries(scores).sort(([, a], [, b]) => b - a);
// [["english", 92], ["math", 90], ["science", 85]]

// Check if a value exists
Object.values(student).includes("Alice"); // true
```

---

### 28. How do you deep clone an object?

```js
const original = {
  name: "Alice",
  age: 25,
  address: { city: "Mumbai", zip: "400001" },
  hobbies: ["reading", "coding"],
  dob: new Date("1999-01-01"),
  greet: function () {
    return "Hello";
  },
};

// Method 1: JSON.parse + JSON.stringify (simple but limited)
const clone1 = JSON.parse(JSON.stringify(original));
// ✅ Works for plain objects/arrays/primitives
// ❌ Loses: functions, undefined values, Date (becomes string), Map, Set, RegExp

// Method 2: structuredClone (RECOMMENDED — modern JS)
const clone2 = structuredClone(original);
// ✅ Handles: Date, Map, Set, RegExp, ArrayBuffer, circular references
// ❌ Cannot clone: functions, DOM nodes, class instances with methods

// Method 3: Recursive manual clone
function deepClone(obj) {
  if (obj === null || typeof obj !== "object") return obj;
  if (obj instanceof Date) return new Date(obj.getTime());
  if (Array.isArray(obj)) return obj.map(deepClone);

  const cloned = {};
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloned[key] = deepClone(obj[key]);
    }
  }
  return cloned;
}

// Method 4: Lodash (production-grade)
// const clone4 = _.cloneDeep(original);

// Verify independence
const clone = structuredClone(original);
clone.address.city = "Delhi";
console.log(original.address.city); // "Mumbai" ✅ Not affected
```

---

### 29. What are getters and setters in JavaScript?

**Getters and setters** are special methods that allow you to define **computed properties** and **control how object properties are read and written**. They provide a way to run custom logic when accessing or assigning property values, without the caller knowing they're calling a function.

A **getter** (`get`) is a method that is called automatically when you **read** a property. A **setter** (`set`) is a method that is called automatically when you **write** to a property. From the outside, they look like regular property access (`obj.name`) rather than method calls (`obj.getName()`), which makes for a cleaner API.

```js
// In object literals
const person = {
  _firstName: "Alice",
  _lastName: "Smith",

  get fullName() {
    return `${this._firstName} ${this._lastName}`;
  },

  set fullName(value) {
    const parts = value.split(" ");
    if (parts.length !== 2)
      throw new Error("Please provide first and last name");
    this._firstName = parts[0];
    this._lastName = parts[1];
  },
};

console.log(person.fullName); // "Alice Smith" — getter called
person.fullName = "Bob Jones"; // setter called
console.log(person.fullName); // "Bob Jones"

// In classes (most common usage)
class Temperature {
  constructor(celsius) {
    this._celsius = celsius;
  }

  get celsius() {
    return this._celsius;
  }
  get fahrenheit() {
    return (this._celsius * 9) / 5 + 32;
  }
  get kelvin() {
    return this._celsius + 273.15;
  }

  set celsius(value) {
    if (value < -273.15)
      throw new RangeError("Temperature below absolute zero!");
    this._celsius = value;
  }
}

const temp = new Temperature(100);
console.log(temp.celsius); // 100
console.log(temp.fahrenheit); // 212
console.log(temp.kelvin); // 373.15

temp.celsius = -300; // ❌ RangeError thrown
```

---

### 30. What is `JSON.stringify()` and `JSON.parse()` used for?

**JSON (JavaScript Object Notation)** is a lightweight data interchange format. JavaScript provides two built-in methods for working with JSON:

**`JSON.stringify(value, replacer, space)`** converts a JavaScript value (object, array, primitive) into a **JSON string**. This is used when you need to send data over a network (API requests), store data in localStorage/sessionStorage, or save data to a file.

**`JSON.parse(text, reviver)`** converts a **JSON string back into a JavaScript value**. This is used when you receive data from an API or read it from storage.

```js
const user = {
  name: "Alice",
  age: 25,
  hobbies: ["reading", "coding"],
  address: { city: "Mumbai" },
};

// Stringify — object → string
const jsonString = JSON.stringify(user);
console.log(jsonString);
// '{"name":"Alice","age":25,"hobbies":["reading","coding"],"address":{"city":"Mumbai"}}'

// With pretty printing (space argument)
const pretty = JSON.stringify(user, null, 2);
/*
{
  "name": "Alice",
  "age": 25,
  ...
}
*/

// With replacer (filter fields)
const filtered = JSON.stringify(user, ["name", "age"]);
// '{"name":"Alice","age":25}'

// Parse — string → object
const parsed = JSON.parse(jsonString);
console.log(parsed.name); // "Alice"
console.log(parsed.hobbies[0]); // "reading"
console.log(typeof parsed); // "object"

// Reviver function
const withDate = '{"name":"Alice","createdAt":"2024-01-01T00:00:00.000Z"}';
const obj = JSON.parse(withDate, (key, value) => {
  if (key === "createdAt") return new Date(value); // Convert string → Date
  return value;
});
console.log(obj.createdAt instanceof Date); // true ✅

// What JSON.stringify ignores (cannot represent in JSON)
const complex = {
  fn: function () {}, // → omitted (functions can't be JSON)
  undef: undefined, // → omitted
  sym: Symbol("key"), // → omitted
  date: new Date(), // → converted to string
  nan: NaN, // → null
  inf: Infinity, // → null
};
console.log(JSON.stringify(complex));
// '{"date":"2024-01-01T00:00:00.000Z","nan":null,"inf":null}'

// localStorage use case
localStorage.setItem("user", JSON.stringify(user));
const storedUser = JSON.parse(localStorage.getItem("user"));
```

---

### 31. What is prototype inheritance in JavaScript?

**Prototype inheritance** is the mechanism by which JavaScript objects can **inherit properties and methods from other objects**. Every JavaScript object has an internal property called `[[Prototype]]` (accessible via `__proto__` or `Object.getPrototypeOf()`), which is a reference to another object — its prototype.

When you try to access a property on an object, JavaScript first looks at the object itself. If not found, it looks at its prototype, then the prototype's prototype, and so on — up the **prototype chain** — until it finds the property or reaches `null` (the end of the chain).

```js
// ---- PROTOTYPE BASICS ----
const animal = {
  breathe() {
    console.log(`${this.name} is breathing`);
  },
};

const dog = {
  name: "Rex",
  bark() {
    console.log(`${this.name} says: Woof!`);
  },
};

// Set dog's prototype to animal
Object.setPrototypeOf(dog, animal);

dog.bark(); // "Rex says: Woof!" — found on dog
dog.breathe(); // "Rex is breathing" — found on prototype chain (animal)

// ---- CONSTRUCTOR FUNCTION ----
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Methods on prototype — shared across all instances (memory efficient)
Person.prototype.greet = function () {
  console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
};

const alice = new Person("Alice", 25);
const bob = new Person("Bob", 30);

alice.greet(); // Hi, I'm Alice and I'm 25 years old.
bob.greet(); // Hi, I'm Bob and I'm 30 years old.

// Both instances share the same greet function (not duplicated in memory)
console.log(alice.greet === bob.greet); // true

// ---- ES6 CLASS SYNTAX (syntactic sugar over prototypes) ----
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }

  speak() {
    console.log(`${this.name} barks!`); // Override parent method
  }

  fetch() {
    console.log(`${this.name} fetches the ball.`);
  }
}

const rex = new Dog("Rex", "Labrador");
rex.speak(); // "Rex barks!" (overridden)
rex.fetch(); // "Rex fetches the ball."

// Prototype chain: rex → Dog.prototype → Animal.prototype → Object.prototype → null
console.log(rex instanceof Dog); // true
console.log(rex instanceof Animal); // true
```

---

### 32. What is the difference between object literal and constructor function?

```js
// ---- OBJECT LITERAL ----
// Best for creating a single, one-off object
const car1 = {
  make: "Toyota",
  model: "Camry",
  year: 2023,
  describe() {
    return `${this.year} ${this.make} ${this.model}`;
  }
};
console.log(car1.describe()); // "2023 Toyota Camry"

// Problem: creating multiple similar objects requires copying code
const car2 = { make: "Honda", model: "Civic", year: 2022, describe() { ... } };
const car3 = { make: "Ford",  model: "Focus", year: 2021, describe() { ... } };
// ❌ Code duplication, not scalable

// ---- CONSTRUCTOR FUNCTION ----
// Best for creating multiple instances with the same structure
function Car(make, model, year) {
  this.make  = make;
  this.model = model;
  this.year  = year;
}

Car.prototype.describe = function () {
  return `${this.year} ${this.make} ${this.model}`;
};

const myCar   = new Car("Toyota", "Camry", 2023);
const yourCar = new Car("Honda", "Civic", 2022);
console.log(myCar.describe());   // "2023 Toyota Camry"
console.log(yourCar.describe()); // "2022 Honda Civic"

// ---- ES6 CLASS (modern, recommended) ----
class Vehicle {
  constructor(make, model, year) {
    this.make  = make;
    this.model = model;
    this.year  = year;
  }

  describe() {
    return `${this.year} ${this.make} ${this.model}`;
  }

  static compare(v1, v2) {
    return v1.year - v2.year; // static method — belongs to class, not instance
  }
}

class ElectricCar extends Vehicle {
  constructor(make, model, year, range) {
    super(make, model, year);
    this.range = range;
  }

  describe() {
    return `${super.describe()} (Electric, ${this.range}km range)`;
  }
}

const tesla = new ElectricCar("Tesla", "Model 3", 2023, 500);
console.log(tesla.describe()); // "2023 Tesla Model 3 (Electric, 500km range)"
```

---

### 33. How does optional chaining work?

**Optional chaining** (`?.`) is a syntax feature introduced in ES2020 that allows you to safely access **deeply nested properties** of an object without having to explicitly check if each reference in the chain is `null` or `undefined`. If any part of the chain is `null` or `undefined`, the expression short-circuits and returns `undefined` instead of throwing a `TypeError`.

Without optional chaining, accessing nested properties required multiple null checks or try-catch blocks. Optional chaining makes the code much cleaner and more concise.

```js
// ---- WITHOUT optional chaining (old way) ----
const user = null;

// Would throw TypeError: Cannot read properties of null
// console.log(user.address.city);

// Had to check each level manually
const city1 = user && user.address && user.address.city;
console.log(city1); // null (safe, but verbose)

// ---- WITH optional chaining ----
const city2 = user?.address?.city;
console.log(city2); // undefined (safe and clean!)

// ---- Real-world examples ----
const apiResponse = {
  data: {
    user: {
      profile: {
        name: "Alice",
        social: null,
      },
    },
  },
};

// Property access
const name = apiResponse?.data?.user?.profile?.name; // "Alice"
const twitter = apiResponse?.data?.user?.profile?.social?.twitter; // undefined (no error!)
const instagram = apiResponse?.data?.user?.profile?.social?.instagram; // undefined

// Method calls
const result = apiResponse?.data?.user?.profile?.getAge?.(); // undefined (if method doesn't exist)

// Array access
const users = null;
console.log(users?.[0]?.name); // undefined (safe)

const arr = [{ name: "Alice" }, { name: "Bob" }];
console.log(arr?.[0]?.name); // "Alice"
console.log(arr?.[5]?.name); // undefined (index out of bounds, no error)

// Combining with nullish coalescing (??) for defaults
const displayName = apiResponse?.data?.user?.profile?.name ?? "Anonymous";
console.log(displayName); // "Alice" (or "Anonymous" if null/undefined)

const displayCity =
  apiResponse?.data?.user?.address?.city ?? "Location unknown";
console.log(displayCity); // "Location unknown"

// Optional chaining with function calls
function getUser(id) {
  const users = {
    1: {
      name: "Alice",
      greet() {
        return "Hello!";
      },
    },
  };
  return users[id]; // May return undefined
}

const user1 = getUser(1);
const user2 = getUser(99); // undefined

console.log(user1?.greet()); // "Hello!"
console.log(user2?.greet()); // undefined (no error!)
console.log(user2?.greet() ?? "User not found"); // "User not found"
```

---

### 34. How do you handle errors using `try-catch`?

**Error handling** is a critical part of writing robust, production-ready JavaScript code. The `try...catch...finally` statement provides a structured way to handle both synchronous and asynchronous errors gracefully.

**`try` block** — Contains the code that might throw an error. If an error occurs, execution jumps immediately to the `catch` block.

**`catch` block** — Receives the error object as a parameter. The error object has `name`, `message`, and `stack` properties. You can have conditional catch logic for different error types.

**`finally` block** — Always executes regardless of whether an error occurred, whether it was caught, or even if there's a `return` in the `try` or `catch`. Used for cleanup operations (closing connections, clearing resources).

**Custom Errors** — You can extend the built-in `Error` class to create custom error types for better error identification and handling.

```js
// Basic try-catch
function divide(a, b) {
  try {
    if (typeof a !== "number" || typeof b !== "number") {
      throw new TypeError("Both arguments must be numbers");
    }
    if (b === 0) {
      throw new RangeError("Cannot divide by zero");
    }
    return a / b;
  } catch (error) {
    // Handle different error types
    if (error instanceof TypeError) {
      console.error("Type Error:", error.message);
    } else if (error instanceof RangeError) {
      console.error("Range Error:", error.message);
    } else {
      console.error("Unknown error:", error.message);
    }
    return null;
  } finally {
    console.log("divide() function executed"); // Always runs
  }
}

console.log(divide(10, 2)); // 5
console.log(divide(10, 0)); // null, logs "Range Error: Cannot divide by zero"
console.log(divide("a", 2)); // null, logs "Type Error..."

// Custom Error Classes
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

class NetworkError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.name = "NetworkError";
    this.statusCode = statusCode;
  }
}

function validateUser(user) {
  if (!user.name) throw new ValidationError("Name is required", "name");
  if (!user.email) throw new ValidationError("Email is required", "email");
  if (user.age < 0) throw new ValidationError("Age must be positive", "age");
}

try {
  validateUser({ name: "", email: "alice@test.com", age: 25 });
} catch (error) {
  if (error instanceof ValidationError) {
    console.error(
      `Validation failed on field '${error.field}': ${error.message}`,
    );
  }
}

// Async error handling
async function fetchUser(id) {
  try {
    const response = await fetch(`https://api.example.com/users/${id}`);

    if (!response.ok) {
      throw new NetworkError(`Request failed`, response.status);
    }

    const data = await response.json();
    return data;
  } catch (error) {
    if (error instanceof NetworkError) {
      console.error(`Network error ${error.statusCode}: ${error.message}`);
    } else {
      console.error("Unexpected error:", error.message);
    }
    throw error; // Re-throw to let caller handle it too
  } finally {
    console.log("Fetch attempt completed");
  }
}
```

---

### 35. What are JavaScript generators and iterators?

An **iterator** is an object that follows the **iterator protocol** — it has a `next()` method that returns an object with two properties: `value` (the current value) and `done` (a boolean indicating if iteration is complete).

A **generator** is a special function (declared with `function*`) that can **pause its execution and resume later**, yielding multiple values over time. When you call a generator function, it doesn't execute immediately — it returns a **generator object** that is itself an iterator. Each call to `next()` runs the generator until the next `yield` statement.

Generators are incredibly powerful for handling **infinite sequences**, **lazy evaluation**, **async flows**, and **custom iteration**.

```js
// ---- CUSTOM ITERATOR ----
function createRangeIterator(start, end) {
  let current = start;
  return {
    next() {
      if (current <= end) {
        return { value: current++, done: false };
      }
      return { value: undefined, done: true };
    },
  };
}

const range = createRangeIterator(1, 5);
console.log(range.next()); // { value: 1, done: false }
console.log(range.next()); // { value: 2, done: false }
console.log(range.next()); // { value: 5, done: false }
console.log(range.next()); // { value: undefined, done: true }

// ---- GENERATOR FUNCTION ----
function* generateRange(start, end) {
  for (let i = start; i <= end; i++) {
    yield i; // Pause here, return i
  }
  // When loop ends, done: true automatically
}

const gen = generateRange(1, 5);
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }

// Use in for...of loop
for (const num of generateRange(1, 5)) {
  console.log(num); // 1, 2, 3, 4, 5
}

// Spread generator
const nums = [...generateRange(1, 5)];
console.log(nums); // [1, 2, 3, 4, 5]

// ---- INFINITE GENERATOR (lazy evaluation) ----
function* infiniteSequence() {
  let n = 1;
  while (true) {
    yield n++;
  }
}

const inf = infiniteSequence();
console.log(inf.next().value); // 1
console.log(inf.next().value); // 2
console.log(inf.next().value); // 3
// Never exhausted, generates on demand!

// Take first N values from infinite generator
function take(gen, n) {
  const result = [];
  for (const val of gen) {
    result.push(val);
    if (result.length >= n) break;
  }
  return result;
}

console.log(take(infiniteSequence(), 5)); // [1, 2, 3, 4, 5]

// ---- UNIQUE ID GENERATOR ----
function* idGenerator(prefix = "ID") {
  let id = 1;
  while (true) {
    yield `${prefix}-${String(id++).padStart(4, "0")}`;
  }
}

const userId = idGenerator("USER");
console.log(userId.next().value); // "USER-0001"
console.log(userId.next().value); // "USER-0002"
```

---

## Advanced JavaScript

---

### 36. How do you create a custom event?

```js
// Create a custom event
const loginEvent = new CustomEvent("userLoggedIn", {
  bubbles: true, // Event bubbles up the DOM
  cancelable: true,
  detail: {
    // Custom data
    userId: 123,
    username: "Alice",
    role: "admin",
    timestamp: new Date(),
  },
});

// Listen for the custom event
document.addEventListener("userLoggedIn", function (event) {
  const { userId, username, role } = event.detail;
  console.log(`User logged in: ${username} (ID: ${userId}, Role: ${role})`);
  updateNavbar(username);
  loadUserDashboard(userId);
});

// Dispatch the event
document.dispatchEvent(loginEvent);

// Real-world use case: Custom EventEmitter (pub/sub pattern)
class EventEmitter {
  constructor() {
    this.events = {};
  }

  on(event, listener) {
    if (!this.events[event]) this.events[event] = [];
    this.events[event].push(listener);
    return this; // For chaining
  }

  off(event, listener) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter((l) => l !== listener);
    }
  }

  emit(event, ...args) {
    if (this.events[event]) {
      this.events[event].forEach((listener) => listener(...args));
    }
  }
}

const emitter = new EventEmitter();
emitter.on("data", (data) => console.log("Received:", data));
emitter.on("error", (err) => console.error("Error:", err));
emitter.emit("data", { name: "Alice" }); // "Received: {name: 'Alice'}"
```

---

### 37. What is `setTimeout` and `setInterval`? How are they different?

**`setTimeout`** schedules a function to be executed **once** after a specified delay in milliseconds. It does not block the rest of the code — the delay is handled by the browser's timer API.

**`setInterval`** schedules a function to be executed **repeatedly**, at every specified interval, until it is explicitly cancelled.

Both return a **timer ID** (a positive integer) that can be used to cancel the timer with `clearTimeout()` or `clearInterval()` respectively.

```js
// setTimeout — runs ONCE after delay
console.log("Before timeout");

const timeoutId = setTimeout(() => {
  console.log("This runs after 2 seconds");
}, 2000);

console.log("After timeout setup"); // Runs immediately

// setInterval — runs REPEATEDLY
let count = 0;
const intervalId = setInterval(() => {
  count++;
  console.log(`Tick: ${count}`);

  if (count >= 5) {
    clearInterval(intervalId); // Stop after 5 ticks
    console.log("Interval stopped");
  }
}, 1000);

// Important: setTimeout(fn, 0) doesn't run instantly
console.log("A");
setTimeout(() => console.log("B"), 0);
console.log("C");
// Output: A, C, B (setTimeout goes to macrotask queue)

// Recursive setTimeout vs setInterval
// Recursive setTimeout gives guaranteed delay BETWEEN executions
function recursiveTimeout() {
  doSomething();
  setTimeout(recursiveTimeout, 1000); // 1s AFTER doSomething() completes
}

// setInterval fires every 1s regardless of how long doSomething() takes
// If doSomething() takes 1.5s, calls can overlap!
```

---

### 38. How do you cancel a running `setTimeout`?

```js
// cancelTimeout using the returned ID
const id = setTimeout(() => {
  console.log("This will NOT run");
}, 5000);

// Cancel it before it fires
clearTimeout(id);
console.log("Timeout cancelled!");

// Practical pattern: debounce
function debounce(fn, delay) {
  let timeoutId;
  return function (...args) {
    clearTimeout(timeoutId); // Cancel previous timer
    timeoutId = setTimeout(() => fn.apply(this, args), delay); // Start new one
  };
}

const searchHandler = debounce((query) => {
  console.log("Searching for:", query);
}, 500);

// Called rapidly — only the last call fires after 500ms of silence
searchHandler("j");
searchHandler("ja");
searchHandler("jav");
searchHandler("java"); // Only this one executes (after 500ms)
```

---

### 39. What is localStorage, sessionStorage, and cookies?

These are three different browser storage mechanisms for persisting data on the client side, each with different scopes, capacities, and use cases.

**`localStorage`** stores data with **no expiration date**. Data persists even when the browser is closed and reopened. It is accessible only via JavaScript on the same origin (domain + protocol + port). Capacity is typically 5–10 MB.

**`sessionStorage`** is similar to localStorage, but data is cleared when the **browser tab or window is closed**. Each tab has its own sessionStorage — data is NOT shared between tabs. Capacity is typically 5–10 MB.

**Cookies** are small text files (up to **4 KB**) stored by the browser. They can have an **expiration date**. Unlike localStorage and sessionStorage, cookies are automatically sent to the server with every HTTP request for the same domain, making them essential for **server-side sessions and authentication**. They can be accessed both from JavaScript and from the server.

```js
// ---- localStorage ----
// setItem, getItem, removeItem, clear
localStorage.setItem("theme", "dark");
localStorage.setItem("language", "en");
localStorage.setItem("user", JSON.stringify({ id: 1, name: "Alice" }));

console.log(localStorage.getItem("theme")); // "dark"
console.log(JSON.parse(localStorage.getItem("user")).name); // "Alice"
console.log(localStorage.length); // 3

localStorage.removeItem("language");
// localStorage.clear(); // Removes all items

// ---- sessionStorage ----
// Same API as localStorage
sessionStorage.setItem("cart", JSON.stringify([{ id: 1, qty: 2 }]));
const cart = JSON.parse(sessionStorage.getItem("cart"));
// Cleared when tab closes

// ---- Cookies ----
// Set a cookie with expiry
document.cookie =
  "username=Alice; expires=Fri, 31 Dec 2025 23:59:59 GMT; path=/";
document.cookie = "token=abc123; max-age=3600; secure; SameSite=Strict"; // 1 hour

// Read cookies (returns all cookies as one string)
console.log(document.cookie); // "username=Alice; token=abc123"

// Parse cookies into an object
function getCookies() {
  return document.cookie.split("; ").reduce((acc, cookie) => {
    const [key, value] = cookie.split("=");
    acc[key] = value;
    return acc;
  }, {});
}

// Delete a cookie by setting its expiry to the past
document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";
```

| Feature        | localStorage | sessionStorage | Cookies                |
| -------------- | ------------ | -------------- | ---------------------- |
| Expires        | Never        | Tab closes     | Custom / on close      |
| Max size       | ~5–10 MB     | ~5–10 MB       | ~4 KB                  |
| Sent to server | ❌ No        | ❌ No          | ✅ Yes (every request) |
| Accessible     | Client only  | Client only    | Client + Server        |
| Shared tabs    | ✅ Yes       | ❌ No          | ✅ Yes                 |

---

### 40. How does `fetch()` work and how does it compare to Axios?

**`fetch()`** is a modern, built-in browser API for making HTTP requests. It returns a **Promise** that resolves with a `Response` object. One important caveat: `fetch()` does NOT automatically throw an error for HTTP error status codes (like 404 or 500) — you must manually check `response.ok`.

**Axios** is a third-party library (available on npm) that is built on top of `XMLHttpRequest`. It provides additional features out of the box like automatic JSON parsing, better error handling (throws for 4xx/5xx), request/response interceptors, request cancellation, timeout configuration, and upload progress tracking.

```js
// ---- fetch() ----
async function getUser(id) {
  try {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/users/${id}`,
      {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          Authorization: "Bearer your-token",
        },
      },
    );

    // fetch does NOT throw on 4xx/5xx — must check manually!
    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status} ${response.statusText}`);
    }

    const user = await response.json(); // Must manually parse JSON
    return user;
  } catch (error) {
    console.error("Fetch failed:", error.message);
  }
}

// POST with fetch
async function createUser(userData) {
  const response = await fetch("https://api.example.com/users", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(userData), // Must manually stringify
  });
  return response.json();
}

// ---- Axios ----
// npm install axios
import axios from "axios";

async function getUserAxios(id) {
  try {
    // Axios auto-parses JSON, auto-throws on 4xx/5xx
    const { data } = await axios.get(`https://api.example.com/users/${id}`, {
      timeout: 5000,
      headers: { Authorization: "Bearer your-token" },
    });
    return data;
  } catch (error) {
    if (error.response) {
      // Server responded with error status
      console.error(`Error ${error.response.status}:`, error.response.data);
    } else if (error.request) {
      // No response received
      console.error("No response received");
    } else {
      console.error("Request setup error:", error.message);
    }
  }
}

// Axios interceptors — run before every request/response
axios.interceptors.request.use((config) => {
  config.headers.Authorization = `Bearer ${localStorage.getItem("token")}`;
  return config;
});

axios.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response.status === 401) {
      // Auto logout on 401
      localStorage.removeItem("token");
      window.location.href = "/login";
    }
    return Promise.reject(error);
  },
);
```

| Feature              | `fetch()`             | `Axios`                 |
| -------------------- | --------------------- | ----------------------- |
| Built-in             | ✅ Yes                | ❌ npm install needed   |
| Auto JSON parse      | ❌ Manual `.json()`   | ✅ Automatic            |
| Error on 4xx/5xx     | ❌ Must check `ok`    | ✅ Throws automatically |
| Request timeout      | ❌ Manual (AbortCtrl) | ✅ Built-in             |
| Interceptors         | ❌ No                 | ✅ Yes                  |
| Upload progress      | ❌ No                 | ✅ Yes                  |
| Request cancellation | ✅ AbortController    | ✅ CancelToken          |
| Node.js support      | ✅ (Node 18+)         | ✅ Works everywhere     |

---

## DOM & Browser APIs

---

### 41. What is event bubbling and event capturing?

When an event occurs on a DOM element, it doesn't just trigger on that element. It goes through a **propagation process** in three phases:

1. **Capture phase** — The event travels from the root (`window`) down through all ancestors toward the target element.
2. **Target phase** — The event reaches the actual target element that was clicked/interacted with.
3. **Bubble phase** — The event travels back up from the target through all ancestors to the root.

**Event bubbling** (default behavior) means the event propagates **upward** — from child to parent to grandparent.
**Event capturing** means the event propagates **downward** — from root to the target.

```js
// HTML structure: body > div#outer > div#inner > button#btn

document.querySelector("#btn").addEventListener("click", () => {
  console.log("Button clicked");
});

document.querySelector("#inner").addEventListener("click", () => {
  console.log("Inner div");
});

document.querySelector("#outer").addEventListener("click", () => {
  console.log("Outer div");
});

document.body.addEventListener("click", () => {
  console.log("Body");
});

// Click on button — output (bubbling, default):
// Button clicked → Inner div → Outer div → Body

// Stop bubbling
document.querySelector("#btn").addEventListener("click", (e) => {
  e.stopPropagation(); // Event won't bubble up
  console.log("Button clicked — stops here");
});

// Capturing phase (add true as 3rd argument)
document.querySelector("#outer").addEventListener(
  "click",
  () => {
    console.log("Outer div — CAPTURE");
  },
  true,
); // Capturing!

// Click on button with both:
// Outer div — CAPTURE (capture phase first)
// Button clicked
// Inner div
// Outer div (bubble)
```

---

### 42. What is event delegation?

```js
// Instead of adding listeners to each list item individually:
// ❌ Bad approach — if 1000 items, 1000 listeners!
document.querySelectorAll(".task").forEach((task) => {
  task.addEventListener("click", handleTaskClick);
});

// ✅ Event delegation — ONE listener on the parent
document.querySelector("#taskList").addEventListener("click", function (e) {
  // Check which child was actually clicked
  if (e.target.classList.contains("task")) {
    console.log("Task clicked:", e.target.textContent);
    e.target.classList.toggle("completed");
  }

  if (e.target.classList.contains("delete-btn")) {
    e.target.closest(".task").remove();
  }
});

// Works for dynamically added elements too!
const newTask = document.createElement("li");
newTask.className = "task";
newTask.textContent = "New Dynamic Task";
document.querySelector("#taskList").appendChild(newTask);
// The delegation listener will automatically handle clicks on this new item ✅
```

---

### 43. What is the `this` keyword in JavaScript?

The value of `this` depends entirely on **how a function is called**, not where it is defined (except for arrow functions).

```js
// 1. Global context
console.log(this); // window (browser) / global (Node.js)

// 2. Regular function
function regular() {
  console.log(this); // window (non-strict) or undefined (strict mode)
}

// 3. Object method
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name); // "Alice" — 'this' is the object
  },
};
obj.greet(); // "Alice"

// Detaching method loses 'this'
const greetFn = obj.greet;
greetFn(); // undefined (or window.name)

// 4. Constructor function / class
function Person(name) {
  this.name = name; // 'this' is the new instance
}
const alice = new Person("Alice");
console.log(alice.name); // "Alice"

// 5. Arrow function — inherits 'this' from surrounding scope
const arrowObj = {
  name: "Bob",
  greet: () => {
    console.log(this.name); // undefined! Arrow uses outer (window) 'this'
  },
  greetCorrect() {
    const inner = () => console.log(this.name); // 'this' = arrowObj
    inner();
  },
};
arrowObj.greetCorrect(); // "Bob"

// 6. Explicit binding with call/apply/bind
function introduce() {
  console.log(this.name);
}
introduce.call({ name: "Carol" }); // "Carol"
```

---

### 44. What are WeakMap and WeakSet?

```js
// WeakMap — keys must be objects, weakly referenced
let obj1 = { id: 1, name: "Alice" };
let obj2 = { id: 2, name: "Bob" };

const weakMap = new WeakMap();
weakMap.set(obj1, { role: "admin", loginCount: 5 });
weakMap.set(obj2, { role: "user", loginCount: 1 });

console.log(weakMap.get(obj1)); // { role: "admin", loginCount: 5 }
console.log(weakMap.has(obj1)); // true

obj1 = null; // obj1 is now unreachable
// WeakMap entry for obj1 is automatically garbage collected ✅
// No memory leak!

// WeakSet — stores objects weakly
const visitedPages = new WeakSet();
let page = { url: "/home" };
visitedPages.add(page);

console.log(visitedPages.has(page)); // true
page = null; // Auto-removed from WeakSet ✅

// Practical use case: Tracking DOM elements without memory leaks
const processedElements = new WeakSet();

function processElement(el) {
  if (processedElements.has(el)) {
    console.log("Already processed");
    return;
  }
  // ... do processing ...
  processedElements.add(el);
}

// When DOM element is removed, it's auto-removed from WeakSet too ✅
```

---

### 45. What is the difference between `== null` check and optional chaining?

```js
const user = null;
const obj = { name: "Alice", address: null };

// Old style: explicit null/undefined check
if (user != null) {
  // Catches both null AND undefined
  console.log(user.name);
}

// Optional chaining: safe property access
console.log(user?.name); // undefined (no error)
console.log(obj?.address?.city); // undefined (address is null)

// Nullish coalescing (??) — default value for null or undefined
// (not the same as || which also catches 0, "", false)
const name = user?.name ?? "Guest"; // "Guest"
const count = 0 ?? 10; // 0 (not 10 — 0 is not null/undefined!)
const count2 = 0 || 10; // 10 (|| treats 0 as falsy)
```

---

## Bonus Questions

---

### 46. What are higher-order functions?

A **higher-order function (HOF)** is a function that either **takes one or more functions as arguments** or **returns a function** as its result (or both). Higher-order functions are a core concept in functional programming and are used extensively in JavaScript.

Built-in HOFs you use every day: `map`, `filter`, `reduce`, `forEach`, `sort`, `setTimeout`, `addEventListener`.

```js
// ---- HOF that takes a function as argument ----
function applyOperation(numbers, operation) {
  return numbers.map(operation);
}

const numbers = [1, 2, 3, 4, 5];
const doubled = applyOperation(numbers, (n) => n * 2); // [2, 4, 6, 8, 10]
const squared = applyOperation(numbers, (n) => n ** 2); // [1, 4, 9, 16, 25]
const asString = applyOperation(numbers, (n) => `#${n}`); // ["#1", "#2", ...]

// ---- HOF that returns a function ----
function makeMultiplier(factor) {
  return function (number) {
    return number * factor;
  };
}

const double = makeMultiplier(2);
const triple = makeMultiplier(3);
const tenX = makeMultiplier(10);

console.log(double(5)); // 10
console.log(triple(5)); // 15
console.log(tenX(5)); // 50

// ---- Function composition ----
const compose =
  (...fns) =>
  (x) =>
    fns.reduceRight((acc, fn) => fn(acc), x);
const pipe =
  (...fns) =>
  (x) =>
    fns.reduce((acc, fn) => fn(acc), x);

const addOne = (x) => x + 1;
const double2 = (x) => x * 2;
const square = (x) => x * x;

const transform = pipe(addOne, double2, square);
// (5 + 1) = 6 → 6 * 2 = 12 → 12 * 12 = 144
console.log(transform(5)); // 144
```

---

### 47. What is memoization?

**Memoization** is an optimization technique where the results of expensive function calls are **cached** based on their input arguments. When the same function is called with the same arguments again, the cached result is returned immediately instead of recalculating.

```js
// ---- Basic memoization ----
function memoize(fn) {
  const cache = new Map();

  return function (...args) {
    const key = JSON.stringify(args);

    if (cache.has(key)) {
      console.log(`Cache hit for args: ${key}`);
      return cache.get(key);
    }

    console.log(`Computing for args: ${key}`);
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Expensive Fibonacci without memoization — O(2^n)
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}

// Memoized Fibonacci — O(n)
const memoFib = memoize(function fib(n) {
  if (n <= 1) return n;
  return memoFib(n - 1) + memoFib(n - 2);
});

console.time("without memo");
fib(40); // Takes seconds
console.timeEnd("without memo");

console.time("with memo");
memoFib(40); // Nearly instant
console.timeEnd("with memo");

// React useMemo (for components)
import { useMemo } from "react";

function ExpensiveComponent({ data }) {
  const processedData = useMemo(() => {
    return data
      .filter((item) => item.active)
      .sort((a, b) => a.name.localeCompare(b.name));
  }, [data]); // Only recomputes when 'data' changes

  return (
    <div>
      {processedData.map((item) => (
        <p key={item.id}>{item.name}</p>
      ))}
    </div>
  );
}
```

---

### 48. What is the difference between `Promise.all`, `Promise.race`, `Promise.any`, and `Promise.allSettled`?

```js
const p1 = new Promise((resolve) => setTimeout(() => resolve("First"), 1000));
const p2 = new Promise((resolve) => setTimeout(() => resolve("Second"), 2000));
const p3 = new Promise((_, reject) => setTimeout(() => reject("Error!"), 500));

// Promise.all — ALL must resolve; ONE rejection fails the whole thing
Promise.all([p1, p2])
  .then((results) => console.log(results)) // ["First", "Second"] after 2s
  .catch((err) => console.error(err));

Promise.all([p1, p3])
  .then((results) => console.log(results))
  .catch((err) => console.error(err)); // "Error!" after 500ms (p3 rejects)

// Promise.allSettled — waits for ALL, never rejects, gives full picture
Promise.allSettled([p1, p2, p3]).then((results) => {
  results.forEach((result) => {
    if (result.status === "fulfilled") {
      console.log("✅ Success:", result.value);
    } else {
      console.log("❌ Failed:", result.reason);
    }
  });
});

// Promise.race — resolves/rejects as soon as FIRST settles
Promise.race([p1, p2, p3])
  .then((val) => console.log("First:", val))
  .catch((err) => console.error("First error:", err)); // "Error!" after 500ms

// Timeout pattern with race
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error(`Timed out after ${ms}ms`)), ms),
  );
  return Promise.race([promise, timeout]);
}

// Promise.any — first FULFILLMENT (ignores rejections unless ALL reject)
Promise.any([p1, p2, p3])
  .then((val) => console.log("Any:", val)) // "First" after 1s (ignores p3 rejection)
  .catch((err) => console.error(err)); // AggregateError only if ALL reject
```

---

### 49. What is the difference between `setTimeout(fn, 0)` and `queueMicrotask(fn)`?

```js
console.log("1 - Sync start");

setTimeout(() => console.log("2 - Macrotask (setTimeout 0)"), 0);

Promise.resolve().then(() => console.log("3 - Microtask (Promise)"));

queueMicrotask(() => console.log("4 - Microtask (queueMicrotask)"));

requestAnimationFrame(() => console.log("5 - Animation frame")); // Before next repaint

console.log("6 - Sync end");

// Output order:
// 1 - Sync start
// 6 - Sync end
// 3 - Microtask (Promise)       ← Microtask queue (drains completely first)
// 4 - Microtask (queueMicrotask)← Microtask queue
// 5 - Animation frame           ← Before repaint (browser only)
// 2 - Macrotask (setTimeout 0)  ← Macrotask queue (last)

// Rule: Microtasks drain completely before the next macrotask
// This means if microtasks add more microtasks, they ALL run before any macrotask!

Promise.resolve().then(() => {
  console.log("Microtask 1");
  Promise.resolve().then(() => console.log("Nested microtask!")); // Runs before setTimeout
});

setTimeout(() => console.log("Macrotask"), 0);
// Microtask 1 → Nested microtask → Macrotask
```

---

### 50. What are IIFE (Immediately Invoked Function Expressions)?

An **IIFE (Immediately Invoked Function Expression)** is a function that is **defined and called immediately** at the same time. It creates its own scope, which means variables declared inside it don't pollute the global scope.

IIFEs were heavily used before ES6 modules existed to encapsulate code and create private scopes. They are still useful today in certain patterns.

```js
// Basic IIFE syntax
(function () {
  const secret = "hidden value";
  console.log("IIFE runs immediately:", secret);
})(); // The () at the end invokes it

// Arrow function IIFE
(() => {
  console.log("Arrow IIFE");
})();

// IIFE with parameters
(function (name, greeting) {
  console.log(`${greeting}, ${name}!`);
})("Alice", "Hello");
// "Hello, Alice!"

// IIFE returning a value
const result = (function () {
  const x = 10;
  const y = 20;
  return x + y; // Returns value to the outside
})();
console.log(result); // 30

// Module pattern using IIFE — private state with public interface
const counter = (function () {
  let count = 0; // Private variable

  return {
    increment() {
      count++;
    },
    decrement() {
      count--;
    },
    getCount() {
      return count;
    },
    reset() {
      count = 0;
    },
  };
})();

counter.increment();
counter.increment();
counter.increment();
console.log(counter.getCount()); // 3
counter.decrement();
console.log(counter.getCount()); // 2
// console.log(counter.count);   // undefined — private!

// IIFE for async initialization
(async function () {
  try {
    const data = await fetch("/api/config").then((r) => r.json());
    initializeApp(data);
  } catch (err) {
    console.error("Init failed:", err);
  }
})();
```

---

_Good luck with your JavaScript interviews! 🚀_
_Remember: Understanding the WHY behind each concept is more important than memorizing the syntax._

<!-- # JavaScript Interview Questions & Answers

> A comprehensive guide covering General JS, Arrays, Objects, Async, DOM, and more.

---

## 📌 Table of Contents

1. [General JavaScript](#general-javascript)
2. [Array & Object Questions](#array--object-questions)
3. [Advanced JavaScript](#advanced-javascript)
4. [DOM & Browser APIs](#dom--browser-apis)
5. [Bonus Questions](#bonus-questions)

---

## General JavaScript

---

### 1. What are the differences between `let`, `const`, and `var`?

| Feature        | `var`           | `let`           | `const`         |
| -------------- | --------------- | --------------- | --------------- |
| Scope          | Function-scoped | Block-scoped    | Block-scoped    |
| Hoisting       | Yes (undefined) | Yes (TDZ error) | Yes (TDZ error) |
| Re-declaration | Allowed         | Not allowed     | Not allowed     |
| Re-assignment  | Allowed         | Allowed         | Not allowed     |

```js
var x = 1;
var x = 2; // ✅ allowed

let y = 1;
// let y = 2; ❌ SyntaxError

const z = 1;
// z = 2; ❌ TypeError
```

> **TDZ** = Temporal Dead Zone — the period between entering scope and the variable being initialized.

---

### 2. What is the difference between `null` and `undefined`?

- **`undefined`** — a variable has been declared but not yet assigned a value.
- **`null`** — an intentional absence of value, assigned explicitly.

```js
let a;
console.log(a); // undefined

let b = null;
console.log(b); // null

console.log(typeof undefined); // "undefined"
console.log(typeof null); // "object" (historical JS bug)

console.log(null == undefined); // true  (loose equality)
console.log(null === undefined); // false (strict equality)
```

---

### 3. What is the difference between `==` and `===`?

- **`==`** (loose equality) — compares values after **type coercion**.
- **`===`** (strict equality) — compares values **without** type coercion (both value and type must match).

```js
0 == "0"; // true  (string "0" is coerced to number 0)
0 === "0"; // false (number vs string)

null == undefined; // true
null === undefined; // false

false == 0; // true
false === 0; // false
```

> ✅ Always prefer `===` to avoid unexpected bugs.

---

### 4. What is type coercion in JavaScript?

Type coercion is the **automatic or implicit conversion** of values from one data type to another.

```js
// Implicit coercion
"5" + 2; // "52"  (number 2 coerced to string)
"5" - 2; // 3     (string "5" coerced to number)
true + 1; // 2     (true coerced to 1)
false + 1; // 1     (false coerced to 0)
null + 1; // 1     (null coerced to 0)

// Explicit coercion
Number("5"); // 5
String(5); // "5"
Boolean(0); // false
```

---

### 5. What are JavaScript closures? Provide an example.

A **closure** is a function that **remembers its outer scope** even after the outer function has finished executing.

```js
function makeCounter() {
  let count = 0; // private variable

  return function () {
    count++;
    return count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

> The inner function "closes over" the `count` variable. This is used for **data encapsulation**, **memoization**, and **factory functions**.

---

### 6. What is the event loop in JavaScript?

JavaScript is **single-threaded** but non-blocking. The event loop manages the execution of code, handling async callbacks.

**Order of execution:**

1. **Call Stack** — executes synchronous code.
2. **Web APIs** — handles async tasks (setTimeout, fetch, etc.).
3. **Callback Queue (Macrotask)** — stores callbacks like `setTimeout`.
4. **Microtask Queue** — stores Promises (`.then`, `async/await`) — runs **before** macrotasks.

```js
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve().then(() => console.log("3"));

console.log("4");

// Output: 1, 4, 3, 2
```

---

### 7. What are Promises, and how do they work?

A **Promise** represents the eventual result of an asynchronous operation.

**States:**

- `pending` → initial state
- `fulfilled` → operation succeeded
- `rejected` → operation failed

```js
const fetchData = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Data fetched!");
  } else {
    reject("Error occurred!");
  }
});

fetchData
  .then((result) => console.log(result)) // "Data fetched!"
  .catch((error) => console.error(error))
  .finally(() => console.log("Done"));
```

---

### 8. How does `async/await` work in JavaScript?

`async/await` is **syntactic sugar** over Promises, making async code look synchronous.

```js
async function getUser() {
  try {
    const response = await fetch("https://api.example.com/user/1");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Error:", error);
  }
}

getUser();
```

- `async` makes a function return a Promise.
- `await` pauses execution until the Promise resolves.

---

### 9. What is the difference between synchronous and asynchronous programming?

| Feature     | Synchronous            | Asynchronous                         |
| ----------- | ---------------------- | ------------------------------------ |
| Execution   | Line by line, blocking | Non-blocking, continues without wait |
| Example     | `console.log`, loops   | `setTimeout`, `fetch`, Promises      |
| Performance | Blocks UI/thread       | Keeps UI responsive                  |

```js
// Synchronous
console.log("A");
console.log("B"); // Waits for A to complete

// Asynchronous
console.log("A");
setTimeout(() => console.log("B"), 1000); // Doesn't block
console.log("C"); // Prints before B
```

---

### 10. What are JavaScript callbacks?

A **callback** is a function passed as an argument to another function and executed later.

```js
function greet(name, callback) {
  console.log("Hello, " + name);
  callback();
}

function sayBye() {
  console.log("Goodbye!");
}

greet("Alice", sayBye);
// Hello, Alice
// Goodbye!
```

> Overuse of callbacks leads to **"Callback Hell"** — deeply nested callbacks that are hard to read. Promises and async/await solve this.

---

### 11. What is hoisting in JavaScript?

**Hoisting** is JavaScript's behavior of moving declarations (not initializations) to the top of their scope before execution.

```js
// Variable hoisting
console.log(a); // undefined (not an error)
var a = 5;

// Function hoisting
greet(); // "Hello!" ✅
function greet() {
  console.log("Hello!");
}

// let/const — NOT hoisted (TDZ)
console.log(b); // ❌ ReferenceError
let b = 10;
```

---

### 12. What is the difference between `call`, `apply`, and `bind`?

All three are used to set the `this` context of a function.

| Method  | Invocation       | Arguments       |
| ------- | ---------------- | --------------- |
| `call`  | Immediately      | Comma-separated |
| `apply` | Immediately      | Array           |
| `bind`  | Returns new func | Comma-separated |

```js
function introduce(city, country) {
  console.log(`I'm ${this.name} from ${city}, ${country}`);
}

const person = { name: "Alice" };

introduce.call(person, "Mumbai", "India");
introduce.apply(person, ["Mumbai", "India"]);

const boundFn = introduce.bind(person, "Mumbai", "India");
boundFn(); // Called later
```

---

### 13. What is the difference between deep copy and shallow copy?

- **Shallow copy** — copies only the top-level properties; nested objects are still **referenced**.
- **Deep copy** — recursively copies all nested objects/arrays; fully **independent**.

```js
// Shallow copy
const original = { a: 1, b: { c: 2 } };
const shallow = { ...original };
shallow.b.c = 99;
console.log(original.b.c); // 99 — affected!

// Deep copy
const deep = JSON.parse(JSON.stringify(original));
deep.b.c = 42;
console.log(original.b.c); // 99 — not affected ✅

// Modern deep clone
const deep2 = structuredClone(original);
```

---

### 14. How does JavaScript handle memory management and garbage collection?

JavaScript uses **automatic garbage collection** via the **Mark-and-Sweep** algorithm:

1. The GC marks all objects reachable from the root (global scope, call stack).
2. Objects **not** reachable are swept (freed from memory).

```js
// Memory leak example
let arr = [];
setInterval(() => {
  arr.push(new Array(10000).fill("leak")); // Never cleaned up
}, 100);
```

**Best practices to avoid memory leaks:**

- Clear event listeners when not needed.
- Clear intervals/timeouts.
- Avoid global variables.
- Use `WeakMap` / `WeakSet` for object references.

---

### 15. What is the difference between function declaration and function expression?

```js
// Function Declaration — hoisted ✅
greet(); // Works!
function greet() {
  console.log("Hello");
}

// Function Expression — NOT hoisted ❌
sayHi(); // TypeError: sayHi is not a function
var sayHi = function () {
  console.log("Hi");
};

// Arrow Function Expression
const add = (a, b) => a + b;
```

---

## Array & Object Questions

---

### 16. What is destructuring in JavaScript?

Destructuring extracts values from arrays or properties from objects into variables.

```js
// Array destructuring
const [a, b, c] = [1, 2, 3];
console.log(a, b, c); // 1 2 3

// Object destructuring
const { name, age } = { name: "Alice", age: 25 };
console.log(name, age); // Alice 25

// With default values
const { city = "Unknown" } = {};
console.log(city); // Unknown

// Renaming
const { name: fullName } = { name: "Bob" };
console.log(fullName); // Bob
```

---

### 17. How do you merge two arrays in JavaScript?

```js
const a = [1, 2, 3];
const b = [4, 5, 6];

// Spread operator (most common)
const merged = [...a, ...b]; // [1, 2, 3, 4, 5, 6]

// concat()
const merged2 = a.concat(b); // [1, 2, 3, 4, 5, 6]
```

---

### 18. How do you remove duplicates from an array?

```js
const arr = [1, 2, 2, 3, 3, 4];

// Using Set (most common)
const unique = [...new Set(arr)]; // [1, 2, 3, 4]

// Using filter
const unique2 = arr.filter((val, index) => arr.indexOf(val) === index);

// Using reduce
const unique3 = arr.reduce((acc, val) => {
  if (!acc.includes(val)) acc.push(val);
  return acc;
}, []);
```

---

### 19. What is the difference between `map()`, `filter()`, and `reduce()`?

| Method     | Purpose                         | Returns      |
| ---------- | ------------------------------- | ------------ |
| `map()`    | Transform each element          | New array    |
| `filter()` | Keep elements matching criteria | New array    |
| `reduce()` | Accumulate into a single value  | Single value |

```js
const nums = [1, 2, 3, 4, 5];

const doubled = nums.map((n) => n * 2); // [2, 4, 6, 8, 10]
const evens = nums.filter((n) => n % 2 === 0); // [2, 4]
const sum = nums.reduce((acc, n) => acc + n, 0); // 15
```

---

### 20. How do you sort an array of objects?

```js
const users = [
  { name: "Charlie", age: 30 },
  { name: "Alice", age: 25 },
  { name: "Bob", age: 28 },
];

// Sort by age ascending
users.sort((a, b) => a.age - b.age);

// Sort by name alphabetically
users.sort((a, b) => a.name.localeCompare(b.name));
```

---

### 21. What is the difference between `forEach` and `map`?

| Feature   | `forEach`    | `map`             |
| --------- | ------------ | ----------------- |
| Returns   | `undefined`  | New array         |
| Use case  | Side effects | Transforming data |
| Chainable | ❌ No        | ✅ Yes            |

```js
const arr = [1, 2, 3];

arr.forEach((n) => console.log(n)); // Just logs, returns undefined

const doubled = arr.map((n) => n * 2); // [2, 4, 6]
```

---

### 22. How do you flatten a nested array?

```js
const nested = [1, [2, [3, [4]]]];

// flat() with depth
nested.flat(); // [1, 2, [3, [4]]]
nested.flat(2); // [1, 2, 3, [4]]
nested.flat(Infinity); // [1, 2, 3, 4] ✅

// Using reduce (manual)
const flatten = (arr) =>
  arr.reduce(
    (acc, val) =>
      Array.isArray(val) ? acc.concat(flatten(val)) : acc.concat(val),
    [],
  );
```

---

### 23. How do you check if an object is empty?

```js
const obj = {};

// Method 1
Object.keys(obj).length === 0; // true ✅

// Method 2
JSON.stringify(obj) === "{}"; // true

// Method 3 (ES2022)
Object.hasOwn(obj, "key"); // false
```

---

### 24. How do you convert an object to an array?

```js
const obj = { a: 1, b: 2, c: 3 };

Object.keys(obj); // ["a", "b", "c"]
Object.values(obj); // [1, 2, 3]
Object.entries(obj); // [["a", 1], ["b", 2], ["c", 3]]
```

---

### 25. What is the difference between `Object.freeze()` and `Object.seal()`?

| Feature           | `freeze()` | `seal()` |
| ----------------- | ---------- | -------- |
| Add properties    | ❌ No      | ❌ No    |
| Delete properties | ❌ No      | ❌ No    |
| Modify values     | ❌ No      | ✅ Yes   |

```js
const obj = { a: 1 };

Object.freeze(obj);
obj.a = 99; // Silently fails
console.log(obj.a); // 1

const obj2 = { b: 1 };
Object.seal(obj2);
obj2.b = 99; // ✅ Allowed
obj2.c = 5; // ❌ Cannot add
```

---

### 26. How do you merge two objects?

```js
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };

// Spread (last one wins for duplicate keys)
const merged = { ...obj1, ...obj2 }; // { a: 1, b: 3, c: 4 }

// Object.assign
const merged2 = Object.assign({}, obj1, obj2);
```

---

### 27. What is the difference between `Object.keys()` and `Object.values()`?

```js
const obj = { name: "Alice", age: 25 };

Object.keys(obj); // ["name", "age"]
Object.values(obj); // ["Alice", 25]
Object.entries(obj); // [["name", "Alice"], ["age", 25]]
```

---

### 28. How do you deep clone an object?

```js
const obj = { a: 1, b: { c: 2 } };

// Method 1: JSON (simple, loses functions/dates)
const clone1 = JSON.parse(JSON.stringify(obj));

// Method 2: structuredClone (modern, recommended ✅)
const clone2 = structuredClone(obj);

// Method 3: Lodash
// const clone3 = _.cloneDeep(obj);
```

---

### 29. What are getters and setters in JavaScript?

```js
const person = {
  _name: "Alice",
  get name() {
    return this._name.toUpperCase();
  },
  set name(value) {
    if (typeof value !== "string") throw new Error("Invalid name");
    this._name = value;
  },
};

console.log(person.name); // "ALICE"
person.name = "Bob";
console.log(person.name); // "BOB"
```

---

### 30. What is `JSON.stringify()` and `JSON.parse()` used for?

```js
const obj = { name: "Alice", age: 25 };

// Object → JSON string
const json = JSON.stringify(obj); // '{"name":"Alice","age":25}'

// JSON string → Object
const parsed = JSON.parse(json); // { name: "Alice", age: 25 }
```

> Commonly used for **localStorage**, **API requests/responses**, and **deep cloning**.

---

### 31. What is prototype inheritance in JavaScript?

Every JavaScript object has a hidden `[[Prototype]]` property. When a property isn't found on an object, JS looks up the **prototype chain**.

```js
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function () {
  console.log(`${this.name} makes a sound.`);
};

const dog = new Animal("Rex");
dog.speak(); // "Rex makes a sound."

// ES6 class syntax (syntactic sugar over prototype)
class Cat extends Animal {
  speak() {
    console.log(`${this.name} meows.`);
  }
}
```

---

### 32. What is the difference between an object literal and a constructor function?

```js
// Object Literal — one-off objects
const car = { make: "Toyota", speed: () => "Fast" };

// Constructor Function — reusable blueprint
function Car(make) {
  this.make = make;
  this.speed = () => "Fast";
}
const myCar = new Car("Honda");

// ES6 Class (preferred modern approach)
class Vehicle {
  constructor(make) {
    this.make = make;
  }
  speed() {
    return "Fast";
  }
}
```

---

### 33. How does optional chaining work?

Optional chaining (`?.`) allows safe access to deeply nested properties without throwing an error if a value is `null` or `undefined`.

```js
const user = { profile: { address: { city: "Surat" } } };

console.log(user?.profile?.address?.city); // "Surat"
console.log(user?.contact?.phone); // undefined (no error)

// With methods
user?.greet?.(); // undefined (no error if greet doesn't exist)

// With arrays
const arr = null;
console.log(arr?.[0]); // undefined
```

---

### 34. How do you handle errors using `try-catch`?

```js
function divide(a, b) {
  try {
    if (b === 0) throw new Error("Cannot divide by zero");
    return a / b;
  } catch (error) {
    console.error("Error:", error.message);
    return null;
  } finally {
    console.log("Executed always");
  }
}

divide(10, 0);
// Error: Cannot divide by zero
// Executed always
```

---

### 35. What are JavaScript generators and iterators?

A **generator** is a special function that can **pause** and **resume** execution using `yield`.

```js
function* counter() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = counter();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }

// Infinite generator
function* infiniteId() {
  let id = 1;
  while (true) yield id++;
}
```

---

## Advanced JavaScript

---

### 36. How do you create a custom event?

```js
// Create
const myEvent = new CustomEvent("userLoggedIn", {
  detail: { username: "Alice" },
});

// Listen
document.addEventListener("userLoggedIn", (e) => {
  console.log("User:", e.detail.username);
});

// Dispatch
document.dispatchEvent(myEvent);
```

---

### 37. What is `setTimeout` and `setInterval`? How are they different?

| Feature | `setTimeout`       | `setInterval`          |
| ------- | ------------------ | ---------------------- |
| Runs    | Once after delay   | Repeatedly at interval |
| Returns | Timer ID           | Timer ID               |
| Cancel  | `clearTimeout(id)` | `clearInterval(id)`    |

```js
// setTimeout — runs once after 2s
const t = setTimeout(() => console.log("Once!"), 2000);

// setInterval — runs every 1s
const i = setInterval(() => console.log("Tick!"), 1000);

// Cancel
clearTimeout(t);
clearInterval(i);
```

---

### 38. How do you cancel a running `setTimeout`?

```js
const timerId = setTimeout(() => {
  console.log("This won't run");
}, 3000);

clearTimeout(timerId); // Cancelled ✅
```

---

### 39. What is localStorage, sessionStorage, and cookies?

| Feature        | localStorage     | sessionStorage    | Cookies                 |
| -------------- | ---------------- | ----------------- | ----------------------- |
| Expires        | Never            | Tab/window closes | Set by expiry date      |
| Capacity       | ~5–10 MB         | ~5–10 MB          | ~4 KB                   |
| Accessible     | Client-side only | Client-side only  | Client + Server         |
| Sent to server | ❌ No            | ❌ No             | ✅ Yes (auto with req.) |

```js
// localStorage
localStorage.setItem("name", "Alice");
localStorage.getItem("name"); // "Alice"
localStorage.removeItem("name");

// sessionStorage (same API, cleared on tab close)
sessionStorage.setItem("token", "abc123");

// Cookies
document.cookie = "user=Alice; expires=Fri, 31 Dec 2025 23:59:59 GMT";
```

---

### 40. How does `fetch()` work and how does it compare to Axios?

```js
// fetch — built-in browser API
fetch("https://api.example.com/users")
  .then((res) => res.json()) // Must manually parse JSON
  .then((data) => console.log(data))
  .catch((err) => console.error(err));

// async/await version
const getUsers = async () => {
  const res = await fetch("https://api.example.com/users");
  const data = await res.json();
  return data;
};
```

| Feature          | `fetch()`            | `Axios`                   |
| ---------------- | -------------------- | ------------------------- |
| Built-in         | ✅ Yes               | ❌ (npm install required) |
| Auto JSON parse  | ❌ Manual            | ✅ Automatic              |
| Request timeout  | ❌ Manual            | ✅ Built-in               |
| Interceptors     | ❌ No                | ✅ Yes                    |
| Error on 4xx/5xx | ❌ No (manual check) | ✅ Auto throws error      |
| Upload progress  | ❌ No                | ✅ Yes                    |

---

## DOM & Browser APIs

---

### 41. What is event bubbling and event capturing?

**Bubbling** — event starts at the target and bubbles up to ancestors.
**Capturing** — event starts at the root and goes down to the target.

```js
// Bubbling (default)
document.querySelector("#child").addEventListener("click", (e) => {
  console.log("Child clicked");
  e.stopPropagation(); // Stops bubbling
});

// Capturing (3rd arg = true)
document.querySelector("#parent").addEventListener(
  "click",
  () => {
    console.log("Parent - capturing phase");
  },
  true,
);
```

---

### 42. What is event delegation?

Instead of attaching listeners to many child elements, attach one listener to the **parent** and use `event.target`.

```js
document.querySelector("#list").addEventListener("click", (e) => {
  if (e.target.tagName === "LI") {
    console.log("Clicked:", e.target.textContent);
  }
});
```

> ✅ More performant, handles dynamically added elements automatically.

---

### 43. What is the `this` keyword in JavaScript?

`this` refers to the **context** in which a function is called.

```js
// Global context
console.log(this); // window (browser)

// Object method
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name);
  }, // "Alice"
};

// Arrow function — no own `this`, inherits from parent
const arrow = () => console.log(this); // window

// Class
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(this.name);
  }
}
```

---

### 44. What are WeakMap and WeakSet?

- **WeakMap** — keys must be objects; entries are garbage collected when the key is no longer referenced.
- **WeakSet** — only stores objects; entries are garbage collected when unreferenced.

```js
let obj = { name: "Alice" };
const wm = new WeakMap();
wm.set(obj, "metadata");

obj = null; // obj is garbage collected, WeakMap entry too ✅
```

> Used to avoid **memory leaks** when associating data with objects.

---

### 45. What is the difference between `==` null check and optional chaining?

```js
const user = null;

// Old way
if (user != null) console.log(user.name);

// Optional chaining (modern)
console.log(user?.name); // undefined — no error

// Nullish coalescing (??)
const name = user?.name ?? "Guest"; // "Guest"
```

---

## Bonus Questions

---

### 46. What are higher-order functions?

A function that **takes another function as argument** or **returns a function**.

```js
// Takes function as argument
function doTwice(fn, value) {
  return fn(fn(value));
}
doTwice((x) => x + 3, 5); // 11

// Returns a function
function multiplier(factor) {
  return (n) => n * factor;
}
const double = multiplier(2);
double(5); // 10
```

---

### 47. What is memoization?

Memoization is an optimization technique where the **result of a function call is cached** based on its arguments to avoid redundant computation.

```js
function memoize(fn) {
  const cache = {};
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache[key]) {
      console.log("From cache");
      return cache[key];
    }
    cache[key] = fn(...args);
    return cache[key];
  };
}

const factorial = memoize((n) => (n <= 1 ? 1 : n * factorial(n - 1)));
factorial(5); // Computed
factorial(5); // From cache ✅
```

---

### 48. What is the difference between `Promise.all`, `Promise.race`, `Promise.any`, and `Promise.allSettled`?

```js
const p1 = Promise.resolve(1);
const p2 = Promise.resolve(2);
const p3 = Promise.reject("error");

// all — resolves when all resolve, rejects if any reject
Promise.all([p1, p2]).then(console.log); // [1, 2]
Promise.all([p1, p3]).catch(console.error); // "error"

// allSettled — waits for all, never rejects
Promise.allSettled([p1, p3]).then(console.log);
// [{status: "fulfilled", value: 1}, {status: "rejected", reason: "error"}]

// race — resolves/rejects with the first settled promise
Promise.race([p1, p2]).then(console.log); // 1

// any — resolves with first fulfilled, rejects only if ALL reject
Promise.any([p3, p1]).then(console.log); // 1
```

---

### 49. What is the difference between `setTimeout(fn, 0)` and `queueMicrotask(fn)`?

```js
console.log("start");

setTimeout(() => console.log("macrotask"), 0);

queueMicrotask(() => console.log("microtask"));

Promise.resolve().then(() => console.log("promise"));

console.log("end");

// Output:
// start
// end
// microtask
// promise
// macrotask
```

> Microtasks (Promise, queueMicrotask) always execute **before** macrotasks (setTimeout, setInterval).

---

### 50. What are IIFE (Immediately Invoked Function Expressions)?

An **IIFE** is a function that runs immediately after it's defined.

```js
(function () {
  const secret = "hidden";
  console.log("IIFE runs!");
})();

// Arrow IIFE
(() => console.log("Arrow IIFE"))();
```

> Used to create a **private scope** and avoid polluting the global namespace.

---
 -->
