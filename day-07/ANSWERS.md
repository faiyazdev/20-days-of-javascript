# Day 07 — Answers

## The `this` Keyword: Complete Reference Guide

> `this` is one of the most powerful — and most misunderstood — features in JavaScript. This guide breaks down every rule that governs what `this` refers to, when it changes, and how to control it. Master this, and you'll debug faster, write cleaner code, and nail interviews.

---

## 🔵 Understanding `this`

---

### 1. What is the `this` Keyword in JavaScript?

`this` is a **special keyword** in JavaScript that refers to an **object** — but which object depends entirely on **how and where the function is called**, not where it is defined.

Think of `this` as an invisible parameter that JavaScript passes into every function call, pointing to the "owner" or "context" of that call.

```js
const user = {
  name: "Rahim",
  greet() {
    console.log(this.name); // `this` = user object
  }
};

user.greet(); // "Rahim"
```

The value of `this` is determined at **runtime**, not at **definition time** (except for arrow functions — more on that later).

There are **4 rules** that determine what `this` is:

| Rule | How it's triggered | `this` is... |
|------|--------------------|-------------|
| **Default Binding** | Standalone function call | Global object or `undefined` (strict) |
| **Implicit Binding** | `obj.fn()` — called on an object | The object before the dot |
| **Explicit Binding** | `fn.call(obj)` / `apply` / `bind` | Whatever you pass in |
| **`new` Binding** | `new Fn()` | The newly created object |

---

### 2. What Does `this` Refer to in the Global Scope?

In the **global scope** (outside any function), `this` refers to the **global object**:

- In **browsers**: `this === window`
- In **Node.js** (top-level): `this === module.exports` (an empty object `{}`)
- In **Node.js** (inside a function): `this === global`

```js
// Browser:
console.log(this); // Window { ... }
console.log(this === window); // true

// Global variable becomes a property of `this` in non-strict mode:
var message = "Hello";
console.log(this.message); // "Hello" (in browser — var attaches to window)
```

```js
// Node.js top level:
console.log(this); // {} (module.exports)

// Node.js inside a plain function:
function test() {
  console.log(this); // global object (in non-strict mode)
}
test();
```

> **Important:** `let` and `const` do NOT attach to the global object, only `var` does.

```js
var a = 1;
let b = 2;

console.log(this.a); // 1   (attached to global)
console.log(this.b); // undefined (not attached)
```

---

### 3. What Does Implicit Binding of `this` Mean?

**Implicit binding** is the most common rule. It says: when a function is called as a method of an object (with a dot), **`this` is set to the object on the left of the dot**.

```js
const person = {
  name: "Karim",
  greet() {
    console.log(`Hi, I am ${this.name}`);
  }
};

person.greet(); // "Hi, I am Karim"
// `this` implicitly bound to `person` because of person.greet()
```

**The dot rule — `this` = the object immediately to the left:**

```js
const company = {
  name: "TechCorp",
  ceo: {
    name: "Siam",
    introduce() {
      console.log(`I'm ${this.name} from ${this.company}`);
      // `this` = ceo (the object directly before the dot)
    }
  }
};

company.ceo.introduce(); // "I'm Siam from undefined"
// `this` = company.ceo — NOT company
```

**⚠️ Implicit Binding Loss — the classic gotcha:**

```js
const user = {
  name: "Nadia",
  greet() {
    console.log(this.name);
  }
};

user.greet(); // "Nadia" ✅ — implicit binding

const fn = user.greet; // extract the function
fn(); // undefined ❌ — implicit binding LOST! No object before the dot
```

When you extract a method from an object and call it as a standalone function, `this` is no longer bound to the original object. This is a frequent source of bugs — especially in **React event handlers** and **callbacks**.

---

### 4. What Does a Standalone Function Mean?

A **standalone function** (also called a "plain function call" or "default binding") is a function called **without any object context** — no dot, no `call/apply/bind`, no `new`.

```js
function sayHello() {
  console.log("Hello from:", this);
}

sayHello(); // standalone call — no object before it
```

Some examples of standalone function calls:

```js
// Direct call — standalone
greet();

// Extracted method — becomes standalone
const fn = obj.greet;
fn(); // standalone, binding lost

// Callback — often becomes standalone
setTimeout(obj.greet, 1000); // standalone inside setTimeout

// Array method callback — standalone
[1, 2, 3].forEach(obj.greet); // standalone
```

The key is: **if there's no object before the call, it's standalone** — and default binding rules apply.

---

### 5. What is the Behaviour of `this` Inside a Standalone Function?

In **non-strict mode**, a standalone function's `this` defaults to the **global object** (`window` in browsers, `global` in Node.js).

```js
function showThis() {
  console.log(this);
}

showThis(); // Window { ... } in browser (global object)
```

This is called **default binding** — JavaScript falls back to the global object when no other rule applies.

```js
var name = "Global Rahim"; // var attaches to global

function greet() {
  console.log(this.name); // reads from global
}

greet(); // "Global Rahim" — this = global object
```

**The problem this causes:**

```js
function Person(name) {
  this.name = name;
}

// ❌ Called without `new` — standalone function
const p = Person("Karim");
console.log(p);           // undefined (no return value)
console.log(window.name); // "Karim" — accidentally polluted the global!
```

This is one of the most dangerous behaviors in JavaScript — accidentally writing to the global object.

---

### 6. What is the Behaviour of `this` Inside an Arrow Function?

Arrow functions **do not have their own `this`**. Instead, they **lexically inherit `this`** from the surrounding (enclosing) scope where the arrow function was defined.

This is called **lexical `this`** — determined at definition time, not call time.

```js
const user = {
  name: "Tanvir",
  // Regular method — has its own `this`
  greetRegular() {
    console.log(this.name); // "Tanvir"
  },
  // Arrow function — NO own `this`, inherits from... where?
  greetArrow: () => {
    console.log(this.name); // undefined (inherits from global scope, not user)
  }
};

user.greetRegular(); // "Tanvir"
user.greetArrow();   // undefined ❌
```

**Arrow functions shine inside methods — they capture the method's `this`:**

```js
const timer = {
  name: "MyTimer",
  start() {
    // `this` here = timer object (implicit binding from start())
    setTimeout(() => {
      // Arrow function — inherits `this` from start()
      console.log(this.name); // "MyTimer" ✅
    }, 1000);
  }
};

timer.start(); // "MyTimer" after 1s
```

**Without arrow function — `this` is lost in callback:**

```js
const timer = {
  name: "MyTimer",
  start() {
    setTimeout(function () {
      // Regular function — `this` = global (default binding)
      console.log(this.name); // undefined ❌
    }, 1000);
  }
};
```

**Key rule:** Arrow functions look at where they are **written** (lexical scope), not how they are called.

```js
function outer() {
  this.value = 42;

  const inner = () => {
    console.log(this.value); // inherits `this` from outer
  };

  inner(); // 42 ✅ — regardless of how inner is called
}

const obj = new outer();
```

---

## 🔵 Strict Mode & `this`

---

### 7. What Effects Does Strict Mode Have on `this` in a Standalone Function?

In **strict mode** (`"use strict"`), the default binding rule changes: instead of falling back to the global object, `this` becomes **`undefined`** in standalone function calls.

```js
"use strict";

function showThis() {
  console.log(this); // undefined
}

showThis(); // undefined — NOT global object
```

**Why is this better?**

It prevents accidental global pollution. Without strict mode, forgetting `new` silently writes properties to the global object. In strict mode, it throws immediately:

```js
"use strict";

function Person(name) {
  this.name = name; // `this` is undefined
}

const p = Person("Rahim"); // ❌ TypeError: Cannot set properties of undefined
```

This **fails loudly** — which is safer than silently corrupting globals.

**Strict mode is automatically enabled in:**
- ES6 modules (`import`/`export`)
- Class bodies
- TypeScript files (by default)

So in modern Next.js / TypeScript / React code, you're almost always in strict mode — and standalone `this` is `undefined`.

```js
// In a module (strict mode automatic):
function test() {
  console.log(this); // undefined
}
test();
```

---

### 8. Does Arrow Function `this` Refer to `undefined` in Strict Mode?

**No.** Arrow functions are **not affected by strict mode** when it comes to `this`. They always use lexical `this` — whatever the enclosing scope's `this` is. Strict mode does not override this.

```js
"use strict";

const obj = {
  name: "Rafi",
  start() {
    // `this` here = obj (implicit binding, strict mode doesn't change this)
    const show = () => {
      console.log(this.name); // inherits `this` from start()
    };
    show(); // "Rafi" ✅ — no change from strict mode
  }
};

obj.start();
```

**If the enclosing scope's `this` is `undefined` (strict standalone), then the arrow inherits `undefined`:**

```js
"use strict";

function outer() {
  console.log(this); // undefined (strict standalone)

  const inner = () => {
    console.log(this); // also undefined — inherits from outer
  };

  inner();
}

outer();
// undefined
// undefined
```

So the arrow function does not independently become `undefined` — it just inherits whatever its enclosing context has, which in strict standalone mode, happens to be `undefined`.

---

## 🔵 Explicit Binding

---

### 9. What is Explicit Binding?

**Explicit binding** means manually telling JavaScript what `this` should be — using `call`, `apply`, or `bind`. You're being **explicit** about the context instead of relying on how the function was called.

```js
function greet() {
  console.log(`Hello, I am ${this.name}`);
}

const user = { name: "Karim" };

// Explicitly set `this` to `user`:
greet.call(user); // "Hello, I am Karim"
```

This overrides both default and implicit binding rules. You have full control over what `this` is.

---

### 10. What is the `call` Method?

`Function.prototype.call(thisArg, arg1, arg2, ...)` — **immediately invokes** the function with a specified `this` value and **individual arguments** passed one by one.

```js
function introduce(greeting, punctuation) {
  console.log(`${greeting}, I am ${this.name}${punctuation}`);
}

const person1 = { name: "Rahim" };
const person2 = { name: "Sana" };

introduce.call(person1, "Hello", "!");  // "Hello, I am Rahim!"
introduce.call(person2, "Hi", ".");     // "Hi, I am Sana."
```

**Using `call` to borrow a method:**

```js
const admin = {
  name: "Admin",
  getRole() {
    return `${this.name} is an admin`;
  }
};

const user = { name: "Fahim" };

// Borrow admin's method for user:
console.log(admin.getRole.call(user)); // "Fahim is an admin"
```

**`call` for inheritance (old pattern before `class`):**

```js
function Animal(name) {
  this.name = name;
}

function Dog(name, breed) {
  Animal.call(this, name); // call Animal constructor with Dog's `this`
  this.breed = breed;
}

const dog = new Dog("Rex", "Labrador");
console.log(dog.name);  // "Rex"
console.log(dog.breed); // "Labrador"
```

---

### 11. What is `apply`?

`Function.prototype.apply(thisArg, [argsArray])` — **immediately invokes** the function with a specified `this` value and arguments passed as an **array** (or array-like object).

```js
function introduce(greeting, punctuation) {
  console.log(`${greeting}, I am ${this.name}${punctuation}`);
}

const person = { name: "Tanvir" };

introduce.apply(person, ["Hey", "!"]); // "Hey, I am Tanvir!"
// Arguments passed as an array, not individually
```

**Classic use case — spreading array into Math functions:**

```js
const numbers = [5, 2, 9, 1, 7];

// Math.max doesn't accept an array directly:
Math.max(5, 2, 9, 1, 7); // 9 ✅
Math.max(numbers);        // NaN ❌

// Old way — use apply to spread the array:
Math.max.apply(null, numbers); // 9 ✅

// Modern way — spread operator (no need for apply):
Math.max(...numbers); // 9 ✅
```

> Today, `apply` is largely replaced by the **spread operator** (`...`). But knowing it is important for interviews and understanding older codebases.

---

### 12. What is `bind`?

`Function.prototype.bind(thisArg, arg1, arg2, ...)` — does **not** invoke the function immediately. Instead, it returns a **new function** with `this` permanently bound to the specified value.

```js
function greet(greeting) {
  console.log(`${greeting}, I am ${this.name}`);
}

const user = { name: "Lina" };

const greetLina = greet.bind(user); // returns a new function
greetLina("Hello");  // "Hello, I am Lina"
greetLina("Hi");     // "Hi, I am Lina"
// `this` is always `user`, no matter how greetLina is called
```

**Bind with pre-set arguments (partial application):**

```js
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2); // pre-set first arg to 2
console.log(double(5));  // 10
console.log(double(9));  // 18
```

**Most common use — React event handlers:**

```js
class Counter extends React.Component {
  constructor() {
    super();
    this.state = { count: 0 };
    // Without this bind, `this` inside handleClick would be undefined:
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState({ count: this.state.count + 1 }); // `this` = component ✅
  }
}
```

**Bind to preserve context in callbacks:**

```js
const user = {
  name: "Arif",
  greet() {
    console.log(`Hi, I'm ${this.name}`);
  }
};

// setTimeout loses `this` — greet becomes standalone:
setTimeout(user.greet, 1000);          // undefined ❌

// bind fixes it:
setTimeout(user.greet.bind(user), 1000); // "Hi, I'm Arif" ✅
```

---

### 13. Differences Between `call`, `apply`, and `bind`

All three explicitly set `this` — but they differ in **when** they execute and **how** they accept arguments.

```
call  → executes immediately, args passed individually
apply → executes immediately, args passed as array
bind  → does NOT execute, returns a new bound function
```

```js
function show(a, b) {
  console.log(this.name, a, b);
}

const obj = { name: "Rafi" };

show.call(obj, 1, 2);           // "Rafi 1 2" — immediate, individual args
show.apply(obj, [1, 2]);        // "Rafi 1 2" — immediate, array args
const bound = show.bind(obj, 1);
bound(2);                       // "Rafi 1 2" — deferred, partial args possible
```

**Full Comparison Table:**

| Feature | `call` | `apply` | `bind` |
|---------|--------|---------|--------|
| Invokes immediately | ✅ Yes | ✅ Yes | ❌ No |
| Returns new function | ❌ No | ❌ No | ✅ Yes |
| Argument format | Individual: `(ctx, a, b)` | Array: `(ctx, [a, b])` | Individual: `(ctx, a, b)` |
| Partial application | ❌ Not directly | ❌ Not directly | ✅ Yes |
| Use case | Borrow + run now | Spread array + run now | Callback / event handlers |

**Memory trick:**
- **`c`all** → **c**omma-separated args
- **`a`pply** → **a**rray args
- **`b`ind** → **b**ookmark the context for later

**How they relate to explicit binding:**

All three are the mechanism for **explicit binding** — they let you override JavaScript's automatic `this` resolution and say: *"No matter how this function is called, use THIS as the context."*

```js
// Without explicit binding — this depends on call site:
greet();           // default: global / undefined
obj.greet();       // implicit: obj

// With explicit binding — you decide:
greet.call(myObj); // explicit: myObj, always
greet.apply(myObj, args);
const g = greet.bind(myObj);
g(); // still myObj, even as standalone
```

---

## 🔵 `new` Keyword & Overview

---

### 14. How Does the `new` Keyword in a Constructor Function Relate to `this`?

When you call a function with `new`, JavaScript performs 4 steps automatically:

1. **Creates** a brand new empty object `{}`
2. **Sets `this`** to point to that new object
3. **Runs** the constructor function body (which adds properties via `this`)
4. **Returns `this`** automatically (unless you explicitly return a different object)

```js
function User(name, age) {
  // Step 2: `this` = {} (new empty object)
  this.name = name;   // Step 3: add properties
  this.age = age;
  // Step 4: implicit `return this`
}

const u = new User("Siam", 25);
// u = { name: "Siam", age: 25 }
```

This is called **`new` binding** — the strongest binding rule. It always wins over default and implicit binding.

**`new` binding vs explicit binding:**

```js
function Fn() {
  console.log(this);
}

const obj = { name: "explicitObj" };
const BoundFn = Fn.bind(obj);

// Explicit bind says `this` = obj
BoundFn();      // { name: "explicitObj" }

// But `new` overrides even bind!
new BoundFn();  // Fn {} — a new empty object, not obj
```

**`new` binding priority:** `new` > `explicit (bind)` > `implicit` > `default`

---

### 15. Quick Overview of the `this` Keyword

The **4 Rules of `this`** — applied in order of priority:

```
1. new binding      → new Fn()           → this = new object
2. Explicit binding → fn.call/apply/bind → this = what you specify
3. Implicit binding → obj.fn()           → this = obj
4. Default binding  → fn()               → this = global or undefined (strict)
```

```js
// 1. new binding
function Person(name) { this.name = name; }
const p = new Person("Rahim");   // this = new {}

// 2. Explicit binding
greet.call({ name: "Karim" });   // this = { name: "Karim" }

// 3. Implicit binding
user.greet();                    // this = user

// 4. Default binding
greet();                         // this = window or undefined
```

**Arrow functions — the exception:**

Arrow functions don't follow any of these 4 rules. They always use **lexical `this`** — whatever `this` was in the scope where the arrow was written.

```js
const obj = {
  name: "Nadia",
  // Regular: follows rules above
  regular() { console.log(this.name); },
  // Arrow: inherits from where it's defined (global scope here)
  arrow: () => { console.log(this.name); }
};

obj.regular(); // "Nadia" (implicit binding)
obj.arrow();   // undefined (lexical — global this.name doesn't exist)
```

**`this` cheat sheet:**

| Context | Non-strict | Strict |
|---------|-----------|--------|
| Global scope | `window` / `global` | `window` / `global` |
| Standalone function | `window` / `global` | `undefined` |
| Object method | The object | The object |
| Arrow function | Lexical (enclosing scope) | Lexical (unchanged) |
| `call` / `apply` / `bind` | Specified object | Specified object |
| `new Fn()` | New instance | New instance |
| Class method | The instance | The instance |

---

## 🔵 Interview Questions & Answers

---

### Q1. What will this code output?

```js
const obj = {
  name: "Rahim",
  greet() {
    console.log(this.name);
  }
};

const fn = obj.greet;
fn();
```

**Answer:** `undefined` (or error in strict mode)

When `fn = obj.greet` extracts the method, it **loses** implicit binding. Calling `fn()` is a standalone call — `this` becomes global (non-strict) or `undefined` (strict). Neither has a `name` property, so the result is `undefined`.

**Fix:** `fn.call(obj)` or `const fn = obj.greet.bind(obj)`

---

### Q2. What does the following log?

```js
function Timer() {
  this.count = 0;

  setInterval(function () {
    this.count++;
    console.log(this.count);
  }, 1000);
}

new Timer();
```

**Answer:** `NaN` repeatedly (or errors)

The callback inside `setInterval` is a standalone function — `this` is the global object (or `undefined` in strict). `global.count` is `undefined`, and `undefined++` is `NaN`.

**Fix:** Use an arrow function to inherit `this` from the `Timer` constructor:

```js
function Timer() {
  this.count = 0;

  setInterval(() => {  // arrow inherits `this` from Timer
    this.count++;
    console.log(this.count); // 1, 2, 3...
  }, 1000);
}
```

---

### Q3. What is the difference between `call` and `bind`?

**Answer:**

- `call` **immediately invokes** the function with the given `this` and arguments.
- `bind` **returns a new function** with `this` permanently set — does not call it.

```js
function greet(msg) {
  console.log(`${msg}, ${this.name}`);
}

const user = { name: "Karim" };

greet.call(user, "Hello");           // "Hello, Karim" — runs NOW
const boundGreet = greet.bind(user);
boundGreet("Hi");                    // "Hi, Karim" — runs LATER
```

Use `call` when you want to invoke immediately. Use `bind` when you need to pass the function as a callback and preserve context.

---

### Q4. What will this log?

```js
const obj = {
  val: 10,
  double: () => {
    return this.val * 2;
  }
};

console.log(obj.double());
```

**Answer:** `NaN` (or `undefined * 2`)

`double` is an **arrow function** defined in the global scope's context. Arrow functions don't get their own `this` — they inherit from the enclosing lexical scope (global here). `this.val` in global scope is `undefined`. `undefined * 2 = NaN`.

**Fix:** Use a regular function:

```js
const obj = {
  val: 10,
  double() {     // regular method
    return this.val * 2; // this = obj
  }
};
console.log(obj.double()); // 20
```

---

### Q5. Can you re-bind a `bind`-ed function?

**Answer:** No. Once a function is bound with `bind`, the `this` cannot be overridden by another `call`, `apply`, or `bind`.

```js
function greet() {
  console.log(this.name);
}

const user1 = { name: "Rahim" };
const user2 = { name: "Karim" };

const boundGreet = greet.bind(user1);
boundGreet();                     // "Rahim"

boundGreet.call(user2);           // "Rahim" — call cannot override bind!
boundGreet.bind(user2)();         // "Rahim" — bind cannot override bind!
```

The first `bind` wins permanently. The only exception is `new`, which overrides even `bind`.

---

### Q6. What is the output?

```js
"use strict";

function show() {
  console.log(this);
}

show();
show.call(undefined);
show.call(null);
```

**Answer:**
```
undefined
undefined
null
```

In strict mode, `this` is not coerced to the global object. A standalone call gives `undefined`. `call(undefined)` passes `undefined`. `call(null)` passes `null` — strict mode preserves it as `null`.

In non-strict mode, all three would print the global object.

---

### Q7. What does this print?

```js
function Foo() {
  this.name = "Foo";

  return { name: "Bar" }; // explicit return of an object
}

const f = new Foo();
console.log(f.name);
```

**Answer:** `"Bar"`

Normally, `new` returns `this`. But if the constructor **explicitly returns an object**, that object is returned instead of `this`. Since `{ name: "Bar" }` is an object, it wins. (If the explicit return is a primitive, `this` is returned as usual.)

---

### Q8. Explain the output step by step:

```js
const user = {
  name: "Sana",
  greetAfterDelay() {
    setTimeout(function () {
      console.log(this.name); // A
    }, 100);

    setTimeout(() => {
      console.log(this.name); // B
    }, 200);
  }
};

user.greetAfterDelay();
```

**Answer:**
- **A → `undefined`** — regular function in `setTimeout` is a standalone call. `this` = global. `global.name` = `undefined`.
- **B → `"Sana"`** — arrow function inherits `this` from `greetAfterDelay`, which was called with implicit binding (`user.greetAfterDelay()`), so `this = user`.

---

### Q9. What is the priority order of `this` binding rules?

**Answer (highest to lowest):**

```
1. new binding       new Fn()
2. Explicit binding  .call() / .apply() / .bind()
3. Implicit binding  obj.fn()
4. Default binding   fn()
```

Arrow functions are outside this system — they use lexical `this` determined at write time.

---

### Q10. How would you fix lost `this` in three different ways?

```js
const counter = {
  count: 0,
  increment() {
    this.count++;
  }
};

const inc = counter.increment; // binding lost
inc(); // this.count++ fails
```

**Fix 1 — `bind`:**
```js
const inc = counter.increment.bind(counter);
inc(); // ✅
```

**Fix 2 — Arrow wrapper:**
```js
const inc = () => counter.increment();
inc(); // ✅ — always calls through the object
```

**Fix 3 — `call` at call site:**
```js
inc.call(counter); // ✅
```

---

## 🎯 `this` Quick Reference

```
Where?                    What is `this`?
──────────────────────────────────────────────────────
Global scope              window / global
Standalone function       window / global (non-strict)
                          undefined (strict)
Object method (obj.fn())  obj
Arrow function            Lexical — inherited from enclosing scope
call / apply / bind       Whatever you pass in
new Fn()                  New instance
Class method              The instance
```

**Priority:**
```
new > bind > call/apply > obj.method() > standalone
```

**Arrow functions don't play by these rules — they're always lexical.**

---

*Day 07 — Part 01 & Part 02 of Full Stack JavaScript Learning Journey*

---

---

# Part 02 — Errors & Error Handling

> Errors are not failures — they're information. JavaScript gives you a rich system for catching, inspecting, creating, and re-raising errors. Mastering this system means your code fails gracefully, communicates clearly, and is far easier to debug in production.

---

## 🔴 Errors & `try-catch`

---

### 17. What Are Errors in JavaScript and How Do You Handle Them with `try-catch`?

JavaScript has two broad categories of errors:

**Syntax Errors** — caught before code runs. The engine refuses to execute the file at all.

```js
const x = ; // SyntaxError: Unexpected token
```

**Runtime Errors** — occur while the code is running. These can be caught and handled.

```js
null.toString(); // TypeError: Cannot read properties of null
```

When a runtime error occurs and is **not caught**, it crashes the program (or the current call stack in a browser). The `try-catch` block lets you intercept these errors and respond gracefully.

**Basic `try-catch` structure:**

```js
try {
  // Code that might throw an error
  const result = riskyOperation();
  console.log(result);
} catch (error) {
  // Runs only if try block throws
  console.error("Something went wrong:", error.message);
}
```

**Real example:**

```js
function parseJSON(str) {
  try {
    const data = JSON.parse(str);
    return data;
  } catch (error) {
    console.error("Invalid JSON:", error.message);
    return null;
  }
}

parseJSON('{"name":"Rahim"}'); // { name: "Rahim" }
parseJSON("not json at all");  // Invalid JSON: Unexpected token...  → null
```

**Important rules about `try-catch`:**

```js
// 1. Only catches synchronous errors:
try {
  setTimeout(() => {
    throw new Error("Async error"); // ❌ NOT caught by this try-catch
  }, 100);
} catch (e) {
  console.log("won't reach here");
}

// 2. For async code, use try-catch with async/await:
async function fetchData() {
  try {
    const res = await fetch("https://api.example.com/data");
    const data = await res.json();
    return data;
  } catch (error) {
    console.error("Fetch failed:", error.message);
  }
}
```

**Common built-in error types:**

| Error Type | When it occurs |
|-----------|----------------|
| `TypeError` | Wrong type — `null.foo`, calling non-function |
| `ReferenceError` | Accessing undeclared variable |
| `SyntaxError` | Invalid JS syntax (usually can't be caught) |
| `RangeError` | Value out of allowed range — `new Array(-1)` |
| `URIError` | Malformed URI — `decodeURI('%')` |
| `EvalError` | Issues with `eval()` (rare) |

---

### 18. What is the Error Object?

When JavaScript throws an error — or when you create one manually — it creates an **Error object**. This object carries structured information about what went wrong.

**Core properties:**

```js
try {
  undefined.toString();
} catch (error) {
  console.log(error.name);    // "TypeError"
  console.log(error.message); // "Cannot read properties of undefined..."
  console.log(error.stack);   // Full stack trace (call history)
}
```

| Property | Type | Description |
|----------|------|-------------|
| `name` | `string` | Type of error: `"TypeError"`, `"ReferenceError"`, etc. |
| `message` | `string` | Human-readable description of what went wrong |
| `stack` | `string` | Stack trace — where the error originated and how it got there |

**Stack trace — reading it:**

```
TypeError: Cannot read properties of undefined (reading 'name')
    at getUserName (app.js:12:18)      ← where error happened
    at processUser (app.js:25:5)       ← who called it
    at main (app.js:40:3)              ← who called that
```

Read from **top to bottom**: the top line is where the crash happened; below it is the call chain that led there.

**Manually creating an Error object:**

```js
const err = new Error("Something went wrong");

console.log(err.name);    // "Error"
console.log(err.message); // "Something went wrong"
console.log(err.stack);   // Stack trace from this line
```

**Checking error type in a catch block:**

```js
try {
  riskyCode();
} catch (error) {
  if (error instanceof TypeError) {
    console.log("Type issue:", error.message);
  } else if (error instanceof ReferenceError) {
    console.log("Reference issue:", error.message);
  } else {
    console.log("Unknown error:", error.message);
  }
}
```

---

### 19. What Does Throwing an Error Mean?

**Throwing** an error means manually signaling that something has gone wrong — stopping the current execution and passing control to the nearest `catch` block.

You use the `throw` keyword followed by any value — though throwing an `Error` object is the convention.

```js
throw new Error("This went wrong");
// Execution stops here and jumps to nearest catch
```

**You can technically throw anything:**

```js
throw "a string";     // possible but bad practice
throw 42;             // possible but bad practice
throw { code: 500 };  // possible but bad practice
throw new Error("msg"); // ✅ correct — use Error objects
```

Always throw `Error` objects (or subclasses) because they carry a `stack` trace, which is invaluable for debugging.

**Throwing to enforce preconditions (guard clauses):**

```js
function divide(a, b) {
  if (typeof a !== "number" || typeof b !== "number") {
    throw new TypeError("Both arguments must be numbers");
  }
  if (b === 0) {
    throw new RangeError("Cannot divide by zero");
  }
  return a / b;
}

try {
  console.log(divide(10, 2));  // 5
  console.log(divide(10, 0));  // throws RangeError
} catch (error) {
  console.error(`${error.name}: ${error.message}`);
  // "RangeError: Cannot divide by zero"
}
```

**Throwing inside async functions:**

```js
async function getUser(id) {
  if (!id) {
    throw new Error("User ID is required"); // caught by await's try-catch
  }
  const res = await fetch(`/api/users/${id}`);
  if (!res.ok) {
    throw new Error(`HTTP error: ${res.status}`);
  }
  return res.json();
}

// Caller:
try {
  const user = await getUser(null);
} catch (error) {
  console.error(error.message); // "User ID is required"
}
```

---

### 20. What is Rethrowing an Error?

**Rethrowing** means catching an error, inspecting it, and if it's not the kind of error you know how to handle — **throwing it again** so a higher-level handler can deal with it.

The idea: **each catch block should only handle the errors it understands**. If you get something unexpected, let it bubble up.

**Basic pattern:**

```js
try {
  riskyOperation();
} catch (error) {
  if (error instanceof TypeError) {
    // I know how to handle this one
    console.log("Handled TypeError:", error.message);
  } else {
    // Not mine to handle — rethrow it
    throw error;
  }
}
```

**Detailed example — layered error handling:**

```js
function readConfig(path) {
  try {
    const raw = fs.readFileSync(path, "utf-8");
    return JSON.parse(raw);
  } catch (error) {
    if (error instanceof SyntaxError) {
      // JSON was malformed — we handle this specifically
      throw new Error(`Config file at "${path}" has invalid JSON: ${error.message}`);
    }
    // File not found, permission error, etc. — rethrow as-is
    throw error;
  }
}

function startApp() {
  try {
    const config = readConfig("./config.json");
    console.log("App started with:", config);
  } catch (error) {
    if (error.code === "ENOENT") {
      // File not found — startApp handles this
      console.warn("No config file found. Using defaults.");
    } else {
      // Anything else — rethrow to the top-level handler
      throw error;
    }
  }
}

try {
  startApp();
} catch (error) {
  // Last resort — log and exit
  console.error("Fatal error:", error.message);
  process.exit(1);
}
```

**Why rethrowing matters — the wrong way:**

```js
// ❌ Swallowing all errors — dangerous!
try {
  riskyOperation();
} catch (error) {
  console.log("oops"); // catches EVERYTHING silently, bugs get hidden
}
```

```js
// ✅ Handle what you know, rethrow the rest
try {
  riskyOperation();
} catch (error) {
  if (error instanceof NetworkError) {
    showRetryButton();
  } else {
    throw error; // let it propagate
  }
}
```

**Rethrowing with added context:**

Sometimes you want to enrich the error before rethrowing — keeping the original cause while adding context:

```js
async function fetchUserOrders(userId) {
  try {
    const res = await fetch(`/api/users/${userId}/orders`);
    return await res.json();
  } catch (error) {
    // Wrap with context, preserve original via `cause`
    throw new Error(`Failed to fetch orders for user ${userId}`, { cause: error });
  }
}

// Caller can inspect both:
try {
  await fetchUserOrders(42);
} catch (error) {
  console.error(error.message);       // "Failed to fetch orders for user 42"
  console.error(error.cause.message); // original network error message
}
```

The `{ cause }` option was introduced in ES2022 and is the modern way to chain errors without losing the original stack trace.

**Summary of rethrowing rules:**

| Scenario | Action |
|----------|--------|
| Error type matches what you handle | Handle it — don't rethrow |
| Error type is unexpected | Rethrow as-is: `throw error` |
| Want to add context | Wrap and rethrow: `throw new Error("...", { cause: error })` |
| Never do this | Catch everything silently — bugs get buried |

---

### 21. What is the Use Case of `finally` in `try-catch`?

The `finally` block runs **always** — whether the `try` succeeded, threw an error, or even if `catch` itself threw. It is guaranteed to execute.

```js
try {
  // ... code
} catch (error) {
  // ... error handling
} finally {
  // Always runs — no matter what
}
```

**Core use cases:**

**1. Cleanup — releasing resources:**

```js
let connection = null;

try {
  connection = openDatabaseConnection();
  const data = connection.query("SELECT * FROM users");
  return data;
} catch (error) {
  console.error("Query failed:", error.message);
} finally {
  // Always close the connection — success or failure
  if (connection) connection.close();
}
```

**2. UI loading state — always hide the spinner:**

```js
async function loadUserData(userId) {
  setLoading(true); // show spinner

  try {
    const user = await fetchUser(userId);
    setUser(user);
  } catch (error) {
    setError(error.message);
  } finally {
    setLoading(false); // always hide spinner — success or error
  }
}
```

**3. Logging / telemetry — always record:**

```js
async function processPayment(order) {
  const startTime = Date.now();

  try {
    const result = await chargeCard(order);
    return result;
  } catch (error) {
    logger.error("Payment failed", { orderId: order.id, error });
    throw error;
  } finally {
    // Log duration regardless of outcome
    const duration = Date.now() - startTime;
    metrics.record("payment_duration", duration);
  }
}
```

**4. `finally` runs even after `return`:**

```js
function test() {
  try {
    return "from try";
  } finally {
    console.log("finally ran!"); // ← this STILL runs
  }
}

console.log(test());
// "finally ran!"
// "from try"
```

**5. `finally` overrides `return` if it has its own `return`:**

```js
function tricky() {
  try {
    return "try";
  } finally {
    return "finally"; // ⚠️ overrides the try's return!
  }
}

console.log(tricky()); // "finally"
```

> This is generally bad practice — avoid `return` inside `finally`.

**`finally` without `catch`:**

You can use `try-finally` without a `catch` — useful when you don't want to handle the error but still need cleanup:

```js
function withLock(fn) {
  acquireLock();
  try {
    return fn();
  } finally {
    releaseLock(); // always release, even if fn() throws
  }
}
```

The error still propagates to the caller — you just ensured cleanup happened first.

---

### 22. Is `Error` a Constructor Function?

**Yes.** `Error` is a **built-in constructor function** in JavaScript. You can call it with `new` to create error instances, and it has a prototype chain just like any other constructor.

```js
const err = new Error("Something broke");

console.log(err instanceof Error);  // true
console.log(typeof Error);          // "function"
console.log(Error.prototype);       // { name: "Error", message: "", ... }
```

JavaScript has **7 built-in error constructors**, all inheriting from `Error`:

```
Error (base)
├── TypeError
├── ReferenceError
├── SyntaxError
├── RangeError
├── URIError
└── EvalError
```

Each is its own constructor:

```js
const t = new TypeError("wrong type");
console.log(t instanceof TypeError); // true
console.log(t instanceof Error);     // true — TypeError extends Error
console.log(t.name);                 // "TypeError"
```

You can also call `Error` without `new` — it behaves the same way:

```js
const e1 = new Error("with new");
const e2 = Error("without new"); // also works

console.log(e1 instanceof Error); // true
console.log(e2 instanceof Error); // true
```

Since `Error` is a constructor function, it can be **extended** with ES6 `class` syntax — which brings us to custom errors.

---

### 23. Custom Errors Using the Built-in `Error` Constructor

Custom error classes let you create **domain-specific error types** that are meaningful to your application. They extend the built-in `Error` and can carry extra context.

**Basic custom error:**

```js
class ValidationError extends Error {
  constructor(message) {
    super(message);          // pass message to Error
    this.name = "ValidationError"; // override the default name
  }
}

try {
  throw new ValidationError("Email is required");
} catch (error) {
  console.log(error.name);       // "ValidationError"
  console.log(error.message);    // "Email is required"
  console.log(error instanceof ValidationError); // true
  console.log(error instanceof Error);           // true
}
```

**Custom error with extra properties:**

```js
class HttpError extends Error {
  constructor(statusCode, message) {
    super(message);
    this.name = "HttpError";
    this.statusCode = statusCode;
  }
}

async function fetchData(url) {
  const res = await fetch(url);

  if (!res.ok) {
    throw new HttpError(res.status, `Request failed: ${res.statusText}`);
  }

  return res.json();
}

try {
  await fetchData("https://api.example.com/data");
} catch (error) {
  if (error instanceof HttpError) {
    console.log(`HTTP ${error.statusCode}: ${error.message}`);
    if (error.statusCode === 404) showNotFoundPage();
    if (error.statusCode === 401) redirectToLogin();
  } else {
    throw error; // rethrow non-HTTP errors
  }
}
```

**A full custom error hierarchy:**

```js
// Base app error
class AppError extends Error {
  constructor(message, { cause } = {}) {
    super(message, { cause });
    this.name = "AppError";
  }
}

// Domain-specific errors
class ValidationError extends AppError {
  constructor(field, message) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

class NotFoundError extends AppError {
  constructor(resource, id) {
    super(`${resource} with id "${id}" not found`);
    this.name = "NotFoundError";
    this.resource = resource;
    this.id = id;
  }
}

class DatabaseError extends AppError {
  constructor(message, cause) {
    super(message, { cause });
    this.name = "DatabaseError";
  }
}

// Usage:
function getUserById(id) {
  if (!id) throw new ValidationError("id", "User ID must be provided");

  const user = db.find(id);
  if (!user) throw new NotFoundError("User", id);

  return user;
}

try {
  getUserById(null);
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(`Validation failed on field "${error.field}": ${error.message}`);
  } else if (error instanceof NotFoundError) {
    console.log(`${error.resource} #${error.id} does not exist`);
  } else {
    throw error;
  }
}
```

**`instanceof` still works across the whole chain:**

```js
const err = new NotFoundError("User", 42);

console.log(err instanceof NotFoundError); // true
console.log(err instanceof AppError);      // true
console.log(err instanceof Error);         // true
```

**One gotcha — always set `this.name`:**

Without it, your custom error will display as `"Error"` in logs, losing the type information:

```js
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = "MyError"; // ✅ always set this!
  }
}

const e = new MyError("oops");
console.log(e.toString()); // "MyError: oops" ✅
// Without name: "Error: oops" ❌
```

---

## 🎯 Error Handling Quick Reference

```js
// Basic structure
try {
  mightThrow();
} catch (error) {
  console.error(error.name, error.message);
} finally {
  cleanup(); // always runs
}

// Throw
throw new Error("message");
throw new TypeError("wrong type");

// Rethrow
catch (error) {
  if (error instanceof KnownError) handle(error);
  else throw error; // not mine — bubble up
}

// Wrap with cause (ES2022)
throw new Error("context message", { cause: originalError });

// Custom error
class MyError extends Error {
  constructor(message, extraData) {
    super(message);
    this.name = "MyError";
    this.extraData = extraData;
  }
}

// Check type
error instanceof TypeError
error instanceof MyError
error.name === "ValidationError"
```

---

*Day 07 — Part 01 & Part 02 of Full Stack JavaScript Learning Journey*
