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

