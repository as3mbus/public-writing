# Java script object

> [!info] Working with objects  
> JavaScript is designed on a simple object-based paradigm.  
> [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)  

## doesn't need class / constructor

can be made without any limitation

```JavaScript
var Object =
{
    property1 : "value", // value can be string, number, another object, function etc2
    property2 : function(arg1,arg2) {}
    funcname(arg1,argN) {} // also work as above

}
```

## Constructor Function

function to create an object

```JavaScript
function Car(make, model, year) 
{
  this.make = make;
  this.model = model;
  this.year = year;
}
```

## `this` as Object reference

this.something ⇒ (this) object.something

```JavaScript
const Manager = {
  name: "John",
  age: 27,
  job: "Software Engineer"
}
const Intern= {
  name: "Ben",
  age: 21,
  job: "Software Engineer Intern"
}

function sayHi() {
    console.log('Hello, my name is', this.name)
}

// add sayHi function to both objects
Manager.sayHi = sayHi;
Intern.sayHi = sayHi; 

Manager.sayHi() // Hello, my name is John'
Intern.sayHi() // Hello, my name is Ben'
```

## Getter and setter

similar to other lang getter and setter purposes

```Plain
var o = {
  a: 7,
  get b() { 
    return this.a + 1;
  },
  set c(x) {
    this.a = x / 2;
  }
};

console.log(o.a); // 7
console.log(o.b); // 8 <-- At this point the get b() method is initiated.
o.c = 50;         //   <-- At this point the set c(x) method is initiated
console.log(o.a); // 25
```

## Object Comparison

Object is reference type and never equal

```JavaScript
// Two variables, two distinct objects with the same properties
var fruit = {name: 'apple'};
var fruitbear = {name: 'apple'};

fruit == fruitbear; // return false
fruit === fruitbear; // return false

// Two variables, a single object
var fruit = {name: 'apple'};
var fruitbear = fruit;  // Assign fruit object reference to fruitbear

// Here fruit and fruitbear are pointing to same object
fruit == fruitbear; // return true
fruit === fruitbear; // return true

fruit.name = 'grape';
console.log(fruitbear); // output: { name: "grape" }, instead of { name: "apple" }
```