# Questions
> **Topic:** Hoisting, TDZ, Scope, Scope Chain
> **Parts:** 2 Parts | 40+ Questions Covered

---

## 🗺️ Key Concepts Covered Today

| # | Concept | What It's About |
|---|---------|-----------------|
| 1 | Hoisting | How JS allocates memory before execution |
| 2 | TDZ | Temporal Dead Zone — why `let`/`const` throw errors |
| 3 | Function Hoisting | Why function declarations work before definition |
| 4 | Expression Hoisting | Why function expressions and arrows don't |
| 5 | Scope | Global, function, and block scope |
| 6 | var vs let vs const | How each keyword scopes differently |
| 7 | Scope Chain | How JS looks up variables across scopes |

---

> 👋 Hey fellow devs — try to answer these yourself before looking anything up.
> Challenge yourself first. If you get stuck or want to verify your answers,
> refer to the [ANSWER.md](./ANSWER.md) file.
> You might even find a better way to explain something — feel free to build on it. 🚀

---

## 🔼 Part 01 — Hoisting & TDZ

### Hoisting Basics

1. What is hoisting?
2. Why do function declarations work before definition?
3. Is hoisting a real physical movement of code?
4. What is the difference between the memory phase and hoisting?

---

### Temporal Dead Zone (TDZ)

5. When does TDZ occur?
6. Why does `let` throw a ReferenceError before its declaration?
7. Does `var` have TDZ? Why?
8. Are `let` and `const` hoisted?
9. What is the difference between hoisting and TDZ?
10. Why was TDZ introduced in JavaScript?

---

### Function Hoisting

11. What is function hoisting?
12. Why can function declarations be called before their definition?
13. What gets stored during the memory phase for function declarations?

---

### Expression & Arrow Function Hoisting

14. Why do function expressions fail before assignment?
15. Are arrow functions fully hoisted?
16. What is the difference between declaration hoisting and expression hoisting?
17. What error happens when you call `undefined()`?
18. Why does a `const` arrow function give a TDZ error instead of a TypeError?

---

## 🔍 Part 02 — Scope

### Scope Basics

1. What is scope in JavaScript?
2. What is global scope?
3. What is function scope?
4. What is block scope?

---

### var vs let vs const — Scope Behavior

5. Does `var` have block scope?
6. Does `var` respect `{}` blocks?
7. Are `let` and `const` function scoped?
8. Which keyword attaches to `window` globally?
7. Why are `let` and `const` safer than `var`?
8. Which keywords are block scoped?
9. Why is `var` discouraged in modern JS?

---

### Function Scope Deep Dive

10. What is function scope? (explain in your own words)
11. Can variables inside a function be accessed outside?
12. Does each function call create a new scope?
13. What is the difference between function scope and block scope?
14. Why does `var` behave differently inside blocks?
15. Can a variable declared using `var` inside a function be accessed outside of that function?
16. Can a variable declared using `var` inside a block be accessed outside of that block?

---

### Scope Chain

17. What is the scope chain?
18. How does JS find a variable if it's not in local scope?
19. What is the order of lookup in the scope chain?
20. Can inner scope access outer variables? Why?
21. Can outer scope access inner variables?

---

## 🗂️ Quick Reference Cheatsheet

> Fill this in yourself as a revision exercise — then check against [ANSWER.md](./ANSWER.md) 👇

**Hoisting behavior for each:**
```
var declaration      →
let / const          →
function declaration →
function expression  →
arrow function       →
```

**TDZ — fill in the blanks:**
```
Applies to →
Period     →
Access     →
Purpose    →
```

**Scope types:**
```
Global scope   →
Function scope →
Block scope    →
```

**var vs let vs const:**

| | `var` | `let` | `const` |
|-|-------|-------|---------|
| Scope | | | |
| Hoisted | | | |
| Re-declarable | | | |
| Re-assignable | | | |
| Attaches to `window` | | | |
| TDZ | | | |
| Modern usage | | | |

**Scope chain lookup order:**
```
1. →
2. →
3. →
4. →
5. →
```

---

> 📅 **Day 05 — Done!**
> Stuck on any of these? Head to [ANSWER.md](./ANSWER.md) for full explanations. 💡
