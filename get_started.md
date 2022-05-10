## chapter 2: Surverying JS

### Values

Values come in two forms in JS: primitive and object. Values are data.
Interpolation is ${} in template back-tick string(`\${}`).

### Arrays And Objects

Arrays are a special type of object that’s
comprised of an ordered and numerically indexed list of data

#### Function declaration

Function () {}

#### Function expression

var func = function () {}

#### Function expression vs Function declaration

1. Function expressions in JavaScript are not hoisted, unlike function declarations. You can't use function expressions before you create them.
2. Function declarations load before any code is executed while Function expressions load only when the interpreter reaches that line of code.

#### === and == and other ( <, >, <=, >= )

1. === is strict equality, whereas == is loose equality.
2. === also check type.
3. == allows type conversions first, it convert to number and then compare.
4. ( <, >, <=, >= ) also act like == operator.

###### 32 == '32' // true

###### 32 === '32' // false

###### '' == 0 //ture

###### '' == false // true
