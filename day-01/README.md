# 🔑 Key Concepts of Day 01

* Script loading behavior (`<script>`, `defer`, `type="module"`)
* Browser parsing and execution flow
* Variables and naming conventions
* Specifiers (`var`, `let`, `const`)
* Primitive vs non-primitive data types
* Pass by value concept
* Pass by reference concept
* Memory storage (stack vs heap)

---

## Part 01

1. Why can putting `<script>` in `<head>` cause errors?
2. What does the browser do when it encounters a `<script>` tag?
3. What are the two main phases of loading JavaScript?
4. Why is placing `<script>` at the end of `<body>` safer?
5. What is the main disadvantage of putting scripts at the end of `<body>`?
6. What does `defer` actually do?
7. Which is better for performance: end of `<body>` or `defer`? Why?
8. What does `type="module"` do differently from a normal `<script>`?
9. Does a module script block HTML parsing? Why or why not?
10. What problem does `defer` solve?
11. Why doesn’t a module script need `defer`?
12. What extra features do modules provide beyond `defer`?

---

## Part 02

1. What is a variable in JavaScript, and why is it used?
2. What are the rules for naming variables in JavaScript (naming conventions)?
3. What are best practices for writing clean and meaningful variable names?
4. What is a specifier in JavaScript, and where is it commonly used?
5. What are different types of specifiers (e.g., import/export specifiers)?
6. What does “pass by value” mean in JavaScript?
7. How does pass by value behave when assigning or passing variables to functions?
8. What are primitive data types in JavaScript?
9. What are non-primitive (reference) data types in JavaScript?
10. What are the key differences between primitive and non-primitive values?
11. How are primitive values stored in memory?
12. How are non-primitive values stored in memory?
13. Why are non-primitive values called reference types?
14. What happens in memory when you copy a primitive vs a non-primitive value?
15. How does memory storage affect comparison between primitive and non-primitive values?
