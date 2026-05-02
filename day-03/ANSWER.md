
## 🔁 Part 01 — Loops

---

### What is a loop?

A loop is a control structure that **repeats a block of code** as long as a condition is true.

```js
for (let i = 0; i < 3; i++) {
  console.log(i); // runs 3 times
}
```

---

### What is an iteration?

An **iteration** is one single execution of the loop body — one "pass" through the code inside the loop.

```js
for (let i = 0; i < 3; i++) {
  console.log(i);
  // i = 0 → 1st iteration
  // i = 1 → 2nd iteration
  // i = 2 → 3rd iteration
}
```

---

### What is the difference between looping and iteration?

- **Looping** is the overall process — the loop running repeatedly
- **Iteration** is each individual cycle within that loop

> A loop *iterates*. Each run of the loop body is one *iteration*.

---

### What is a nested loop?

A loop inside another loop. The **inner loop completes all its iterations** for every single iteration of the outer loop.

```js
for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 3; j++) {
    console.log(i, j);
  }
}
```

---

### How many times does the inner loop run?

The inner loop runs its **full cycle** for every outer iteration.

```
Outer runs 3 times × Inner runs 3 times = 9 total iterations
```

---

### What is the total iteration formula?

```
Total iterations = outer loop count × inner loop count
```

```js
// Outer: 4 times, Inner: 3 times
// Total: 4 × 3 = 12 iterations
for (let i = 0; i < 4; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(i, j);
  }
}
```

---

### What does `break` do?

`break` **immediately exits** the current loop — no more iterations run.

```js
for (let i = 0; i < 5; i++) {
  if (i === 3) break;
  console.log(i); // 0, 1, 2
}
```

---

### What does `continue` do?

`continue` **skips the current iteration** and jumps to the next one.

```js
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;
  console.log(i); // 0, 1, 3, 4
}
```

---

### Does `break` stop the outer loop in nested loops?

**No.** `break` only stops the **loop it is directly inside**. The outer loop continues.

```js
for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 3; j++) {
    if (j === 2) break;    // only breaks inner loop
    console.log(i, j);
  }
  // outer loop keeps going
}
// Output: 1 1 | 2 1 | 3 1
```

---

### Predict the output:

```js
for (let i = 1; i <= 4; i++) {
  if (i === 2) continue;
  if (i === 4) break;
  console.log(i);
}
```

**Output: `1` and `3`**

Step by step:
- `i = 1` → no condition matches → prints `1`
- `i = 2` → `continue` → skips print, goes to next iteration
- `i = 3` → no condition matches → prints `3`
- `i = 4` → `break` → exits loop entirely

---

### What is an expression?

An expression is **any piece of code that evaluates to a value**.

```js
5 + 3          // evaluates to 8
"hello"        // evaluates to "hello"
true && false  // evaluates to false
x > 10         // evaluates to true or false
greet()        // evaluates to whatever the function returns
```

---

### What is a statement?

A statement is a **complete instruction that performs an action**. It does not produce a value.

```js
let x = 5;           // declaration statement
if (x > 3) { ... }  // if statement
for (...) { ... }    // loop statement
```

---

### Can an expression exist inside a statement?

**Yes.** Statements often contain expressions.

```js
let result = 5 + 3;
//           ↑↑↑↑↑ expression (evaluates to 8)
// the whole line is a statement
```

---

### Why can't we assign an `if` statement to a variable?

Because `if` is a **statement** — it performs an action but doesn't produce a value. You can only assign expressions (things that evaluate to a value) to variables.

```js
let x = if (true) { 5 }  // ❌ SyntaxError — if is a statement
```

---

### Why is ternary allowed in assignment?

Because the ternary operator `? :` is an **expression** — it always evaluates to a value.

```js
let label = score > 50 ? "Pass" : "Fail";  // ✅ ternary returns a value
```

---

### Can a `for` loop have multiple variables? How?

Yes — using a comma `,` in the initialization and update parts.

```js
for (let i = 0, j = 10; i < 5; i++, j--) {
  console.log(i, j);
}
// 0 10 | 1 9 | 2 8 | 3 7 | 4 6
```

---

### Why do we use the same index for multiple arrays?

When two arrays are **related by position** — index `0` in both arrays refers to the same entity.

```js
let names  = ["Alice", "Bob", "Charlie"];
let scores = [90,      85,    92];

for (let i = 0; i < names.length; i++) {
  console.log(names[i], scores[i]);
  // Alice 90 | Bob 85 | Charlie 92
}
```

---

### What is a better structure than multiple arrays?

An **array of objects** — keeps related data together, easier to read and maintain.

```js
// ❌ multiple arrays — fragile
let names  = ["Alice", "Bob"];
let scores = [90, 85];

// ✅ array of objects — clean and self-contained
let students = [
  { name: "Alice", score: 90 },
  { name: "Bob",   score: 85 }
];
```

---

### What is the main difference between `while` and `do...while`?

| | Condition checked | Minimum runs |
|-|------------------|--------------|
| `while` | **before** each iteration | 0 times |
| `do...while` | **after** each iteration | always 1 time |

```js
let x = 10;

while (x < 5) {
  console.log("while"); // never runs
}

do {
  console.log("do...while"); // runs once
} while (x < 5);
```

---

### Which loop always runs at least once?

**`do...while`** — because the condition is checked *after* the first execution.

---

### Why can `while` cause infinite loops?

If the condition **never becomes false**, the loop runs forever, crashing the program.

```js
let i = 0;
while (i < 5) {
  console.log(i);
  // forgot i++ → condition never changes → infinite loop ♾️
}
```

---

### When would you prefer `do...while`?

When you need the code to **run at least once regardless of the condition** — like asking for user input and validating it.

```js
let input;
do {
  input = prompt("Enter a number greater than 0:");
} while (input <= 0);
```

---

## 🧩 Part 02 — Functions

---

### What is a function?

A function is a **reusable block of code** that performs a specific task. You define it once and call it whenever needed.

```js
function greet(name) {
  return `Hello, ${name}!`;
}

greet("Alice"); // "Hello, Alice!"
greet("Bob");   // "Hello, Bob!"
```

---

### What is a function expression?

A function stored in a **variable** instead of declared with the `function` keyword at the top level.

```js
const greet = function(name) {
  return `Hello, ${name}!`;
};
```

---

### What is the difference between declaration and expression?

| | Function Declaration | Function Expression |
|-|---------------------|---------------------|
| Syntax | `function name() {}` | `const name = function() {}` |
| Hoisted? | ✅ Yes — available before definition | ❌ No — only available after assignment |
| Named? | Always named | Can be anonymous |

```js
// ✅ works — declaration is hoisted
greet();
function greet() { console.log("Hi"); }

// ❌ error — expression is NOT hoisted
greet();
const greet = function() { console.log("Hi"); };
```

---

### What is a default parameter?

A **fallback value** used when no argument is passed for that parameter.

```js
function greet(name = "stranger") {
  return `Hello, ${name}!`;
}

greet("Alice"); // "Hello, Alice!"
greet();        // "Hello, stranger!" — default kicks in
```

---

### What is `rest` (`...rest`)?

`...rest` collects **all remaining arguments** into an array.

```js
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3, 4); // 10
```

---

### When does a default parameter apply?

When the argument is `undefined` — either not passed at all, or explicitly passed as `undefined`.

```js
function greet(name = "stranger") {
  console.log(name);
}

greet();            // "stranger" — not passed
greet(undefined);   // "stranger" — explicitly undefined
greet(null);        // null       — null is NOT undefined
greet("Alice");     // "Alice"    — value provided
```

---

### What does `...rest` return?

An **array** of all remaining arguments passed to the function.

```js
function demo(first, ...rest) {
  console.log(first); // "a"
  console.log(rest);  // ["b", "c", "d"]
}

demo("a", "b", "c", "d");
```

---

### Why must `rest` be last?

Because `...rest` collects **everything that's left**. If it weren't last, JS wouldn't know where it ends and the next parameter begins.

```js
function demo(...rest, last) { } // ❌ SyntaxError
function demo(first, ...rest) { } // ✅ correct
```

---

### Can an inner function access outer variables? Why?

**Yes** — through **closure**. Inner functions have access to variables in their outer (parent) scope.

```js
function outer() {
  let message = "Hello";

  function inner() {
    console.log(message); // ✅ accesses outer variable
  }

  inner();
}
```

---

### Can an outer function access inner variables?

**No.** Inner variables are scoped to the inner function — they don't exist outside it.

```js
function outer() {
  function inner() {
    let secret = "hidden";
  }
  console.log(secret); // ❌ ReferenceError
}
```

---

### What is an arrow function?

A **shorter syntax** for writing functions, introduced in ES6.

```js
// Regular function
function add(a, b) { return a + b; }

// Arrow function
const add = (a, b) => a + b;
```

Key differences from regular functions:
- No own `this` binding — inherits from surrounding scope
- Cannot be used as constructors
- No `arguments` object

---

### What is an IIFE?

**Immediately Invoked Function Expression** — a function that is **defined and called at the same time**.

```js
(function() {
  console.log("Runs immediately!");
})();

// Arrow function IIFE
(() => {
  console.log("Also runs immediately!");
})();
```

---

### When does an IIFE run?

**Immediately** — the moment the JavaScript engine reaches that line. No need to call it separately.

---

### Why use arrow functions in callbacks?

Arrow functions are **shorter** and don't bind their own `this`, making them ideal for callbacks where you want to inherit the outer `this`.

```js
// Regular function callback
setTimeout(function() {
  console.log("Done");
}, 1000);

// Arrow function callback — cleaner
setTimeout(() => {
  console.log("Done");
}, 1000);
```

---

### What is a callback function in JavaScript?

A **function passed as an argument** to another function, to be called later.

```js
function greet(name, callback) {
  console.log("Hello, " + name);
  callback();
}

greet("Alice", function() {
  console.log("Callback ran!");
});
```

---

### Why do we pass a function as an argument to another function?

To let the **receiving function decide when to call it** — useful for async operations, event handling, and custom behavior.

```js
function doTask(task, onComplete) {
  task();
  onComplete(); // called after task finishes
}
```

---

### What is the role of a callback in `setTimeout`?

The callback is the function `setTimeout` will **call after the delay**.

```js
setTimeout(() => {
  console.log("Runs after 2 seconds");
}, 2000);
// The arrow function is the callback
```

---

### Is a callback function executed immediately or later?

**It depends on the function receiving it.** It can be immediate or deferred.

```js
// Immediate — called right away inside the function
[1, 2, 3].forEach(n => console.log(n)); // runs now

// Deferred — called after a delay
setTimeout(() => console.log("later"), 1000); // runs after 1s
```

---

### Can a callback be a named function and an anonymous function?

**Yes — both are valid.**

```js
// Anonymous callback
setTimeout(function() {
  console.log("anonymous");
}, 1000);

// Named callback
function onDone() {
  console.log("named");
}
setTimeout(onDone, 1000);
```

> ✅ Named callbacks are easier to debug — they show up by name in stack traces.

---

### What is the difference between calling a function and passing it as a callback?

| | Syntax | What happens |
|-|--------|-------------|
| **Calling** | `greet()` | Executes immediately, passes the return value |
| **Passing** | `greet` | Passes the function itself to be called later |

```js
setTimeout(greet(), 1000);  // ❌ calls greet NOW, passes return value
setTimeout(greet, 1000);    // ✅ passes greet to be called after 1s
```

---

### What is a pure function?

A function that:
1. Always returns the **same output** for the same input
2. Has **no side effects** — doesn't change anything outside itself

```js
// ✅ Pure
function add(a, b) {
  return a + b;
}

// ❌ Not pure — depends on external state
let tax = 0.1;
function getTotal(price) {
  return price + price * tax; // result changes if tax changes
}
```

---

### What are the two main rules of a pure function?

1. **Deterministic** — same input always produces same output
2. **No side effects** — doesn't modify external variables, DOM, files, or databases

---

### What is a side effect in JavaScript?

Any action a function performs that **affects something outside its own scope**.

```js
// Side effects include:
let count = 0;
function increment() { count++; }       // modifies external variable
function log(msg) { console.log(msg); } // writes to console
function save(data) { db.save(data); }  // writes to database
```

---

### Is this function pure? Why or why not?

```js
let count = 0;
function increment() {
  count++;
}
```

**No — it is NOT pure.**

Two reasons:
1. It **modifies `count`** which is outside its own scope — that's a side effect
2. It returns `undefined` every time — not based on input at all

---

### Why are pure functions easier to debug and test?

Because they are **completely predictable** — given the same input, they always return the same output with no hidden dependencies.

```js
// Easy to test — no setup needed
expect(add(2, 3)).toBe(5); // always true
```

---

### Can a pure function use variables outside itself?

**Only if it doesn't modify them and they never change (constants).**

```js
const PI = 3.14159;

// ✅ Still pure — reads a constant, doesn't modify anything
function circleArea(r) {
  return PI * r * r;
}
```

---

### What is a higher order function (HOF)?

A function that either:
- **Takes a function as an argument**, or
- **Returns a function**

```js
// Takes a function → HOF
function repeat(fn, times) {
  for (let i = 0; i < times; i++) fn();
}

// Returns a function → HOF
function multiplier(x) {
  return (n) => n * x;
}
const double = multiplier(2);
double(5); // 10
```

---

### What are the two ways a function becomes a HOF?

1. It **receives** a function as a parameter
2. It **returns** a function as its result

---

### Is a function that takes a callback a HOF? Why?

**Yes** — because it takes another function (the callback) as an argument. That's the definition of a HOF.

```js
function greet(name, callback) {  // ← takes a function
  callback(name);
}
// greet is a HOF
```

---

### Is a function that returns another function a HOF?

**Yes** — returning a function is the second way to qualify as a HOF.

```js
function makeGreeter(greeting) {
  return function(name) {       // ← returns a function
    return `${greeting}, ${name}!`;
  };
}

const sayHi = makeGreeter("Hi");
sayHi("Alice"); // "Hi, Alice!"
```

---

### Identify the HOF:

```js
function operate(fn, value) {
  return fn(value);
}
```

**`operate` is the HOF** — it takes `fn` (a function) as an argument and calls it. Any function that receives another function as a parameter is a HOF.

---

### What is the relationship between callbacks and HOFs?

**Every function that uses a callback is a HOF.** A callback is the function being passed in, and the HOF is the function receiving it.

```js
// forEach is a HOF
// the arrow function is the callback
[1, 2, 3].forEach(n => console.log(n));
```

---

### What is the call stack in JavaScript?

The call stack is a **data structure that tracks which function is currently executing** and what to return to when it's done. It works like a stack of plates — last in, first out (LIFO).

```
Call Stack
┌─────────────┐
│   greet()   │  ← currently executing (top)
│   main()    │  ← waiting
└─────────────┘
```

---

### Why is it called a "stack"?

Because it follows **LIFO — Last In, First Out**. The last function pushed onto the stack is the first one removed when it finishes.

---

### What happens in the call stack when a function is called?

The function is **pushed onto the top** of the stack and begins executing.

```js
function greet() { console.log("Hi"); }
function main() { greet(); }

main();
// Stack: main → greet pushed on top → greet runs
```

---

### What happens when a function finishes execution?

It is **popped off** the call stack. Control returns to the function below it.

```
Before:            After greet() finishes:
┌──────────┐       ┌──────────┐
│ greet()  │  →    │          │
│ main()   │       │ main()   │
└──────────┘       └──────────┘
```

---

### What is stack overflow? When does it happen?

A **stack overflow** happens when the call stack runs out of space — caused by infinite recursion (a function calling itself with no base case to stop it).

```js
function infinite() {
  infinite(); // calls itself forever
}
infinite(); // ❌ RangeError: Maximum call stack size exceeded
```

---

### Is JavaScript single-threaded? How does that relate to the call stack?

**Yes** — JavaScript has **one call stack**, meaning it can only execute one thing at a time. This is what "single-threaded" means. No two functions run simultaneously in the JS engine itself.

---

### What is recursion in JavaScript?

Recursion is when a **function calls itself** to solve a problem by breaking it into smaller versions of the same problem.

```js
function countdown(n) {
  if (n === 0) return;   // base case
  console.log(n);
  countdown(n - 1);      // recursive call
}

countdown(3); // 3, 2, 1
```

---

### What is a base case in recursion? Why is it important?

The **base case** is the condition that **stops the recursion**. Without it, the function calls itself forever → stack overflow.

```js
function factorial(n) {
  if (n === 1) return 1;       // base case — STOPS here
  return n * factorial(n - 1); // recursive case
}
```

---

### What happens if a recursive function has no base case?

It calls itself **infinitely** until the call stack runs out of space, throwing:

```
RangeError: Maximum call stack size exceeded
```

---

### How does recursion use the call stack?

Each recursive call is **pushed onto the call stack**. Once the base case is hit, the calls start **popping off** one by one as each returns its value.

```
factorial(3)
  → factorial(2)
    → factorial(1)  ← base case hit
    ← returns 1
  ← returns 2 × 1 = 2
← returns 3 × 2 = 6
```

---

### What is the difference between iteration (loops) and recursion?

| | Iteration | Recursion |
|-|-----------|-----------|
| How | Uses loops (`for`, `while`) | Function calls itself |
| State | Loop variable changes | Arguments change each call |
| Stack usage | Constant | Grows with each call |
| Risk | Infinite loop | Stack overflow |
| Readability | Easier for sequential tasks | Cleaner for tree/nested structures |

---

### How are recursion and the call stack related?

Every recursive call **adds a new frame to the call stack**. The stack grows deeper with each call and unwinds as each call returns. This is why recursion without a base case causes a stack overflow.

---

### What happens in the call stack when a recursive function keeps calling itself?

Each call is pushed onto the stack without anything being popped off — the stack grows until it hits the **maximum size limit**, then crashes with a `RangeError`.

```
Stack at depth 3:
┌──────────────┐
│ factorial(1) │
│ factorial(2) │
│ factorial(3) │
│ main()       │
└──────────────┘
```

---

## 🗂️ Quick Reference Summary

### Loop Types

```
for          → best when iterations are known
while        → best when condition-based, may run 0 times
do...while   → always runs at least once
for...of     → iterates over values (arrays, strings)
for...in     → iterates over keys (objects)
```

### Loop Control

```
break      → exits the loop entirely
continue   → skips current iteration, goes to next
```

### Function Types

```
Declaration   → function name() {}          hoisted ✅
Expression    → const name = function() {}  not hoisted ❌
Arrow         → const name = () => {}       no own `this`
IIFE          → (() => {})()                runs immediately
```

### Pure Function Rules

```
1. Same input → always same output
2. No side effects — doesn't touch anything outside itself
```

### HOF — Two Ways

```
1. Takes a function as argument  → const hof = (fn) => fn()
2. Returns a function            → const hof = () => () => {}
```

### Call Stack — LIFO

```
Function called  → pushed onto stack
Function returns → popped off stack
Stack overflow   → too many calls, no base case
```

### Recursion Checklist

```
✅ Base case defined?
✅ Each call moves closer to base case?
✅ Returns a value at each step?
```

---
