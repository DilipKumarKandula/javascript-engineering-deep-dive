## 1. JavaScript Engine

### WHAT IT IS

The JavaScript Engine is the core component that reads, compiles, and executes JavaScript code.
It is responsible for parsing source code, allocating memory, creating execution contexts, and running instructions line by line.

It does not provide browser features or APIs. It only executes JavaScript.

---

WHAT IT IS NOT

- Not the browser
- Not the DOM
- Not Web APIs
- Not Node.js
- Not the Event Loop

The engine executes code.
The runtime environment (browser or Node.js) provides additional capabilities.

---

### HOW IT WORKS (Execution Flow)

#### **High-Level Flow:**

```js
Your JavaScript Code
↓
JavaScript Engine
↓

1. Parsing
2. Memory Allocation
3. Execution
```

#### **Detailed Flow:**

```
Step 1: Parsing

- Code is converted into tokens
- Syntax tree is generated

Step 2: Creation Phase

- Global Execution Context created
- Variables allocated
- Functions stored

Step 3: Execution Phase

- Code runs line by line
- Execution contexts pushed to call stack
- Instructions executed synchronously
```

#### **Flow Diagram:**

```
Code
↓
Parser
↓
Global Execution Context Created
↓
Call Stack
↓
Execution
↓
Completion
```

---

HOW IT SHOULD BE USED

The engine itself is not something you manually use.
However, you must write code that respects how the engine executes:

- Avoid blocking synchronous loops
- Avoid deep recursion without base case
- Understand hoisting rules
- Predict execution order

---

### HOW IT WILL NOT WORK

1. If code contains syntax error → engine stops parsing
2. If infinite recursion → stack overflow
3. If infinite loop → execution never completes
4. If call stack never empties → async callbacks never execute

---

### POSSIBLE ERRORS RELATED TO ENGINE

SyntaxError
Occurs during parsing phase.

ReferenceError
Occurs when accessing variable in TDZ or undefined reference.

TypeError
Occurs when invoking undefined as function or invalid operation.

RangeError
Occurs during stack overflow.

---

### RULES

1. JavaScript engine is single-threaded.
2. Only one execution context runs at a time.
3. Engine never waits for async tasks.
4. Engine uses one call stack.
5. Engine does not handle timers or I/O directly.

---

#### CONDITIONS WHERE IT WILL NOT WORK AS EXPECTED

- Heavy synchronous computation blocks execution.
- Recursive calls without exit condition cause crash.
- Blocking while(true) loops freeze program.

---

#### LIMITATIONS

1. Single-threaded execution
2. Blocking behavior
3. Limited by stack size
4. Cannot perform parallel computation internally

---

#### BEST PRACTICES

- Avoid long-running synchronous loops
- Break heavy tasks into smaller chunks
- Use asynchronous APIs properly
- Avoid deep recursive patterns
- Always define base case in recursion

---

#### REAL-TIME PROBLEMS

1. UI freezing during large data processing
2. Node.js server becoming unresponsive
3. React app hanging due to heavy computation
4. Stack overflow from recursive rendering

---

EXAMPLES

Example 1 – Blocking

```js
console.log("Start");
while (true) {}
console.log("End");
```

“End” never prints.

Example 2 – Stack Overflow

```js
function recurse() {
  recurse();
}
recurse();
```

Maximum call stack exceeded.

---

EXCEPTION HANDLING

Errors thrown by engine can be handled using try/catch:

```js
try {
  nonExistingFunction();
} catch (error) {
  console.log(error.message);
}
```

However, syntax errors cannot be caught because parsing fails before execution.

---

MANDATORY TO LEARN FROM THIS TOPIC

- Engine executes code, runtime supports it.
- Engine is single-threaded.
- Engine never waits.
- Only one call stack exists.
- Blocking happens because stack is busy.

---

SUMMARY

The JavaScript engine is a single-threaded execution engine that parses code, creates execution contexts, and runs instructions synchronously using a single call stack.

---

## 2. Execution Context

---

### WHAT IT IS

An Execution Context is the environment created by the JavaScript engine to evaluate and run JavaScript code.

Every piece of JavaScript runs inside an execution context.
No execution context = no execution.

There are three types:

1. Global Execution Context (GEC)
2. Function Execution Context (FEC)
3. Eval Execution Context (rarely used)

In real-world engineering, we focus on Global and Function execution contexts.

---

### WHY IT EXISTS

The engine needs a structured way to:

- Store variables
- Store functions
- Track scope
- Manage the value of "this"
- Control execution order

Execution context provides that structured container.

---

#### TYPES

1. Global Execution Context (GEC)
   - Created once when program starts
   - Exists until program ends
   - Contains global variables and functions

2. Function Execution Context (FEC)
   - Created every time a function is called
   - Has its own local memory
   - Destroyed after function finishes

Important Rule:
Function declaration does NOT create FEC.
Function call creates FEC.

---

#### INTERNAL STRUCTURE

Each execution context has:

1. Memory Component (Variable Environment)
   - Stores variables
   - Stores function declarations

2. Code Component
   - Executes code line by line

---

TWO PHASES OF EXECUTION CONTEXT

```
Phase 1 – Creation Phase
Phase 2 – Execution Phase
```

---

CREATION PHASE FLOW

Engine scans the code and:

- Allocates memory for variables
- Initializes var as undefined
- Allocates let/const (TDZ)
- Stores function declarations fully
- Sets up scope chain
- Sets up "this"

Code is NOT executed here.

---

EXECUTION PHASE FLOW

- Code runs line by line
- Variables receive assigned values
- Functions are invoked
- New execution contexts created when functions are called

---

FLOW DIAGRAM

```
Program Start
↓
Global Execution Context Created
↓
Creation Phase
↓
Execution Phase
↓
Function Call
↓
New Function Execution Context Created
↓
Function Execution
↓
Function Context Destroyed
```

---

HOW IT SHOULD BE USED

- Understand memory allocation behavior
- Predict hoisting correctly
- Understand variable shadowing
- Predict output before execution

---

HOW IT WILL NOT WORK

If you misunderstand creation phase:

- You will mispredict undefined
- You will mispredict ReferenceError
- You will confuse hoisting
- You will misunderstand closures

---

RULES

1. Global execution context is created first.
2. Only one global context exists.
3. Each function call creates a new execution context.
4. Creation phase happens before execution phase.
5. Variables inside function shadow outer variables.

---

CONDITIONS

If a function is not called → no function execution context.  
If variable exists locally → global variable will not be used.  
If let/const accessed before declaration → ReferenceError.

---

LIMITATIONS

Execution contexts rely on stack.
Too many nested calls → stack overflow.
Blocking synchronous code prevents new contexts from executing.

---

COMMON ERRORS

ReferenceError – accessing variable in TDZ  
TypeError – calling undefined as function  
RangeError – recursion overflow

---

EXAMPLES

```js
Example 1 – var behavior

console.log(a);
var a = 5;

Creation Phase:
a = undefined

Execution Phase:
prints undefined
```

---

Example 2 – let behavior

```js
console.log(a);
let a = 5;

Creation Phase:
a allocated (TDZ)

Execution Phase:
ReferenceError
```

---

Example 3 – Shadowing

```js
var a = 1;

function test() {
  console.log(a);
  var a = 2;
}

test();
```

Prints undefined
Because local a exists in creation phase.

---

REAL-TIME PROBLEMS

1. React component using variable before initialization
2. API callback referencing shadowed variable
3. Unexpected undefined values in production

---

BEST PRACTICES

- Always declare variables at top of scope (clarity)
- Avoid var in modern development
- Use let for mutable variables
- Use const by default
- Understand function call behavior

---

EXCEPTION HANDLING

Most execution context errors occur before runtime logic.
Use try/catch for runtime errors only.

---

MANDATORY TO LEARN

- Creation vs execution difference
- Function call creates execution context
- Local memory overrides global
- Hoisting is memory allocation

---

SUMMARY

Execution context is the structured environment created by the JavaScript engine to store variables, functions, and scope information, and every function call generates a new execution context that runs through creation and execution phases.

---

## Creation Phase vs Execution Phase (Deep Dive)

---

### WHAT IT IS

Every execution context (Global or Function) runs in two phases:

1. Creation Phase
2. Execution Phase

Creation Phase prepares memory.
Execution Phase runs code.

This explains hoisting, undefined behavior, TDZ, and function differences.

---

### WHY THIS EXISTS

The engine must:

- Know all variable names before running code
- Allocate memory before execution
- Prepare function references
- Set up scope chain

So it first scans and prepares (Creation Phase),
then executes (Execution Phase).

---

### CREATION PHASE – EXACT RULES

During this phase:

1. Memory is allocated.
2. var → initialized as undefined.
3. let and const → allocated but not initialized (Temporal Dead Zone).
4. Function declarations → stored completely in memory.
5. Function expressions → treated as variable declarations.
6. Scope chain is prepared.
7. this binding is determined.

No code runs in this phase.

---

### EXECUTION PHASE – EXACT RULES

1. Code runs line by line.
2. Variable assignments happen.
3. Function calls create new execution contexts.
4. If function not called → no execution context created.
5. Values replace undefined during execution.

---

FLOW DIAGRAM

```JS
Execution Context Created
↓
Creation Phase

- Memory allocated
- Variables prepared
- Functions stored
  ↓
  Execution Phase
- Code executes
- Assignments happen
- Function calls create new contexts
```

---

VAR BEHAVIOR IN CREATION PHASE

Example:

```js
console.log(a);
var a = 10;
```

Creation Phase:
a → undefined

Execution Phase:
console.log(a) → undefined
a = 10

No error.

---

LET BEHAVIOR (TDZ)

Example:

```js
console.log(a);
let a = 10;
```

Creation Phase:
a allocated but not initialized

Execution Phase:
console.log(a) → ReferenceError

This period is called Temporal Dead Zone.

---

CONST BEHAVIOR

Example:

```js
const a;
```

SyntaxError – must initialize immediately.

Example:

```js
const a = 5;
a = 10;
```

TypeError – cannot reassign.

Important:
Const blocks reassignment, not mutation of object properties.

---

### FUNCTION DECLARATION VS EXPRESSION

Function Declaration

```js
sayHi();

function sayHi() {
  console.log("Hi");
}
```

Creation Phase:
sayHi stored fully.

Execution Phase:
call works.

---

Function Expression

```js
sayHi();

var sayHi = function () {};
```

Creation Phase:
sayHi → undefined

Execution Phase:
TypeError: undefined is not a function

---

FUNCTION EXPRESSION WITH LET

```js
sayHi();

let sayHi = function() {};

Creation Phase:
sayHi allocated (TDZ)

Execution Phase:
ReferenceError
```

---

VARIABLE SHADOWING CASE

```js
var a = 1;

function outer() {
  console.log(a);
  var a = 2;
}

outer();
```

Creation Phase (outer):
a → undefined

Execution Phase:
console.log(a) → undefined

Local variable shadows global.

---

POSSIBLE ERRORS

```js
SyntaxError – invalid declaration
ReferenceError – TDZ access
TypeError – calling undefined as function
RangeError – recursion overflow
```

---

CONDITIONS WHERE IT FAILS

If you:

- Access let/const before declaration → ReferenceError
- Call function expression before assignment → TypeError
- Redeclare let/const in same scope → SyntaxError
- Miss base case in recursion → RangeError

---

LIMITATIONS

- Hoisting can create confusing behavior
- Shadowing can cause unexpected undefined
- Function expression behavior can surprise developers

---

BEST PRACTICES

1. Always declare variables at top of scope.
2. Use const by default.
3. Avoid var in modern applications.
4. Never rely on hoisting intentionally.
5. Avoid redeclaration patterns.

---

REAL-TIME PROBLEMS

1. Production bug due to undefined variable.
2. React hook using variable before initialization.
3. Calling arrow function before assignment.
4. Shadowed variable causing wrong API request.

---

EXCEPTION HANDLING

Runtime errors from TDZ or TypeError can be caught with try/catch.
Syntax errors cannot be caught because parsing fails before execution.

---

MANDATORY TO LEARN

- Hoisting is memory allocation, not code movement.
- Creation phase happens inside functions too.
- Function declaration is different from function expression.
- Shadowing is determined during creation phase.
- TDZ exists for let and const.

---

SUMMARY

JavaScript execution contexts run in two phases: creation, where memory is allocated and variables/functions are prepared, and execution, where code runs line by line and function calls create new contexts.

---

Perfect 👍
Now we move to the execution backbone of JavaScript.

---

## TOPIC 4 Call Stack (Deep Dive + Execution Simulation + Blocking Behavior)

---

WHAT IT IS

The Call Stack is a data structure used by the JavaScript engine to manage execution contexts.

It follows the LIFO principle (Last In, First Out).

Only execution contexts are pushed into the call stack.

It determines:

- Which function is currently running
- Where control returns after a function finishes
- Why JavaScript executes one task at a time

---

WHY IT EXISTS

The engine needs a structured way to:

- Track active execution contexts
- Pause and resume functions
- Manage nested function calls
- Ensure proper return flow

Without call stack:

- Nested calls wouldn’t work
- Recursion wouldn’t work
- Execution order would be unpredictable

---

HOW IT WORKS (FLOW METHOD)

Rule:
Only one execution context runs at a time.
Only the top of the stack executes.

Execution Flow Example:

function first() {
second();
}

function second() {
console.log("Hello");
}

first();

Flow Diagram:

Program Start
↓
Push Global Execution Context
↓
Call first()
↓
Push first() Execution Context
↓
Call second()
↓
Push second() Execution Context
↓
Execute console.log
↓
Pop second()
↓
Pop first()
↓
Continue Global

---

KEY RULES

1. JavaScript has only ONE call stack.
2. Only execution contexts go into stack.
3. Only the top of stack executes.
4. When a function finishes, it is popped.
5. Stack must be empty for new tasks to run.

---

BLOCKING BEHAVIOR

Because JavaScript has one call stack,
long-running synchronous code blocks everything.

Example:

```js
console.log("Start");

for (let i = 0; i < 1e9; i++) {}

console.log("End");
```

Output:
Start
End (after long delay)

During loop:

- Stack is busy
- No other code executes
- UI freezes

---

INFINITE LOOP BLOCKING

```js
console.log("Start");

while (true) {}

console.log("End");
```

Output:
Start  
End never prints.

Stack never clears.

---

STACK OVERFLOW

```js
function recurse() {
  recurse();
}

recurse();
```

Each call pushes new execution context.
No pop happens.
Eventually:

RangeError: Maximum call stack size exceeded.

Stack overflow is different from blocking.

Blocking:
Stack busy with one long task.

Overflow:
Stack grows too large.

---

HOW IT SHOULD BE USED

- Avoid deep recursion without base case.
- Avoid heavy synchronous loops.
- Break heavy tasks into smaller asynchronous chunks.
- Always understand execution order.

---

HOW IT WILL NOT WORK

If stack never clears:

- Async callbacks cannot execute.
- UI becomes unresponsive.
- Server becomes unresponsive.

---

LIMITATIONS

1. Only one execution thread.
2. CPU-heavy tasks block system.
3. Limited stack size.
4. No parallel execution inside engine.

---

REAL-TIME PROBLEMS

1. React UI freezing during large data transformation.
2. Node.js API hanging during heavy CPU computation.
3. Infinite recursion causing crash.
4. Long JSON parsing blocking request handling.

---

BEST PRACTICES

- Avoid synchronous heavy computation.
- Use async APIs properly.
- Use worker threads for CPU-heavy tasks in Node.js.
- Avoid recursive patterns when possible.
- Always include base condition in recursion.

---

EXCEPTION HANDLING

Stack overflow errors (RangeError) can be caught with try/catch,
but system stability may already be compromised.

Infinite loops cannot be caught because execution never proceeds.

---

MANDATORY TO LEARN

- Only one stack exists.
- Only top executes.
- Blocking occurs when stack is busy.
- Async cannot run until stack clears.
- Recursion must have exit condition.

---

SUMMARY

The call stack is a LIFO data structure used by the JavaScript engine to manage execution contexts, ensuring that only one function executes at a time in a single-threaded environment.

Perfect 👍
Now we close Phase 1 with the architectural reasoning.

---

## TOPIC 5: Why JavaScript Is Single-Threaded (Design + Concurrency Model Foundation)

---

WHAT IT IS

JavaScript is single-threaded, meaning the JavaScript engine executes one task at a time using a single call stack.

There is only one thread of execution inside the engine.

---

WHY IT WAS DESIGNED THIS WAY

JavaScript was originally built for browsers to:

- Manipulate the DOM
- Handle user interactions
- Keep behavior predictable
- Avoid race conditions

If multiple threads modified the DOM simultaneously:

- Inconsistent UI state
- Race conditions
- Crashes
- Extremely difficult debugging

Single-threaded design ensures:

- Predictable execution
- Deterministic behavior
- Simpler concurrency model

---

HOW IT WORKS INTERNALLY

Engine Components:

- One Memory Heap
- One Call Stack
- One Execution Thread

Flow:

Task arrives
↓
Push into Call Stack
↓
Execute completely
↓
Pop
↓
Next task

No parallel execution inside engine.

---

IMPORTANT CLARIFICATION

Single-threaded does NOT mean:

- Only one execution context exists
- Only one function in memory
- Only one async operation possible

It means:

Only one execution context executes at a time.

---

HOW ASYNC STILL WORKS

JavaScript engine stays single-threaded.

The runtime environment (Browser / Node.js):

- Handles timers
- Handles network requests
- Handles file system
- Handles event listeners

When async task finishes:
It queues callback.
Callback executes only when stack is empty.

Engine never becomes multi-threaded.

---

FLOW DIAGRAM

JS Code
↓
Call Stack (Single Thread)
↓
If Async → Handed to Runtime
↓
Runtime Completes Task
↓
Callback Queued
↓
Executed When Stack Empty

---

HOW IT SHOULD BE USED

- Avoid heavy synchronous loops
- Break heavy tasks into async parts
- Avoid blocking Node.js event loop
- Use Web Workers for CPU-heavy browser tasks
- Use Worker Threads in Node.js for parallel CPU work

---

HOW IT WILL NOT WORK

If you block the stack:

while(true) {}

- Async callbacks never execute
- UI freezes
- Server stops responding

If you misunderstand single-threaded behavior:

- You mispredict execution order
- You create performance bottlenecks

---

LIMITATIONS

1. CPU-heavy operations block execution.
2. No internal parallel computation.
3. Performance depends on non-blocking design.
4. Requires asynchronous patterns for scalability.

---

REAL-TIME PROBLEMS

1. Node.js API unable to serve multiple requests due to blocking loop.
2. React app freezing while processing large dataset.
3. Large synchronous JSON parsing freezing UI.
4. Heavy crypto operations blocking server.

---

BEST PRACTICES

- Write non-blocking code.
- Avoid synchronous file operations in Node.
- Use async/await properly.
- Offload CPU-heavy work to workers.
- Keep functions small and fast.

---

COMMON MISUNDERSTANDING

“JavaScript is multi-threaded because it handles async.”

Wrong.

Correct:
JavaScript engine is single-threaded.
Concurrency is handled by runtime environment.

---

POSSIBLE ERRORS RELATED TO SINGLE-THREADED DESIGN

- RangeError (stack overflow)
- System unresponsiveness due to blocking
- Performance bottlenecks

---

MANDATORY TO LEARN

- One call stack.
- One execution at a time.
- Async does not make engine multi-threaded.
- Blocking prevents all other execution.
- Stack must clear before next task runs.

---

SUMMARY

JavaScript is single-threaded because its engine uses a single call stack to execute one task at a time, ensuring predictable DOM manipulation and simplified concurrency, while asynchronous operations are handled externally by the runtime environment.
