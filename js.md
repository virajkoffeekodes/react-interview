# JavaScript Interview Questions & Answers

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

