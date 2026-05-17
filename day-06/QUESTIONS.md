> 👋 Hey fellow devs — try to answer these yourself before looking anything up.
> Challenge yourself first. If you get stuck or want to verify your answers,
> refer to the [ANSWER.md](./ANSWER.md) file.
> You might even find a better way to explain something — feel free to build on it. 🚀

---

## 🔗 Part 01 — Lexical Scope, Closures and Objects

### Lexical Scope

1. What is lexical scope? (give a detailed definition)
2. Why does an inner function access outer variables?
3. What creates the scope chain?
4. Can calling location change a function's scope?
5. Why does lexical scope make JavaScript predictable?

---

### Closures

6. What is a closure in JavaScript? (give a detailed definition)
7. Why do closures exist?
8. What does a closure remember?
9. Does a closure copy variables or reference them?
10. Why does the outer variable stay alive after the outer function finishes execution?
11. How are lexical scope and closures related?
12. What real-world features use closures?
13. What is preserved in memory during a closure?
14. Can closures modify outer variables?

---

### Scope vs Closure — Common Confusions

15. Why doesn't normal function scope preserve state across multiple calls?
16. What exactly does a closure preserve?
17. "Variables are already private inside functions because of function scope… so why do people say closures create private variables?"


---
## 🔗 Part 02 — JavaScript Objects: Deep Dive

### 🔵 Object Basics

1. What is an object in JavaScript?
2. How many ways are there for creating an object?
3. What is an object literal?
4. What is the difference between dot and bracket notation?
5. When should bracket notation be used — dynamic key-value and special characters?
6. How do you add a property to an object?
7. How do you delete a property?

---

### 🔵 Constructor Functions

8. What is a constructor function?
9. What does `this` refer to inside constructor functions?
10. What happens if you forget `new`?
11. Why are constructor functions useful?

---

### 🔵 Object Constructor & Factory

12. What is the `Object` constructor in JavaScript?
13. Why do we usually prefer `{}` over `new Object()`?
14. Do object literals and constructor objects represent the same object in JS?
15. How do you create objects using a factory function?

---

### 🔵 Object Features & Iteration

16. What is object shorthand?
17. What are object methods inside of an object?
18. How does the `in` operator come in handy in terms of objects?
19. What is the use case of the `for...in` loop?
20. What are `Object.keys()`, `Object.values()`, `Object.entries()`, and `Object.fromEntries()` in JavaScript?

---

### 🔵 Object References & Memory

21. What is an object reference?
22. Do variables store actual objects directly?
23. Why does changing one object variable affect another?
24. Why is `{x:1} === {x:1}` false?
25. Is JS technically pass-by-reference?
26. What is pass by value and pass by reference?

---

### 🔵 Copying & Immutability

27. What does `Object.assign()` do? What are target and source objects? Does it create a deep copy or shallow copy? What modern syntax often replaces it?
28. What is the difference between shallow copy and deep copy?
29. What is the difference between `Object.freeze()` and `Object.seal()`?

---

### 🔵 Advanced Object Concepts

30. What are static methods in JS?
31. What is the `Object.hasOwn()` method?
32. What is object destructuring (as well as nested)?
33. What are aliases in destructuring?
34. What is optional chaining (`?.`)?

## 🗂️ Quick Reference Cheatsheet

> Fill this in yourself as a revision exercise — then check against [ANSWER.md](./ANSWER.md) 👇

**Lexical scope — complete the sentence:**
```
Scope is determined by WHERE the function is ________ — not where it's ________.
```

**Closure — complete the definition:**
```
A closure = __________ + its __________
```

**What does a closure hold — copy or reference?**
```
Answer:
```

**Lexical scope vs Closure:**
```
Lexical scope →
Closure       →
```

**Function scope vs Closure privacy:**
```
Function scope → private but ________
Closure        → private AND ________
```

**List 4 real-world use cases of closures:**
```
1.
2.
3.
4.
```

---

> 📅 **Day 06 — Done!**
> Stuck on any of these? Head to [ANSWER.md](./ANSWER.md) for full explanations. 💡
