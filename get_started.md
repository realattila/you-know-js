## chapter 2: Surverying JS

### Values

Values come in two forms in JS: primitive and object. Values are data.
Interpolation is ${} in template back-tick string(`\${}`).

### Arrays And Objects

Arrays are a special type of object that’s
comprised of an ordered and numerically indexed list of data

#### Function declaration

```
function () {}

function \*two() { }

export function five() { }
```

#### Function expression

```
var func = function () {}

(function(){ })();

f = () => {}
```

#### Function expression vs Function declaration

1. Function expressions in JavaScript are not hoisted, unlike function declarations. You can't use function expressions before you create them.
2. Function declarations load before any code is executed while Function expressions load only when the interpreter reaches that line of code.

### === and == and other ( <, >, <=, >= )

1. === is strict equality, whereas == is loose equality.
2. === also check type.
3. == allows type conversions first, it convert to number and then compare.
4. ( <, >, <=, >= ) also act like == operator.

```
32 == '32' // true

32 === '32' // false

'' == 0 //ture

'' == false // true
```

### How we organize in JS

ESMs are always file-based; one file, one module.

## Chapter 3: Digging to the Roots of JS

### This keyword

when a function is defined, it is attached to its enclosing scope via closure.

### prototypes

Object.create(null) creates an object that is not prototype linked anywhere

when we use Object.create , it copies the prototype chain from the original object.it shallow copy properties
when we use Object.assign , it not copies the prototype chain from the original object.it deep copy properties

```
const a = {b: 'b'}; const c = Object.create(a); c.b = 'd'; console.log(a.b); // 'b' console.log(c.b); // 'd';

const a = {b: 'b'}; const c = Object.create(a); a.b = 'd'; console.log(a.b); // 'd' console.log(c.b); // 'd';
```

## Chapter 4: The Bigger Picture

### Pillar 1:Scope And Closure

lexical scope called how scopes behave in JS.

let/const declarations have a peculiar error behavior called the "Temporal Dead Zone". which results in observable but unusable variables.

When a function makes reference to variables from an outer scope, and that function is passed around as a value and executed in other scopes, it maintains access to its original scope variables; this is closure.
