## Chapter 1:What`s the Scope?

### Compiling Code

JS programs are parsed before any execution begins. see example below:

```
function saySomething() {
    var greeting = "Hello";
    {
        greeting = "Howdy"; // error comes from here
        let greeting = "Hi";
        console.log(greeting);
    }
}
saySomething();
// ReferenceError: Cannot access 'greeting' before
// initialization
```

```
console.log("Howdy");
saySomething("Hello","Hi");
// Uncaught SyntaxError: Duplicate parameter name not
// allowed in this context
function saySomething(greeting,greeting) {
    "use strict";
    console.log(greeting);
}
```

## Chapter 2:Illustrating Lexical Scope

Lexical Scoping defines how variable names are resolved in nested functions: inner functions contain the scope of parent functions even if the parent function has returned.
A lexical scope in JavaScript means that a variable defined outside a function can be accessible inside another function defined after the variable declaration. But the opposite is not true; the variables defined inside a function will not be accessible outside that function.

```
function a()
{
    var x = 5;

    function b()
    {
        console.log(x);
    }
}
// Every inner level can access its outer levels.
```

### A Converstion Among Friends

the members of the JS engine that will have conversations as they process our program

- Engine: responsible for start-to-finish compilation and execution of our JavaScript program.
- Compiler: one of Engine’s friends; handles all the dirty work of parsing and code-generation.
- Scope Manager: another friend of Engine; collects and maintains a lookup list of all the declared variables/identifiers, and enforces a set of rules as to how these are accessible to currently executing code.

Compiler will handle during compilation, and the other which Engine will handle during execution.
The first thing Compiler will do with this program is perform lexing to break it down into tokens, which it will then parse into a tree (AST).

### Nested Scope

One of the key aspects of lexical scope is that any time an identifier reference cannot be found in the current scope, the next outer scope in the nesting is consulted; that process is repeated until an answer is found or there are no more scopes to consult.
when js engine ask scope manager, variable and scop manager can not locate variable, scope manager in non-strict-mode create variable with undefind value. in stric-mode scope manager throw ReferenceError.

## Chapter 3:The Scope Chain

### Lookup Is (Mostly) Conceptual

Consider a reference to a variable that isn’t declared in any lexically available scopes in the current file, asserts that each file is its own separate program from the perspective of JS compilation. If no declaration is found, that’s not necessarily an error. Another file (program) in the runtime may indeed declare that variable in the shared global scope.

### Backing Out

When a function (declaration or expression) is defined, a new scope is created.

## Chapter 4: Around the Global Scope

### Where Exactly is this Global Scope?

#### Globals Shadowing Globals

The let declaration adds a something global variable but not a global object property.

a global object property can be shadowed by a global variable.

```
window.something = 42;
let something = "Kyle";
console.log(something);
// Kyle
console.log(window.something);
// 42
```

#### DOM Globals

a DOM element with an id attribute automatically creates a global variable that references it.

```
<ul id="my-todo-list">
<li id="first">Write a book</li>
..
</ul>
```

```
first;
// <li id="first">..</li>

window["my-todo-list"];
// <ul id="my-todo-list">..</ul>
```

If the id value is a valid lexical name (like first ), the lexical variable is created. If not, the only way to access that global is through the global object ( window[..] ).

#### Web Workers

Web Workers are a web platform extension on top of browser JS behavior, which allows a JS file to run in a completely separate thread (operating system wise) from the thread that’s running the main JS program.

Web Worker code does not have access to the DOM.

Since a Web Worker is treated as a wholly separate program, it does not share the global scope with the main JS program.

## Chapter 5: The (Not So) Secret Lifecycle of Variables

### When Can i Use a Variable?

```
greeting();
// Hello!
function greeting() {
console.log("Hello!");
}
```

The term most commonly used for a variable being visible from the beginning of its enclosing scope, even though its declaration may appear further down in the scope, is called hoisting.

When a function declaration’s name identifier is registered at the top of its scope, it’s additionally auto-initialized to that function’s reference. That’s why the function can be called throughout the entire scope!

#### Hositing: Declaration vs. Expression

```
greeting();
// TypeError
var greeting = function greeting() {
console.log("Hello!");
};
```

A TypeError means we’re trying to do something with a value that is not allowed. Notice that the error is not a ReferenceError. JS isn’t telling us that it couldn’t find greeting as an identifier in the scope. It’s telling us that greeting was found but doesn’t hold a function reference at that moment.

### Hoisting: Yet Another Metaphor

```
greeting = "Hello!";
console.log(greeting);
// Hello!
var greeting = "Howdy!";
```

The typical assertion of what hoisting means: lifting—like lifting a heavy weight upward—any identifiers all the way to the top of a scope. The explanation often asserted is that the JS engine will actually rewrite that program before execution,so that it looks more like this:

```
var greeting;           // hoisted declaration
greeting = "Hello!";    // the original line 1
console.log(greeting);  // Hello!
greeting = "Howdy!";    // `var` is gone!
```

```
studentName = "Suzy";
greeting();
// Hello Suzy!
function greeting() {
console.log(`Hello ${ studentName }!`);
}
var studentName;
```

The “rule” of the hoisting metaphor is that function declarations are hoisted first, then variables are hoisted immediately after all the functions. Thus, the hoisting story suggests that program is re-arranged by the JS engine to look like this:

```
function greeting() {
console.log(`Hello ${ studentName }!`);
}
var studentName;
studentName = "Suzy";
greeting();
// Hello Suzy!
```

### Re-declaration?

```
var studentName = "Frank";
console.log(studentName);
// Frank
var studentName;
console.log(studentName);
// ???
```

If you consider this program from the perspective of the hoisting metaphor, the code would be re-arranged like this for execution purposes:

```
var studentName;
var studentName;    // clearly a pointless no-op!
studentName = "Frank";
console.log(studentName);
// Frank
console.log(studentName);
// Frank
```

```
var greeting;
function greeting() {
console.log("Hello!");
}
// basically, a no-op
var greeting;
typeof greeting;    // "function"
var greeting = "Hello!";
typeof greeting;    // "string"
```

The first greeting declaration registers the identifier to the scope, and because it’s a var the auto-initialization will be undefined . The function declaration doesn’t need to reregister the identifier, but because of function hoisting it overrides the auto-initialization to use the function reference. The second var greeting by itself doesn’t do anything since greeting is already an identifier and function hoisting already took precedence for the auto-initialization.

```
let studentName = "Frank";
console.log(studentName);
let studentName = "Suzy";
```

This program will not execute, but instead immediately throw a SyntaxError .

```
var studentName = "Frank";
let studentName = "Suzy";
```

and:

```
let studentName = "Frank";
var studentName = "Suzy";
```

In both cases, a SyntaxError is thrown on the second declaration. In other words, the only way to “re-declare” a variable is to use var for all (two or more) of its declarations.

#### Constants?

The const keyword requires a variable to be initialized, so omitting an assignment from the declaration results in a SyntaxError :

```
const empty;
// SyntaxError

```

const declarations create variables that cannot be re-assigned:

```
const studentName = "Frank";
console.log(studentName);
// Frank
studentName = "Suzy";   // TypeError
```

One way to keep this all straight is to remember that var , let , and const keywords are effectively removed from the code by the time it starts to execute. They’re handled entirely by the compiler.

#### Loops

```
for (let i = 0; i < 3; i++) {
let value = i * 10;
console.log(`${ i }: ${ value }`);
}
// 0: 0
// 1: 10
// 2: 20
```

It should be clear that there’s only one value declared per scope instance. But what about i ? Is it being “re-declared”?
To answer that, consider what scope i is in. It might seem like it would be in the outer (in this case, global) scope, but it’s not. It’s in the scope of for -loop body, just like value is. In fact, you could sorta think about that loop in this more verbose equivalent form:

```
{
// a fictional variable for illustration
    let $$i = 0;
    for ( /* nothing */; $$i < 3; $$i++) {
    // here's our actual loop `i`!
    let i = $$i;
    let value = i * 10;
    console.log(`${ i }: ${ value }`);
    }
    // 0: 0
    // 1: 10
    // 2: 20
}
```

```
for (const i = 0; i < 3; i++) {
// oops, this is going to fail with
// a Type Error after the first iteration
}
```

```
{
// a fictional variable for illustration
const $$i = 0;
    for ( ; $$i < 3; $$i++) {
    // here's our actual loop `i`!
    const i = $$i;
    // ..
    }
}
```

Do you spot the problem? Our i is indeed just created once inside the loop. That’s not the problem. The problem is the conceptual $$i that must be incremented each time with the $$i++ expression. That’s re-assignment (not “redeclaration”), which isn’t allowed for constants.

#### Uninitialized Variables (aka, TDZ)

The TDZ is the time window where a variable exists but is still uninitialized, and therefore cannot be accessed in any way.

A var also has technically has a TDZ, but it’s zero in length and thus unobservable to our programs! Only let and const have an observable TDZ.

## Chapter 6: Limiting Scope Exposure
