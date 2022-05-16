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
