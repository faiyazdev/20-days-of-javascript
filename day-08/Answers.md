# Day 08 — Answers

## JavaScript Arrays: The Complete Reference Guide

> Arrays are the most-used data structure in JavaScript. This guide walks through every creation method, every built-in array method (mutating and non-mutating), destructuring patterns, static methods, iterator methods, and the new immutable array API — with practical code for each.

---

# 🔵 Array Fundamentals

---

### 1. What is an Array in JavaScript?

An **array** is an ordered, **index-based** collection of values. Unlike objects (which use named keys), arrays use **numeric indices** starting at `0`.

```js
const fruits = ["apple", "banana", "mango"];

console.log(fruits[0]); // "apple"
console.log(fruits[1]); // "banana"
console.log(fruits.length); // 3
```

Arrays can hold **any type** of value, even mixed types:

```js
const mixed = [1, "two", true, { name: "object" }, [1, 2, 3], null];
```

Technically, arrays in JS are a special type of **object** — `typeof []` returns `"object"` — but they have special behavior (the `length` property auto-updates, numeric keys, and a huge set of built-in array methods).

```js
console.log(typeof []);          // "object"
console.log(Array.isArray([]));  // true ← the correct way to check
```

---

### 2. How to Create an Array in JavaScript?

There are several ways:

```js
// 1. Array literal — most common ✅
const a = [1, 2, 3];

// 2. Array constructor
const b = new Array(1, 2, 3);     // [1, 2, 3]
const c = new Array(5);           // ⚠️ creates an EMPTY array of length 5, not [5]!
console.log(c);                   // [ <5 empty items> ]
console.log(c.length);            // 5

// 3. Array.of() — fixes the single-number issue
const d = Array.of(5);            // [5]

// 4. Array.from() — build from iterable or array-like
const e = Array.from("hello");    // ["h", "e", "l", "l", "o"]
const f = Array.from({ length: 3 }, (_, i) => i * 2); // [0, 2, 4]
```

> **Prefer array literals `[]`** for everyday use — they're shorter, clearer, and avoid the `new Array(5)` ambiguity trap.

---

### 3. How to Get Elements from an Array in JS?

**By index:**

```js
const colors = ["red", "green", "blue"];

console.log(colors[0]);  // "red"
console.log(colors[2]);  // "blue"
console.log(colors[10]); // undefined — out of bounds, no error
```

**By negative index — using `at()` (modern):**

```js
console.log(colors.at(-1)); // "blue" — last element
console.log(colors[colors.length - 1]); // "blue" — old way, same result
```

**By destructuring:**

```js
const [first, second] = colors;
console.log(first, second); // "red" "green"
```

**Getting a sub-range with `slice()`:**

```js
const nums = [10, 20, 30, 40, 50];
console.log(nums.slice(1, 3)); // [20, 30] — non-mutating
```

---

### 4. How to Add Elements to an Array in JS?

```js
const arr = [1, 2, 3];

// Add to the end — push() (mutating, returns new length)
arr.push(4);
console.log(arr); // [1, 2, 3, 4]

// Add to the beginning — unshift() (mutating, returns new length)
arr.unshift(0);
console.log(arr); // [0, 1, 2, 3, 4]

// Add in the middle — splice() (mutating)
arr.splice(2, 0, "inserted"); // at index 2, delete 0, insert "inserted"
console.log(arr); // [0, 1, "inserted", 2, 3, 4]

// Add without mutating — spread operator ✅ preferred in modern JS
const original = [1, 2, 3];
const withEnd = [...original, 4];        // [1, 2, 3, 4]
const withStart = [0, ...original];      // [0, 1, 2, 3]
```

| Method | Position | Mutates? | Returns |
|--------|----------|----------|---------|
| `push()` | End | ✅ Yes | New length |
| `unshift()` | Start | ✅ Yes | New length |
| `splice()` | Anywhere | ✅ Yes | Removed items (empty if just adding) |
| `[...arr, x]` | Anywhere | ❌ No | New array |

---

### 5. How to Remove Elements from an Array in JS?

```js
const arr = [1, 2, 3, 4, 5];

// Remove from end — pop() (mutating, returns removed item)
const last = arr.pop();
console.log(last, arr); // 5  [1, 2, 3, 4]

// Remove from start — shift() (mutating, returns removed item)
const first = arr.shift();
console.log(first, arr); // 1  [2, 3, 4]

// Remove from middle — splice() (mutating, returns removed items as array)
const removed = arr.splice(1, 1); // at index 1, remove 1 item
console.log(removed, arr); // [3]  [2, 4]

// Remove without mutating — filter() ✅ preferred for non-mutating removal
const nums = [1, 2, 3, 4, 5];
const withoutThree = nums.filter(n => n !== 3);
console.log(withoutThree); // [1, 2, 4, 5]
console.log(nums);         // [1, 2, 3, 4, 5] — original untouched
```

| Method | Position | Mutates? |
|--------|----------|----------|
| `pop()` | End | ✅ Yes |
| `shift()` | Start | ✅ Yes |
| `splice()` | Anywhere | ✅ Yes |
| `filter()` | Conditional | ❌ No |

---

### 6. How to Copy and Clone an Array in JS?

**Shallow copy methods:**

```js
const original = [1, 2, { nested: true }];

// Spread operator ✅ most common
const copy1 = [...original];

// slice() with no arguments
const copy2 = original.slice();

// Array.from()
const copy3 = Array.from(original);

// concat() with nothing
const copy4 = [].concat(original);

copy1.push(99);
console.log(original.length); // 3 — unaffected at top level
```

**⚠️ All of these are shallow** — nested objects/arrays are still shared:

```js
copy1[2].nested = false;
console.log(original[2].nested); // false — shared reference!
```

**Deep clone — for nested structures:**

```js
// Modern, built-in:
const deepCopy = structuredClone(original);
deepCopy[2].nested = true;
console.log(original[2].nested); // false — untouched ✅

// JSON method (loses functions, undefined, Dates become strings)
const deepCopy2 = JSON.parse(JSON.stringify(original));
```

| Method | Depth | Notes |
|--------|-------|-------|
| `[...arr]` | Shallow | Most common, clean syntax |
| `arr.slice()` | Shallow | Pre-ES6 way |
| `Array.from(arr)` | Shallow | Also converts array-likes |
| `structuredClone(arr)` | Deep | Modern, handles most types |
| `JSON.parse(JSON.stringify(arr))` | Deep | Loses functions/undefined |

---

### 7. How to Determine if a Value is an Array in JS?

Use **`Array.isArray()`** — the only reliable way.

```js
console.log(Array.isArray([1, 2, 3]));     // true
console.log(Array.isArray("hello"));        // false
console.log(Array.isArray({ length: 0 }));  // false — array-like, not an array
console.log(Array.isArray(new Array()));    // true
```

**Why not `typeof`?**

```js
console.log(typeof []); // "object" — useless for distinguishing arrays from objects
```

**Why not `instanceof`?**

```js
console.log([] instanceof Array); // true — works, BUT...

// Fails across different execution contexts (e.g. iframes):
// an array created in another frame won't be `instanceof` this frame's Array
```

`Array.isArray()` works correctly across all contexts and is the **industry standard** check.

---

# 🔵 Array Destructuring

---

### 8. What is Array Destructuring in JavaScript?

**Array destructuring** lets you unpack values from an array into individual variables, **by position**, in one concise statement.

```js
const colors = ["red", "green", "blue"];

// Old way:
const c1 = colors[0];
const c2 = colors[1];

// Destructuring:
const [first, second, third] = colors;

console.log(first);  // "red"
console.log(second); // "green"
console.log(third);  // "blue"
```

Position matters — the order of variables on the left matches the order of elements on the right.

```js
function getCoordinates() {
  return [40.7128, -74.0060];
}

const [latitude, longitude] = getCoordinates();
console.log(latitude, longitude); // 40.7128 -74.006
```

---

### 9. How to Assign a Default Value to a Variable?

If the array doesn't have a value at that position (`undefined`), you can provide a **default**:

```js
const [a = 10, b = 20, c = 30] = [1, 2];

console.log(a); // 1  (from array)
console.log(b); // 2  (from array)
console.log(c); // 30 (default — array had nothing at index 2)
```

**Practical use — function parameters with defaults:**

```js
function createUser([name = "Guest", role = "user"] = []) {
  console.log(`${name} is a ${role}`);
}

createUser(["Rahim", "admin"]); // "Rahim is a admin"
createUser(["Karim"]);          // "Karim is a user" — role defaulted
createUser([]);                  // "Guest is a user" — both defaulted
createUser();                    // "Guest is a user" — default param `= []` kicks in
```

> Defaults only apply when the value is **exactly `undefined`** — not `null`, `0`, or `""`.

```js
const [x = 100] = [null];
console.log(x); // null — default NOT applied (null is not undefined)

const [y = 100] = [undefined];
console.log(y); // 100 — default applied
```

---

### 10. How to Skip a Value in an Array?

Use an **empty comma** to skip positions you don't need:

```js
const numbers = [10, 20, 30, 40, 50];

const [first, , third, , fifth] = numbers;

console.log(first); // 10
console.log(third);  // 30
console.log(fifth);  // 50
```

**Practical example — skipping unwanted return values:**

```js
function getStats() {
  return [100, 50, 25, 10]; // [total, average, min, max]
}

const [total, , min, max] = getStats(); // skip average
console.log(total, min, max); // 100 25 10
```

---

### 11. Nested Array Destructuring in JS

When arrays contain other arrays, you can destructure **multiple levels deep** by mirroring the nested structure on the left side.

```js
const nested = [1, [2, 3], [4, [5, 6]]];

const [a, [b, c], [d, [e, f]]] = nested;

console.log(a, b, c, d, e, f); // 1 2 3 4 5 6
```

**Real-world example — matrix/coordinate data:**

```js
const points = [[0, 0], [10, 20], [30, 40]];

const [[x1, y1], [x2, y2]] = points;

console.log(x1, y1); // 0 0
console.log(x2, y2); // 10 20
```

**Mixing nested destructuring with defaults and skips:**

```js
const data = [1, [2], [3, [4, 5]]];

const [a, [b = 99], [, [, e]]] = data;

console.log(a, b, e); // 1 2 5
```

---

### 12. How to Use the Rest Parameter in JS?

The **rest parameter** (`...`) collects the **remaining elements** into a new array. It must be the **last** item in the destructuring pattern.

```js
const numbers = [1, 2, 3, 4, 5];

const [first, second, ...rest] = numbers;

console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]
```

**Rest in function parameters — accept unlimited arguments:**

```js
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}

console.log(sum(1, 2, 3));       // 6
console.log(sum(1, 2, 3, 4, 5)); // 15
```

**Combining named params with rest:**

```js
function logScores(first, second, ...others) {
  console.log("1st:", first);
  console.log("2nd:", second);
  console.log("Others:", others);
}

logScores(100, 95, 80, 75, 60);
// 1st: 100
// 2nd: 95
// Others: [80, 75, 60]
```

> ⚠️ Rest must always come **last** — `[...rest, last]` is a `SyntaxError`.

---

### 13. How to Use the Spread Operator in JS?

The **spread operator** (`...`) does the **opposite** of rest — it expands an array (or any iterable) into individual elements.

```js
const nums = [1, 2, 3];

console.log(...nums); // 1 2 3 (as separate arguments)

// Spreading into a new array:
const moreNums = [...nums, 4, 5];
console.log(moreNums); // [1, 2, 3, 4, 5]

// Spreading into function arguments:
function add(a, b, c) { return a + b + c; }
console.log(add(...nums)); // 6
```

**Common spread use cases:**

```js
// 1. Copying an array
const copy = [...nums];

// 2. Merging arrays
const merged = [...nums, ...[4, 5, 6]];

// 3. Converting a string to an array of characters
const chars = [...."hello"]; // ["h", "e", "l", "l", "o"]

// 4. Converting a Set to an array
const unique = [...new Set([1, 1, 2, 2, 3])]; // [1, 2, 3]

// 5. Spreading into Math functions
console.log(Math.max(...[5, 2, 9, 1])); // 9
```

**Rest vs Spread — they look the same but do opposite things:**

| | Rest (`...`) | Spread (`...`) |
|---|------------|----------------|
| Direction | Gathers values **into** an array | Expands an array **out** into values |
| Where used | Left side of destructuring, function params | Right side, function calls, array/object literals |
| Example | `const [a, ...rest] = arr` | `const newArr = [...arr]` |

---

### 14. Destructuring Use Cases in JavaScript

**1. Returning multiple values from a function:**

```js
function getMinMax(arr) {
  return [Math.min(...arr), Math.max(...arr)];
}

const [min, max] = getMinMax([3, 7, 1, 9, 4]);
console.log(min, max); // 1 9
```

**2. Looping with destructuring:**

```js
const users = [["Rahim", 25], ["Karim", 30], ["Sana", 28]];

for (const [name, age] of users) {
  console.log(`${name} is ${age} years old`);
}
```

**3. Destructuring `Object.entries()`:**

```js
const settings = { theme: "dark", lang: "en" };

for (const [key, value] of Object.entries(settings)) {
  console.log(`${key}: ${value}`);
}
```

**4. Ignoring unwanted return values:**

```js
const [, secondItem] = ["skip me", "use me"];
console.log(secondItem); // "use me"
```

---

### 15. How to Swap Values with Destructuring?

Before destructuring, swapping two variables required a **temporary variable**:

```js
// Old way:
let a = 1, b = 2;
let temp = a;
a = b;
b = temp;
console.log(a, b); // 2 1
```

**With array destructuring — one clean line:**

```js
let a = 1, b = 2;
[a, b] = [b, a];
console.log(a, b); // 2 1
```

**Swapping more than two values:**

```js
let x = 1, y = 2, z = 3;
[x, y, z] = [z, x, y];
console.log(x, y, z); // 3 1 2
```

**Swapping array elements (common in sorting algorithms):**

```js
const arr = [10, 20, 30];
[arr[0], arr[2]] = [arr[2], arr[0]];
console.log(arr); // [30, 20, 10]
```

---

### 16. How to Merge Arrays?

**1. Spread operator — most common, non-mutating:**

```js
const a = [1, 2, 3];
const b = [4, 5, 6];

const merged = [...a, ...b];
console.log(merged); // [1, 2, 3, 4, 5, 6]
```

**2. `concat()` — non-mutating, traditional method:**

```js
const merged2 = a.concat(b);
console.log(merged2); // [1, 2, 3, 4, 5, 6]

// Multiple arrays at once:
const c = [7, 8];
const merged3 = a.concat(b, c); // [1,2,3,4,5,6,7,8]
```

**3. Merging with extra values mixed in:**

```js
const merged4 = [0, ...a, 3.5, ...b, 7];
console.log(merged4); // [0, 1, 2, 3, 3.5, 4, 5, 6, 7]
```

**4. Removing duplicates while merging — combine with `Set`:**

```js
const x = [1, 2, 3];
const y = [3, 4, 5];

const uniqueMerged = [...new Set([...x, ...y])];
console.log(uniqueMerged); // [1, 2, 3, 4, 5]
```

| Method | Mutates original? | Notes |
|--------|-------------------|-------|
| `[...a, ...b]` | ❌ No | Most readable, ES6+ |
| `a.concat(b)` | ❌ No | Pre-ES6, still valid |
| `a.push(...b)` | ✅ Yes (mutates `a`) | Use only if mutation is intended |

---

# 🔵 Core Properties & Methods

---

### 17. The `length` Property

`length` returns the number of elements in an array. It's **automatically updated** whenever you add or remove elements.

```js
const arr = [1, 2, 3];
console.log(arr.length); // 3

arr.push(4);
console.log(arr.length); // 4
```

**`length` is writable — you can use it to truncate or extend an array:**

```js
const nums = [1, 2, 3, 4, 5];

nums.length = 3; // truncate
console.log(nums); // [1, 2, 3]

nums.length = 5; // extend with empty slots
console.log(nums); // [1, 2, 3, <2 empty items>]

nums.length = 0; // clear the entire array
console.log(nums); // []
```

> Setting `arr.length = 0` is a common (and fast) way to empty an array in place.

---

### 18. JavaScript Array Methods — Create, Remove, Update, Access

Here's a high-level map before diving into individual methods:

| Operation | Methods |
|-----------|---------|
| **Create** | `[]`, `new Array()`, `Array.of()`, `Array.from()` |
| **Access** | `[]`, `at()`, `slice()`, `find()` |
| **Add** | `push()`, `unshift()`, `splice()`, spread |
| **Remove** | `pop()`, `shift()`, `splice()`, `filter()` |
| **Update** | direct index assignment, `splice()`, `fill()`, `with()` |
| **Search** | `indexOf()`, `includes()`, `find()`, `findIndex()` |
| **Transform** | `map()`, `flat()`, `flatMap()`, `sort()`, `reverse()` |
| **Combine/Reduce** | `concat()`, `reduce()`, `join()` |

---

### 19. The `concat()` Array Method

Merges two or more arrays into a **new array**. Does not mutate the originals.

```js
const a = [1, 2];
const b = [3, 4];

const result = a.concat(b);
console.log(result); // [1, 2, 3, 4]
console.log(a);       // [1, 2] — unchanged

// Concat multiple arrays + values:
const result2 = a.concat(b, [5, 6], 7);
console.log(result2); // [1, 2, 3, 4, 5, 6, 7]
```

---

### 20. The `join()` Array Method

Converts an array into a **string**, joining elements with a specified separator (default is comma).

```js
const fruits = ["apple", "banana", "mango"];

console.log(fruits.join());      // "apple,banana,mango"
console.log(fruits.join(" - ")); // "apple - banana - mango"
console.log(fruits.join(""));    // "applebananamango"

// Common use — CSV-style strings:
const csvRow = [101, "Rahim", "Dhaka"].join(",");
console.log(csvRow); // "101,Rahim,Dhaka"
```

---

### 21. The `fill()` Array Method

Overwrites elements in an array with a **static value**, mutating the original array. Accepts optional `start` and `end` indices.

```js
const arr = [1, 2, 3, 4, 5];

arr.fill(0); // fill entire array
console.log(arr); // [0, 0, 0, 0, 0]

const arr2 = [1, 2, 3, 4, 5];
arr2.fill(9, 1, 3); // fill with 9, from index 1 up to (not including) 3
console.log(arr2); // [1, 9, 9, 4, 5]
```

**Common use — initializing an array:**

```js
const zeros = new Array(5).fill(0);
console.log(zeros); // [0, 0, 0, 0, 0]

const grid = Array.from({ length: 3 }, () => new Array(3).fill(null));
console.log(grid); // [[null,null,null],[null,null,null],[null,null,null]]
```

---

### 22. The `includes()` Array Method

Checks whether an array **contains** a value — returns a `boolean`.

```js
const nums = [1, 2, 3, 4, 5];

console.log(nums.includes(3));  // true
console.log(nums.includes(10)); // false

// Works for strings too:
const fruits = ["apple", "banana"];
console.log(fruits.includes("banana")); // true
```

**`includes()` correctly detects `NaN`, unlike `indexOf()`:**

```js
const arr = [NaN];
console.log(arr.indexOf(NaN));  // -1 ❌ — indexOf uses strict equality, NaN !== NaN
console.log(arr.includes(NaN)); // true ✅ — includes uses SameValueZero algorithm
```

---

### 23. The `indexOf()` Array Method

Returns the **first index** at which a given value is found, or `-1` if not found. Uses **strict equality** (`===`).

```js
const fruits = ["apple", "banana", "mango", "banana"];

console.log(fruits.indexOf("banana"));  // 1 (first match)
console.log(fruits.indexOf("grape"));   // -1 (not found)

// With a starting search index:
console.log(fruits.indexOf("banana", 2)); // 3 (search starts from index 2)
```

**Common use — checking existence (older style, before `includes()`):**

```js
if (fruits.indexOf("mango") !== -1) {
  console.log("Found mango!");
}
```

**`lastIndexOf()` — the reverse-search version:**

```js
console.log(fruits.lastIndexOf("banana")); // 3 (last match)
```

---

### 24. The `reverse()` Array Method

Reverses the order of elements **in place** — **mutates** the original array.

```js
const arr = [1, 2, 3, 4, 5];

arr.reverse();
console.log(arr); // [5, 4, 3, 2, 1] — original array IS changed!
```

To avoid mutation, copy first (or use the new `toReversed()` — see Immutability section):

```js
const original = [1, 2, 3];
const reversedCopy = [...original].reverse();

console.log(original);     // [1, 2, 3] — untouched
console.log(reversedCopy); // [3, 2, 1]
```

---

### 25. The `sort()` Array Method

Sorts the array elements **in place** — **mutates** the original. By default, sorts as **strings** (which causes a classic bug with numbers).

```js
const fruits = ["banana", "apple", "mango"];
fruits.sort();
console.log(fruits); // ["apple", "banana", "mango"] ✅ works fine for strings

// ⚠️ The classic numeric sort bug:
const nums = [10, 1, 21, 2];
nums.sort();
console.log(nums); // [1, 10, 2, 21] ❌ sorted as STRINGS, not numbers!
```

**Always provide a compare function for numbers:**

```js
const nums = [10, 1, 21, 2];

// Ascending:
nums.sort((a, b) => a - b);
console.log(nums); // [1, 2, 10, 21] ✅

// Descending:
nums.sort((a, b) => b - a);
console.log(nums); // [21, 10, 2, 1] ✅
```

**Sorting objects by a property:**

```js
const users = [
  { name: "Karim", age: 30 },
  { name: "Rahim", age: 22 },
  { name: "Sana", age: 28 }
];

users.sort((a, b) => a.age - b.age);
console.log(users.map(u => u.name)); // ["Rahim", "Sana", "Karim"]
```

**Compare function logic:**

| Return Value | Meaning |
|--------------|---------|
| Negative | `a` comes before `b` |
| Positive | `b` comes before `a` |
| `0` | Order unchanged (equal) |

---

### 26. The `splice()` Array Method

The **swiss-army knife** of array methods — can **remove**, **add**, and **replace** elements, all in one call. **Mutates** the original array.

```js
splice(start, deleteCount, ...itemsToInsert)
```

```js
const arr = [1, 2, 3, 4, 5];

// Remove 2 elements starting at index 1:
const removed = arr.splice(1, 2);
console.log(removed); // [2, 3] — the removed items
console.log(arr);      // [1, 4, 5]
```

```js
const arr2 = [1, 2, 3];

// Insert without removing (deleteCount = 0):
arr2.splice(1, 0, "a", "b");
console.log(arr2); // [1, "a", "b", 2, 3]
```

```js
const arr3 = [1, 2, 3, 4];

// Replace elements (remove + insert in one call):
arr3.splice(1, 2, "x", "y", "z");
console.log(arr3); // [1, "x", "y", "z", 4]
```

**Negative start index — counts from the end:**

```js
const arr4 = [1, 2, 3, 4, 5];
arr4.splice(-2, 1); // remove 1 item starting 2 from the end
console.log(arr4);  // [1, 2, 3, 5]
```

> `splice()` vs `slice()` — easy to confuse: **splice mutates**, **slice doesn't**.

---

### 27. The `at()` Method

Returns the element at a given index — but unlike bracket notation, it supports **negative indices** to count from the end.

```js
const arr = [10, 20, 30, 40, 50];

console.log(arr.at(0));  // 10
console.log(arr.at(-1)); // 50 — last element
console.log(arr.at(-2)); // 40 — second to last

// Old way to get last element (more verbose):
console.log(arr[arr.length - 1]); // 50
```

**Useful when chaining:**

```js
const lastItem = getItems().at(-1); // cleaner than getItems()[getItems().length - 1]
```

---

### 28. The `copyWithin()` Method

Copies a **sequence of elements** within the same array, to another position in that same array — **mutating** it. Rarely used, but good to know.

```js
copyWithin(target, start, end)
```

```js
const arr = [1, 2, 3, 4, 5];

// Copy elements from index 0–2 (exclusive of 2) to position 3:
arr.copyWithin(3, 0, 2);
console.log(arr); // [1, 2, 3, 1, 2]
```

```js
const arr2 = ["a", "b", "c", "d", "e"];
arr2.copyWithin(0, 3); // copy from index 3 to the end, paste starting at 0
console.log(arr2); // ["d", "e", "c", "d", "e"]
```

---

### 29. The `flat()` Method

Flattens **nested arrays** into a single-level array, up to a specified depth (default: `1`).

```js
const nested = [1, [2, 3], [4, [5, 6]]];

console.log(nested.flat());    // [1, 2, 3, 4, [5, 6]]  — depth 1
console.log(nested.flat(2));   // [1, 2, 3, 4, 5, 6]    — depth 2
console.log(nested.flat(Infinity)); // flattens ALL levels, no matter how deep
```

**Practical use — flattening API results or grouped data:**

```js
const ordersByUser = [[1, 2], [3], [4, 5, 6]];
const allOrders = ordersByUser.flat();
console.log(allOrders); // [1, 2, 3, 4, 5, 6]
```

**Removing empty slots (sparse arrays) with `flat()`:**

```js
const sparse = [1, , 3, , 5];
console.log(sparse.flat()); // [1, 3, 5] — empty slots removed
```

---

### 30. Grouping Elements in an Array

Modern JavaScript (ES2024) introduced `Object.groupBy()` and `Map.groupBy()` to group array elements by a computed key — replacing manual `reduce()` patterns.

**`Object.groupBy()` — groups into a plain object:**

```js
const products = [
  { name: "Laptop", category: "Electronics" },
  { name: "Shirt", category: "Clothing" },
  { name: "Phone", category: "Electronics" },
  { name: "Pants", category: "Clothing" }
];

const grouped = Object.groupBy(products, (item) => item.category);

console.log(grouped);
// {
//   Electronics: [{ name: "Laptop", ... }, { name: "Phone", ... }],
//   Clothing: [{ name: "Shirt", ... }, { name: "Pants", ... }]
// }
```

**`Map.groupBy()` — groups into a `Map` (useful for non-string keys):**

```js
const groupedMap = Map.groupBy(products, (item) => item.category);
console.log(groupedMap.get("Electronics")); // [Laptop, Phone]
```

**Manual grouping with `reduce()` (the pattern used before ES2024):**

```js
const grouped2 = products.reduce((acc, item) => {
  const key = item.category;
  if (!acc[key]) acc[key] = [];
  acc[key].push(item);
  return acc;
}, {});
```

> `Object.groupBy()` and `Map.groupBy()` are relatively new (2024) — check browser/Node compatibility before relying on them in production without a polyfill.

---

# 🔵 Static Array Methods

---

### 31. What Are Static Array Methods in JavaScript?

**Static methods** are called directly on the `Array` constructor itself — not on an array instance. They typically help **create** arrays from other data sources.

```js
Array.isArray([1, 2, 3]); // called on Array, not on an instance
Array.from("hello");
Array.of(1, 2, 3);
```

Compare to **instance methods** which are called on an actual array:

```js
const arr = [1, 2, 3];
arr.map(x => x * 2); // called ON the array instance
```

| Static Methods | Instance Methods |
|----------------|-------------------|
| `Array.isArray()` | `arr.map()` |
| `Array.from()` | `arr.filter()` |
| `Array.fromAsync()` | `arr.push()` |
| `Array.of()` | `arr.sort()` |

---

### 32. What is the Array-Like?

An **array-like object** has a `length` property and indexed elements (`0`, `1`, `2`, ...), but it is **not** a true array — meaning it doesn't have access to array methods like `map`, `filter`, `push`, etc.

```js
const arrayLike = {
  0: "a",
  1: "b",
  2: "c",
  length: 3
};

console.log(arrayLike.length); // 3
console.log(arrayLike[0]);     // "a"
console.log(Array.isArray(arrayLike)); // false ❌ — not a real array

arrayLike.map(x => x); // ❌ TypeError: arrayLike.map is not a function
```

**Common real-world array-likes:**

```js
// 1. The `arguments` object inside regular functions:
function showArgs() {
  console.log(arguments);          // Arguments(3) [1, 2, 3]
  console.log(Array.isArray(arguments)); // false
}
showArgs(1, 2, 3);

// 2. DOM NodeLists:
const divs = document.querySelectorAll("div"); // NodeList — array-like
```

To use array methods on array-likes, you must first convert them to a real array — using **`Array.from()`**.

---

### 33. The `Array.from()` Array Method

Converts an **iterable** (string, Set, Map, etc.) or an **array-like** object into a true array.

```js
// From a string (iterable):
console.log(Array.from("hello")); // ["h", "e", "l", "l", "o"]

// From a Set:
console.log(Array.from(new Set([1, 2, 2, 3]))); // [1, 2, 3]

// From an array-like:
const arrayLike = { 0: "a", 1: "b", length: 2 };
console.log(Array.from(arrayLike)); // ["a", "b"]

// From `arguments`:
function sumAll() {
  const args = Array.from(arguments);
  return args.reduce((a, b) => a + b, 0);
}
console.log(sumAll(1, 2, 3)); // 6
```

**With a map function (2nd argument) — transform while converting:**

```js
// Generate an array of squares:
const squares = Array.from({ length: 5 }, (_, i) => i * i);
console.log(squares); // [0, 1, 4, 9, 16]

// Convert NodeList and extract text:
const headings = Array.from(document.querySelectorAll("h2"), el => el.textContent);
```

---

### 34. The `Array.fromAsync()` Array Method

A newer (ES2024) method that creates an array from an **async iterable**, an iterable of promises, or an array-like object — and **returns a Promise** that resolves to the array.

```js
async function* generateNumbers() {
  yield 1;
  yield 2;
  yield 3;
}

const result = await Array.fromAsync(generateNumbers());
console.log(result); // [1, 2, 3]
```

**With an array of Promises:**

```js
const promises = [Promise.resolve(1), Promise.resolve(2), Promise.resolve(3)];

const resolved = await Array.fromAsync(promises);
console.log(resolved); // [1, 2, 3]
```

**Compared to `Promise.all(Array.from(...))`:**

```js
// Old way:
const result1 = await Promise.all(Array.from(promises));

// New way — cleaner:
const result2 = await Array.fromAsync(promises);
```

> `Array.fromAsync()` is very new — verify Node.js/browser support before using it in production.

---

### 35. The `Array.of()` Array Method

Creates a new array from its arguments — regardless of how many or what type. **Fixes the ambiguity of `new Array(n)`**.

```js
console.log(Array.of(7));        // [7]       ✅ array with one element: 7
console.log(new Array(7));       // [ <7 empty items> ] ❌ array of length 7!

console.log(Array.of(1, 2, 3));  // [1, 2, 3]
console.log(Array.of("a", "b")); // ["a", "b"]
```

This is the key difference: `Array.of()` always treats its arguments as **elements**, while `Array(n)` treats a **single number** argument as a **length**.

```js
console.log(Array(1, 2, 3)); // [1, 2, 3] — multiple args work fine
console.log(Array(3));       // [ <3 empty items> ] — single number = length!
console.log(Array.of(3));    // [3] — always treated as an element ✅
```

---

# 🔵 Array Iterator Methods

> These are the methods that make JavaScript arrays so powerful for **functional, declarative** programming — instead of manual `for` loops, you describe *what* you want.

---

### 36. The `filter()` Array Method

Creates a **new array** containing only the elements that pass a test function. **Non-mutating.**

```js
const nums = [1, 2, 3, 4, 5, 6];

const evens = nums.filter(n => n % 2 === 0);
console.log(evens); // [2, 4, 6]
console.log(nums);  // [1, 2, 3, 4, 5, 6] — unchanged
```

**Real-world example — filtering objects:**

```js
const users = [
  { name: "Rahim", active: true },
  { name: "Karim", active: false },
  { name: "Sana", active: true }
];

const activeUsers = users.filter(user => user.active);
console.log(activeUsers.map(u => u.name)); // ["Rahim", "Sana"]
```

---

### 37. The `map()` Array Method

Creates a **new array** by transforming **every** element with a callback. The result is **always the same length** as the original. **Non-mutating.**

```js
const nums = [1, 2, 3];

const doubled = nums.map(n => n * 2);
console.log(doubled); // [2, 4, 6]
```

**Real-world example — transforming API data (extremely common in React):**

```js
const users = [
  { id: 1, firstName: "Rahim", lastName: "Khan" },
  { id: 2, firstName: "Karim", lastName: "Ahmed" }
];

const fullNames = users.map(u => `${u.firstName} ${u.lastName}`);
console.log(fullNames); // ["Rahim Khan", "Karim Ahmed"]

// In React JSX:
// {users.map(user => <UserCard key={user.id} user={user} />)}
```

> `map()` vs `forEach()`: `map` **returns a new array**; `forEach` returns `undefined`. Use `map` when you need the transformed results.

---

### 38. The `reduce()` Array Method

**Folds** an array down into a **single accumulated value** by running a reducer function over each element.

```js
reduce((accumulator, currentValue, index, array) => {...}, initialValue)
```

```js
const nums = [1, 2, 3, 4, 5];

const total = nums.reduce((acc, curr) => acc + curr, 0);
console.log(total); // 15
```

**Step-by-step trace:**

```
acc=0, curr=1 → return 1
acc=1, curr=2 → return 3
acc=3, curr=3 → return 6
acc=6, curr=4 → return 10
acc=10, curr=5 → return 15  ← final result
```

**`reduce()` can build objects, not just sum numbers:**

```js
const fruits = ["apple", "banana", "apple", "mango", "apple", "banana"];

const counts = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});

console.log(counts); // { apple: 3, banana: 2, mango: 1 }
```

**Finding max value with `reduce()`:**

```js
const nums2 = [4, 8, 2, 10, 6];
const max = nums2.reduce((a, b) => (a > b ? a : b));
console.log(max); // 10
```

> ⚠️ Always provide an `initialValue` for sums/objects to avoid bugs on empty arrays.

```js
const empty = [];
empty.reduce((a, b) => a + b); // ❌ TypeError: Reduce of empty array with no initial value
empty.reduce((a, b) => a + b, 0); // ✅ 0
```

---

### 39. The `reduceRight()` Array Method

Same as `reduce()`, but processes the array from **right to left** instead of left to right.

```js
const letters = ["a", "b", "c", "d"];

const leftToRight = letters.reduce((acc, curr) => acc + curr, "");
console.log(leftToRight); // "abcd"

const rightToLeft = letters.reduceRight((acc, curr) => acc + curr, "");
console.log(rightToLeft); // "dcba"
```

**Practical use — function composition (right-to-left, like math notation):**

```js
const compose = (...fns) => (x) =>
  fns.reduceRight((acc, fn) => fn(acc), x);

const double = x => x * 2;
const addOne = x => x + 1;

const transform = compose(double, addOne); // double(addOne(x))
console.log(transform(5)); // (5+1)*2 = 12
```

---

### 40. The `some()` Array Method

Tests whether **at least one** element passes a test. Returns a `boolean`. Stops as soon as it finds a match (short-circuits).

```js
const nums = [1, 3, 5, 8, 9];

console.log(nums.some(n => n % 2 === 0)); // true — 8 is even
console.log(nums.some(n => n > 100));      // false
```

**Real-world example — permission checking:**

```js
const userRoles = ["editor", "viewer"];
const allowedRoles = ["admin", "editor"];

const hasAccess = userRoles.some(role => allowedRoles.includes(role));
console.log(hasAccess); // true
```

---

### 41. The `every()` Array Method

Tests whether **ALL** elements pass a test. Returns a `boolean`. Stops as soon as one fails (short-circuits).

```js
const nums = [2, 4, 6, 8];

console.log(nums.every(n => n % 2 === 0)); // true — all even
console.log(nums.every(n => n > 5));        // false — not all > 5
```

**Real-world example — form validation:**

```js
const formFields = [
  { name: "email", value: "test@test.com" },
  { name: "password", value: "secret123" }
];

const allFilled = formFields.every(field => field.value.trim() !== "");
console.log(allFilled); // true
```

**`some()` vs `every()` quick comparison:**

| Method | Returns `true` when... |
|--------|------------------------|
| `some()` | At least ONE element passes |
| `every()` | ALL elements pass |

---

### 42. The `find()` Array Method

Returns the **first element** that satisfies a test function — or `undefined` if none match.

```js
const users = [
  { id: 1, name: "Rahim" },
  { id: 2, name: "Karim" },
  { id: 3, name: "Sana" }
];

const user = users.find(u => u.id === 2);
console.log(user); // { id: 2, name: "Karim" }

const notFound = users.find(u => u.id === 99);
console.log(notFound); // undefined
```

---

### 43. The `findIndex()` Array Method

Like `find()`, but returns the **index** of the first match instead of the element itself. Returns `-1` if not found.

```js
const users = [{ id: 1 }, { id: 2 }, { id: 3 }];

const index = users.findIndex(u => u.id === 2);
console.log(index); // 1

const notFoundIndex = users.findIndex(u => u.id === 99);
console.log(notFoundIndex); // -1
```

**Practical use — updating an item by id:**

```js
const index2 = users.findIndex(u => u.id === 2);
if (index2 !== -1) {
  users[index2] = { ...users[index2], updated: true };
}
```

---

### 44. The `findLast()` Array Method

Like `find()`, but searches from the **end** of the array and returns the **last** matching element.

```js
const nums = [5, 12, 8, 130, 44];

const lastEven = nums.findLast(n => n % 2 === 0);
console.log(lastEven); // 44 — the LAST even number found
// (find() would have returned 12, the FIRST even number)
```

---

### 45. The `findLastIndex()` Array Method

Like `findIndex()`, but searches from the end — returns the **index** of the **last** matching element.

```js
const nums = [5, 12, 8, 130, 44];

const lastEvenIndex = nums.findLastIndex(n => n % 2 === 0);
console.log(lastEvenIndex); // 4 (index of 44)
```

**`find` family summary:**

| Method | Direction | Returns |
|--------|-----------|---------|
| `find()` | Left → Right | First matching element |
| `findIndex()` | Left → Right | Index of first match |
| `findLast()` | Right → Left | Last matching element |
| `findLastIndex()` | Right → Left | Index of last match |

---

### 46. Array Method Chaining

Since `filter()`, `map()`, and similar methods return **new arrays**, you can chain them together to build powerful data pipelines.

```js
const orders = [
  { id: 1, status: "completed", total: 100 },
  { id: 2, status: "pending", total: 50 },
  { id: 3, status: "completed", total: 200 },
  { id: 4, status: "completed", total: 75 }
];

const totalCompletedRevenue = orders
  .filter(order => order.status === "completed")  // keep only completed
  .map(order => order.total)                       // extract totals
  .reduce((sum, total) => sum + total, 0);         // sum them up

console.log(totalCompletedRevenue); // 375
```

**Reading chains left to right — each step transforms the data further:**

```js
const names = users
  .filter(u => u.active)        // step 1: keep active users
  .map(u => u.name)             // step 2: extract names
  .sort()                       // step 3: alphabetical order
  .slice(0, 3);                 // step 4: take top 3
```

> Chaining is core to functional, declarative JS — and is used everywhere in React, Express data processing, and database result shaping.

---

### 47. The `forEach()` Array Method

Executes a callback **for each element** — but always returns `undefined`. Used purely for **side effects** (logging, DOM updates, API calls), not for transforming data.

```js
const nums = [1, 2, 3];

nums.forEach((value, index) => {
  console.log(`Index ${index}: ${value}`);
});
// Index 0: 1
// Index 1: 2
// Index 2: 3
```

**`forEach()` cannot be chained or `break`-ed:**

```js
const result = nums.forEach(n => n * 2);
console.log(result); // undefined — forEach doesn't return anything useful

// Can't break out early — use a regular for...of loop instead if you need to stop:
for (const n of nums) {
  if (n === 2) break; // ✅ works
}
```

| | `forEach()` | `map()` |
|---|------------|---------|
| Returns | `undefined` | New array |
| Use for | Side effects | Transformations |
| Chainable | ❌ No | ✅ Yes |

---

### 48. The `entries()` Method

Returns an **iterator** of `[index, value]` pairs — useful with `for...of` loops when you need both the index and value.

```js
const fruits = ["apple", "banana", "mango"];

for (const [index, value] of fruits.entries()) {
  console.log(index, value);
}
// 0 "apple"
// 1 "banana"
// 2 "mango"
```

**Converting to an array of pairs:**

```js
console.log([...fruits.entries()]);
// [[0, "apple"], [1, "banana"], [2, "mango"]]
```

---

### 49. The `values()` Method

Returns an **iterator** of the array's values — mostly used implicitly (e.g. `for...of` already uses it by default).

```js
const fruits = ["apple", "banana"];

const iterator = fruits.values();

console.log(iterator.next()); // { value: "apple", done: false }
console.log(iterator.next()); // { value: "banana", done: false }
console.log(iterator.next()); // { value: undefined, done: true }

// for...of uses values() under the hood automatically:
for (const fruit of fruits) {
  console.log(fruit); // "apple", "banana"
}
```

---

### 50. The `flatMap()` Array Method

Combines `map()` and `flat(1)` into a single, more efficient step: maps each element, then flattens the result by **one level**.

```js
const sentences = ["hello world", "foo bar"];

// Using map + flat separately:
const words1 = sentences.map(s => s.split(" ")).flat();
console.log(words1); // ["hello", "world", "foo", "bar"]

// Using flatMap — same result, one pass:
const words2 = sentences.flatMap(s => s.split(" "));
console.log(words2); // ["hello", "world", "foo", "bar"]
```

**Practical use — expanding items, filtering by returning empty arrays:**

```js
const nums = [1, 2, 3, 4, 5];

// Skip evens, double the odds — using flatMap as filter+map combined:
const result = nums.flatMap(n =>
  n % 2 === 0 ? [] : [n * 2]
);
console.log(result); // [2, 6, 10]
```

---

# 🔵 Immutability (ES2023)

> Older array methods like `sort()`, `reverse()`, and `splice()` **mutate** the original array — which can cause subtle bugs in React state, Redux, or anywhere immutability matters. ES2023 introduced **non-mutating twins** of these methods.

---

### 51. The `toReversed()` Method

The **immutable version** of `reverse()`. Returns a **new** reversed array, leaving the original untouched.

```js
const original = [1, 2, 3, 4, 5];

const reversed = original.toReversed();

console.log(reversed); // [5, 4, 3, 2, 1]
console.log(original); // [1, 2, 3, 4, 5] — untouched ✅
```

Compare to the old mutating way:

```js
const arr = [1, 2, 3];
arr.reverse(); // mutates `arr` directly — no copy needed beforehand
```

---

### 52. The `toSorted()` Method

The **immutable version** of `sort()`. Returns a new sorted array; original stays the same.

```js
const original = [3, 1, 4, 1, 5, 9];

const sorted = original.toSorted((a, b) => a - b);

console.log(sorted);   // [1, 1, 3, 4, 5, 9]
console.log(original); // [3, 1, 4, 1, 5, 9] — untouched ✅
```

**Especially useful in React state — no need for manual spread + sort:**

```js
// Old pattern (manual copy before mutating sort):
setItems([...items].sort((a, b) => a.price - b.price));

// New pattern — cleaner:
setItems(items.toSorted((a, b) => a.price - b.price));
```

---

### 53. The `toSpliced()` Method

The **immutable version** of `splice()`. Returns a new array with the splice operation applied — original is untouched.

```js
const original = [1, 2, 3, 4, 5];

const result = original.toSpliced(1, 2, "a", "b"); // remove 2 at index 1, insert "a","b"

console.log(result);   // [1, "a", "b", 4, 5]
console.log(original); // [1, 2, 3, 4, 5] — untouched ✅
```

---

### 54. The `with()` Method

Returns a **new array** with the element at a given index **replaced** — without mutating the original. The immutable alternative to `arr[index] = value`.

```js
const original = ["a", "b", "c", "d"];

const updated = original.with(2, "Z");

console.log(updated);  // ["a", "b", "Z", "d"]
console.log(original); // ["a", "b", "c", "d"] — untouched ✅
```

**Negative indices supported too:**

```js
const updated2 = original.with(-1, "LAST");
console.log(updated2); // ["a", "b", "c", "LAST"]
```

**Common use — updating a single item in React state immutably:**

```js
// Old pattern:
setItems(items.map((item, i) => (i === targetIndex ? newItem : item)));

// New pattern — cleaner:
setItems(items.with(targetIndex, newItem));
```

**Full Immutability Cheat Sheet:**

| Mutating (old) | Non-mutating (ES2023) |
|----------------|------------------------|
| `arr.reverse()` | `arr.toReversed()` |
| `arr.sort(fn)` | `arr.toSorted(fn)` |
| `arr.splice(...)` | `arr.toSpliced(...)` |
| `arr[i] = val` | `arr.with(i, val)` |

> ⚠️ Browser/Node support for these is relatively recent (2023+) — verify your target environment supports them, or use a polyfill / transpiler (Babel, core-js) if needed.

---

# 🔵 Tasks, Quizzes & Interview Questions

---

## 📝 Practice Tasks

### Task 1 — Flatten and Sum

Given a nested array of order totals per region, flatten it and calculate the grand total.

```js
const regionalOrders = [[100, 200], [50], [300, 150, 75]];

// Your task: write a one-liner using flat() + reduce()
const total = regionalOrders.flat().reduce((sum, val) => sum + val, 0);
console.log(total); // 875
```

---

### Task 2 — Deduplicate and Sort

Given an array of numbers with duplicates, return a sorted array of unique values — without mutating the original.

```js
const nums = [5, 3, 8, 3, 1, 5, 9];

const result = [...new Set(nums)].toSorted((a, b) => a - b);
console.log(result);  // [1, 3, 5, 8, 9]
console.log(nums);    // [5, 3, 8, 3, 1, 5, 9] — original untouched
```

---

### Task 3 — Group and Count

Given a list of transactions, group them by `type` and count how many of each.

```js
const transactions = [
  { type: "deposit", amount: 100 },
  { type: "withdrawal", amount: 50 },
  { type: "deposit", amount: 200 },
  { type: "deposit", amount: 75 },
  { type: "withdrawal", amount: 30 }
];

const counts = transactions.reduce((acc, t) => {
  acc[t.type] = (acc[t.type] || 0) + 1;
  return acc;
}, {});

console.log(counts); // { deposit: 3, withdrawal: 2 }
```

---

### Task 4 — Find the Second Highest Value

```js
const scores = [88, 95, 72, 95, 60, 91];

const secondHighest = [...new Set(scores)]
  .toSorted((a, b) => b - a)[1];

console.log(secondHighest); // 91
```

---

### Task 5 — Chunk an Array

Split an array into smaller arrays of a given size.

```js
function chunk(arr, size) {
  const result = [];
  for (let i = 0; i < arr.length; i += size) {
    result.push(arr.slice(i, i + size));
  }
  return result;
}

console.log(chunk([1, 2, 3, 4, 5, 6, 7], 3));
// [[1, 2, 3], [4, 5, 6], [7]]
```

---

## ❓ Quiz Questions

**Q1.** What does this log?

```js
const arr = [1, 2, 3];
const result = arr.map(x => x * 2).filter(x => x > 3);
console.log(result, arr);
```

<details><summary>Answer</summary>

`[4, 6]` and `[1, 2, 3]` — `map` creates `[2, 4, 6]`, `filter` keeps values `> 3` → `[4, 6]`. The original `arr` is never mutated since both methods are non-mutating.

</details>

---

**Q2.** What's the output and why?

```js
console.log([1, [2, 3], [4, [5, 6]]].flat());
```

<details><summary>Answer</summary>

`[1, 2, 3, 4, [5, 6]]` — `flat()` with no argument defaults to depth `1`, so it only flattens one level deep. The `[5, 6]` nested two levels deep stays as-is.

</details>

---

**Q3.** Will this cause an error? What's the output?

```js
const arr = [1, 2, 3];
console.log(arr.indexOf(NaN));
console.log(arr.includes(NaN));
```

<details><summary>Answer</summary>

No error. `indexOf(NaN)` returns `-1` because `indexOf` uses strict equality (`===`), and `NaN !== NaN`. But `includes(NaN)` returns `false` here too — but if `NaN` were actually IN the array, `includes` would correctly find it (using SameValueZero), while `indexOf` never would.

</details>

---

**Q4.** What's the trick here?

```js
const arr = new Array(3);
console.log(arr);
console.log(arr.length);
console.log(arr[0]);
```

<details><summary>Answer</summary>

`[ <3 empty items> ]`, `3`, `undefined`. `new Array(3)` with a single number creates an array with `length: 3` but no actual elements (sparse array) — not `[3]`. Use `Array.of(3)` to get `[3]`.

</details>

---

**Q5.** True or false: `sort()` always sorts numbers correctly by default.

<details><summary>Answer</summary>

**False.** `sort()` defaults to converting elements to strings and comparing them lexicographically. `[10, 1, 21, 2].sort()` gives `[1, 10, 2, 21]`, not `[1, 2, 10, 21]`. Always pass a compare function for numbers: `.sort((a, b) => a - b)`.

</details>

---

**Q6.** What does this print?

```js
let a = [1, 2, 3];
let b = a;
b.push(4);
console.log(a);
```

<details><summary>Answer</summary>

`[1, 2, 3, 4]` — Arrays are reference types. `b = a` doesn't copy the array; both variables point to the same array in memory. Mutating `b` mutates `a` too.

</details>

---

**Q7.** What's wrong with this reduce call, and what's the fix?

```js
const empty = [];
const sum = empty.reduce((a, b) => a + b);
```

<details><summary>Answer</summary>

`TypeError: Reduce of empty array with no initial value`. Without an `initialValue`, `reduce()` on an empty array has nothing to start with. Fix: `empty.reduce((a, b) => a + b, 0)`.

</details>

---

**Q8.** What does `splice` return here, and what does `arr` become?

```js
const arr = [1, 2, 3, 4, 5];
const result = arr.splice(1, 2);
```

<details><summary>Answer</summary>

`result` is `[2, 3]` (the removed elements). `arr` becomes `[1, 4, 5]` — `splice` mutates the original array and returns the removed items, not the modified array.

</details>

---

## 💼 Interview Questions

---

**Q1. How would you remove duplicate values from an array?**

```js
const nums = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(nums)];
console.log(unique); // [1, 2, 3, 4, 5]
```

`Set` automatically enforces uniqueness; spreading it back into an array gives you a deduplicated list in one line.

---

**Q2. What's the difference between `map()` and `forEach()`? When would you use each?**

`map()` returns a **new array** of transformed values — use it when you need the results. `forEach()` returns `undefined` — use it for **side effects** only (logging, pushing to an external array, API calls).

```js
// map — you need the output
const doubled = nums.map(n => n * 2);

// forEach — you just need to "do something"
nums.forEach(n => console.log(n));
```

---

**Q3. How do you check if an array is empty?**

```js
function isEmpty(arr) {
  return Array.isArray(arr) && arr.length === 0;
}

console.log(isEmpty([]));      // true
console.log(isEmpty([1, 2]));  // false
console.log(isEmpty("not arr")); // false — not even an array
```

---

**Q4. How would you flatten a deeply nested array of unknown depth?**

```js
const deeplyNested = [1, [2, [3, [4, [5]]]]];
console.log(deeplyNested.flat(Infinity)); // [1, 2, 3, 4, 5]
```

`Infinity` as the depth argument flattens however deep the nesting goes.

---

**Q5. Implement your own version of `map()` using `reduce()`.**

```js
function customMap(arr, callback) {
  return arr.reduce((acc, curr, index) => {
    acc.push(callback(curr, index, arr));
    return acc;
  }, []);
}

console.log(customMap([1, 2, 3], x => x * 2)); // [2, 4, 6]
```

This question tests understanding that `map`, `filter`, and even `forEach` can all be built on top of `reduce` — showing you understand what's happening under the hood.

---

**Q6. What's the safest way to clone an array containing nested objects?**

```js
const data = [{ a: 1 }, { b: { c: 2 } }];

// Shallow — nested objects still shared, often a bug source:
const shallow = [...data];

// Deep — fully independent:
const deep = structuredClone(data);
```

Mention that `JSON.parse(JSON.stringify())` is an older alternative but loses functions, `undefined`, and `Date` objects (converts dates to strings).

---

**Q7. Why is `sort()` considered risky in functional/immutable codebases, and what's the modern fix?**

`sort()` mutates the original array in place, which can cause bugs in React state, Redux reducers, or anywhere referential transparency is assumed. The fix is the ES2023 `toSorted()` method, which returns a new sorted array without touching the original.

```js
// Risky:
items.sort((a, b) => a.price - b.price); // mutates `items`!

// Safe:
const sorted = items.toSorted((a, b) => a.price - b.price);
```

---

**Q8. Given an array of user objects, write code to find the user with the highest score using `reduce()`.**

```js
const users = [
  { name: "Rahim", score: 85 },
  { name: "Karim", score: 92 },
  { name: "Sana", score: 78 }
];

const topUser = users.reduce((best, current) =>
  current.score > best.score ? current : best
);

console.log(topUser); // { name: "Karim", score: 92 }
```

---

**Q9. What does `Array.isArray(arguments)` return inside a regular function, and why?**

```js
function test() {
  console.log(Array.isArray(arguments)); // false
}
test(1, 2, 3);
```

`arguments` is an **array-like object** — it has `length` and indexed properties but is not a true `Array` instance. To use array methods on it, convert it first: `Array.from(arguments)` or use rest parameters (`...args`) instead, which IS a real array.

---

**Q10. What's the output, and what concept does this test?**

```js
const arr = [1, 2, 3];
const arr2 = arr;
const arr3 = [...arr];

arr2.push(4);
arr3.push(5);

console.log(arr);   // ?
console.log(arr2);  // ?
console.log(arr3);  // ?
```

<details><summary>Answer</summary>

```
arr  → [1, 2, 3, 4]   (arr2 is the SAME reference as arr — push affects both)
arr2 → [1, 2, 3, 4]   (same array as arr)
arr3 → [1, 2, 3, 5]   (arr3 is a COPY via spread — independent array)
```

This tests understanding of **reference vs copy** — `arr2 = arr` shares the same memory reference, while `[...arr]` creates a brand-new array.

</details>

---

## 🎯 Array Methods Master Cheat Sheet

```js
// CREATE
[];  new Array();  Array.of(1,2,3);  Array.from(iterable);

// ACCESS
arr[i];  arr.at(-1);  arr.slice(start, end);

// ADD (mutating)
arr.push(x);  arr.unshift(x);  arr.splice(i, 0, x);

// ADD (non-mutating)
[...arr, x];  [x, ...arr];

// REMOVE (mutating)
arr.pop();  arr.shift();  arr.splice(i, 1);

// REMOVE (non-mutating)
arr.filter(fn);

// SEARCH
arr.indexOf(x);  arr.includes(x);  arr.find(fn);  arr.findIndex(fn);

// TRANSFORM
arr.map(fn);  arr.flat(depth);  arr.flatMap(fn);

// REORDER (mutating)
arr.sort(fn);  arr.reverse();

// REORDER (non-mutating, ES2023)
arr.toSorted(fn);  arr.toReversed();  arr.toSpliced(...);  arr.with(i, val);

// REDUCE
arr.reduce(fn, init);  arr.reduceRight(fn, init);

// TEST
arr.some(fn);  arr.every(fn);

// ITERATE
arr.forEach(fn);  arr.entries();  arr.values();

// CHECK TYPE
Array.isArray(value);
```

---

*Day 08 — Part 01 of Full Stack JavaScript Learning Journey*
