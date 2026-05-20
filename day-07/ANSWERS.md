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

*Day 07 — Part 01 of Full Stack JavaScript Learning Journey*
