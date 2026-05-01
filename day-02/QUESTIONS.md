## ⚙️ Part 01 — Operators

---

### 1. Arithmetic Operators

1. What is an operator and operand in JavaScript?
2. What's the difference between `+` and other arithmetic operators in JS?
3. What will `"5" + 3` return? Why?
4. What does `%` do? Give a real use case.
5. What's the difference between `x++` and `++x`?
6. Why does `"10" - 2` work but `"10" + 2` behaves differently?

---

### 2. Assignment Operators

1. What does `=` actually do?
2. Why is `x = x + 1` valid in JS?
3. What does `+=` mean internally?
4. What's the difference between `=` and `===`?
5. Why are assignment operators useful?

---

### 3. Comparison Operators

1. What does a comparison operator return?
2. What's the difference between `==` and `===`?
3. Why is `===` preferred in real projects?
4. What will `"10" > 5` return? Why?
5. What does `!==` mean?

---

### 4. Logical Operators

1. What does `&&` require to return `true`?
2. What does `||` require to return `true`?
3. What does `!` do?
4. What will `true && false` return?
5. What will `false || true` return?
6. What does `&&` return if it finds a falsy value?
7. What does `||` return if it finds a truthy value?
8. What are all falsy values in JavaScript?
9. What will `0 && "Hello"` return? Why?
10. What will `"" || "Hi"` return? Why?

---

### 5. Ternary, typeof & Nullish Coalescing

1. When should you use ternary instead of `if`?
2. What does `typeof null` return? Why is it weird?
3. What's the difference between `||` and `??`?
4. What will `false ?? "hello"` return?
5. What will `false || "hello"` return?

---

## 🔀 Part 02 — Control Flow

---

### Control Flow Basics

1. What is control flow in simple words?
2. What is the default execution order in JavaScript?
3. Why do we need control flow?

---

### if / else Statement

1. A condition in `if` can be any expression — what does that mean?
2. What is an `if...else` statement in JavaScript?
3. What is the purpose of `if...else`?
4. What happens if the condition in `if` is true?
5. What happens if the condition is false?
6. What is the difference between `if`, `else if`, and `else`?

---

### 🧩 Predict the Output

**Q1.** What will this print? Why?
```js
if ("0") {
  console.log("A");
}
if (0) {
  console.log("B");
}
```

---

**Q2.** What will this print? Careful — it's a trick.
```js
let a = 5;
if (a > 3)
  console.log("X");
  console.log("Y");
```

---

**Q3.** What will this print?
```js
if (null) {
  console.log("A");
} else {
  console.log("B");
}
```

---

**Q4.** Explain why this runs:
```js
if ([]) {
  console.log("Runs");
}
```

---

**Q5.** Explain this step-by-step:
```js
let x = 0;
if (x || x === 0) {
  console.log("Runs");
}
```

---

### switch Statement

1. What is a `switch` statement?
2. What happens if you forget `break`?
3. When should you NOT use `switch`?
4. Can `switch` handle ranges like `> 10`? Why?
5. What is the difference between `if/else` and `switch`?
6. What is fall-through in `switch`? When is it useful?

---

## 🗂️ Quick Reference Cheatsheet

> Fill this in yourself as a revision exercise — then check against [ANSWER.md](./ANSWER.md) 👇

```
Arithmetic:   +  -  *  /  %  **  ++  --
Assignment:   =  +=  -=  *=  /=  %=
Comparison:   ==  ===  !=  !==  >  <  >=  <=
Logical:      &&  ||  !  ??
Ternary:      condition ? __ : __
```

**Falsy values (list all 8):**
```
?, ?, ?, ?, ?, ?, ?, ?
```

**`||` vs `??` — what's the key difference?**
```
||  →
??  →
```

---

> 📅 **Day 02 — Done!**
> Stuck on any of these? Head to [ANSWER.
