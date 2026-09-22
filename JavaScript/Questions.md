JavaScript Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. var vs. let vs. const
~~~~~~~~~~~~~~~~~~~~~~~~

var — function-scoped, hoisted with undefined, can be redeclared/reassigned
let — block-scoped, hoisted but in TDZ (not accessible before declaration), can be reassigned not redeclared
const — block-scoped, in TDZ, cannot be reassigned (but object/array contents can still be mutated)

2. Data types: Primitive vs. Non-Primitive
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Primitive (stored by value): string, number, boolean, null, undefined, symbol, bigint
Non-Primitive/Reference (stored by reference): object, array, function
Primitives are immutable; objects are mutable and compared by reference, not value

3. == vs. ===
~~~~~~~~~~~~~

== — loose equality, performs type coercion before comparing
=== — strict equality, compares both value and type, no coercion
=== is generally preferred to avoid unexpected coercion bugs (e.g. "5" == 5 is true, "5" === 5 is false)

4. Hoisting
~~~~~~~~~~~

JavaScript moves declarations to the top of their scope before execution
Variable hoisting: var hoisted and initialized as undefined; let/const hoisted but stay in TDZ until declaration
Function hoisting: function declarations are hoisted fully (usable before defined); function expressions/arrow functions are not

5. Closures
~~~~~~~~~~~

A closure is a function that retains access to its outer (lexical) scope's variables even after the outer function has returned
Practical example:
js
  function counter() {
    let count = 0;
    return () => ++count;
  }
  const increment = counter();
  increment(); // 1
  increment(); // 2
Used for: data privacy, function factories, memoization, event handlers

6. call, apply, bind
~~~~~~~~~~~~~~~~~~~~

call — invokes function immediately, arguments passed individually: fn.call(thisArg, a, b)
apply — invokes function immediately, arguments passed as an array: fn.apply(thisArg, [a, b])
bind — returns a new function with this bound, doesn't invoke immediately: const bound = fn.bind(thisArg)

7. Event Loop, Call Stack, Task Queue
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Call Stack — where function calls are executed, LIFO order
Web APIs — handle async operations (timers, fetch, DOM events) outside the stack
Task Queue (callback queue) — holds completed async callbacks waiting to run
Microtask Queue — holds promise callbacks, has higher priority than the task queue
Event Loop — continuously checks if the call stack is empty, then pushes queued callbacks (microtasks first, then macrotasks) onto it

8. Regular function vs. Arrow function
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Regular function — has its own this (depends on how it's called), has arguments object, can be used as constructor
Arrow function — no own this (inherits from enclosing scope), no arguments object, cannot be used as constructor, more concise syntax

9. Promises & Callback Hell
~~~~~~~~~~~~~~~~~~~~~~~~~~~

A Promise represents a value that may be available now, later, or never — has states: pending, fulfilled, rejected
Solves Callback Hell (deeply nested callbacks) by allowing chaining with .then()/.catch() instead of nesting
Makes async code flatter, more readable, and easier to handle errors for

10. async/await & error handling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

async — makes a function return a Promise automatically
await — pauses execution until the Promise resolves, makes async code look synchronous
Error handling done via try/catch blocks wrapping the await calls
js
  async function getData() {
    try {
      const res = await fetch(url);
    } catch (err) {
      console.error(err);
    }
  }

11. Promise.all() vs. allSettled() vs. any()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Promise.all() — resolves when all promises succeed; rejects immediately if any one fails
Promise.allSettled() — waits for all promises to complete regardless of success/failure, returns status of each
Promise.any() — resolves as soon as any one promise succeeds; rejects only if all fail

12. LocalStorage vs. SessionStorage vs. Cookies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

LocalStorage — persists indefinitely, ~5-10MB, client-side only, not sent to server
SessionStorage — persists for tab session only, similar size limit, client-side only
Cookies — small size (~4KB), can have expiration set, sent to server with every HTTP request, can be used for authentication

13. Event Bubbling, Capturing, Delegation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Bubbling — event propagates from the target element up to its ancestors
Capturing — event propagates from the ancestor down to the target element (opposite direction, happens first)
Delegation — attaching a single event listener to a parent to handle events from its children, using bubbling — improves performance for dynamic/many elements

14. Shallow copy vs. Deep copy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Shallow copy — copies only the top-level properties; nested objects are still shared by reference (e.g. Object.assign(), spread {...obj})
Deep copy — recursively copies all nested levels, fully independent copy (e.g. structuredClone(), JSON.parse(JSON.stringify()) with limitations, or a deep-clone utility)

15. Higher-Order Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~

Functions that take another function as an argument, return a function, or both
Examples: map() — transforms each element; filter() — selects elements matching a condition; reduce() — accumulates values into a single result
Enable functional, declarative programming style

16. 'this' keyword
~~~~~~~~~~~~~~~~~~

Refers to the execution context, value depends on how a function is called
Global context — this refers to window (non-strict) or undefined (strict mode)
Object method — this refers to the object the method is called on
Arrow function — this inherited from the enclosing lexical scope
call/apply/bind — this explicitly set

17. Prototype & Prototypal Inheritance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Every JS object has an internal link to another object called its prototype
Prototypal inheritance — objects can inherit properties/methods from their prototype chain
Accessed via Object.getPrototypeOf() or the __proto__ property (or .prototype on constructor functions)
Enables method/property sharing without duplicating code across instances

18. Null vs. Undefined
~~~~~~~~~~~~~~~~~~~~~~

undefined — a variable declared but not assigned a value; default value JS assigns
null — an intentional absence of value, explicitly assigned by the developer
typeof undefined is "undefined"; typeof null is "object" (a known JS quirk)

19. Debouncing & Throttling
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Debouncing — delays function execution until after a pause in triggering events (e.g. waits until user stops typing)
Throttling — limits function execution to at most once per specified time interval, regardless of how often the event fires
Used for: performance optimization on frequent events like scroll, resize, keypress, search input

20. Temporal Dead Zone (TDZ)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The period between entering a scope and the actual declaration line where let/const variables exist but can't be accessed
Accessing them in this zone throws a ReferenceError
Exists to enforce safer coding practices, unlike var's silent undefined hoisting

21. ES6 Modules
~~~~~~~~~~~~~~~

export — makes variables/functions/classes available to other files
import — brings exported code into another file
Two types: named exports (export const x) and default exports (export default)
Enables modular, maintainable, reusable code organization

22. Rest vs. Spread operator (...)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Rest — collects multiple elements into a single array/object, used in function parameters or destructuring: function sum(...nums)
Spread — expands an array/object into individual elements, used in function calls, array/object literals: [...arr1, ...arr2]
Same syntax, opposite purpose: rest gathers, spread expands

23. Object & Array Destructuring
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Syntax to unpack values from arrays or properties from objects into variables
Array destructuring: const [a, b] = [1, 2];
Object destructuring: const { name, age } = person;
Supports default values, renaming, and nested destructuring

24. Synchronous vs. Asynchronous execution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Synchronous — code executes line by line, each operation blocks the next until it completes
Asynchronous — operations can run in the background (e.g. API calls, timers) without blocking the main thread, completing via callbacks/promises/async-await
Async is essential for non-blocking UI and I/O operations in JS

25. Currying
~~~~~~~~~~~~

A technique of transforming a function with multiple arguments into a sequence of functions, each taking a single argument
Example:
js
  const add = a => b => c => a + b + c;
  add(1)(2)(3); // 6
Use cases: function reusability, partial application, creating specialized functions from general ones