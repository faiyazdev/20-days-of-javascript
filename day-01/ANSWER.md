## 📦 Part 01 — How JavaScript Loads in the Browser

### 01. Why can putting `<script>` in `<head>` cause errors?

When the browser encounters a `<script>` tag in `<head>`, it **pauses HTML parsing** to download and execute the script. At that point, the DOM hasn't been built yet — so if your script tries to access elements like `document.getElementById("btn")`, they simply don't exist yet, causing errors.

```html
<!-- ❌ Dangerous — DOM not ready yet -->
<head>
  <script src="app.js"></script>
</head>
```

---

### 02. What does the browser do when it encounters a `<script>` tag?

The browser follows these steps:

1. **Stops** parsing HTML
2. **Downloads** the script (if external)
3. **Executes** the script immediately
4. **Resumes** HTML parsing after execution is complete

This is called **render-blocking** behavior.

---

### 03. What are the two main phases of loading JavaScript?

| Phase | Description |
|-------|-------------|
| **Fetching** | The browser downloads the `.js` file from the server |
| **Execution** | The JS engine runs the downloaded code |

Both phases block HTML parsing by default (without `defer` or `async`).

---

### 04. Why is placing `<script>` at the end of `<body>` safer?

Because by the time the browser reaches the bottom of `<body>`, the entire DOM has already been parsed. Your script can safely access any HTML element without errors.

```html
<!-- ✅ Safe — DOM is fully built -->
<body>
  <div id="app"></div>

  <script src="app.js"></script>
</body>
```

---

### 05. What is the main disadvantage of putting scripts at the end of `<body>`?

The script **download doesn't begin until the browser finishes parsing all the HTML**. This wastes time — the browser could have been fetching the JS file in parallel while parsing HTML, but it doesn't start until the very end.

> 💡 On slow networks or large HTML files, this noticeably delays interactivity.

---

### 06. What does `defer` actually do?

`defer` tells the browser:

- ✅ **Continue parsing HTML** without blocking
- ✅ **Download the script in parallel** (in the background)
- ✅ **Execute the script AFTER** the full HTML is parsed, but **before** `DOMContentLoaded`

```html
<head>
  <script src="app.js" defer></script>
</head>
```

---

### 07. Which is better for performance: end of `<body>` or `defer`? Why?

**`defer` is better.** Here's why:

| Strategy | Download Starts | Blocks Parsing? |
|----------|-----------------|-----------------|
| End of `<body>` | After HTML is fully parsed | No |
| `defer` in `<head>` | Immediately, in parallel | No |

With `defer`, the script is **already downloaded** by the time HTML finishes parsing. End of `<body>` wastes that time.

---

### 08. What does `type="module"` do differently from a normal `<script>`?

A module script behaves differently in several key ways:

- It has **`defer` behavior built-in** by default
- It runs in **strict mode** automatically
- It supports `import` / `export` syntax
- It has its own **module scope** (variables don't leak to global)
- It is fetched with **CORS** rules applied

```html
<script type="module" src="main.js"></script>
```

---

### 09. Does a module script block HTML parsing? Why or why not?

**No, it does not block HTML parsing.**

Because `type="module"` is **deferred by default** — the browser downloads the module in the background while continuing to parse HTML, then executes it after parsing is complete. You get non-blocking behavior automatically.

---

### 10. What problem does `defer` solve?

`defer` solves two problems at once:

1. **Placement problem** — No need to put scripts at the bottom of `<body>`; you can declare them in `<head>` cleanly
2. **Timing problem** — Scripts execute after the DOM is ready, so no "element not found" errors

---

### 11. Why doesn't a module script need `defer`?

Because **`defer` is the default behavior of `type="module"`**. The spec defines modules as always deferred. Adding `defer` to a module script is redundant — it already acts exactly like a deferred script.

---

### 12. What extra features do modules provide beyond `defer`?

Beyond deferred loading, `type="module"` also gives you:

| Feature | Description |
|---------|-------------|
| `import` / `export` | Modular code splitting |
| Strict mode | `"use strict"` applied automatically |
| Module scope | No accidental global variable pollution |
| `import.meta` | Access to module metadata (e.g., `import.meta.url`) |
| Top-level `await` | Can use `await` outside of `async` functions |

---

## 🧠 Part 02 — JavaScript Core Language Concepts

### 01. What is a Variable in JS and Naming Convention?

A **variable** is a named container that holds a value in memory.

```js
let name = "Alice";
const PI = 3.14;
var age = 25; // older style, avoid
```

**Naming Conventions:**

| Rule | Example |
|------|---------|
| Use `camelCase` | `firstName`, `userAge` |
| Start with a letter, `_`, or `$` | `_count`, `$value` |
| Case-sensitive | `name` ≠ `Name` |
| No reserved words | ❌ `let let = 5` |
| Descriptive names | ✅ `userEmail` not `x` |

---

### 02. Specifiers — `var`, `let`, `const`

| Specifier | Scope | Re-declare | Re-assign | Hoisted |
|-----------|-------|-----------|-----------|---------|
| `var` | Function | ✅ Yes | ✅ Yes | ✅ Yes (as `undefined`) |
| `let` | Block | ❌ No | ✅ Yes | ⚠️ TDZ |
| `const` | Block | ❌ No | ❌ No | ⚠️ TDZ |

> **TDZ** = Temporal Dead Zone — the variable exists but cannot be accessed before its declaration line.

```js
let score = 10;      // can be reassigned
const MAX = 100;     // cannot be reassigned
var old = "avoid";   // function-scoped, avoid in modern JS
```

---

### 03. Pass by Value

In JavaScript, **primitive values are passed by value** — meaning a copy of the value is made.

```js
let a = 10;
let b = a;  // b gets a COPY of a's value

b = 20;

console.log(a); // 10 — unchanged
console.log(b); // 20
```

Changing `b` has **no effect** on `a` because they hold independent copies.

---

### 04. Primitive vs Non-Primitive

**Primitive Types** (7 total):

```js
string      → "hello"
number      → 42
boolean     → true / false
undefined   → undefined
null        → null
bigint      → 9007199254740991n
symbol      → Symbol("id")
```

**Non-Primitive (Reference) Types:**

```js
object      → { name: "Alice" }
array       → [1, 2, 3]
function    → function() {}
```

> Primitives are **immutable** — you can't change the value itself, only reassign the variable.

---

### 05. How Primitive vs Non-Primitive Values Are Stored in Memory

**Primitives** — stored directly in the **Stack**:

```js
let x = 42;
// Stack: [ x → 42 ]
```

**Non-Primitives (Objects/Arrays)** — the object is stored in the **Heap**, and the variable holds a **reference** (memory address) on the Stack:

```js
let user = { name: "Alice" };
// Stack: [ user → 0x001 ]   ← reference/pointer
// Heap:  [ 0x001 → { name: "Alice" } ]
```

This is why two variables can point to the same object:

```js
let a = { score: 10 };
let b = a;        // both point to the same heap object

b.score = 99;
console.log(a.score); // 99 — same object!
```

---

### 06. Memory — Stack and Heap

| | Stack | Heap |
|-|-------|------|
| **Stores** | Primitives, references | Objects, Arrays, Functions |
| **Size** | Fixed, small | Dynamic, large |
| **Access** | Fast (LIFO order) | Slower (via pointer) |
| **Management** | Auto (call stack) | Garbage collected |
| **Lifetime** | Lives with scope | Lives until no references remain |

```
STACK                    HEAP
┌──────────────┐         ┌──────────────────────────┐
│ x = 42       │         │                          │
│ name = "Ali" │         │  0x001 → { age: 25 }     │
│ obj → 0x001  │ ──────► │  0x002 → [1, 2, 3]       │
└──────────────┘         └──────────────────────────┘
```

---

### 07. How the JS Engine Sees Code

The JavaScript engine processes your code in **two phases**:

#### Phase 1 — Creation / Memory Phase (Hoisting)
Before executing a single line, the engine scans the code and:
- Allocates memory for all `var` variables (set to `undefined`)
- Registers function declarations fully in memory
- Notes `let` / `const` but puts them in TDZ (not accessible yet)

#### Phase 2 — Execution Phase
The engine runs code line by line, assigning actual values.

```js
console.log(a); // undefined (hoisted)
console.log(b); // ❌ ReferenceError (TDZ)

var a = 5;
let b = 10;
```

**Simplified Engine Pipeline:**

```
Source Code
    ↓
Tokenizer / Lexer   → breaks code into tokens
    ↓
Parser              → builds AST (Abstract Syntax Tree)
    ↓
Interpreter         → executes AST (Ignition in V8)
    ↓
JIT Compiler        → optimizes hot code paths (TurboFan in V8)
    ↓
Machine Code        → runs on CPU
```

---

## 🗂️ Quick Reference Summary

### Script Loading Strategies

```
<script>              → Blocks parsing. Executes immediately. ❌ Avoid in <head>
<script defer>        → Non-blocking. Executes after DOM. ✅ Best for most scripts
<script async>        → Non-blocking download, executes ASAP (unordered). ⚠️ Use carefully
<script type="module">→ Deferred by default + ES module features. ✅ Modern standard
```

### Variable Specifiers

```
var   → function-scoped, hoisted, avoid in modern code
let   → block-scoped, reassignable, TDZ applies
const → block-scoped, not reassignable, TDZ applies
```

### Memory Model

```
Primitive  → copied by VALUE  → stored in STACK
Object     → copied by REFERENCE → object in HEAP, pointer in STACK
```

---

## 🧩 Part 02 — 

### 01. What is a variable in JavaScript, and why is it used?

A variable is a **named container that stores a value** in memory. It allows you to label data so you can reference, reuse, and manipulate it throughout your code.

```js
let userName = "Alice";
const MAX_SCORE = 100;
```

Without variables, you'd have to hardcode every value — making code impossible to maintain or reuse.

---

### 02. What are the rules for naming variables in JavaScript?

These are **hard rules** — breaking them causes errors:

- Must start with a letter, `_`, or `$` — not a number
- Cannot contain spaces or special characters (except `_` and `$`)
- Cannot use reserved keywords (`let`, `return`, `class`, etc.)
- Case-sensitive — `name` and `Name` are two different variables

```js
let $price = 10;    // ✅ valid
let _count = 0;     // ✅ valid
let 1score = 5;     // ❌ invalid — starts with number
let let = 3;        // ❌ invalid — reserved keyword
```

---

### 03. What are best practices for writing clean and meaningful variable names?

These are **conventions** — not enforced by JS, but expected by every team:

| Practice | Bad | Good |
|----------|-----|------|
| Use `camelCase` | `username` | `userName` |
| Be descriptive | `x` | `totalPrice` |
| Avoid abbreviations | `usr` | `user` |
| Use `UPPER_SNAKE_CASE` for constants | `maxscore` | `MAX_SCORE` |
| Boolean names should sound like questions | `active` | `isActive`, `hasError` |

```js
// ❌ unclear
let d = 7;
let fn = "Alice";

// ✅ clean and readable
let daysInWeek = 7;
let firstName = "Alice";
```

---

### 04. What is a specifier in JavaScript, and where is it commonly used?

A **specifier** refers to the identifier used to specify *what* you want to import or export in ES Modules. It tells the JS engine exactly which binding to bring in or send out from a module.

```js
// "userName" here is the specifier
import { userName } from "./user.js";
```

The term also loosely refers to `var`, `let`, and `const` as **variable declaration specifiers** — they specify *how* a variable is declared and scoped.

---

### 05. What are the different types of specifiers?

**Variable Declaration Specifiers:**

| Specifier | Scope | Re-assignable | Hoisted |
|-----------|-------|---------------|---------|
| `var` | Function | ✅ Yes | ✅ Yes (`undefined`) |
| `let` | Block | ✅ Yes | ⚠️ TDZ |
| `const` | Block | ❌ No | ⚠️ TDZ |

**ES Module Specifiers:**

```js
// Named export specifier
export { add, subtract };

// Named import specifier
import { add, subtract } from "./math.js";

// Default specifier
export default function greet() {}
import greet from "./greet.js";

// Aliased specifier
import { add as sum } from "./math.js";
```

---

### 06. What does "pass by value" mean in JavaScript?

Pass by value means when you assign or pass a **primitive value**, JavaScript copies the actual value — not a reference to it. The original and the copy are completely independent.

```js
let a = 10;
let b = a;  // b gets a COPY of a's value

b = 99;

console.log(a); // 10 — unchanged
console.log(b); // 99
```

Changing `b` has zero effect on `a`.

---

### 07. How does pass by value behave when passing variables to functions?

The function receives a **copy** of the value. Modifying it inside the function does not affect the original variable outside.

```js
function double(num) {
  num = num * 2;
  console.log(num); // 20
}

let score = 10;
double(score);

console.log(score); // 10 — original untouched
```

The function worked on its own copy of `score`, not the original.

---

### 08. What are primitive data types in JavaScript?

Primitives are the most basic data types. JavaScript has **7 primitive types**:

| Type | Example |
|------|---------|
| `string` | `"hello"` |
| `number` | `42`, `3.14` |
| `boolean` | `true`, `false` |
| `undefined` | `let x;` |
| `null` | `let y = null` |
| `bigint` | `9007199254740991n` |
| `symbol` | `Symbol("id")` |

Primitives are **immutable** — you can't change the value itself, only reassign the variable.

---

### 09. What are non-primitive (reference) data types in JavaScript?

Non-primitives are complex data structures that can hold **collections of values**:

```js
// Object
let user = { name: "Alice", age: 25 };

// Array
let scores = [10, 20, 30];

// Function
function greet() { return "Hello"; }
```

Everything that is not a primitive is an **object** under the hood in JavaScript.

---

### 10. What are the key differences between primitive and non-primitive values?

| | Primitive | Non-Primitive |
|-|-----------|---------------|
| Stored in | Stack | Heap (reference in Stack) |
| Copied by | Value | Reference |
| Mutable? | ❌ No | ✅ Yes |
| Comparison | By value | By reference |
| Examples | `string`, `number`, `boolean` | `object`, `array`, `function` |

---

### 11. How are primitive values stored in memory?

Primitives are stored **directly in the Stack**. The Stack is fast, fixed-size memory that holds the actual value right next to the variable name.

```
STACK
┌─────────────────┐
│ name = "Alice"  │
│ age  = 25       │
│ isOn = true     │
└─────────────────┘
```

The value lives right there — no indirection, no pointer.

---

### 12. How are non-primitive values stored in memory?

Non-primitives are stored in the **Heap**. The variable in the Stack holds only a **reference (memory address)** pointing to where the actual object lives in the Heap.

```
STACK                        HEAP
┌──────────────────┐         ┌──────────────────────────┐
│ user → 0x001     │ ──────► │ 0x001 → { name: "Alice" }│
│ scores → 0x002   │ ──────► │ 0x002 → [10, 20, 30]     │
└──────────────────┘         └──────────────────────────┘
```

---

### 13. Why are non-primitive values called reference types?

Because the variable doesn't hold the object itself — it holds a **reference** (a pointer) to the object's location in Heap memory. When you use the variable, JS follows that pointer to find the actual data.

This is why they're called **reference types** — you're always working through a reference, not with the value directly.

---

### 14. What happens in memory when you copy a primitive vs a non-primitive?

**Primitive — a new copy is made:**

```js
let a = 10;
let b = a;  // new independent copy in Stack
b = 99;
console.log(a); // 10 — unaffected
```

**Non-primitive — the reference is copied, not the object:**

```js
let user1 = { name: "Alice" };
let user2 = user1;  // both point to the SAME heap object

user2.name = "Bob";
console.log(user1.name); // "Bob" — same object!
```

Both `user1` and `user2` are looking at the same place in Heap memory.

---

### 15. How does memory storage affect comparison between primitives and non-primitives?

**Primitives compare by value:**

```js
let a = "hello";
let b = "hello";
console.log(a === b); // true — same value
```

**Non-primitives compare by reference:**

```js
let arr1 = [1, 2, 3];
let arr2 = [1, 2, 3];
console.log(arr1 === arr2); // false — different objects in memory

let arr3 = arr1;
console.log(arr1 === arr3); // true — same reference
```

Even if two objects look identical, they are `===` only if they point to the **exact same object in Heap memory**.

---

## 🗂️ Quick Reference Summary

### Script Loading Strategies

```
<script>               → Blocks parsing. Executes immediately. ❌ Avoid in <head>
<script defer>         → Non-blocking. Executes after DOM. ✅ Best for most scripts
<script async>         → Non-blocking download, executes ASAP (unordered). ⚠️ Use carefully
<script type="module"> → Deferred by default + ES module features. ✅ Modern standard
```

### Variable Specifiers

```
var   → function-scoped, hoisted, avoid in modern code
let   → block-scoped, reassignable, TDZ applies
const → block-scoped, not reassignable, TDZ applies
```

### Memory Model

```
Primitive      → copied by VALUE     → stored in STACK
Non-Primitive  → copied by REFERENCE → object in HEAP, pointer in STACK
```

### Primitive vs Non-Primitive Comparison

```
Primitive      → compared by VALUE     → "hello" === "hello" ✅
Non-Primitive  → compared by REFERENCE → [] === [] ❌ (different objects)
```

---

> 📅 **Day 01 Complete — 19 days to go!**
