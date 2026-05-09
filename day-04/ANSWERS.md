## Answers

---

### What is the Global Execution Context?

The **Global Execution Context (GEC)** is the default environment in which JavaScript code runs. It is the **very first context created** when a JS program starts — before any function is called.

It gives your code access to:
- The global object (`window` in browsers, `global` in Node.js)
- The `this` keyword (at global level, `this === window` in browsers)
- All globally declared variables and functions

```js
var name = "Alice";   // lives in GEC
function greet() {}   // lives in GEC

console.log(this);    // window (in browser)
```

---

### When is it created?

The GEC is created **the moment JavaScript starts executing your script** — before any line of your code runs. It is the foundation everything else sits on top of.

---

### What are the two phases of GEC?

| Phase | Also Called | What Happens |
|-------|-------------|--------------|
| Phase 1 | **Memory / Creation Phase** | Variables and functions are allocated in memory |
| Phase 2 | **Execution Phase** | Code is executed line by line |

---

### What happens in the memory phase?

The JS engine scans the entire code **before executing a single line** and:

- Allocates memory for all **`var` variables** → set to `undefined`
- Stores **function declarations** in memory completely (the full function body)
- Notes `let` and `const` but puts them in the **Temporal Dead Zone (TDZ)**

```js
console.log(a); // undefined — hoisted in memory phase
console.log(b); // ❌ ReferenceError — TDZ

var a = 10;
let b = 20;

// Memory phase snapshot:
// a → undefined
// b → TDZ (not accessible yet)
// greet → [full function stored]
function greet() {}
```

---

### What happens in the execution phase?

The JS engine goes through the code **line by line** and:

- Assigns actual values to variables
- Executes function calls
- Performs all operations

```js
var a = 10;
// Memory phase: a = undefined
// Execution phase: a = 10  ← value assigned now

console.log(a); // 10
```

---

### Is there only one GEC or many?

**Only one.** There is exactly **one Global Execution Context** per JavaScript program. It is created once at the start and destroyed when the program finishes.

```
Call Stack at program start:
┌─────────────────────┐
│  Global EC          │  ← only one, always at the bottom
└─────────────────────┘
```

---

### How is GEC related to the call stack?

The GEC is **always the first item pushed onto the call stack** when a program starts. It sits at the bottom and is the last to be removed — only when the entire program finishes executing.

```
Program starts:          Function called:        Function returns:
┌──────────────┐         ┌──────────────┐        ┌──────────────┐
│              │         │  greet() FEC │        │              │
│  Global EC   │    →    │  Global EC   │   →    │  Global EC   │
└──────────────┘         └──────────────┘        └──────────────┘
```

---

### When is FEC created?

A **Function Execution Context (FEC)** is created **every time a function is called** — not when it is defined, but when it is actually invoked.

```js
function greet() {
  console.log("Hi");
}

// FEC created here ↓
greet(); // FEC for greet() is created, runs, then destroyed
greet(); // a brand new FEC is created again
```

---

### How is FEC related to GEC?

The FEC is **created on top of the GEC** in the call stack. It has access to the GEC's global variables through the **scope chain**, but it also has its own local memory for its own variables.

```
┌──────────────────┐
│  greet() FEC     │  ← local scope: own variables
│  Global EC       │  ← global scope: accessible from FEC
└──────────────────┘
```

---

### What are the two phases inside FEC?

Same as GEC — every execution context has two phases:

| Phase | What Happens |
|-------|-------------|
| **Memory / Creation Phase** | Local variables set to `undefined`, parameters assigned, inner functions stored |
| **Execution Phase** | Function body runs line by line |

```js
function add(a, b) {
  // Memory phase: a = undefined, b = undefined, result = undefined
  // Execution phase: a = 2, b = 3, result = 5
  let result = a + b;
  return result;
}

add(2, 3);
```

---

### Does every function call create a new FEC?

**Yes — every single call.** Even if you call the same function multiple times, each call creates its own **brand new FEC** with its own memory space.

```js
function greet(name) {
  let msg = "Hello " + name;
  return msg;
}

greet("Alice"); // FEC 1 created → runs → destroyed
greet("Bob");   // FEC 2 created → runs → destroyed
// Each call gets its own isolated `msg` variable
```

---

### What happens to FEC after function execution?

Once the function finishes (returns a value or reaches the end), its FEC is **popped off the call stack and destroyed**. All local variables inside it are cleared from memory.

```
Before return:           After return:
┌──────────────┐         ┌──────────────┐
│  greet() FEC │    →    │              │
│  Global EC   │         │  Global EC   │
└──────────────┘         └──────────────┘
FEC destroyed ✅
```

---

### Is FEC part of the call stack?

**Yes.** Every time a function is called, its FEC is **pushed onto the call stack**. When the function returns, the FEC is **popped off**. The call stack is essentially a stack of execution contexts.

```
Call stack with nested calls:
┌──────────────────┐
│  multiply() FEC  │  ← currently executing
│  add() FEC       │  ← waiting
│  Global EC       │  ← always at bottom
└──────────────────┘
```

---

### Is FEC stored in the call stack or heap?

The **FEC itself is stored in the call stack** (specifically its record/frame). However:

- **Primitive values** inside the function → stored in the stack
- **Objects and arrays** created inside the function → stored in the **heap**, with their reference held in the stack

```js
function demo() {
  let num = 42;              // stack → value stored directly
  let obj = { name: "Al" }; // stack → holds reference | heap → stores actual object
}
```

---

### What does the call stack mainly store?

The call stack stores **execution contexts** — specifically:

- The current **execution context** (which function is running)
- **Local primitive variables** and their values
- **References (pointers)** to objects/arrays that live in the heap
- The **return address** — where to go back after the function finishes

---

### What does the stack usually store for objects?

For objects and arrays, the stack stores only the **reference (memory address / pointer)** — not the actual data. The actual object data lives in the **heap**.

```js
function demo() {
  let user = { name: "Alice" };
  //  ↑
  // Stack: user → 0x001 (pointer)
  // Heap:  0x001 → { name: "Alice" } (actual data)
}
```

```
STACK (Call Stack / FEC)       HEAP
┌───────────────────────┐      ┌──────────────────────────┐
│ num = 42              │      │                          │
│ user → 0x001          │ ───► │ 0x001 → { name: "Alice" }│
└───────────────────────┘      └──────────────────────────┘
```

---

## 🗂️ Quick Reference Summary

### Execution Context Types

```
Global EC (GEC)    → created once at program start, lives at bottom of call stack
Function EC (FEC)  → created every time a function is called, destroyed after return
```

### Two Phases — Both GEC and FEC

```
Phase 1 — Memory / Creation:
  var        → allocated, set to undefined
  function   → fully stored in memory
  let/const  → noted but in TDZ (not accessible)

Phase 2 — Execution:
  Code runs line by line
  Variables get their actual values
  Functions get called
```

### Call Stack Behavior

```
Program starts     → GEC pushed onto stack
Function called    → FEC pushed on top
Function returns   → FEC popped off, destroyed
Program ends       → GEC popped off
```

### Stack vs Heap — What Goes Where

```
Stack  → primitives, references (pointers), execution context frames
Heap   → objects, arrays, functions (the actual data)
```

### Key Rules

```
✅ Only ONE GEC per program
✅ One NEW FEC per function call
✅ FEC is destroyed after function returns
✅ Call stack = stack of execution contexts
✅ Objects live in heap, their pointers live in stack
```

---

> 📅 **Day 04 Complete — 16 days to go!**
