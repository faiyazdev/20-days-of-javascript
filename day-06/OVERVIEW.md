# 📘 Day 06 — Lexical Scope, Closures & Objects
> **Topic:** Lexical Scope, Closures, Memory Preservation & Private State, Objects

---

## 🗺️ Key Concepts Covered Today

| # | Concept | What It's About |
|---|---------|-----------------|
| 1 | Lexical Scope | Scope determined by where code is written, not where it runs |
| 2 | Scope Chain | How nested scopes connect to each other |
| 3 | Closure | A function that remembers its outer scope even after execution |
| 4 | Memory Preservation | Why outer variables stay alive when closure exists |
| 5 | Private State | How closures simulate private variables |
| 6 | Closure vs Scope | The key difference between the two concepts |

## Part 02
 
| # | Topic | Key Concept |
|---|-------|-------------|
| 1 | What is an Object? | A collection of key-value pairs (properties & methods) |
| 2 | Ways to Create an Object | Literal, Constructor fn, `new Object()`, Factory fn, Class |
| 3 | Object Literal | Simplest and most common way: `{}` |
| 4 | Dot vs Bracket Notation | `obj.key` vs `obj["key"]` — when to use which |
| 5 | Dynamic Keys & Special Characters | Why bracket notation is essential for runtime keys |
| 6 | Adding a Property | `obj.newKey = value` or `obj["newKey"] = value` |
| 7 | Deleting a Property | `delete obj.key` |
| 8 | Constructor Function | Blueprint for creating multiple similar objects |
| 9 | `this` in Constructor Functions | Refers to the newly created instance |
| 10 | Forgetting `new` | `this` becomes global/undefined — bugs ensue |
| 11 | Why Constructor Functions are Useful | Reusable templates for object creation |
| 12 | `Object` Constructor | `new Object()` — the base of all JS objects |
| 13 | `{}` vs `new Object()` | Prefer `{}` — cleaner, faster, more readable |
| 14 | Object Literals vs Constructor Objects | Same structure, different identity |
| 15 | Factory Functions | Functions that return objects — no `new` needed |
| 16 | Object Shorthand | `{ name }` instead of `{ name: name }` |
| 17 | Object Methods | Functions stored as object properties |
| 18 | `in` Operator | Check if a key exists in an object |
| 19 | `for...in` Loop | Iterate over all enumerable keys |
| 20 | Object Static Methods | `keys()`, `values()`, `entries()`, `fromEntries()` |
| 21 | Object Reference | Variables hold a reference, not the object itself |
| 22 | Do Variables Store Objects? | No — they store memory addresses (references) |
| 23 | Why Changing One Affects Another | Both point to the same memory location |
| 24 | `{x:1} === {x:1}` is `false` | Different references = not equal |
| 25 | Is JS Pass-by-Reference? | Technically pass-by-value of a reference |
| 26 | Pass by Value vs Reference | Primitives copied, objects referenced |
| 27 | `Object.assign()` | Shallow merge/copy of objects |
| 28 | Shallow vs Deep Copy | Surface copy vs fully independent clone |
| 29 | `Object.freeze()` vs `Object.seal()` | Immutability levels |
| 30 | Static Methods in JS | Methods on the constructor, not the instance |
| 31 | `Object.hasOwn()` | Safe own-property check |
| 32 | Object Destructuring | Extract values cleanly from objects |
| 33 | Nested Destructuring | Destructure deeply nested properties |
| 34 | Aliases in Destructuring | Rename while destructuring |
| 35 | Optional Chaining (`?.`) | Safe property access on null/undefined |

