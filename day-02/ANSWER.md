
## ⚙️ Part 01 — Operators

---

### 1. Arithmetic Operators

#### What is an operator and operand in JavaScript?

An **operator** is a symbol that performs an operation. An **operand** is the value the operator acts on.

```js
let result = 10 + 5;
//            ↑   ↑   → operands
//              ↑     → operator
```

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `10 - 4` | `6` |
| `*` | Multiplication | `3 * 4` | `12` |
| `/` | Division | `10 / 2` | `5` |
| `%` | Modulus | `10 % 3` | `1` |
| `**` | Exponentiation | `2 ** 3` | `8` |
| `++` | Increment | `x++` | adds 1 |
| `--` | Decrement | `x--` | subtracts 1 |

---

#### What's the difference between `+` and other arithmetic operators?

`+` is the **only operator that works with strings**. All other operators (`-`, `*`, `/`, `%`) always try to convert values to numbers first.

```js
"5" + 3   // "53"  → string concatenation
"5" - 3   // 2     → numeric subtraction
"5" * 2   // 10    → numeric multiplication
"5" / 1   // 5     → numeric division
```

> 💡 `+` sees a string → it concatenates. Others see a string → they coerce to number.

---

#### What will `"5" + 3` return? Why?

```js
"5" + 3  // "53"
```

Because `+` checks the operands — if **either side is a string**, it converts the other to a string and **concatenates** instead of adding. `3` becomes `"3"`, then `"5" + "3"` = `"53"`.

---

#### What does `%` do?

The **modulus operator** returns the **remainder** after division.

```js
10 % 3  // 1  → because 10 = (3 × 3) + 1
15 % 5  // 0  → divides evenly, no remainder
7 % 2   // 1  → 7 = (2 × 3) + 1
```

Common uses: checking if a number is even/odd, cycling through indexes.

```js
if (num % 2 === 0) console.log("even");
```

---

#### What's the difference between `x++` and `++x`?

| | Returns | Then increments |
|-|---------|-----------------|
| `x++` (postfix) | current value first | ✅ yes, after |
| `++x` (prefix) | incremented value | ✅ yes, before |

```js
let x = 5;
console.log(x++); // 5  → returns 5, THEN x becomes 6
console.log(x);   // 6

let y = 5;
console.log(++y); // 6  → increments FIRST, then returns 6
console.log(y);   // 6
```

---

#### Why does `"10" - 2` work but `"10" + 2` behaves differently?

```js
"10" - 2  // 8   → JS coerces "10" to number → 10 - 2
"10" + 2  // "102" → JS sees string + number → concatenates
```

`-` has no string behavior — it's purely numeric, so JS **coerces** `"10"` to `10`. But `+` has a dual role: arithmetic AND string concatenation — and **string wins**.

---

### 2. Assignment Operators

#### What does `=` actually do?

`=` is the **assignment operator**. It evaluates the right side and stores the result into the variable on the left.

```js
let x = 10; // evaluate 10, store it in x
```

> ⚠️ `=` is NOT equality. It's assignment. Equality is `==` or `===`.

---

#### Why is `x = x + 1` valid in JS?

Because JS evaluates the **right side first**, then assigns:

```js
let x = 5;
x = x + 1;
// Step 1: evaluate x + 1 → 5 + 1 → 6
// Step 2: assign 6 to x
console.log(x); // 6
```

---

#### What does `+=` mean internally?

`+=` is shorthand — it expands to `x = x + value`:

```js
let x = 10;
x += 5;  // same as: x = x + 5
console.log(x); // 15
```

| Shorthand | Expands To |
|-----------|-----------|
| `x += 5` | `x = x + 5` |
| `x -= 5` | `x = x - 5` |
| `x *= 5` | `x = x * 5` |
| `x /= 5` | `x = x / 5` |
| `x %= 5` | `x = x % 5` |
| `x **= 2` | `x = x ** 2` |

---

#### What's the difference between `=` and `===`?

| | Purpose | Example |
|-|---------|---------|
| `=` | **Assignment** — stores a value | `let x = 5` |
| `===` | **Strict equality** — compares value AND type | `x === 5 // true` |

```js
let x = 5;      // assigns 5 to x
x === 5;        // checks: is x equal to 5? → true
x === "5";      // false → same value, different type
```

---

#### Why are assignment operators useful?

They make code **shorter, cleaner, and more readable** — especially when updating a variable based on its current value.

```js
// Without shorthand
score = score + 10;
score = score * 2;

// With shorthand
score += 10;
score *= 2;
```

---

### 3. Comparison Operators

#### What does a comparison operator return?

Always a **boolean** — either `true` or `false`.

```js
5 > 3    // true
5 < 3    // false
5 === 5  // true
5 !== 5  // false
```

---

#### What's the difference between `==` and `===`?

| | Checks Value | Checks Type | Name |
|-|-------------|-------------|------|
| `==` | ✅ Yes | ❌ No | Loose equality |
| `===` | ✅ Yes | ✅ Yes | Strict equality |

```js
"5" == 5   // true  → JS coerces "5" to number before comparing
"5" === 5  // false → different types (string vs number)

null == undefined  // true  → special JS rule
null === undefined // false → different types
```

---

#### Why is `===` preferred in real projects?

Because `==` performs **implicit type coercion** which leads to unexpected bugs. `===` is predictable — it never coerces.

```js
0 == false    // true  ← surprising!
0 === false   // false ← expected
"" == false   // true  ← surprising!
"" === false  // false ← expected
```

> ✅ Always use `===` unless you have a specific reason for `==`.

---

#### What will `"10" > 5` return? Why?

```js
"10" > 5  // true
```

Because `>` is a numeric operator — it converts `"10"` to `10`, then compares `10 > 5` → `true`.

---

#### What does `!==` mean?

**Strict inequality** — returns `true` if the values are NOT equal in value OR type.

```js
5 !== "5"  // true  → different types
5 !== 5    // false → same value and type
5 != "5"   // false → loose: coerces first, then compares
```

---

### 4. Logical Operators

#### What does `&&` require to return `true`?

**Both** operands must be truthy.

```js
true && true   // true
true && false  // false
false && true  // false
```

---

#### What does `||` require to return `true`?

**At least one** operand must be truthy.

```js
true || false  // true
false || true  // true
false || false // false
```

---

#### What does `!` do?

`!` is the **logical NOT** — it flips a boolean value.

```js
!true   // false
!false  // true
!0      // true  → 0 is falsy, so !0 is true
!"hi"   // false → "hi" is truthy, so !"hi" is false
```

---

#### What will `true && false` return?

```js
true && false  // false
```

`&&` requires both to be truthy. Since the second is `false`, the result is `false`.

---

#### What will `false || true` return?

```js
false || true  // true
```

`||` needs at least one truthy. The second operand is `true`, so the result is `true`.

---

#### What does `&&` return if it finds a falsy value?

`&&` returns the **first falsy value** it finds. If all are truthy, it returns the **last value**.

```js
0 && "Hello"      // 0       → 0 is falsy, stop here
"Hi" && "Hello"   // "Hello" → both truthy, return last
null && "test"    // null    → null is falsy, stop here
```

---

#### What does `||` return if it finds a truthy value?

`||` returns the **first truthy value** it finds. If all are falsy, it returns the **last value**.

```js
"Hi" || "Hello"   // "Hi"    → "Hi" is truthy, stop here
0 || "Hello"      // "Hello" → 0 is falsy, keep going
0 || ""           // ""      → both falsy, return last
```

---

#### What are all falsy values in JavaScript?

There are exactly **8 falsy values** in JavaScript:

```js
false
0
-0
0n        // BigInt zero
""        // empty string
null
undefined
NaN
```

> 💡 Everything else is **truthy** — including `"0"`, `[]`, `{}`, and `"false"`.

---

#### What will `0 && "Hello"` return? Why?

```js
0 && "Hello"  // 0
```

`&&` evaluates left to right. It finds `0` which is **falsy** — so it immediately returns `0` without even looking at `"Hello"`. This is called **short-circuit evaluation**.

---

#### What will `"" || "Hi"` return? Why?

```js
"" || "Hi"  // "Hi"
```

`||` evaluates left to right. `""` is **falsy** — so it moves on and returns `"Hi"` which is truthy.

---

### 5. Ternary, typeof & Nullish Coalescing

#### When should you use ternary instead of `if`?

Use ternary when you need a **quick, single-expression decision** — especially for assigning a value.

```js
// if/else version
let label;
if (score > 50) {
  label = "Pass";
} else {
  label = "Fail";
}

// ternary version — cleaner
let label = score > 50 ? "Pass" : "Fail";
```

> ⚠️ Avoid nesting ternaries — it hurts readability.

---

#### What does `typeof null` return? Why is it weird?

```js
typeof null  // "object"
```

This is a **long-standing bug in JavaScript** from its very first version. `null` is a primitive, not an object — but `typeof null` returns `"object"` due to how types were encoded in early JS. It was never fixed to avoid breaking the web.

```js
typeof undefined  // "undefined"
typeof "hi"       // "string"
typeof 42         // "number"
typeof null       // "object" ← the famous bug
```

---

#### What's the difference between `||` and `??`?

| | Returns fallback when... |
|-|--------------------------|
| `\|\|` | value is **falsy** (`0`, `""`, `false`, `null`, `undefined`) |
| `??` | value is **only** `null` or `undefined` |

```js
0 || "default"   // "default" → 0 is falsy
0 ?? "default"   // 0         → 0 is NOT null/undefined

"" || "default"  // "default" → "" is falsy
"" ?? "default"  // ""        → "" is NOT null/undefined

null ?? "default"      // "default"
undefined ?? "default" // "default"
```

> 💡 Use `??` when `0` or `""` are **valid values** you want to keep.

---

#### What will `false ?? "hello"` return?

```js
false ?? "hello"  // false
```

`??` only triggers on `null` or `undefined`. `false` is neither — so it returns `false` as-is.

---

#### What will `false || "hello"` return?

```js
false || "hello"  // "hello"
```

`||` triggers on any falsy value. `false` is falsy — so it skips it and returns `"hello"`.

---

## 🔀 Part 02 — Control Flow

---

### Control Flow Basics

#### What is control flow in simple words?

Control flow is **the order in which JavaScript executes your code**. By default it runs top to bottom — but with control flow tools, you can make it skip, repeat, or branch.

```js
// Without control flow — runs every line
console.log("A");
console.log("B");
console.log("C");

// With control flow — only some lines run
if (isLoggedIn) {
  console.log("Welcome!");  // only runs if true
}
```

---

#### What is the default execution order in JavaScript?

**Top to bottom, left to right** — one line at a time. This is called **sequential execution**.

```js
let x = 10;   // line 1 runs first
let y = 20;   // line 2 runs second
let z = x + y; // line 3 runs third
```

---

#### Why do we need control flow?

Because real programs need to **make decisions**, handle different scenarios, and respond to different inputs. Without control flow, every user would see the same output regardless of their data.

```js
// Without control flow — everyone gets the same message
console.log("Hello!");

// With control flow — response depends on the situation
if (user.isAdmin) {
  console.log("Welcome, Admin!");
} else {
  console.log("Welcome, User!");
}
```

---

### if / else Statement

#### A condition in `if` can be any expression

A condition is **any expression that evaluates to truthy or falsy** — not just `true` or `false`.

```js
if (1)          // truthy ✅
if ("hello")    // truthy ✅
if ([])         // truthy ✅ (empty array is truthy!)
if ({})         // truthy ✅ (empty object is truthy!)
if (0)          // falsy ❌
if ("")         // falsy ❌
if (null)       // falsy ❌
if (undefined)  // falsy ❌
```

---

#### What is an `if...else` statement in JavaScript?

A structure that runs **different code blocks** depending on whether a condition is `true` or `false`.

```js
if (condition) {
  // runs if condition is truthy
} else {
  // runs if condition is falsy
}
```

---

#### What is the purpose of `if...else`?

To allow your program to **make decisions** and execute different code paths based on conditions — the foundation of all logic in programming.

---

#### What happens if the condition in `if` is true?

The code inside the `if` block executes. The `else` block (if present) is **skipped entirely**.

```js
let age = 20;
if (age >= 18) {
  console.log("Adult");  // ✅ this runs
} else {
  console.log("Minor");  // ⛔ skipped
}
```

---

#### What happens if the condition is false?

The `if` block is **skipped**. If an `else` block exists, it runs.

```js
let age = 15;
if (age >= 18) {
  console.log("Adult");  // ⛔ skipped
} else {
  console.log("Minor");  // ✅ this runs
}
```

---

#### What is the difference between `if`, `else if`, and `else`?

| | When it runs |
|-|-------------|
| `if` | Always checked first |
| `else if` | Checked only if previous conditions were false |
| `else` | Runs if ALL above conditions were false |

```js
let score = 75;

if (score >= 90) {
  console.log("A");
} else if (score >= 75) {
  console.log("B");       // ✅ this runs
} else if (score >= 60) {
  console.log("C");
} else {
  console.log("Fail");
}
```

---

#### Predict the output:

```js
if ("0") {
  console.log("A");
}
if (0) {
  console.log("B");
}
```

**Output: `A`**

- `"0"` is a **non-empty string** → truthy → `"A"` prints ✅
- `0` is a **number zero** → falsy → `"B"` never prints ⛔

> 💡 `"0"` (string) ≠ `0` (number). The string `"0"` is truthy even though its numeric value is zero.

---

#### Predict the output:

```js
let a = 5;
if (a > 3)
  console.log("X");
  console.log("Y");
```

**Output: `X` and `Y`**

This is a **common trick**. Without curly braces `{}`, only the **first line** after `if` is part of the condition. `console.log("Y")` is outside the `if` — it always runs regardless.

```js
// What JS actually sees:
if (a > 3) {
  console.log("X");  // inside if
}
console.log("Y");    // always runs — NOT inside if
```

> ✅ Always use curly braces `{}` to avoid this confusion.

---

#### Trick question — predict the output:

```js
if (null) {
  console.log("A");
} else {
  console.log("B");
}
```

**Output: `B`**

`null` is **falsy** — the `if` block is skipped, `else` runs.

---

#### Explain why this runs:

```js
if ([]) {
  console.log("Runs");
}
```

**Output: `Runs`**

An **empty array `[]` is truthy** in JavaScript. Only the 8 falsy values are falsy — everything else, including `[]` and `{}`, is truthy. This surprises many developers.

```js
Boolean([])   // true
Boolean({})   // true
Boolean("")   // false ← empty STRING is falsy
```

---

#### Explain this step-by-step:

```js
let x = 0;
if (x || x === 0) {
  console.log("Runs");
}
```

**Output: `Runs`**

Step by step:
1. `x` is `0` → falsy → `||` moves to the right side
2. `x === 0` → `0 === 0` → `true`
3. `false || true` → `true`
4. Condition is truthy → block runs ✅

> 💡 This pattern is useful to explicitly check for `0` since `0` alone is falsy.

---

### switch Statement

#### What is a switch statement?

A `switch` statement evaluates an expression and **matches it against multiple cases**, running the matching block.

```js
let day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of the week");
    break;
  case "Friday":
    console.log("End of the week");
    break;
  default:
    console.log("Midweek");
}
```

---

#### What happens if you forget `break`?

**Fall-through** occurs — JavaScript continues executing every case below the matched one until it hits a `break` or the end of the switch.

```js
let x = 1;

switch (x) {
  case 1:
    console.log("One");   // ✅ matches
  case 2:
    console.log("Two");   // ⚠️ also runs — no break!
  case 3:
    console.log("Three"); // ⚠️ also runs — no break!
    break;
  case 4:
    console.log("Four");  // ⛔ stopped by break above
}

// Output: One, Two, Three
```

---

#### When should you NOT use switch?

- When conditions involve **ranges** (`> 10`, `< 50`)
- When you need **complex logic** inside each branch
- When you're comparing against different variables in each branch

```js
// ❌ switch can't do this
switch (score) {
  case score > 90:  // won't work — switch uses strict ===
    ...
}

// ✅ use if/else instead
if (score > 90) { ... }
else if (score > 75) { ... }
```

---

#### Can switch handle ranges like `> 10`? Why?

**No** — `switch` uses **strict equality (`===`)** to match cases. It can't evaluate expressions like `> 10`.

```js
let score = 85;

// ❌ This does NOT work as expected
switch (score) {
  case score > 90:   // evaluates to false, not compared to score
    console.log("A");
}

// ✅ Workaround — switch(true) pattern
switch (true) {
  case score >= 90:
    console.log("A");
    break;
  case score >= 75:
    console.log("B");   // this runs
    break;
  default:
    console.log("C");
}
```

---

#### Difference between `if/else` and `switch`

| | if / else | switch |
|-|-----------|--------|
| Comparison | Any expression | Strict equality (`===`) |
| Ranges | ✅ Yes | ❌ No |
| Readability | Better for complex logic | Better for many exact values |
| Fall-through | ❌ No | ✅ Yes (needs `break`) |
| Performance | Similar for few cases | Slightly faster for many cases |

```js
// switch is cleaner for many exact values
switch (status) {
  case "active": ...; break;
  case "inactive": ...; break;
  case "pending": ...; break;
}

// if/else is better for ranges or complex conditions
if (age < 13) { ... }
else if (age < 18) { ... }
else { ... }
```

---

#### What is fall-through in switch?

Fall-through is when execution **continues into the next case** after a match, because there is no `break`. It can be a bug — or sometimes used intentionally.

```js
// ⚠️ Accidental fall-through (bug)
switch (role) {
  case "admin":
    console.log("Full access");  // no break!
  case "user":
    console.log("Limited access"); // also runs for admin
}

// ✅ Intentional fall-through (grouping cases)
switch (day) {
  case "Saturday":
  case "Sunday":
    console.log("Weekend!"); // runs for both Saturday and Sunday
    break;
  default:
    console.log("Weekday");
}
```

---

## 🗂️ Quick Reference Summary

### Operators at a Glance

```
Arithmetic:   +  -  *  /  %  **  ++  --
Assignment:   =  +=  -=  *=  /=  %=
Comparison:   ==  ===  !=  !==  >  <  >=  <=
Logical:      &&  ||  !  ??
Ternary:      condition ? valueIfTrue : valueIfFalse
```

### Type Coercion Rules

```
"5" + 3   → "53"   (string wins with +)
"5" - 3   → 2      (numeric coercion)
"5" == 5  → true   (loose equality coerces)
"5" === 5 → false  (strict — no coercion)
```

### Falsy Values (Only 8)

```
false | 0 | -0 | 0n | "" | null | undefined | NaN
```

### `||` vs `??`

```
||  → returns first TRUTHY value (skips all falsy)
??  → returns first NON-NULL/UNDEFINED value (keeps 0 and "")
```

### Control Flow Tools

```
if / else if / else  → condition-based branching
switch               → exact value matching with fall-through
ternary (? :)        → inline single-expression decision
```

---

> 📅 **Day 02 Complete — 18 days to go!**
> Next up: Loops — `for`, `while`, `do...while`, `for...of`, `for...in`
