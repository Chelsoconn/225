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

```js
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
```

**Node**

In non-browser JavaScript environments, such as Node, the global object is not `window`, but `global`.

Another thing of note, is that Node introduces an additional "module" scope. Variables in the module scope are those declared at the top level (not inside a function definition or object) of a node.js file. Consequently, module-scoped variables are not added to the global object, `global` (again `window` for browsers). Module-scoped variables are only accessible from within the file but not accessible anywhere else. Here's an example:

```js
// Use "node moduleScope.js" to run this code

var foo = 'foo';
// with `var`, the variable is in the module scope
// and not added to the global object

bar = 'bar';
// without `var` declaration, the variable is in the global scope
// and added to the global object

let qux = 'qux';
// with `let`, the variable is in the module scope
// and is not added to the global object

console.log(global.foo);    // => undefined
console.log(global.bar);    // => bar
console.log(global.qux);    // => undefined
console.log(qux);           // => qux
```

Even more mysteriously, none of the above variables are properties of `this`:

```js
// Use "node moduleThis.js" to run this code

var foo = 'foo';
bar = 'bar';
let qux = 'qux';

console.log(this.foo);    // => undefined
console.log(this.bar);    // => undefined
console.log(this.qux);    // => undefined
console.log(this);        // => {}
```

The reason for this odd behavior is that Node wraps files in this odd looking function:

```js
(function (exports, require, module, __filename, __dirname) {
   // your code is here
});
```

That means all of your variables and functions in Node programs have function scope. That plays a part later when we learn about execution context: the execution context for your top-level program in Node is the empty object, `{}`. That fact often surprises developers that are new to Node.

**Strict Mode**

In strict mode, the global object is not used as the implicit context. Instead, the implicit context is `undefined`:

```js
"use strict";

oneFoo = 1;        // Uncaught ReferenceError: oneFoo is not defined
oneFoo;
```

Thus, we cannot access variables that have not been declared. Among other things, it guards against misspellings adding new properties to the global object:

*With strict mode not enabled, what object serves as the implicit execution context? What happens when strict mode is enabled?*

Window. In strict mode, the global object is not accessible as the implicit execution context.



*What does the code below log?*

```js
a = 10;

console.log(window.a === a);
```

Initializing an undeclared variable, as we do on line 1 in the code above, automatically creates that variable as a property on `window` since the global object is the implicit context for evaluating expressions.





```js
"use strict"

a = 10;

console.log(window.a === a);
```

Nothing is logged as line 3 raises a `ReferenceError`.

In strict mode, using variables that have not been previously declared is not allowed. Since `a` was not previously declared, the assignment on line 3 raises an error.





```js
function func() {
  let b = 1;
}

func();

console.log(b);
```

It raises an error:  Uncaught ReferenceError: b is not defined.

This code throws a `ReferenceError`, because the variable *declaration* on line 2 (which is executed when `func` is invoked on line 5) occurs in function scope. Thus, it isn't a property on the global object and isn't accessible outside the function.





```js
function func() {
  b = 1;
}

func();

console.log(b);
```

1- Unlike in the previous problem, we don't *declare* `b`; rather, we simply initialize it. As a result, `b` is created as a property on the global object, despite the fact that it's initialized in function scope.



```js
"use strict"

function func() {
  b = 1;
}

func();

console.log(b);
```

Line 4 raises a `ReferenceError`. In strict mode, we don't have access to the global object as the implicit execution context. Thus, all variables have to be first declared before being used.



**Execution Context**

Every time a JavaScript function is invoked, it has access to an object called the **execution context** of that function. This execution context is accessible through the keyword `this`. A JavaScript function can be invoked in a variety of ways. Which object `this` refers to depends on how the function was invoked. In this and subsequent assignments, we'll talk about how the execution context of a function changes based on the how the function is invoked.

We will cover two types of execution contexts:

- Implicit: This is an execution context that JavaScript "implicitly" sets.
- Explicit: This is an execution context that you "explicitly" set.

It's important to remember that the rules for `this` are entirely different from the rules for variable scope. While a variable's scope is determined by where you write the code, `this`depends on how you invoke it (JavaScript does not use lexical scoping rules to determine the binding).



**Implicit Execution Context for Functions**

The implicit function execution context (also called implicit binding for functions) is the context for a function that you invoke without supplying an explicit context. JavaScript binds such functions to the global object.



```js
function foo() {
  return 'this here is: ' + this;
}

foo();                // "this here is: [object Window]"
```

```js
let object = {
  foo() {
    return 'this here is: ' + this;
  },
};

object.foo();              // "this here is: [object Object]"

let bar = object.foo;
bar();                     // "this here is: [object Window]"
```

This example shows that binding a function to a context object occurs **when you execute the function, not when you define it**. On line 9, we set `bar` to point to `object`'s `foo` method. When we call `bar`, JavaScript implicitly binds the method to the global object instead of `object`.

In strict mode, the implicit execution context is `undefined`. It's important to note, however, that `this` still points to the global object in the global scope.

```js
"use strict";

function foo() {
  console.log('this here is: ' + this);
}

foo();                // "this here is: undefined"
console.log('this here is: ' + this); // "this here is: [object Window]"
```





**Implicit Execution Context for Methods**

The implicit *method* execution context is the execution context for any method (i.e., function referenced as an object property) invoked without an explicit context provided. JavaScript implicitly binds methods invoked in this manner to the calling object:

```js
let foo = {
  bar() {
    return this;
  },
};

foo.bar() === foo; // true
```

It's important to reiterate here that implicit execution context is bound upon invocation, not definition. 

```js
let foo = {
  bar() {
    return this;
  },
};

let baz = foo.bar;

baz() === foo;    // false
baz() === window; // true
```

Although `bar` begins its life as an object property, when it is invoked as a function (as it is after being assigned to `baz`) its context is bound to the global object.

A possible source of confusion here is the use of the term "implicit." The confusion arises because of the way we invoke the function; in that, it has an "explicit" receiver or calling object. In this sense, it is not far-fetched to describe the type of execution context of a method invocation as "explicit." Contrast this to a function invocation, wherein the calling object (`window` when running JS code in the browser) is "implicit."

That said, whether the calling object is implicit (function invocation) or explicit (method invocation), the execution context is implicitly set for us by JavaScript, and in both cases it sets it to the calling object.

We'll get to appreciate better the distinction between an execution context that is set implicitly versus explicitly in the next section when we talk about the two methods we can use to set the execution context explicitly.



**Explicit Execution Context**

JavaScript lets us use the `call` and `apply` methods to change a function's execution context at execution time. That is, we can explicitly bind a function's execution context to an object when we execute the function.

```js
a = 1;

let object = {
  a: 'hello',
  b: 'world',
};

function foo() {
  return this.a;
}

foo();                  // 1 (context is global object)
foo.call(object);       // "hello" (context is object)
```

```js
let strings = {
  a: 'hello',
  b: 'world',
  foo() {
    return this.a + this.b;
  },
};

let numbers = {
  a: 1,
  b: 2,
};

strings.foo.call(numbers); // 3
```



**apply method**

Identical to `call` except it uses an array to pass arguments



The execution context is determined by how you invoke a function or method. We can't emphasize this enough. `this` is the way.







**Examples**

```js
var message = 'Hello from the global scope!';

function deliverMessage() {
  console.log(this.message);
}

deliverMessage();

let bar = {
  message: 'Hello from the function scope!',
};

bar.deliverMessage = deliverMessage;

bar.deliverMessage();

//Hello from the global scope!
//Hello from the function scope!
```

The first log operation is generated by the *function* call, `deliverMessage()` on line 7. Since this is a function invocation, the implicit function execution context is the global object, `Window`. That means that JavaScript looks in `Window` for the variable `message`.

The second log operation is generated by the *method* call `bar.deliverMessage()` on line 15. Since the implicit method execution context for a method invocation is the calling object, `this` resolves to `bar`, and `this.message` resolves to `bar`'s property `message`.

**Bind**

JavaScript also has a `bind` method that lets us bind a function to a context object permanently:

```js
let object = {
  a: 'hello',
  b: 'world',
  foo() {
    return this.a + ' ' + this.b;
  },
};

let bar = object.foo;
bar();                                // "undefined undefined"

let baz = object.foo.bind(object);
baz();                                // "hello world"

let object2 = {
  a: 'hi',
  b: 'there',
};

baz.call(object2);  // "hello world" - `this` is still `object`
```

Unlike `call` or `apply`, `bind` doesn't execute the function that invokes it. Instead, it creates and returns a new Function that permanently binds the invoking function to a given context object. Thus, we can pass the returned function around without concern that its context will change; it won't -- we will always be able to use it to call the original function with the desired context.

Bind`'s context is the original function, and it returns a new function that calls the original function with the context supplied to `bind` as its first argument. This code also shows why the binding makes permanent changes -- no matter what you do to the returned function, you can't change the value of `context.

A trap that students often fall into is the thinking that `bind` permanently alters the original function. It's important to remember that `bind` returns a new function, and that new function is permanently context-bound to the object provided as the first argument to `bind`. The original function isn't changed, but it's context is altered when it gets called.

```js
let greetings = {
  morning: 'Good morning, ',
  afternoon: 'Good afternoon, ',
  evening: 'Good evening, ',

  greeting(name) {
    let currentHour = (new Date()).getHours();

    if (currentHour < 12) {
      console.log(this.morning + name);
    } else if (currentHour < 18) {
      console.log(this.afternoon + name);
    } else {
      console.log(this.evening + name);
    }
  },
};

let spanishWords = {
  morning: 'Buenos dias, ',
  afternoon: 'Buenas tardes, ',
  evening: 'Buenas noches, ',
};

let spanishGreeter = greetings.greeting.bind(spanishWords);

spanishGreeter('Jose');
spanishGreeter('Juan');
```



*Examples*

What will the code below log to console?

```js
let obj = {
  message: 'JavaScript',
};

function foo() {
  console.log(this.message);
}

foo.bind(obj);
```

Nothing. Unlike `call` and `apply`, `bind` doesn't invoke the receiver function. Rather, it *returns a new function* that is permanently bound to the context argument.



```js
let obj = {
  a: 2,
  b: 3,
};

function foo() {
  return this.a + this.b;
}

let bar = foo.bind(obj);

console.log(bar());
```

5, `foo` is explicitly bound to `obj` on line 10, and as a result references that object's properties `a` and `b` when it is invoked.



```js
let positiveMentality = {
  message: 'JavaScript makes sense!',
};

let negativeMentality = {
  message: 'JavaScript makes no sense!',
};

function foo() {
  console.log(this.message);
}

let bar = foo.bind(positiveMentality);

negativeMentality.logMessage = bar;
negativeMentality.logMessage();
```

JavaScript makes sense! Since `bar` is bound to `positiveMentality` as the return value of the `bind` invocation on line 13, `positiveMentality`'s property `message` is logged by the function call on the last line, despite the fact that the function is invoked as a method on the `negativeMentality`object.



```js
let obj = {
  a: 'Amazebulous!',
};
let otherObj = {
  a: "That's not a real word!",
};

function foo() {
  console.log(this.a);
}

let bar = foo.bind(obj);

bar.call(otherObj);
```

Amazebulous! Once a function has been bound to an execution context with `bind`, its context can't be changed, even explicitly. In keeping with this, the last line of our code outputs `Amazebulous!`, because the function `bar` has been bound to `obj`. Thus, inside of `bar`, `this` points to `obj` when `bar` is invoked on the last line, rather than `otherObj`, despite the fact that the function is being invoked by `call` with `otherObj` as the explicit context argument.



**Method Losing Context when Taken out of Context**

If you remove a method from its containing object and execute it, it loses its context:

```js
let john = {
  firstName: 'John',
  lastName: 'Doe',
  greetings() {
    console.log('hello, ' + this.firstName + ' ' + this.lastName);
  },
};

let foo = john.greetings;
foo();

// => hello, undefined undefined
```

```js
function repeatThreeTimes(func) {
  func();       // can't do func.call(john), out of scope
  func();
  func();
}

function foo() {
  let john = {
    firstName: 'John',
    lastName: 'Doe',
    greetings() {
      console.log('hello, ' + this.firstName + ' ' + this.lastName);
    },
  };

  repeatThreeTimes(john.greetings); //
}

foo();

// => hello, undefined undefined
// => hello, undefined undefined
// => hello, undefined undefined
```

So here bc `greeting` was passed in, we have access to it but we don;t have access to the john object and it's properties 

You can also update the receiving function (`repeatThreeTimes`) by adding an extra parameter to the function and pass in the desired context:

```js
function repeatThreeTimes(func, context) {
  func.call(context);
  func.call(context);
  func.call(context);
}

function foo() {
  let john = {
    firstName: 'John',
    lastName: 'Doe',
    greetings() {
      console.log('hello, ' + this.firstName + ' ' + this.lastName);
    },
  };

  repeatThreeTimes(john.greetings, john);
}

foo();

// hello, John Doe
// hello, John Doe
// hello, John Doe
```



