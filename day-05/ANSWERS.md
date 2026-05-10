
## 🔼 Part 01 — Hoisting & TDZ

---

### What is hoisting?

Hoisting is JavaScript's behavior of **allocating memory for variables and functions during the memory phase** — before any code executes. It gives the impression that declarations are "moved to the top," but nothing physically moves.

```js
console.log(a); // undefined — not an error
var a = 10;

// JS sees it as:
// memory phase: a = undefined
// execution phase: a = 10
```

---

### Why do function declarations work before definition?

Because during the **memory phase**, function declarations are **fully stored in memory** — including their entire body. By the time execution begins, the function is already available.

```js
greet(); // ✅ works — "Hello!"

function greet() {
  console.log("Hello!");
}
```

---

### Is hoisting a real physical movement of code?

**No.** Hoisting is not a physical process. The code doesn't move anywhere. It's simply that the JS engine allocates memory for declarations in the **memory/creation phase** before the execution phase begins. It *behaves as if* declarations were at the top.

---

### Memory Phase vs Hoisting

They are closely related but not the same thing:

| | Memory Phase | Hoisting |
|-|-------------|----------|
| What it is | Phase 1 of execution context creation | The *effect* of what memory phase does |
| When it happens | Before execution | Caused by memory phase |
| What it does | Allocates memory for vars and functions | Makes them accessible before their line |

> The **memory phase causes** hoisting. Hoisting is the *observable effect*.

---

### When does TDZ occur?

The **Temporal Dead Zone (TDZ)** is the period between when a `let` or `const` variable is **hoisted** (memory noted) and when its **actual declaration line is reached** during execution.

```js
console.log(x); // ❌ ReferenceError — in TDZ
let x = 5;
console.log(x); // ✅ 5 — TDZ is over
```

---

### Why does `let` throw ReferenceError?

Because `let` is hoisted but **not initialized** — it sits in the TDZ. Accessing it before its declaration line means accessing uninitialized memory, which JS explicitly forbids.

```js
console.log(a); // undefined — var is initialized to undefined
console.log(b); // ❌ ReferenceError — let is in TDZ

var a = 1;
let b = 2;
```

---

### Does `var` have TDZ? Why?

**No.** `var` is hoisted AND immediately initialized to `undefined` in the memory phase. There is no gap between hoisting and initialization, so no TDZ exists.

```js
console.log(x); // undefined — safe, no TDZ
var x = 10;
```

---

### Are `let` and `const` hoisted?

**Yes — but not initialized.** This is a common misconception. They ARE hoisted (the engine knows they exist), but they are placed in the TDZ until their declaration line is reached.

```js
// JS engine in memory phase:
// var a   → hoisted + initialized (undefined)
// let b   → hoisted + TDZ (not initialized)
// const c → hoisted + TDZ (not initialized)
```

---

### What is the difference between hoisting and TDZ?

| | Hoisting | TDZ |
|-|----------|-----|
| What it is | Memory allocated for the variable | Period where variable exists but can't be accessed |
| Applies to | `var`, `let`, `const`, functions | Only `let` and `const` |
| Effect | Variable is known to the engine | Accessing it throws ReferenceError |

---

### Why was TDZ introduced in JavaScript?

TDZ was introduced with ES6 (`let` and `const`) to **catch bugs early** and enforce better coding practices:

- Prevents accessing variables before they are meaningfully initialized
- Encourages declaring variables before using them
- Makes code more predictable and easier to reason about

> `var`'s silent `undefined` was a common source of bugs — TDZ makes those bugs loud and obvious.

---

### What is function hoisting?

Function hoisting is when **function declarations are fully stored in memory** during the memory phase — including their entire body — making them callable before their definition in the code.

```js
sayHi(); // ✅ "Hi!" — fully hoisted

function sayHi() {
  console.log("Hi!");
}
```

---

### Why can function declarations be called before definition?

Because the entire function — name AND body — is stored in memory during the memory phase. When execution reaches the call, the function is already fully available.

---

### What gets stored during the memory phase for function declarations?

The **complete function object** — including:
- The function name
- The full function body
- The reference to its scope

```js
// Memory phase stores the entire thing:
function add(a, b) {
  return a + b;
}
// add → [complete function] stored immediately
```

---

### Why do function expressions fail before assignment?

Because a function expression is assigned to a variable. The **variable** is hoisted, but only as `undefined` (for `var`) or TDZ (for `let`/`const`). The function itself is not stored during the memory phase.

```js
greet(); // ❌ TypeError: greet is not a function
var greet = function() {
  console.log("Hello");
};

// Memory phase: greet = undefined
// Execution phase: greet = function (only now)
// So calling greet() before this line → undefined() → TypeError
```

---

### Are arrow functions fully hoisted?

**No.** Arrow functions behave like function expressions. They are assigned to variables, so:

- With `var` → variable is `undefined` in memory phase
- With `let`/`const` → variable is in TDZ

```js
greet(); // ❌ ReferenceError (TDZ)
const greet = () => console.log("Hello");

hello(); // ❌ TypeError: hello is not a function
var hello = () => console.log("Hello");
// memory phase: hello = undefined → undefined() → TypeError
```

---

### What is the difference between declaration hoisting and expression hoisting?

| | Function Declaration | Function Expression / Arrow |
|-|---------------------|----------------------------|
| Hoisted? | ✅ Fully (name + body) | ⚠️ Partially (variable only) |
| Callable before definition? | ✅ Yes | ❌ No |
| Memory phase value | Complete function | `undefined` or TDZ |

---

### What error happens with `undefined()`?

A **TypeError** — because `undefined` is not a function, and trying to call it with `()` throws:

```js
var greet = function() {};
greet(); // works

var greet2;
greet2(); // ❌ TypeError: greet2 is not a function
// greet2 is undefined — you're doing undefined()
```

---

### Why does a `const` arrow function give a TDZ error?

Because `const` is hoisted into the TDZ — not initialized. Calling it before its declaration line means accessing a TDZ variable, which throws a `ReferenceError`.

```js
greet(); // ❌ ReferenceError: Cannot access 'greet' before initialization

const greet = () => {
  console.log("Hello");
};
```

> With `var` arrow → `TypeError` (undefined is not a function)
> With `const`/`let` arrow → `ReferenceError` (TDZ)

---

## 🔍 Part 02 — Scope

---

### What is scope in JavaScript?

Scope is the **region of code where a variable is accessible**. Variables are not available everywhere — scope defines the boundaries of their visibility.

```js
function greet() {
  let name = "Alice"; // only accessible inside greet
}

console.log(name); // ❌ ReferenceError — out of scope
```

---

### What is global scope?

Global scope is the **outermost scope** — variables declared here are accessible from anywhere in the program.

```js
let appName = "MyApp"; // global scope

function show() {
  console.log(appName); // ✅ accessible — in scope
}
```

---

### What is function scope?

Variables declared inside a function are **only accessible within that function**. They are destroyed when the function finishes.

```js
function greet() {
  let message = "Hello"; // function scope
  console.log(message);  // ✅
}

console.log(message); // ❌ ReferenceError
```

---

### What is block scope?

A block is any code inside `{}`. **`let` and `const` are block-scoped** — they only exist within the block they are declared in.

```js
{
  let x = 10;
  const y = 20;
  console.log(x, y); // ✅
}

console.log(x); // ❌ ReferenceError — block scope ended
```

---

### Does `var` have block scope?

**No.** `var` ignores block boundaries (like `if`, `for`, `{}`). It is function-scoped — it leaks out of blocks into the nearest function.

```js
{
  var a = 10;
  let b = 20;
}

console.log(a); // ✅ 10 — var leaked out of block
console.log(b); // ❌ ReferenceError — let is block scoped
```

---

### Which keyword attaches to `window` globally?

**`var`** — when declared at the global level, `var` becomes a property of the `window` object in browsers.

```js
var name = "Alice";
console.log(window.name); // "Alice" ✅

let age = 25;
console.log(window.age);  // undefined ❌ — let doesn't attach to window
```

---

### Why are `let` and `const` safer than `var`?

| Reason | `var` problem | `let`/`const` fix |
|--------|--------------|-------------------|
| Block scope | Leaks out of blocks | Stays within the block |
| TDZ | No TDZ — silent `undefined` | TDZ — catches bugs early |
| Re-declaration | Can re-declare same variable | Cannot re-declare |
| Global attach | Attaches to `window` | Does not attach to `window` |

---

### Which keywords are block scoped?

**`let` and `const`** are block scoped. `var` is NOT.

```js
if (true) {
  let x = 1;   // block scoped ✅
  const y = 2; // block scoped ✅
  var z = 3;   // NOT block scoped ❌
}

console.log(z); // 3 — leaked out
console.log(x); // ❌ ReferenceError
```

---

### Why is `var` discouraged in modern JS?

- It **leaks out of blocks**, causing unexpected behavior
- It **attaches to `window`** globally, polluting the global object
- It allows **re-declaration** of the same variable without error
- It has **no TDZ**, making silent `undefined` bugs easy to miss
- `let` and `const` handle all these issues cleanly

---

### What is function scope?

Function scope means variables declared inside a function are **private to that function** — not accessible outside, regardless of whether they use `var`, `let`, or `const`.

```js
function demo() {
  var a = 1;
  let b = 2;
  const c = 3;
}

console.log(a); // ❌ all three are inaccessible outside
console.log(b); // ❌
console.log(c); // ❌
```

---

### Can variables inside a function be accessed outside?

**No** — function scope creates a private boundary. Variables declared inside stay inside.

---

### Does each function call create a new scope?

**Yes** — every function call creates a fresh **Function Execution Context** with its own local scope. Variables from one call don't bleed into another.

```js
function counter() {
  let count = 0;
  count++;
  console.log(count);
}

counter(); // 1
counter(); // 1 — fresh scope, count starts at 0 again
```

---

### What is the difference between function scope and block scope?

| | Function Scope | Block Scope |
|-|---------------|-------------|
| Boundary | `function() {}` | Any `{}` — if, for, while, standalone |
| Keywords | `var`, `let`, `const` | Only `let` and `const` |
| `var` inside | Stays in function | Leaks out of block |

```js
function demo() {
  if (true) {
    var x = 1;  // function scoped — available in whole function
    let y = 2;  // block scoped — only in this if block
  }
  console.log(x); // ✅ 1
  console.log(y); // ❌ ReferenceError
}
```

---

### Why does `var` behave differently inside blocks?

Because `var` is **function-scoped, not block-scoped**. It ignores `if`, `for`, `while` block boundaries and is scoped to the nearest enclosing function (or global if no function).

```js
for (var i = 0; i < 3; i++) {}
console.log(i); // 3 — leaked out of for block

for (let j = 0; j < 3; j++) {}
console.log(j); // ❌ ReferenceError — block scoped
```

---

### Can a variable declared using `var` inside a function be accessed outside?

**No.** `var` respects function boundaries. Even though it ignores block scope, it does NOT escape functions.

```js
function demo() {
  var secret = "inside";
}

console.log(secret); // ❌ ReferenceError — var is trapped in function
```

---

### Can a variable declared using `var` inside a block be accessed outside?

**Yes** — `var` leaks out of blocks (if, for, while, `{}`).

```js
if (true) {
  var leaked = "I escaped";
}

console.log(leaked); // ✅ "I escaped" — var ignores block scope
```

---

### What is the scope chain?

The scope chain is the **mechanism JS uses to look up variables** by searching from the current scope outward through parent scopes until it finds the variable or hits the global scope.

```js
let globalVar = "global";

function outer() {
  let outerVar = "outer";

  function inner() {
    let innerVar = "inner";

    console.log(innerVar);  // ✅ found in own scope
    console.log(outerVar);  // ✅ found in outer scope
    console.log(globalVar); // ✅ found in global scope
  }

  inner();
}
```

---

### `var` vs `let` vs `const` — Comparison Table

| | `var` | `let` | `const` |
|-|-------|-------|---------|
| Scope | Function | Block | Block |
| Hoisted | ✅ (`undefined`) | ✅ (TDZ) | ✅ (TDZ) |
| Re-declarable | ✅ Yes | ❌ No | ❌ No |
| Re-assignable | ✅ Yes | ✅ Yes | ❌ No |
| Attaches to `window` | ✅ Yes | ❌ No | ❌ No |
| TDZ | ❌ No | ✅ Yes | ✅ Yes |
| Modern usage | ❌ Avoid | ✅ Use | ✅ Prefer |

---

### How does JS find a variable if it's not in local scope?

JS walks **up the scope chain** — checking each parent scope until it finds the variable or reaches the global scope. If not found anywhere, it throws a `ReferenceError`.

```js
let x = "global";

function outer() {
  function inner() {
    console.log(x); // not in inner → not in outer → found in global ✅
  }
  inner();
}
```

---

### What is the order of lookup in the scope chain?

1. **Current (local) scope** — look here first
2. **Parent scope** — move up one level
3. **Grandparent scope** — keep moving up
4. **Global scope** — last stop
5. **ReferenceError** — if not found anywhere

---

### Can inner scope access outer variables? Why?

**Yes** — through the scope chain. Inner scopes have a reference to their parent scope, so they can look up variables that don't exist locally.

```js
function outer() {
  let msg = "Hello";

  function inner() {
    console.log(msg); // ✅ found via scope chain
  }

  inner();
}
```

---

### Can outer scope access inner variables?

**No.** Scope is **one-directional** — inner can see outer, but outer cannot see inner. Inner variables are private to their scope.

```js
function outer() {
  function inner() {
    let secret = "private";
  }

  console.log(secret); // ❌ ReferenceError — outer can't see inner
}
```

---

## 🗂️ Quick Reference Summary

### Hoisting Behavior

```
var declaration       → hoisted + initialized (undefined)
let / const           → hoisted + TDZ (not initialized)
function declaration  → hoisted + fully stored (name + body)
function expression   → variable hoisted only (undefined or TDZ)
arrow function        → same as function expression
```

### TDZ — Key Facts

```
Applies to  → let and const only
Period      → from start of scope to declaration line
Access      → ReferenceError if accessed during TDZ
Purpose     → catch bugs, enforce declaration before use
```

### Scope Types

```
Global scope    → accessible everywhere
Function scope  → accessible only inside the function
Block scope     → accessible only inside the {} block (let/const only)
```

### var vs let vs const — Quick

```
var   → function scoped, no TDZ, attaches to window, avoid ❌
let   → block scoped, TDZ, reassignable ✅
const → block scoped, TDZ, not reassignable, prefer ✅
```

### Scope Chain Lookup Order

```
local → parent → grandparent → ... → global → ReferenceError
```

---

> 📅 **Day 05 Complete — 15 days to go!**
