> 👋 Hey fellow devs — try to answer these yourself before looking anything up.
> Challenge yourself first. If you get stuck or want to verify your answers,
> refer to the [ANSWER.md](./ANSWER.md) file.
> You might even find a better way to explain something — feel free to build on it. 🚀

---

## 🔁 Part 01 — Loops

### Loops & Iteration

1. What is a loop?
2. What is an iteration?
3. What is the difference between looping and iteration?
4. What is a nested loop?
5. How many times does the inner loop run in a nested loop?
6. What is the total iteration formula for nested loops?

---

### Break & Continue

7. What does `break` do?
8. What does `continue` do?
9. Does `break` stop the outer loop in nested loops?

---

### 🧩 Predict the Output

**Q1.** What will this print? Explain each step.
```js
for (let i = 1; i <= 4; i++) {
  if (i === 2) continue;
  if (i === 4) break;
  console.log(i);
}
```

---

### Expressions vs Statements

10. What is an expression?
11. What is a statement?
12. Can an expression exist inside a statement?
13. Why can't we assign an `if` statement to a variable?
14. Why is ternary allowed in assignment but `if` is not?

---

### Multi-variable Loops & Arrays

15. Can a `for` loop have multiple variables? How?
16. Why do we use the same index for multiple arrays?
17. What is a better structure than multiple parallel arrays?

---

### while & do...while

18. What is the main difference between `while` and `do...while`?
19. Which loop always runs at least once?
20. Why can `while` cause infinite loops?
21. When would you prefer `do...while` over `while`?

---

## 🧩 Part 02 — Functions

### Function Basics

1. What is a function?
2. What is a function expression?
3. What is the difference between a function declaration and a function expression?

---

### Parameters

4. What is a default parameter?
5. What is `...rest`?
6. When does a default parameter apply?
7. What does `...rest` return?
8. Why must `rest` be the last parameter?

---

### Scope

9. Can an inner function access outer variables? Why?
10. Can an outer function access inner variables?

---

### Arrow Functions & IIFE

11. What is an arrow function? How is it different from a regular function?
12. What is an IIFE?
13. When does an IIFE run?
14. Why use arrow functions in callbacks?

---

### Callback Functions

15. What is a callback function in JavaScript?
16. Why do we pass a function as an argument to another function?
17. What is the role of a callback in `setTimeout`?
18. Is a callback function executed immediately or later? Explain.
19. Can a callback function be both a named function and an anonymous function?
20. What is the difference between calling a function and passing it as a callback?

---

### Pure Functions

21. What is a pure function?
22. What are the two main rules of a pure function?
23. What is a side effect in JavaScript?
24. Is this function pure? Why or why not?
```js
let count = 0;
function increment() {
  count++;
}
```
25. Why are pure functions easier to debug and test?
26. Can a pure function use variables outside itself? Explain.

---

### Higher Order Functions (HOF)

27. What is a higher order function?
28. What are the two ways a function becomes a HOF?
29. Is a function that takes a callback a HOF? Why?
30. Is a function that returns another function a HOF?
31. Identify the HOF in this example and explain why:
```js
function operate(fn, value) {
  return fn(value);
}
```
32. What is the relationship between callback functions and HOFs?

---

### Call Stack

33. What is the call stack in JavaScript?
34. Why is it called a "stack"?
35. What happens in the call stack when a function is called?
36. What happens when a function finishes execution?
37. What is stack overflow? When does it happen?
38. Is JavaScript single-threaded? How does that relate to the call stack?

---

### Recursion

39. What is recursion in JavaScript?
40. What is a base case in recursion? Why is it important?
41. What happens if a recursive function has no base case?
42. How does recursion use the call stack?
43. What is the difference between iteration (loops) and recursion?
44. How are recursion and the call stack related?
45. What happens in the call stack when a recursive function keeps calling itself?

---

## 🗂️ Quick Reference Cheatsheet

> Fill this in yourself as a revision exercise — then check against [ANSWER.md](./ANSWER.md) 👇

**Loop types — when to use each:**
```
for          →
while        →
do...while   →
for...of     →
for...in     →
```

**Function types:**
```
Declaration  →
Expression   →
Arrow        →
IIFE         →
```

**Pure function — 2 rules:**
```
1.
2.
```

**HOF — 2 ways:**
```
1.
2.
```

**Call stack — what happens when:**
```
Function called  →
Function returns →
Stack overflow   →
```

**Recursion checklist:**
```
□ Base case defined?
□ Each call moves closer to base case?
□ Returns a value at each step?
```

---

> 📅 **Day 03 — Done!**
> Stuck on any of these? Head to [ANSWER.md](./ANSWER.md) for full explanations. 💡
