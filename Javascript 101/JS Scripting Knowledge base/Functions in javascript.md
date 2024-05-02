# Generator (syntax : `function*`)

similar to ienumerator for yield return value on an iteration

> [!info] function*  
> The function* declaration ( function keyword followed by an asterisk) defines a generator function, which returns a object.  
> [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)  

## Example Infinite Iterator

```JavaScript
function* infinite() {
    let index = 0;

    while (true) {
        yield index++;
    }
}

const generator = infinite(); // "Generator { }"

console.log(generator.next().value); // 0
console.log(generator.next().value); // 1
console.log(generator.next().value); // 2
// ...
```

## Nested Generator with `yield*`

```JavaScript
function* anotherGenerator(i) {
  yield i + 1;
  yield i + 2;
  yield i + 3;
}

function* generator(i) {
  yield i;
  yield* anotherGenerator(i);
  yield i + 10;
}

var gen = generator(10);

console.log(gen.next().value); // 10
console.log(gen.next().value); // 11
console.log(gen.next().value); // 12
console.log(gen.next().value); // 13
console.log(gen.next().value); // 20
```

---

# Function

can be both void and func at the same time the most random stuff

> [!important]  
> function is also an object so it can be stored with expression and called nestedly e.g function_name()() wot ?  

## Function Declaration

basic declaration similar to most language

```JavaScript
function square(number) {
  return number * number;
}
```

## Function Expression

can be used to represent block scoped (local) function

```JavaScript
function map(f, a) {
  let result = []; // Create a new Array
  let i; // Declare variable
  for (i = 0; i != a.length; i++)
    result[i] = f(a[i]);
  return result;
}
const f = function(x) {
   return x * x * x; 
}
let numbers = [0, 1, 2, 5, 10];
let cube = map(f,numbers);
console.log(cube);
```

### Recursive Function in Expression

Providing a name allows the function to refer to itself, and also makes it easier to identify the function in a debugger's stack traces:

```JavaScript
const factorial = function fac(n) { return n < 2 ? 1 : n * fac(n - 1) }

console.log(factorial(3))
```

## Hoisting

Function Declaration are hoisted

```JavaScript
console.log(square(5));
/* ... */
function square(n) { return n * n } 
```

while expression are limited to expression it stored to

```JavaScript
console.log(square)    // square is hoisted with an initial value undefined.
console.log(square(5)) // Uncaught TypeError: square is not a function
const square = function(n) { 
  return n * n; 
}
```

## Nested Function and nested closure

```JavaScript
function addSquares(a, b) {
  function square(x) {
    return x * x; // can also access a and b
  }
  return square(a) + square(b); // can't access x
}
a = addSquares(2, 3); // returns 13
b = addSquares(3, 4); // returns 25
c = addSquares(4, 5); // returns 41
```

### variable preservation

A closure must preserve the arguments and variables in all scopes it references. Since each call provides potentially different arguments, a new closure is created for each call to outside. The memory can be freed only when the returned inside is no longer accessible

```JavaScript
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}
fn_inside = outside(3); // Think of it like: give me a function that adds 3 to whatever you give
                        // it
result = fn_inside(5); // returns 8

result1 = outside(3)(5); // returns 8
```

### name conflict

When two arguments or variables in the scopes of a closure have the same name, there is a name conflict. More nested scopes take precedence. So, the inner-most scope takes the highest precedence, while the outer-most scope takes the lowest.

```JavaScript
function outside() {
  var x = 5;
  function inside(x) {
    return x * 2;
  }
  return inside;
}

outside()(10); // returns 20 instead of 10
```

## Closures

```JavaScript
var createPet = function(name) {
  var sex;
  
  return {
    setName: function(newName) {
      name = newName;
    },
    
    getName: function() {
      return name;
    },
    
    getSex: function() {
      return sex;
    },
    
    setSex: function(newSex) {
      if(typeof newSex === 'string' && (newSex.toLowerCase() === 'male' || 
        newSex.toLowerCase() === 'female')) {
        sex = newSex;
      }
    }
  }
}

var pet = createPet('Vivie');
pet.getName();                  // Vivie

pet.setName('Oliver');
pet.setSex('male');
pet.getSex();                   // male
pet.getName();                  // Oliver
```

## Using `arguments`

object that store every record of argument provided for the function

```JavaScript
function myConcat(separator) {
   var result = ''; // initialize list
   var i;
   // iterate through arguments
   for (i = 1; i < arguments.length; i++) {
      result += arguments[i] + separator;
   }
   return result;
}
// returns "red, orange, blue, "
myConcat(', ', 'red', 'orange', 'blue');

// returns "elephant; giraffe; lion; cheetah; "
myConcat('; ', 'elephant', 'giraffe', 'lion', 'cheetah');

// returns "sage. basil. oregano. pepper. parsley. "
myConcat('. ', 'sage', 'basil', 'oregano', 'pepper', 'parsley');
```

## Default Parameter

```JavaScript
function multiply(a, b = 1) {
  return a * b;
}

multiply(5); // 5
```