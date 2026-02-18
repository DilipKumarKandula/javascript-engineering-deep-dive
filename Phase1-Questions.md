PHASE 1 – INTERVIEW QUESTIONS

---

LEVEL 1 – CORE CONCEPT QUESTIONS (Very Common)

1. What is the difference between JavaScript engine and runtime environment?

2. What is an execution context? What are its types?

3. Explain creation phase and execution phase.

4. Why does hoisting happen?

5. What is the Temporal Dead Zone?

6. What is the difference between var, let, and const in terms of:
   - Scope
   - Hoisting
   - Reassignment
   - Redeclaration

7. What is the difference between function declaration and function expression?

8. What goes into the call stack?

9. Why is JavaScript single-threaded?

10. What is stack overflow and how is it different from blocking?

---

LEVEL 2 – OUTPUT PREDICTION (High Probability Interview)

Question 1

```js
console.log(a);
var a = 10;

function test() {
  console.log(a);
  var a = 20;
}

test();
```

What is the output?

---

Question 2

```js
console.log(x);
let x = 5;
```

---

Question 3

```js
sayHi();

var sayHi = function () {
  console.log("Hi");
};
```

---

Question 4

```js
function a() {
  b();
  console.log("A");
}

function b() {
  console.log("B");
}

a();
```

What prints and why?

---

```js
Question 5

var x = 1;

function outer() {
var x = 2;

function inner() {
console.log(x);
}

inner();
}

outer();
```

What prints?

---

LEVEL 3 – DEEP THINKING (Senior-Level Understanding)

1. If JavaScript is single-threaded, how does it handle multiple API requests simultaneously in Node.js?

2. Why does setTimeout(fn, 0) not execute immediately?

3. Can multiple execution contexts exist at the same time? Explain precisely.

4. What happens internally when a function is called?

5. Why can a heavy synchronous loop block the entire Node.js server?

6. Why is function declaration fully hoisted but function expression is not?

7. What problem would occur if JavaScript were multi-threaded in the browser?

8. Why does this print undefined?

var a = 5;

function test() {
console.log(a);
var a = 10;
}

test();

Explain in terms of creation phase.

---

LEVEL 4 – TRICKY EDGE CASES

1.

console.log(a);
var a = 5;
console.log(a);

What prints?

---

2.

let a = 5;

{
let a = 10;
}

console.log(a);

What prints?

---

3.

const obj = { name: "A" };
obj.name = "B";
console.log(obj.name);

Why does this work?

---

4.

function test() {
return;
console.log("Hello");
}

test();

What happens?

---

5.

function test() {
console.log(x);
}

var x = 10;
test();

What prints and why?

---
