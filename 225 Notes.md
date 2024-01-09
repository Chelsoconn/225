**Methods**

- Functions with a *reciever*- 
  - the object the method is called on (method invocation)
- If it doesn't have an explicit reciever, it is a function (finction invocation)

```js
let greeter = {
  morning: function() {
    console.log('Good morning!');
  },
};

function evening() {
  console.log('Good evening!');
}

// Method invocation
greeter.morning();   // greeter is the receiver/calling object; morning() is a method

// Function invocation
evening();           // there is no explicit receiver; evening() is a function
```

```js
let greeter = {
  morning: function() {
    console.log('Good morning!');
  },
};

// Method invocation: executing a method
greeter.morning(); // => Good morning!

// Create a variable which points at the greeter.morning method
let functionGreeter = greeter.morning; // returns [Function: morning]

// Function invocation: executing a function
functionGreeter();  // => Good morning!
```

```js
let counter = {
  count: 0,
  advance: function() {
    this.count += 1;
  },
};

counter;                // { count: 0, advance: [Function] }

counter.advance();
counter;                // { count: 1, advance: [Function] }

counter.advance();
counter.advance();

counter;                // { count: 3, advance: [Function] }
```

- Within `advance`, `this` references the `counter` object. The function can use `this` to access and change the object's properties. Here, `advance` uses `this` to increment the `count` property in `counter`.

```js
let car = {
  fuel: 7.8,
  running: false,
  start: function() {
    this.running = true;
  },
};

car.stop = function() {
  this.running = false;
};

car.drive = function(distance) {
  this.fuel -= distance / 52;
};
```



**Collaborator Objects**

Objects that are used to store state within another object are called **collaborator objects**, or more simply, **collaborators**. They work in conjunction -- in collaboration -- with the object to which they belong.

Collaborator objects let you chop up and modularize the problem domain into cohesive pieces; they are at the core of OO programming and play an important role in modeling complicated problem domains.

**Object Oriented**

We can go further still by placing the behaviors that go along with the data in the same object. The object-oriented approach to programming puts data and procedures that manipulate that data into containers for that data, i.e., objects. We no longer deal solely with primitives, or composites of primitives, but "smart" objects that can perform actions on the data they own. This way, we move complexity inside objects instead of exposing it globally. When we must make changes, we can restrict those changes to those objects; they don't ripple throughout the entire project. Maintenance is easier when we can limit the scope of changes.

Refactoring can reduce duplication in a project's code, including this simple project. It's important to understand though, that reducing duplication is not the goal — it's a side effect.

1. Objects:
   - organize related data and code together.
   - are useful when a program needs more than one instance of something.
   - become more useful as the codebase size increases.

- **Object-oriented programming** is a pattern that uses objects to organize code instead of Functions. Objects can also contain data or state.
- You can call the property of an object a `method` when the value you assign to it is a function.
- When you invoke an object's methods, they can access the object to which they belong by using `this`.
- Objects become more useful as projects become larger and more complicated.

https://launchschool.com/exercise_sets/ab289978



**First Class Functions** 

add them to objects, pass them to functions, run them in entirely different contexts.  First-class Functions initially have no context; they receive one when the program executes them.

**Global Object**

JavaScript creates a **global object** when it starts running, which serves as the implicit execution context. In the browser, the global object is the `window` object. 

In the previous course, we talked about *global values* such as `Infinity` and `NaN`, and *global functions*, such as `isNaN` and `parseInt`. All these global entities are properties of the global object! In your console, you can look at the `window` object to examine those properties.

As with other JavaScript objects, you can add properties to the global object at any time:

```js
window.foo = 1;
window.foo;       // 1
```

The global object is the implicit context when we evaluate expressions:

For example, in the code above, even though we aren't using the `let`, `var`, or `const` keyword, the code works since JavaScript gives `foo` an implicit evaluation context: the global object. Thus, the first line is the same as `window.foo = 1`, which assigns the property `foo` on the global object with a value of `1`.

On line 2, JavaScript finds `foo` in the implicit context of the global object. As a result, line 2 is identical to `window.foo`; it returns the value of the property `foo` from the global object.

```js
foo = 1;
foo;              // 1
```

When we declare **global** variables with `var` or functions, JavaScript adds them to the global object as properties. You can verify this by examining the `window` object:

```js
foo = 1;
var moreFoo = 3;
let evenMoreFoo = 42;

function bar() {
  return 1;
}

console.log(window.foo);           // 1
console.log(window.moreFoo);       // 3
console.log(window.evenMoreFoo);   // undefined
console.log(window.bar);           // function bar() { return 1; }
```

The behavior for the `var` variable appears to be identical to what happens when you don't declare the variable. There's a subtle but significant difference, however: you can delete global variables that you don't declare, but not those that you did.

foo = 1;
var moreFoo = 3;

function bar() {
  return 7;
}

delete window.foo;         // deleted
delete window.moreFoo;     // not deleted
delete window.bar;         // not deleted

console.log(window.bar());                         // 7
console.log(window.foo);                           // undefined
console.log(window.hasOwnProperty("foo"));         // false
console.log(window.moreFoo);                       // 3