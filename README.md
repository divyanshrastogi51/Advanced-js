# Advanced-js
A complete  Modern JavaScript Tutorial from scratch
![Course Structure](https://coggle-downloads-production.s3.eu-west-1.amazonaws.com/53ced5424ace05b08e77e053216dd4ba641f0928805bc61b072b7f73eafae4fe/Advanced_Javascript.png?AWSAccessKeyId=ASIA4YTCGXFHDFVUBBGO&Expires=1602625479&Signature=ps6mItO830TBIImVVuG%2FYXOEZ9A%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEHAaCWV1LXdlc3QtMSJGMEQCIEjw4LYNsqPF45OCcU5pAN2zmVphdM1p4GJ9muCFdLAPAiBUGpeAKsVH7x46GmqK2G16254BT%2BpUvyKYatBtYsKioCrfAQip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDg3NzQ1MzAzMTc1OCIMLVqDN59G1YRCjfmaKrMBn0vEWuPQXtuDbWwzhblDzlwdHlkBbIwWHP3DxRgx86R23cHvD5mepfDhDewuT7UuAYYB2AHDAwjbKgwpgOu3lBtWpJSOm0%2BQD%2FLU1VIztLSyuRiZ%2F3cp1erWadJIsVRUqZSL3cukHJ9Z5qEQOnbBdSJISO2U8stZXeMFoP6ZCilTJhZACk4CilL5puv79%2FG1dGoN%2FLjFtxIaOGmn%2BB3sfOrtKinthH6GGoljnkyKcQbnggQw2pSX%2FAU64QEKmUuo4aevQDXpBPpbr9nm95%2FrmjVw68lzKgPgUavu3zRGEyZLCR5iINHpHAv8CQ5KjWemjWTxKcXKq51Jqj0a7X6efjnQ0PATkF2v24k2oeY86qPQHMVTahneKDVNnsywudF6ZhSr2s26ZIznHDyHqD2MG%2B%2FvwJPC1bBNtyDOQrOEcvWWJyBjxiJxbjogdy%2FOxRpZviKJMJhlGj59pKJ49Rc6CgCbSoWIyOYLGhetXZswoCqZxq1pUPcnNNLx%2BBWhPVrWFAtTt92p1qnAE1QGpXsqZAP%2Fg3xNhxO67hquFhw%3D)
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

# 2. Types
# 2.1 Six and a half types ...and "function"

There are 6 data types in JavaScript... or actually 7... wait, no! There are six! Huh!? Whaaa!

## Primitive types

Represent single values. Easy and simply.

1. `number` like 5

2. `string` like "Hello world!"

3. `undefined` when something lacks a definition. Variables that are hoisted or defined without assignment are assigned "undefined".

4. `null` doesn't actually exist as a type, but is an "object". This is one of the quirks of JS.

5. `symbol` for unique object properties


##  Non-primitive types

Can combine multiple types. Do not actually contain the value directly, but is a reference (pointer) to some place in memory.

6. `object` for objects, arrays, functions and... null

7. `function` is not actually a real type because it is actually an object.

- Arrays are `object`s

So up to number five it all makes sense, but then it gets weird. `null` is an object while it shouldn't be. Functions have the type of `function` while they should be `object`s.

# 2.2 Pass by reference vs pass by value

## Passed by value

Primitives are **passed by value**.

There is no link back to memory, but bascially you are dealing with a copy of the original. Changing these values does not affect the original, but the copy.

## Passed by reference

Objects are **passed by reference**.

When objects are passed into a function they still point to the original place in memory. Making changes to the object that are passed into functions changes the original.

## Cloning objects

How can we create copies of objects and not references?

### Shallow cloning

Clone one object, but not child objects.

```
const obj = {
	a: 1,
	b: 2,
	c: {
		deep: "Try and copy me"
	}
};

let clone1 = Object.assign({}, obj);
let clone2 = { ...obj };
```

### Deep cloning

Clone an object and all objects within it.

Below is not a good idea generally, because with massive objects it can take a long time and increased memory consumption. There are better ways of solve problems around objects passed by reference.

```
const obj = {
	a: 1,
	b: 2,
	c: {
		deep: "Try and copy me"
	}
};

let deepClone = JSON.parse(JSON.stringify(obj)); 
```
# 2.3 Coercion

A very controversial topic.

Operators like `==`, `+` result in coercion. Better avoid generally.

# 3 Two Pillars
# 3.1 Functions: First Class Citizens

Functions are data.

Functions are `object`s.

They can be **passed** around in other functions and **returned** by other functions.

They can be stored inside variables (**function expressions**).

Anything you can do with any other type you can do with functions.

This opens up exciting possibilities for **functional programming**.

# 3.2 Higher Order Functions (HOF)

_Don't hassle the Hoff!_

## Powerful and an easy definition

```
function() < function(a,b) < HOF
```

A **Higher Order Function** (HOF) is a function that takes a **function as an argument** or **returns a function**.

## But why are HOF's useful and how to use them properly?

Yeah?


# 3.3 Pillar 1: Closures

Variables in the **variable environment** of an **execution context** of a **higher order function**, i.e. a function that returns a function, are stored in a special box, "**the closure**". These variables are **not collected** by the **garbage collector**.

That all this can happen is thanks to the concept of the **lexical environment** which is checked by the JIT Compiler before executing any code. It sees that there are variables in functions bodies (soon to be execution contexts) that will be referenced later by "lower order" functions.

## Closures are sometimes referred to as "lexical scoping"

**lexical** = where the code is written.
**scope** = what variables the JIT compiler/runtime has access to.

In execution contexts you do not only have access to variables higher in the calling context, but also what is stored in the memory heap through closures.

**Lexical environment === [[scope]]**

Where we write the function matters. Not where we call/invoke the function.
excode: https://repl.it/repls/GlaringLovingCables 
## Closures & Encapsulation

## Principle of least privilege

Data safety when making API's or other code that is accessibily to the public. With closures you can keep data safe for internal reasons.
Code: https://repl.it/repls/ExhaustedDisgustingSet

# 3.4 Pillar 2: Prototypal Inheritance

Almost everything in JavaScript is an object. All objects inherit from other objects until all the way up to the **prototype chain** the base `Object`.

`__proto__` can be used to extend objects to protypal objects, but should never be used!

`.isPrototypeOf(Object)` checks if passed the object it is called upon (`this`) is a prototype of the passed in object.

`.hasOwnProperty`: checks if a given property is on the object it is called upon. There is, in other words, no need to go up the prototype chain.

Make one object inherit from another object:

```
const lizard = {
	strength: 10,
	health: 100,
	attack() {
		return this.strength;
	},
	battleCry() {
		return `Broaoa!`;
	},
}

const dragon = {
	strength: 50,
	health: 200,
};

dragon.__proto__ = lizard;
```

`lizard.isPrototypeOf(dragon) === true`
