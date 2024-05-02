---

Basic JavaScript scripting knowledge to Quick access. this page contain basic information without much detail on architecture information. Go To arch topic for more detailed information to arrange system to be more performant and modular

- ==**Table Of Content**==
    
    Architecture Topic
    
    Hoisting
    
    Not Hoisted
    
    Hoisted
    
    Let, Var, and Const
    
    Let ⇒ block scoped
    
    var ⇒ function scoped
    
    Const ⇒ Block Scoped, no modification possible
    
    Global Property
    
    Redeclaration
    
    Equality (==) vs. Identity (===) Operators
    
    Equality (==)
    
    Identity (===)
    
    Global Scoped Variable
    
    Scopes / Closures
    
    Regular Function {function(x)} and Arrow Function {(x) ⇒}
    
    Arrow Function don't have access it's own This scope
    
    Arguments property Availability
    
    Regular Function (available)
    
    Arrow Function (not available)
    
    prototype property Availability
    
    Promise
    
    Chaining Promise
    
    Using Promise
    
    Creating Promise
    
    Single Quote and Double Quote
    
    Undefined and Null
    

### Architecture Topic

[[Object Oriented JavaScript]]

[[Functions in javascript]]

  

# Hoisting

> [!info] JavaScript Hoisting  
> Hoisting is JavaScript's default behavior of moving declarations to the top.  
> [https://www.w3schools.com/js/js_hoisting.asp](https://www.w3schools.com/js/js_hoisting.asp)  

definition of condition when a statement doesn't need to be line ordered sequence in order to be memory allocated

### Not Hoisted

```JavaScript
const p = new Rectangle(); // ReferenceError

class Rectangle {}
```

### Hoisted

console.log(a) // undefined  
var a = 0  

  

---

# Let, Var, and Const

### Let ⇒ block scoped

- reference error before call

```JavaScript
function checkHoisting() {
  console.log(foo); // ReferenceError
  let foo = "Foo";
  console.log(foo); // Foo
}

checkHoisting();
```

### var ⇒ function scoped

- undefined before called ( memory allocated beforehand) [a.k.a. hoisted]

```JavaScript
function run() {
  console.log(foo); // undefined
  var foo = "Foo";
  console.log(foo); // Foo
}

run();
```

### Const ⇒ Block Scoped, no modification possible

```JavaScript
const abc = 'abc'
abc = 'def' // error
```

### Global Property

```JavaScript
var foo = "Foo";  // globally scoped
let bar = "Bar"; // globally scoped

console.log(window.foo); // Foo
console.log(window.bar); // undefined
```

### Redeclaration

```JavaScript
'use strict';
var foo = "foo1";
var foo = "foo2"; // No problem, 'foo' is replaced.

let bar = "bar1";
let bar = "bar2"; // SyntaxError: Identifier 'bar' has already been declared
```

---

# Equality (==) vs. Identity (===) Operators

### Equality (==)

converts both value to same type before comparing

### Identity (===)

No Type Conversion

---

# Global Scoped Variable

```JavaScript
//Define global scoped 'foo'
var foo = 'I am GLOBAL foo';
 
//Inside if block foo will refer to global foo
if ( true ) {
    var foo = 'I am GLOBAL foo TOO';
    console.log( foo );         //I am GLOBAL foo TOO
}
 
//As blocks do not have their own scope
//So foo in if block referred to global scope foo
//foo refer to new value
console.log( foo );             //I am GLOBAL foo TOO
 
//Inside function - foo has it's own declaration
function test() {
    var foo = 'I am LOCAL foo';
    console.log( foo );         //I am LOCAL foo
}
 
test();
 
//Ouside function foo is still globally declared foo
console.log( foo );             //I am GLOBAL foo TOO
```

---

# Scopes / Closures

```JavaScript
// global
function test() 
{
    // function scope / block a scope
    var foo = 'I am LOCAL foo';
    console.log( foo );         //I am LOCAL foo
    for (let i = 0;i<10 ;i++)
    {
        // block b scope
    }
    // let in block b won't be accessible
    // i isn't accessible here
}
```

---

# `**Regular**` **Function** `**{function(x)}**` **and** `**Arrow**` **Function** `**{(x) ⇒}**`

```JavaScript
let square = function(x){ 
  return (x*x); 
}; 
console.log(square(9)); // 81
```

```JavaScript
var square = (x) => { 
    return (x*x); 
}; 
console.log(square(9)); // 81
```

## Arrow Function don't have access it's own `This` scope

```JavaScript
let user = { 
    name: "GFG", 
    gfg1:() => { 
        console.log("hello " + this.name); // no 'this' binding here 
    }, 
    gfg2(){        
        console.log("Welcome to " + this.name); // 'this' binding works here 
    }   
 }; 
user.gfg1(); // hello undefined
user.gfg2(); // Welcome to GFG
```

## `Arguments` property Availability

### Regular Function (available)

```JavaScript
let user = {       
    show(){ 
        console.log(arguments); 
    } 
}; 
user.show(1, 2, 3); // output : 1, 2 ,3
```

### Arrow Function (not available)

```JavaScript
let user = {      
        show_ar : () => { 
        console.log(...arguments); 
    } 
}; 
user.show_ar(1, 2, 3);
```

## `prototype` property Availability

---

go to [[JS Scripting Knowledge base]] arrow function doesn't have prototype property

# Promise

> [!info] Promise  
> The Promise object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.  
> [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)  

![[Untitled 30.png|Untitled 30.png]]

Chaining Function in a sequence that used to mitigate callback hell

## Chaining Promise

```JavaScript
const myPromise = 
  (new Promise(myExecutorFunc))
  .then(handleFulfilledA,handleRejectedA)
  .then(handleFulfilledB,handleRejectedB)
  .then(handleFulfilledC,handleRejectedC);

// or, perhaps better ...

const myPromise =
  (new Promise(myExecutorFunc))
  .then(handleFulfilledA)
  .then(handleFulfilledB)
  .then(handleFulfilledC)
  .catch(handleRejectedAny);

// another form but with worse form
const promiseA = new Promise(myExecutorFunc);
const promiseB = promiseA.then(handleFulfilled1, handleRejected1);
const promiseC = promiseA.then(handleFulfilled2, handleRejected2);
```

## Using Promise

```JavaScript
Promise
    .then(function(){})//the function also need to return promise in order to 
    .then()
    .catch() // handle any rejection of previous then
```

## Creating Promise

```JavaScript
new Promise(function(resolve,reject) // can use defined function / arrow function
{
    if(true) resolve(); // call then callback
    else reject(); // call catch callback
})
```

# Single Quote and Double Quote

use it as you see fit but keep it uniform

```JavaScript
const singleQuote = 'this is fine';
const doubleQuote = "that's fine";
const singleQuote2 = 'can\'t use single quote in simple way';
const doubleQuote2 = "can't use double quote in simpler way like \"this \""
const singleQuote3 = 'but i can "simply do this as my heart pleases"'
```

# Undefined and Null

> [!info] What is the difference between null and undefined in JavaScript?  
> I picked this from here The undefined value is a primitive value used when a variable has not been assigned a value.  
> [https://stackoverflow.com/questions/5076944/what-is-the-difference-between-null-and-undefined-in-javascript](https://stackoverflow.com/questions/5076944/what-is-the-difference-between-null-and-undefined-in-javascript)  

#### Null and undefined difference

|Difference|undefined|null|
|---|---|---|
|[[overview]]|means a variable has been declared but has not yet been assigned a value|an assignment value. It can be assigned to a variable as a representation of no value:|
|[[Type]]|undefined|object|
|[[ToString]]|undefined|null|
|[[null ===]]|false|true|
|[[null ==]]|true|true|
|[[value assignment]]|OK|Referrence error|