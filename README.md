# Advanced-js
A complete  Modern JavaScript Tutorial from scratch

# 1. Foundations
# 1.1 Inside the Engine

## Components

**parser**,
**interpreter**,
**compiler**,
**abstract syntax tree (AST)**,
**call stack**,
**memory heap**

## First engine

The first JavaScript engine was created by Brendan Eich(Creator of JS). This evolved into the SpiderMonkey engine, still used by the Firefox browser.
while chrome usses Chrome V8 engine

## Flow of a JS-engine:

Parser => AST => Interpreter
	(1) => Bytecode (01010110011)
	(2) => Profiler ==> Compiler ==> Optimized code (100110011010)

**Interpreter**: line by line; from left to right and top to bottom

**Compiler**: code optimizations: first look at the code for optimizations

**JIT (Just In Time) Compiler**: combination of compiler and interpreter.

All Modern JS Engine uses JIT Compilers
# 1.2 Callstack & Memory Heap

## Memory heap

Storage for values ​​(functions, variables) in a prepared space in the working memory (RAM).

## Callstack

In an execution context called functions get added on top of the callstack. Once the interpreter leaves the function through a return or otherwise the function reference gets popped of the callstack.

The values and functions that are in the memory heap are referenced in the callstack.

There is only one callstack, i.e. "single threaded" and "synchronous" JS 

A new thread creates a new callstack

## Stack Overflow

Functions calling themselves or functions being called in a never ending loop.

These days the error "maximum stack size reached"

# 1.3 Garbage Collection

Only the data that is still useful to us remains in the Memory Heap to make sure to not use up all the memory available.

## False sense of security

Automatic garbage collection creates a false sense of safety. Programmers should think about how their programs affect the memory usage.

Memory leaks might still happen with automatic garbage collection. No system is perfect and garbage collectors can only do so much.

It's easy to create a memory leak:

```
let array = [];

for (let i = 0; i > 1; i++) {
	array.push(i-1);
}
```

Risky patterns that may create memory leaks:

- Global variables (keep sitting in memory and might get bigger and bigger)
- Event listeners (should be removed)
- setInterval (objects references inside intervals will never be cleared in memory because the interval runs forever)

# 1.4 Event Loop, Call Stack en Callback Queue

## Event Loop

Continually checking if the **call stack** is empty. Runs functions and sends calls to the appropriate destinations such as the external Web API.

## Call Stack

*Synchronous* functions get added to the call stack.

## Web API

*Asynchronous* functions like setTimeout use the Web API. Their logic is handled "over there" in a separate thread. They return a **callback** which goes to the **callback queue**.

## Callback Queue

Asynchronous callbacks arrive in the callback queue. They are picked up by the **event loop** and send back to the **call stack** *IF* it is empty and the synchronous functions (i.e. execition contexts) have finished running.

# 1.5 Node.js

Node.js is a **runtime**: 
it is an engine (V8), an asycnhronous API and tooling to work with a file and operating system.
## Runtime

Software that is executed while a program is running.
Runtime code is the code required to implement the features of the language itself.

Google Chrome's V8 engine is a runtime that compiles JavaScript to machine code.

Low level languages tend to have no or very small runtimes. Higher level programming languages often have runtimes. Java ("write once, run anywhere") is a famous example: Java Runtime Environment (JRE).

# 1.6 Execution Context

Each function call creates a new **execution context**.

Each execution context is added to the Call Stack. Each function or variable has access to its execution context.

There is always a **global** execution context. So there is always an execution context

# 1.7 Lexical environment

Lexical environment: where code is written (global or inside a function).

**Execution context** tells you which **lexical environment** is currently running.

## Lexical scope

Lexical scope is the **available data** (execution context and memory heap) + **variables where the function was defined** (lexical environment).

This determines our available variables and _not_ where the function is called (dynamic scope).

If a function is written inside another function you have a function lexical environment. The outer function is the **function lexical environment** for the inner function.

## Function lexical environment

If a function is written inside another function you have a function lexical environment. The outer function is the **function lexical environment** for the inner function.

# 1.8 Function scope vs Block scope

Originally JavaScript is only functionally scoped and not block scoped. This means that variables inside functions are private, but variables inside code blocks `{}` such as with if/else-statements are not scoped. Values in such blocks can be accessed.

Most languages do have block scoping.

Because JS was weird in that sense they introduced the `let` and `const` keywords in ECMAScript 6. Variables declared with these keywords are block scoped.

# 1.9 Global variables

Try to avoid them as much as possible. ;-)

# 1.10 `this`

Do you know what *this* is?

**this** is the object that the function is a property of.

`this` basically answers the question: **"what called me?"**

Global functions are a property of the global object. On the web that is `window`.

For methods, functions sitting on objects, *this* refers to those objects that they are a method of.

`this` then refers to what is left of the "." in method calls.

When calling `Person.sayName()` `this` refers to "Person".

When calling `helloWorld()` in the global lexical scope, `this` refers to the global scope such as the Window object in browsers. You could also have written `window.helloWorld()`.

## `this` and dynamic scope vs lexical scope

It's about the calling context (**dynamic scope**) and _NOT_ the lexical scope.

"In JavaScript our lexical scope (available data + variables where the function was defined) determines our available variables. Not where the function is called (dynamic scope)."

```
// Helper function
const isSingular = (number) => {
		return number === 0 || number === 1;
};

const Person = {
		name: null,
		age: null,
		sayName() {
			return this.name;
		},
		sayAge() {
			return this.age;
		},
		sayNameAndAge() {
				return `My name is ${this.sayName} and I am ${this.sayAge} year${isSingular(this.sayAge) ? "" : "s"} old.`;
		},
};

const ruben = { ...Person, name: "Ruben", age: 33 };
```
# 1.11 call(), apply(), bind()

## `call()`

Under the hood all functions use `call()` when they run. Every function gets this method attached to it.

## `apply()`

`apply()` does the same thing as `call()`.

## `call(obj, args...)` and `apply(obj, [args...])`

With call and apply you can borrow methods from other objects and "apply" them to an object that does not have that method. You can also pass in extra parameters.

## `bind()`

Also allows us to use other object's methods, but with the difference that bind returns a new function. It basically copies it for later use without calling the function immediately. 

## bind() and currying

WIth `.bind()` you can extend any function through currying (passing function into functions).

```
function multiply(a, b) {
	return a * b;
}

let multiplyByTwo = multiply.bind(this, 2);
let multiplyByTen = multiply.bind(this, 10);

console.log(multiplyByTwo(4)); // 8
console.log(multiplyByTen(4)); // 40
```
