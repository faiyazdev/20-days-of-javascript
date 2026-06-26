# Day 08 — JavaScript Arrays


## 🗂️ Topics Covered

### 🔵 Array Fundamentals

| # | Topic | Key Concept |
|---|-------|-------------|
| 1 | What is an Array? | An ordered, index-based collection of values |
| 2 | Creating an Array | Literal `[]`, `new Array()`, `Array.of()`, `Array.from()` |
| 3 | Getting Elements | Index access, `at()`, destructuring |
| 4 | Adding Elements | `push`, `unshift`, `splice`, spread |
| 5 | Removing Elements | `pop`, `shift`, `splice`, `filter` |
| 6 | Copying/Cloning | Spread, `slice()`, `Array.from()`, `structuredClone()` |
| 7 | Checking if a Value is an Array | `Array.isArray()` |

### 🔵 Destructuring & Spread/Rest

| # | Topic | Key Concept |
|---|-------|-------------|
| 8 | Array Destructuring | Extract values into variables by position |
| 9 | Default Values | Fallback when a position is `undefined` |
| 10 | Skipping Values | Empty commas `[, , third]` |
| 11 | Nested Destructuring | Destructure arrays inside arrays |
| 12 | Rest Parameter | Gather remaining items into an array |
| 13 | Spread Operator | Expand an array into individual elements |
| 14 | Destructuring Use Cases | Function returns, swapping, merging |
| 15 | Swapping Values | One-line swap without a temp variable |
| 16 | Merging Arrays | Spread-based array combination |

### 🔵 Core Properties & Methods

| # | Topic | Key Concept |
|---|-------|-------------|
| 17 | `length` Property | Number of elements; writable to truncate/extend |
| 18 | `concat()` | Merge arrays — non-mutating |
| 19 | `join()` | Array → string with separator |
| 20 | `fill()` | Overwrite elements with a static value |
| 21 | `includes()` | Boolean existence check |
| 22 | `indexOf()` | First matching index, or `-1` |
| 23 | `reverse()` | Reverses array — **mutating** |
| 24 | `sort()` | Sorts array — **mutating**, needs compare function |
| 25 | `splice()` | Swiss-army knife: add/remove/replace — **mutating** |
| 26 | `at()` | Index access supporting negative indices |
| 27 | `copyWithin()` | Copy part of array to another position in itself |
| 28 | `flat()` | Flatten nested arrays |
| 29 | Grouping Elements | `Object.groupBy()` / `Map.groupBy()` |

### 🔵 Static & Array-Like

| # | Topic | Key Concept |
|---|-------|-------------|
| 30 | Static Array Methods | Methods called on `Array` itself, not instances |
| 31 | Array-Like Objects | Has `length` + indices, but not a real array |
| 32 | `Array.from()` | Convert iterables/array-likes into real arrays |
| 33 | `Array.fromAsync()` | Async iterable → Promise of array |
| 34 | `Array.of()` | Create array from arguments (fixes `Array(7)` issue) |

### 🔵 Iterator Methods

| # | Topic | Key Concept |
|---|-------|-------------|
| 35 | `filter()` | Keep elements matching a condition |
| 36 | `map()` | Transform every element, same length out |
| 37 | `reduce()` | Fold array into a single accumulated value |
| 38 | `reduceRight()` | Same as reduce, right-to-left |
| 39 | `some()` | At least one element passes test? |
| 40 | `every()` | Do ALL elements pass test? |
| 41 | `find()` | First matching element |
| 42 | `findIndex()` | Index of first match |
| 43 | `findLast()` | Last matching element |
| 44 | `findLastIndex()` | Index of last match |
| 45 | Method Chaining | Combine `.filter().map().reduce()` pipelines |
| 46 | `forEach()` | Run a function per element, no return |
| 47 | `entries()` | Iterator of `[index, value]` pairs |
| 48 | `values()` | Iterator of values |
| 49 | `flatMap()` | `map()` + `flat(1)` combined |

### 🔵 Immutability (ES2023)

| # | Topic | Key Concept |
|---|-------|-------------|
| 50 | `toReversed()` | Non-mutating version of `reverse()` |
| 51 | `toSorted()` | Non-mutating version of `sort()` |
| 52 | `toSpliced()` | Non-mutating version of `splice()` |
| 53 | `with()` | Non-mutating single-index update |

### 🔵 Practice

| # | Topic | Key Concept |
|---|-------|-------------|
| 54 | Tasks, Quizzes & Interview Questions | Hands-on problems + common JS interview traps |

---

## 🧠 Why This Matters

Arrays are where **functional programming patterns** in JavaScript really shine — `map`, `filter`, `reduce` are the backbone of clean, declarative data transformations used constantly in React rendering, API data shaping, and backend business logic with Express/MongoDB/Postgres.

---

*Day 08 — Part 01 of Full Stack JavaScript Learning Journey*
