# 📘 Day 06 — Lexical Scope, Closures and Objects


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

# Day 06 — Answers: JavaScript Objects Deep Dive (part -2)

> A complete, practical reference for everything about JavaScript Objects — from creation to copying, references to destructuring. Each answer includes explanation, code examples, and real-world context.

---

## 1. What is an Object in JavaScript?

An **object** is a collection of **key-value pairs** (also called properties). It is the most fundamental and versatile data structure in JavaScript — almost everything in JS is an object or can behave like one.

```js
const user = {
  name: "Rafiq",       // key: "name", value: "Rafiq"
  age: 24,             // key: "age",  value: 24
  isAdmin: false       // key: "isAdmin", value: false
};
```

Objects let you group related data and behavior together. In real apps, every API response, database document, React prop, and config is an object.

---

## 2. How Many Ways Are There to Create an Object?

There are **5 main ways** to create an object in JavaScript:

| # | Method | Syntax |
|---|--------|--------|
| 1 | Object Literal | `const obj = {}` |
| 2 | Constructor Function | `function Person() {} → new Person()` |
| 3 | `Object` Constructor | `new Object()` |
| 4 | Factory Function | `function createUser() { return {} }` |
| 5 | `Object.create()` | `Object.create(proto)` |

```js
// 1. Object Literal
const obj1 = { name: "Alice" };

// 2. Constructor Function
function Person(name) { this.name = name; }
const obj2 = new Person("Bob");

// 3. Object Constructor
const obj3 = new Object();
obj3.name = "Carol";

// 4. Factory Function
function createUser(name) { return { name }; }
const obj4 = createUser("Dave");

// 5. Object.create()
const proto = { greet() { return "Hi!"; } };
const obj5 = Object.create(proto);
obj5.name = "Eve";
```

---

## 3. What is an Object Literal?

An **object literal** is the simplest and most common way to create an object — by directly writing key-value pairs inside curly braces `{}`.

```js
const car = {
  brand: "Toyota",
  year: 2022,
  start() {
    console.log("Vroom!");
  }
};
```

It's called a "literal" because you're *literally* writing the object's value in the source code, rather than constructing it programmatically. This is the go-to syntax in modern JavaScript.

---

## 4. What is the Difference Between Dot and Bracket Notation?

Both access or set object properties, but they work differently:

| Feature | Dot Notation | Bracket Notation |
|---------|-------------|-----------------|
| Syntax | `obj.key` | `obj["key"]` |
| Key type | Must be a valid identifier | Any string or expression |
| Dynamic keys | ❌ No | ✅ Yes |
| Special characters | ❌ No | ✅ Yes |
| Readability | ✅ Cleaner | Slightly verbose |

```js
const user = { name: "Rafiq", age: 24 };

// Dot notation
console.log(user.name);   // "Rafiq"

// Bracket notation
console.log(user["age"]); // 24
```

---

## 5. When Should Bracket Notation Be Used?

### Dynamic Keys (computed at runtime)

```js
const key = "name";
const user = { name: "Rafiq" };

console.log(user[key]);      // ✅ "Rafiq"
console.log(user.key);       // ❌ undefined — looks for a key literally named "key"
```

This is extremely useful when working with form fields, API responses, or any case where the property name is determined at runtime.

### Special Characters in Keys

```js
const obj = {
  "first-name": "Rafiq",    // hyphen not allowed in dot notation
  "123abc": true,            // starts with a number
  "has space": "yes"
};

console.log(obj["first-name"]);  // ✅ "Rafiq"
console.log(obj["123abc"]);      // ✅ true
// obj.first-name  → ❌ SyntaxError
```

### Real-World Example

```js
function getProperty(obj, propName) {
  return obj[propName]; // dynamic — can't use dot notation here
}

const person = { name: "Alice", age: 30 };
console.log(getProperty(person, "name")); // "Alice"
```

---

## 6. How Do You Add a Property to an Object?

You can add a property at any time — objects in JS are dynamic by default.

```js
const user = { name: "Rafiq" };

// Dot notation
user.age = 24;

// Bracket notation
user["email"] = "rafiq@example.com";

// Dynamic key
const key = "city";
user[key] = "Sylhet";

console.log(user);
// { name: "Rafiq", age: 24, email: "rafiq@example.com", city: "Sylhet" }
```

---

## 7. How Do You Delete a Property?

Use the `delete` operator. It removes both the key and the value.

```js
const user = { name: "Rafiq", age: 24, temp: "remove me" };

delete user.temp;

console.log(user); // { name: "Rafiq", age: 24 }
console.log(user.temp); // undefined
```

> **Note:** `delete` returns `true` on success. It does not affect variables, only object properties.

---

## 8. What is a Constructor Function?

A **constructor function** is a regular function used as a blueprint to create multiple similar objects. By convention, the name starts with a **capital letter**.

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.greet = function () {
    return `Hi, I'm ${this.name}`;
  };
}

const alice = new Person("Alice", 30);
const bob   = new Person("Bob", 25);

console.log(alice.greet()); // "Hi, I'm Alice"
console.log(bob.name);      // "Bob"
```

When called with `new`, JS automatically creates a new empty object, sets `this` to it, and returns it.

---

## 9. What Does `this` Refer to Inside Constructor Functions?

Inside a constructor function called with `new`, `this` refers to the **newly created object instance**.

```js
function Car(brand) {
  console.log(this); // {} ← the new empty object being built
  this.brand = brand;
}

const myCar = new Car("Toyota");
console.log(myCar.brand); // "Toyota"
```

Each call to `new Car(...)` creates a fresh object, and `this` points to that specific instance.

---

## 10. What Happens If You Forget `new`?

Without `new`, the function runs as a regular function — `this` refers to the **global object** (`window` in browsers) or is `undefined` in strict mode.

```js
function Person(name) {
  this.name = name; // ⚠️ this = window (non-strict) or crashes (strict)
}

const p = Person("Rafiq"); // forgot new
console.log(p);            // undefined — no return value
console.log(window.name);  // "Rafiq" — polluted the global scope! 😱
```

In **strict mode** (`"use strict"`), `this` is `undefined` and it throws a `TypeError`. Always use `new` with constructor functions.

---

## 11. Why Are Constructor Functions Useful?

They allow you to **create many objects with the same shape** without repeating yourself.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;
  this.describe = function () {
    return `${this.name} costs $${this.price}`;
  };
}

const p1 = new Product("Laptop", 999);
const p2 = new Product("Phone", 499);
const p3 = new Product("Tablet", 299);

console.log(p1.describe()); // "Laptop costs $999"
```

This is the predecessor to ES6 `class` syntax, which is just syntactic sugar over constructor functions.

---

## 12. What is the `Object` Constructor in JavaScript?

`Object` is a built-in constructor function in JS. You can use `new Object()` to create an empty object, just like `{}`.

```js
const obj = new Object();
obj.name = "Rafiq";
obj.age = 24;

console.log(obj); // { name: "Rafiq", age: 24 }
```

`Object` also provides many static utility methods like `Object.keys()`, `Object.assign()`, `Object.freeze()`, etc.

---

## 13. Why Do We Usually Prefer `{}` Over `new Object()`?

| Reason | `{}` (Literal) | `new Object()` |
|--------|---------------|----------------|
| Conciseness | ✅ Short and clean | ❌ Verbose |
| Performance | ✅ Slightly faster | Slightly slower |
| Readability | ✅ Immediately obvious | Less natural |
| Can be overridden | N/A | ⚠️ `Object` can be shadowed |

```js
// ✅ Preferred
const user = { name: "Rafiq" };

// ❌ Avoid
const user = new Object();
user.name = "Rafiq";
```

The literal syntax is idiomatic JavaScript. Only use `new Object()` if you need to pass a value to the constructor (e.g., `new Object(42)` wraps a primitive — but this is rare).

---

## 14. Do Object Literals and Constructor Objects Represent the Same Object?

They produce **equivalent objects** in terms of structure and behavior, but they are **separate instances** — each is its own object in memory.

```js
const a = { x: 1 };
const b = new Object();
b.x = 1;

console.log(a.x === b.x); // true — same values
console.log(a === b);      // false — different references in memory
```

Both are plain objects (`{}`) — no functional difference in day-to-day use.

---

## 15. Object Creation Using Factory Functions

A **factory function** is a plain function (no `new`) that creates and returns an object. It's a clean alternative to constructor functions, especially when you want to avoid `this` and `new`.

```js
function createUser(name, role) {
  return {
    name,
    role,
    greet() {
      return `Hello, I'm ${name} and I'm a ${role}`;
    }
  };
}

const admin = createUser("Rafiq", "admin");
const guest = createUser("Rahim", "guest");

console.log(admin.greet()); // "Hello, I'm Rafiq and I'm a admin"
console.log(guest.greet()); // "Hello, I'm Rahim and I'm a guest"
```

### Factory vs Constructor

| Feature | Factory Function | Constructor Function |
|---------|-----------------|---------------------|
| `new` required | ❌ No | ✅ Yes |
| `this` used | ❌ No (optional) | ✅ Yes |
| Risk of global pollution | ❌ None | ⚠️ If `new` is forgotten |
| Encapsulation (closures) | ✅ Easy | More complex |

Factory functions are widely used in professional JS — especially in functional programming patterns.

---

## 16. What is Object Shorthand?

ES6 introduced **shorthand property and method syntax** to reduce repetition.

### Shorthand Properties

When a variable name matches the key name, you can skip the repetition:

```js
const name = "Rafiq";
const age = 24;

// Old way
const user = { name: name, age: age };

// ✅ ES6 Shorthand
const user = { name, age };

console.log(user); // { name: "Rafiq", age: 24 }
```

### Shorthand Methods

```js
// Old way
const obj = {
  greet: function() {
    return "Hello!";
  }
};

// ✅ ES6 Shorthand
const obj = {
  greet() {
    return "Hello!";
  }
};
```

You'll see this everywhere — in React components, Express route handlers, and any modern JS codebase.

---

## 17. What are Object Methods?

A **method** is a function stored as a property of an object. It gives the object behavior, not just data.

```js
const calculator = {
  value: 0,

  add(n) {
    this.value += n;
    return this; // enables chaining
  },

  subtract(n) {
    this.value -= n;
    return this;
  },

  result() {
    return this.value;
  }
};

console.log(calculator.add(10).add(5).subtract(3).result()); // 12
```

Inside a method, `this` refers to the object the method belongs to. Methods make objects self-contained units with both data and behavior.

---

## 18. How Does the `in` Operator Work with Objects?

The `in` operator returns `true` if a **key exists** in an object (including inherited properties).

```js
const user = { name: "Rafiq", age: 24 };

console.log("name" in user);    // true
console.log("email" in user);   // false
console.log("toString" in user); // true ← inherited from Object.prototype
```

### Use Cases

```js
// Guard clause — don't access what doesn't exist
function greet(user) {
  if ("name" in user) {
    return `Hello, ${user.name}`;
  }
  return "Hello, stranger";
}

// Check before using a feature
if ("geolocation" in navigator) {
  // Safe to use navigator.geolocation
}
```

> Use `Object.hasOwn(obj, key)` when you want to check **own properties only** (not inherited).

---

## 19. What is the Use Case of `for...in` Loop?

`for...in` iterates over **all enumerable string keys** of an object, including inherited ones.

```js
const user = { name: "Rafiq", age: 24, city: "Sylhet" };

for (const key in user) {
  console.log(`${key}: ${user[key]}`);
}
// name: Rafiq
// age: 24
// city: Sylhet
```

### Common Use Cases

```js
// Dynamic key access
const settings = { theme: "dark", lang: "en", fontSize: 14 };

for (const key in settings) {
  console.log(`Setting ${key} = ${settings[key]}`);
}
```

### Important Warning

`for...in` also loops over **inherited (prototype) properties**. Always use `hasOwnProperty` or `Object.hasOwn()` if that matters:

```js
for (const key in user) {
  if (Object.hasOwn(user, key)) {
    console.log(key, user[key]); // only own properties
  }
}
```

> Prefer `Object.keys()` + `forEach` or `for...of` for modern code.

---

## 20. `Object.keys()`, `Object.values()`, `Object.entries()`, `Object.fromEntries()`

These are essential static utility methods for working with objects.

### `Object.keys(obj)`
Returns an array of the object's **own enumerable keys**.

```js
const user = { name: "Rafiq", age: 24 };
console.log(Object.keys(user)); // ["name", "age"]
```

### `Object.values(obj)`
Returns an array of the object's **own enumerable values**.

```js
console.log(Object.values(user)); // ["Rafiq", 24]
```

### `Object.entries(obj)`
Returns an array of **[key, value] pairs**.

```js
console.log(Object.entries(user));
// [["name", "Rafiq"], ["age", 24]]
```

### `Object.fromEntries(iterable)`
The **reverse** of `Object.entries()` — converts an array of `[key, value]` pairs back into an object.

```js
const entries = [["name", "Rafiq"], ["age", 24]];
const obj = Object.fromEntries(entries);
console.log(obj); // { name: "Rafiq", age: 24 }
```

### Power Combo — Transform Object Values

```js
const prices = { apple: 1.5, banana: 0.5, cherry: 3.0 };

// Apply 10% discount to all prices
const discounted = Object.fromEntries(
  Object.entries(prices).map(([key, val]) => [key, val * 0.9])
);

console.log(discounted);
// { apple: 1.35, banana: 0.45, cherry: 2.7 }
```

---

## 21. What is an Object Reference?

When you create an object, it is stored in the **heap** (a region of memory). The variable doesn't hold the object itself — it holds a **reference** (a pointer/address) to where the object lives in memory.

```js
const user = { name: "Rafiq" };
//    ↑ variable holds a memory address, e.g., 0x001
//    The actual object { name: "Rafiq" } lives at 0x001
```

---

## 22. Do Variables Store Actual Objects Directly?

**No.** Variables store a **reference** (memory address) to the object, not the object itself.

```
Stack (variables):         Heap (actual objects):
┌─────────┐               ┌──────────────────────┐
│  user   │──────────────▶│  { name: "Rafiq" }   │
└─────────┘               └──────────────────────┘
```

Primitives (numbers, strings, booleans) *are* stored directly by value. Objects are not.

---

## 23. Why Does Changing One Object Variable Affect Another?

Because both variables point to the **same memory location**.

```js
const a = { name: "Rafiq" };
const b = a; // b now points to the SAME object

b.name = "Karim";

console.log(a.name); // "Karim" ← a was affected!
console.log(b.name); // "Karim"
```

```
Stack:         Heap:
┌───┐          ┌────────────────────┐
│ a │─────────▶│ { name: "Karim" } │
└───┘          └────────────────────┘
┌───┐                ▲
│ b │────────────────┘  (same object)
└───┘
```

To avoid this, create a copy using spread `{...a}` or `Object.assign()`.

---

## 24. Why is `{x:1} === {x:1}` false?

Because the `===` operator compares **object references**, not their contents.

```js
const a = { x: 1 };
const b = { x: 1 };

console.log(a === b); // false — two separate objects in memory
console.log(a === a); // true  — same reference
```

Each `{}` creates a **new object** at a new memory address. Even if they look identical, they're different objects.

To compare object contents, you need a deep comparison:

```js
JSON.stringify(a) === JSON.stringify(b); // true (simple cases)
// Or use a library like lodash: _.isEqual(a, b)
```

---

## 25. Is JS Technically Pass-by-Reference?

This is a classic nuance. JavaScript is **pass-by-value** — but for objects, the **value being passed is a reference**.

```js
function rename(obj) {
  obj.name = "Changed"; // modifies the original object ✅
}

function replace(obj) {
  obj = { name: "New Object" }; // only changes local variable ❌
}

const user = { name: "Rafiq" };
rename(user);
console.log(user.name); // "Changed"

replace(user);
console.log(user.name); // still "Changed" — replace had no effect outside
```

So: the reference is passed by value. You can mutate what the reference points to, but you can't reassign the original variable.

---

## 26. What is Pass by Value and Pass by Reference?

### Pass by Value (Primitives)

A **copy** of the value is passed. Changing it inside a function doesn't affect the original.

```js
let x = 10;

function double(n) {
  n = n * 2;
  console.log(n); // 20
}

double(x);
console.log(x); // 10 — unchanged
```

### Pass by Reference (Objects — sort of)

The **reference** (address) is passed. Mutating the object affects the original.

```js
const user = { score: 10 };

function addPoints(obj) {
  obj.score += 5; // mutates via reference
}

addPoints(user);
console.log(user.score); // 15 ← changed!
```

| Type | Behavior | Examples |
|------|----------|----------|
| Primitive | Pass by value (copy) | `number`, `string`, `boolean`, `null`, `undefined` |
| Object | Pass reference by value | `{}`, `[]`, `function` |

---

## 27. What Does `Object.assign()` Do?

`Object.assign(target, ...sources)` **copies enumerable own properties** from one or more source objects into a target object. It returns the modified target.

```js
const target = { a: 1 };
const source = { b: 2, c: 3 };

const result = Object.assign(target, source);
console.log(result); // { a: 1, b: 2, c: 3 }
console.log(target); // { a: 1, b: 2, c: 3 } ← target is mutated!
```

### Creating a Copy (avoid mutating original)

```js
const original = { name: "Rafiq", age: 24 };
const copy = Object.assign({}, original);

copy.name = "Karim";
console.log(original.name); // "Rafiq" — safe
```

### Is it a Deep Copy?

**No — it's a shallow copy.** Nested objects are still shared by reference.

```js
const obj = { name: "Rafiq", address: { city: "Sylhet" } };
const copy = Object.assign({}, obj);

copy.address.city = "Dhaka"; // ⚠️ modifies the original too!
console.log(obj.address.city); // "Dhaka"
```

### Modern Replacement — Spread Operator

```js
const copy = { ...original }; // same as Object.assign({}, original)

// Merge multiple objects
const merged = { ...obj1, ...obj2, extra: "value" };
```

The spread operator `{...obj}` is cleaner and preferred in modern code.

---

## 28. Shallow vs Deep Copy

### Shallow Copy

Copies only the **top-level** properties. Nested objects still share the same reference.

```js
const original = {
  name: "Rafiq",
  address: { city: "Sylhet" }
};

const shallow = { ...original };

shallow.name = "Karim";           // ✅ doesn't affect original
shallow.address.city = "Dhaka";   // ❌ affects original!

console.log(original.name);         // "Rafiq"
console.log(original.address.city); // "Dhaka" ← leaked mutation
```

Methods for shallow copy: `{...obj}`, `Object.assign({}, obj)`.

### Deep Copy

Copies **all levels** recursively. No shared references.

```js
const original = { name: "Rafiq", address: { city: "Sylhet" } };

// Method 1: JSON (simple but loses functions, undefined, Date, etc.)
const deep = JSON.parse(JSON.stringify(original));

// Method 2: structuredClone (modern, handles more types)
const deep2 = structuredClone(original);

deep2.address.city = "Dhaka";
console.log(original.address.city); // "Sylhet" ✅ safe!
```

| Method | Deep? | Handles Functions? | Handles `Date`? |
|--------|-------|--------------------|-----------------|
| `{...obj}` | ❌ Shallow | ✅ | ✅ |
| `Object.assign` | ❌ Shallow | ✅ | ✅ |
| `JSON.parse(JSON.stringify)` | ✅ Deep | ❌ | ❌ |
| `structuredClone()` | ✅ Deep | ❌ | ✅ |
| Lodash `_.cloneDeep()` | ✅ Deep | ✅ | ✅ |

---

## 29. `Object.freeze()` vs `Object.seal()`

Both restrict object mutation, but to different degrees.

### `Object.freeze(obj)`

Completely locks the object:
- ❌ Cannot add new properties
- ❌ Cannot remove properties
- ❌ Cannot change existing values

```js
const config = Object.freeze({ env: "production", port: 3000 });

config.port = 8080;    // ❌ silently ignored (or throws in strict mode)
config.debug = true;   // ❌ ignored
delete config.env;     // ❌ ignored

console.log(config); // { env: "production", port: 3000 }
```

> **Note:** `freeze()` is **shallow** — nested objects can still be mutated.

```js
const obj = Object.freeze({ nested: { x: 1 } });
obj.nested.x = 99; // ✅ works! nested is not frozen
```

### `Object.seal(obj)`

Partially locks the object:
- ❌ Cannot add new properties
- ❌ Cannot remove properties
- ✅ **Can** change existing values

```js
const user = Object.seal({ name: "Rafiq", age: 24 });

user.name = "Karim";  // ✅ allowed — changing existing
user.email = "...";   // ❌ ignored — can't add new
delete user.age;      // ❌ ignored — can't delete

console.log(user); // { name: "Karim", age: 24 }
```

### Summary Table

| Operation | `freeze()` | `seal()` |
|-----------|-----------|---------|
| Add property | ❌ | ❌ |
| Delete property | ❌ | ❌ |
| Modify existing | ❌ | ✅ |
| Nested objects affected | ❌ (shallow) | ❌ (shallow) |

Use `freeze()` for config objects, constants, or any data that must never change. Use `seal()` when you want to lock the shape but allow value updates.

---

## 30. What Are Static Methods in JS?

**Static methods** belong to the **class or constructor itself**, not to instances. You call them directly on the class/constructor, not on objects created from it.

```js
class MathHelper {
  static square(n) {
    return n * n;
  }

  static cube(n) {
    return n * n * n;
  }
}

console.log(MathHelper.square(4)); // 16
console.log(MathHelper.cube(3));   // 27

const m = new MathHelper();
// m.square(4); // ❌ TypeError — not available on instances
```

### Real-World Example

```js
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  static isValidEmail(email) {
    return email.includes("@");
  }

  static create(name, email) {
    if (!User.isValidEmail(email)) throw new Error("Invalid email");
    return new User(name, email);
  }
}

const u = User.create("Rafiq", "rafiq@example.com"); // ✅
```

All the `Object.keys()`, `Object.assign()`, `Object.freeze()` etc. methods are static methods on the `Object` constructor.

---

## 31. `Object.hasOwn()` Method

`Object.hasOwn(obj, key)` checks whether a property is a **direct (own) property** of the object — not inherited from the prototype chain.

```js
const user = { name: "Rafiq", age: 24 };

console.log(Object.hasOwn(user, "name"));     // true
console.log(Object.hasOwn(user, "age"));      // true
console.log(Object.hasOwn(user, "toString")); // false ← inherited
```

### Why Use It Over `hasOwnProperty`?

The old way was `obj.hasOwnProperty(key)`, but it could fail if:
- The object has its own `hasOwnProperty` property that overrides the built-in
- The object was created with `Object.create(null)` (no prototype)

```js
// Edge case where hasOwnProperty breaks
const obj = Object.create(null); // no prototype
obj.name = "Rafiq";

// obj.hasOwnProperty("name"); // ❌ TypeError: obj.hasOwnProperty is not a function

Object.hasOwn(obj, "name"); // ✅ always works
```

`Object.hasOwn()` is the modern, safe, and recommended alternative. It was introduced in ES2022.

---

## 32. Object Destructuring (& Nested)

**Destructuring** is a concise syntax to extract values from objects into variables.

### Basic Destructuring

```js
const user = { name: "Rafiq", age: 24, city: "Sylhet" };

// Old way
const name = user.name;
const age  = user.age;

// ✅ Destructuring
const { name, age, city } = user;

console.log(name); // "Rafiq"
console.log(age);  // 24
```

### Default Values

```js
const { name, role = "user" } = { name: "Rafiq" };
console.log(role); // "user" ← default applied because role is undefined
```

### Nested Destructuring

```js
const order = {
  id: 101,
  customer: {
    name: "Rafiq",
    address: {
      city: "Sylhet",
      zip: "3100"
    }
  }
};

const { customer: { name, address: { city, zip } } } = order;

console.log(name); // "Rafiq"
console.log(city); // "Sylhet"
console.log(zip);  // "3100"
```

### In Function Parameters

```js
function displayUser({ name, age, role = "guest" }) {
  console.log(`${name}, ${age}, ${role}`);
}

displayUser({ name: "Rafiq", age: 24 }); // "Rafiq, 24, guest"
```

This pattern is extremely common in React (props destructuring), Express (req destructuring), and API handlers.

---

## 33. Aliases in Destructuring

You can rename variables during destructuring using the `:` syntax.

```js
const user = { name: "Rafiq", age: 24 };

const { name: userName, age: userAge } = user;

console.log(userName); // "Rafiq"
console.log(userAge);  // 24
// console.log(name);  // ❌ ReferenceError — 'name' is not declared
```

### With Defaults and Aliases

```js
const { name: displayName = "Anonymous", role: userRole = "viewer" } = {};

console.log(displayName); // "Anonymous"
console.log(userRole);    // "viewer"
```

### Real-World Use Case

```js
// API response has snake_case, but you want camelCase
const { first_name: firstName, last_name: lastName } = apiResponse;
console.log(firstName, lastName);
```

This is especially useful when working with API responses or libraries where keys don't match your preferred naming conventions.

---

## 34. Optional Chaining `?.`

**Optional chaining** (`?.`) safely accesses nested properties without throwing an error if an intermediate value is `null` or `undefined`. Instead, it returns `undefined`.

### Without Optional Chaining (dangerous)

```js
const user = { profile: null };

console.log(user.profile.avatar); // ❌ TypeError: Cannot read properties of null
```

### With Optional Chaining (safe)

```js
console.log(user.profile?.avatar); // ✅ undefined — no crash
```

### Deeply Nested Access

```js
const order = {
  customer: {
    address: {
      city: "Sylhet"
    }
  }
};

console.log(order?.customer?.address?.city);   // "Sylhet"
console.log(order?.billing?.address?.city);    // undefined — safe
```

### With Methods

```js
const user = { greet: () => "Hello!" };

console.log(user.greet?.());    // "Hello!"
console.log(user.farewell?.());  // undefined — method doesn't exist, no crash
```

### With Arrays

```js
const data = { items: ["a", "b", "c"] };

console.log(data.items?.[0]); // "a"
console.log(data.tags?.[0]);  // undefined — safe
```

### Real-World Use Case

```js
// API response might be incomplete
function getCity(user) {
  return user?.address?.city ?? "Unknown";
}

console.log(getCity({ address: { city: "Sylhet" } })); // "Sylhet"
console.log(getCity({ address: null }));                // "Unknown"
console.log(getCity(null));                             // "Unknown"
```

Optional chaining is one of the most useful modern JS features — it eliminates dozens of `if` checks when working with API data, user input, or optional configurations.

---

> 📅 **Day 06 Part 01 Complete — 14 days to go!**
