# 📘 Day 06 — Lexical Scope & Closures


## 🔗 Part 01 — Lexical Scope & Closures

---

### What is lexical scope? (detailed definition)

**Lexical scope** means that the scope of a variable is determined by **where it is written in the source code** — not where the function is called from or when it runs. So Lexical scope determines which parent/outer scopes a function can access based on where it is defined


```js
let name = "global";

function outer() {
  let name = "outer";

  function inner() {
    console.log(name); // "outer" — not "global"
  }

  inner();
}

outer();
```

`inner` is **written inside** `outer` — so its lexical scope includes `outer`'s variables. It doesn't matter where `inner` is called from. It always looks at where it was **defined**.

```js
let x = "global";

function getX() {
  console.log(x); // always "global" — lexically scoped here
}

function run() {
  let x = "local";
  getX(); // even called from here — still uses "global"
}

run(); // "global" — calling location does NOT change scope
```

---

### Why does an inner function access outer variables?

Because of **lexical scope** — when a function is written inside another function, it is part of that outer function's lexical environment. The inner function has a **reference to the outer scope** baked in at definition time.

```js
function outer() {
  let count = 0; // outer variable

  function inner() {
    console.log(count); // ✅ inner can see count — it's in its lexical scope
  }

  inner();
}
```

The JS engine, during the memory/creation phase, links `inner`'s scope to `outer`'s scope. This link is permanent — it doesn't go away when execution moves around.

---

### What creates the scope chain?

The **nesting of functions and blocks in your source code** creates the scope chain. Every function has an internal reference to the scope where it was **defined** (not called). These references form a chain from inner to outer to global.

```js
let global = "G";

function outer() {
  let outerVar = "O";

  function middle() {
    let middleVar = "M";

    function inner() {
      // Scope chain: inner → middle → outer → global
      console.log(global);    // ✅ found at top
      console.log(outerVar);  // ✅ found in outer
      console.log(middleVar); // ✅ found in middle
    }

    inner();
  }

  middle();
}
```

Each scope has a hidden `[[Environment]]` reference pointing to its parent — this chain is what JS follows when looking up variables.

---


### Why does lexical scope make JS predictable?

Because you can **read the code and know exactly which variables a function will access** — without needing to trace where or when it gets called. The scope is visible in the structure of the code itself.

Without lexical scope (dynamic scope), the same function could behave differently depending on who calls it — which is an unpredictable nightmare to debug.

```js
// With lexical scope — always predictable:
function greet() {
  console.log(message); // you can trace this just by reading
}

let message = "Hello";
greet(); // "Hello" — you knew this from reading the code
```

---

### What is a closure in JavaScript? (detailed definition)

A **closure** is a function that **retains access to its lexical scope** — including the variables from its outer function — **even after the outer function has finished executing**.

More precisely:
> A function remembers and can access variables from its outer scope even after the outer function has finished executing.

When a function is created, it doesn't just store its code — it also stores a **reference to the environment (scope) it was born in**. This is the closure.

```js
function makeCounter() {
  let count = 0; // this variable will be "closed over"

  return function() {
    count++;
    console.log(count);
  };
}

const counter = makeCounter();
// makeCounter() has finished executing — count should be gone, right?

counter(); // 1 — count is still alive!
counter(); // 2 — still remembering!
counter(); // 3
```

`makeCounter` returned the inner function. That inner function **closed over** `count` — it holds a reference to that variable. Even though `makeCounter` is done, `count` lives on because the returned function still needs it.

---

### Why do closures exist?

Closures are a **natural consequence of two JavaScript features working together**:

1. **Functions are first-class values** — they can be returned and passed around
2. **Lexical scope** — a function always carries its definition environment with it

When you return a function, it takes its scope with it. That's a closure. It's not a special feature you "opt into" — it happens automatically whenever a function references variables from an outer scope.

---

### What does a closure remember?

A closure remembers **the variables from its outer scope** — specifically the **live variable bindings**, not copies of values.

```js
function makeAdder(x) {
  return function(y) {
    return x + y; // remembers x from outer scope
  };
}

const add5 = makeAdder(5);
const add10 = makeAdder(10);

add5(3);  // 8  — x is remembered as 5
add10(3); // 13 — x is remembered as 10
```

Each call to `makeAdder` creates a **new closure** with its own `x`. They don't share the same variable.

---

### Does a closure copy variables or reference them?

A closure holds a **reference** to the variable — not a copy of its value. This means if the variable changes, the closure sees the updated value.

```js
function makeCounter() {
  let count = 0;

  return {
    increment() { count++; },
    decrement() { count--; },
    value()     { return count; }
  };
}

const c = makeCounter();
c.increment();
c.increment();
c.increment();
c.decrement();
console.log(c.value()); // 2 — all three methods share the same `count` reference
```

All three methods close over the **same `count` variable** — when one changes it, the others see the change.

---

### Why does the outer variable stay alive after function execution?

Normally, when a function finishes, its **execution context is destroyed** and local variables are garbage collected. But if an inner function (closure) still holds a **reference to those variables**, the garbage collector **cannot remove them** — they must stay alive.

```js
function outer() {
  let secret = "I'm alive!"; // would normally be destroyed

  return function() {
    console.log(secret); // closure holds a reference to secret
  };
}

const fn = outer(); // outer() finishes — but secret lives on
fn();               // "I'm alive!" — because fn still references it
```

JavaScript's garbage collector uses **reachability** — if a value is still reachable through any reference, it stays in memory. The closure keeps `secret` reachable.

---

### How are lexical scope and closures related?

**Closures are built on top of lexical scope.** Lexical scope defines what variables a function can see. Closures are what happens when that function is returned or passed elsewhere — it takes its lexical scope with it.

```
Lexical scope  → determines WHAT variables a function can access
Closure        → preserves THAT ACCESS even after the outer function is done
```

Without lexical scope, closures couldn't exist. Lexical scope is the rule. Closure is the consequence of that rule when functions outlive their creation context.

---

### What real-world features use closures?

Closures are everywhere in JavaScript:

| Feature | How closure is used |
|---------|---------------------|
| **Event handlers** | Callback remembers variables from outer scope |
| **setTimeout / setInterval** | Callback accesses outer variables after delay |
| **Module pattern** | Private variables exposed through public functions |
| **React hooks** (`useState`, `useEffect`) | Hooks close over component state and props |
| **Memoization** | Cached results stored in closed-over variables |
| **Currying** | Functions remember earlier arguments |
| **Factory functions** | Each instance gets its own closed-over state |

```js
// Event handler closure
function setupButton(buttonText) {
  let clicks = 0;

  document.getElementById("btn").addEventListener("click", function() {
    clicks++; // closes over clicks
    console.log(`${buttonText} clicked ${clicks} times`);
  });
}
```

---

### What is preserved in memory during a closure?

The **entire lexical environment** that the inner function needs — specifically:

- The **variables** that the inner function references from outer scope
- Their **current values** (as live references, not copies)
- The **scope chain** back up to global

Note: JS engines are smart — they only keep variables that are actually **used** by the closure, not the entire outer function's scope indiscriminately.

```js
function outer() {
  let used = "I'm needed";
  let unused = "Nobody references me";

  return function() {
    console.log(used); // only `used` is preserved in the closure
  };
}
```

---

### Can closures modify outer variables?

**Yes** — because closures hold a **reference** to the variable, not a copy. They can both read and write to it.

```js
function makeCounter() {
  let count = 0;

  return {
    increment() { count++; },   // modifies outer variable
    reset()     { count = 0; }, // modifies outer variable
    get()       { return count; }
  };
}

const counter = makeCounter();
counter.increment();
counter.increment();
console.log(counter.get()); // 2
counter.reset();
console.log(counter.get()); // 0
```

---

### Why doesn't normal function scope preserve state across calls?

Because every function call creates a **brand new execution context** with fresh local variables. Once the function returns, that context is destroyed. The next call starts from scratch.

```js
function counter() {
  let count = 0; // created fresh every single call
  count++;
  console.log(count);
}

counter(); // 1
counter(); // 1 — not 2! count was reset
counter(); // 1 — still 1! new scope every time
```

There is no memory between calls. Each call is isolated. This is why closures are needed when you want to **persist state** across multiple calls.

---

### What exactly does a closure preserve?

A closure preserves the **variable binding** from the outer lexical environment — the actual live variable, not a snapshot of its value.

To be precise, a closure preserves:

1. **The variable itself** (the binding/slot in memory) — not a copy
2. **The scope chain** needed to resolve any further lookups
3. **Persistence across calls** — the variable doesn't reset between calls to the closure

```js
function make() {
  let x = 0;          // one variable binding

  return function() {
    x++;              // same binding, every call
    return x;
  };
}

const fn = make();
fn(); // 1 — x was 0, now 1
fn(); // 2 — x was 1, now 2 (same x, not reset)
fn(); // 3
```

---

### "Variables are already private inside functions because of function scope… so why do people say closures create private variables?"

This is a great question — and the distinction is subtle but important.

**Function scope alone gives you privacy within one call.** The variable exists privately while the function runs, then it's gone. You can't access it from outside during that call, but you also can't access it after the call either — it's destroyed.

**Closures give you persistent private variables** — variables that are private from the outside world AND survive between calls through the returned function.

```js
// Function scope — private but temporary
function demo() {
  let secret = "hidden"; // private ✅ but destroyed after this call ❌
}
demo();
// secret is gone — you can't access it, but neither can you keep it

// Closure — private AND persistent
function makeVault() {
  let secret = "hidden"; // private ✅ AND survives ✅

  return {
    get() { return secret; },
    set(val) { secret = val; }
  };
}

const vault = makeVault();
console.log(vault.get()); // "hidden" — readable through the API
vault.set("new secret");
console.log(vault.get()); // "new secret"
// secret itself is never directly accessible — only through get/set
```

| | Function Scope | Closure |
|-|---------------|---------|
| Private from outside? | ✅ Yes | ✅ Yes |
| Survives after function returns? | ❌ No — destroyed | ✅ Yes — kept alive |
| Accessible across multiple calls? | ❌ No — resets | ✅ Yes — persists |
| Use case | Temporary isolation | Stateful private data |

> **Function scope creates temporary privacy.**
> **Closures create persistent privacy.**
> That's the real difference.

---

## 🗂️ Quick Reference Summary

### Lexical Scope

```
Scope is determined by WHERE the function is WRITTEN — not where it's called.
Calling location never changes a function's scope.
The engine resolves scope at parse/lex time — hence "lexical."
```

### Closure

```
A closure = function + its lexical environment (the scope it was born in)
Closures hold REFERENCES to variables — not copies
Outer variables stay alive as long as a closure references them
Closures can READ and MODIFY outer variables
```

### Lexical Scope vs Closure

```
Lexical scope → determines what a function CAN access (static rule)
Closure       → preserves that access after the outer context is gone (runtime behavior)
```

### Function Scope vs Closure Privacy

```
Function scope → private but temporary (destroyed after call)
Closure        → private AND persistent (survives across calls)
```

### Common Closure Use Cases

```
✅ Stateful functions (counters, toggles)
✅ Private variables (module pattern)
✅ Event handlers remembering outer state
✅ setTimeout / setInterval callbacks
✅ React hooks (useState, useEffect)
✅ Currying and partial application
✅ Memoization / caching
```

---

> 📅 **Day 06 Part 01 Complete — 14 days to go!**
