## Chapter 1:What`s the Scope?

### Compilter Speak

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
