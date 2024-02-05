**Methods**

A **method** is a function stored in a property of an object. For example:

`this` *is the* **object that owns the method** *in a method invocation*

- Functions with a *reciever*- 
  - the object the method is called on (method invocation)
  - *Function invocation* expression cannot be a [property accessor](https://web.archive.org/web/20180209163541/https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Property_accessors) `obj.myFunc()`, which creates a *method invocation*. For example `[1,5].join(',')` is **not** a function invocation, but a method call. 
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

Add them to objects, pass them to functions, run them in entirely different contexts.  First-class Functions initially have no context; they receive one when the program executes them.

**Global Object**

The global object is determined by the execution environment. In a browser, it is the [`window`](https://web.archive.org/web/20180209163541/https://developer.mozilla.org/en-US/docs/Web/API/Window) object.

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

Strict mode is enabled at the top of the scope- All embedded functions will also be in strict mode 

A single JavaScript file may contain both strict and non-strict modes. So it is possible to have different context behavior in a single script for the same invocation type:



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

**What is `this`?**

`this` is the current execution context of a function.

- **Context** of an invocation is the value of `this` within function body. For example the invocation of `map.set('key', 'value')` has the context `map`.

- **Scope** of a function is the set of variables, objects, functions accessible within a function body.

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



```js
// object definition
const personName = {
  firstName: "Taylor",
  lastName: "Jackson",
};

// function definition
function greet(wish, message) {
  return `${this.firstName}, ${wish}. ${message}`;
}

// calling greet() function by passing two arguments
let result = greet.apply(personName, ["Good morning", "How are you?"]);


console.log(result);

// Output:
// Taylor, Good morning. How are you?
```





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

**Indirect invocation** is performed when a function is called using `myFun.call()` or `myFun.apply()` methods.

​	From the [list of methods](https://web.archive.org/web/20180209163541/https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function#Methods) that a function object has, [`.call()`](https://web.archive.org/web/20180209163541/https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) and [`.apply()`](https://web.archive.org/web/20180209163541/https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) are used to invoke the function with a configurable context: 



If you remove a method from its containing object and execute it, it loses its context:  The indirect invocation is useful when a function should be executed with a specific context.

For example to solve the context problems with function invocation, where `this` is always `window` or `undefined` in strict mode (see [2.3.](https://web.archive.org/web/20180209163541/https://dmitripavlutin.com/gentle-explanation-of-this-in-javascript/#23pitfallthisinaninnerfunction)). It can be used to simulate a method call on an object (see the previous code sample).

 A common trap with the function invocation is thinking that `this` is the same in an inner function as in the outer function. 
 Correctly the context of the inner function depends only on invocation, but not on the outer function's context.

To have the expected `this`, modify the inner function's context with indirect invocation (using [`.call()`](https://web.archive.org/web/20180209163541/https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) or [`.apply()`](https://web.archive.org/web/20180209163541/https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply), see [5.](https://web.archive.org/web/20180209163541/https://dmitripavlutin.com/gentle-explanation-of-this-in-javascript/#5indirectinvocation)) or create a bound function (using [`.bind()`](https://web.archive.org/web/20180209163541/https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind), see [6.](https://web.archive.org/web/20180209163541/https://dmitripavlutin.com/gentle-explanation-of-this-in-javascript/#6boundfunction)).

- The method `.call(thisArg[, arg1[, arg2[, ...]]])` accepts the first argument `thisArg` as the context of the invocation and a list of arguments `arg1, arg2, ...` that are passed as arguments to the called function.
- The method `.apply(thisArg, [arg1, arg2, ...])` accepts the first argument `thisArg` as the context of the invocation and an [array-like object](https://web.archive.org/web/20180209163541/http://www.2ality.com/2013/05/quirk-array-like-objects.html) of values `[arg1, arg2, ...]` that are passed as arguments to the called function.

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

What happens, though, if you can't update the function or can't supply a context object? Then you need a different approach. **Hard binding** with `bind` often works:

**A bound function** is a function connected with an object.

The original and bound functions share the same code and scope, but different contexts on execution.

Contrary to `.apply()` and `.call()` methods (see [5.](https://web.archive.org/web/20180209163541/https://dmitripavlutin.com/gentle-explanation-of-this-in-javascript/#5indirectinvocation)), which invokes the function right away, the `.bind()` method only returns a new function that it supposed to be invoked later with a pre-configured `this`.  `this` *is the* **first argument** *of* `.bind()` *when invoking a bound function*

`.bind()` makes a **permanent context link** and will always keep it. A bound function cannot change its linked context when using `.call()` or `.apply()`with a different context, or even a rebound doesn't have any effect.

```js
function multiply(number) {  
  'use strict';
  return this * number;
}
// create a bound function with context
var double = multiply.bind(2);  
// invoke the bound function
double(3);  // => 6  
double(10); // => 20  
```



```js
function repeatThreeTimes(func) {
  func();
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

  repeatThreeTimes(john.greetings.bind(john));
}

foo();

// => hello, John Doe
// => hello, John Doe
// => hello, John Doe
```



**Internal Function Losing Method Context**

```js
let obj = {
  a: 'hello',
  b: 'world',
  foo() {
    function bar() {
      console.log(this.a + ' ' + this.b);
    }

    bar();
  },
};

obj.foo();
> undefined undefined
```

Even though `foo` executes within the `obj` context, the call to `bar` on line 9 does not provide an explicit context, which means that JavaScript binds the global object to the function.

One common approach is the `let self = this` or `let that = this` fix. You save `this` in a variable named `self` or `that` before calling the function, then reference the variable in the function.

```js
let obj = {
  a: 'hello',
  b: 'world',
  foo() {
    let self = this;

    function bar() {
      console.log(self.a + ' ' + self.b);
    }

    bar();
  }
};

obj.foo();
> hello world
```

or

```js
let obj = {
  a: 'hello',
  b: 'world',
  foo() {
    function bar() {
      console.log(this.a + ' ' + this.b);
    }

    bar.call(this);
  }
};

obj.foo();
> hello world
```

Or..

You can use `bind` when you define the function to provide a permanent context to `bar`. Note that you must call `bind` with a function expression, not a function declaration; using `bind` with a function declaration results in an error.

```js
let obj = {
  a: 'hello',
  b: 'world',
  foo() {
    let bar = function() {
      console.log(this.a + ' ' + this.b);
    }.bind(this);

    // some lines of code

    bar();

    // more lines of code

    bar();

    // ...
  }
};

obj.foo();
> hello world
> hello world
```

One advantage of using `bind` is that you can do it once and then call it as often as you want without explicitly providing a context.

Or..

We can define `bar` using an arrow function since the value of `this` when using an arrow function is the current value of `this` in the defining function, namely, the function where the arrow function is defined:

**Arrow function** is designed to declare the function in a shorter form and [lexically](https://web.archive.org/web/20180209163541/https://en.wikipedia.org/wiki/Scope_(computer_science)#Lexical_scoping) bind the context. `this` *is the* **enclosing context** *where the arrow function is defined* The arrow function doesn't create its own execution context, but takes `this`from the outer function where it is defined.



```js
let obj = {
  a: 'hello',
  b: 'world',
  foo() {
    let bar = () => console.log(this.a + ' ' + this.b);
    bar();
  }
};

obj.foo();
> hello world
```

Thus, in the above code, `this` inside the arrow function points to `obj`, which is the current execution context of `foo`.





This can also make it lose context..

```js
let obj = {
  a: 'hello',
  b: 'world',
  foo() {
    [1, 2, 3].forEach(function(number) {
      console.log(String(number) + ' ' + this.a + ' ' + this.b);
    });
  },
};

obj.foo();

// => 1 undefined undefined
// => 2 undefined undefined
// => 3 undefined undefined
```

It looks simple enough; the code loops over an array and logs some information to the console. The problem, though, is that `forEach` executes the anonymous function passed to it, so it gets executed with the global object as context. Once again, `this`has the wrong value, and the function doesn't do what we want.

so you can add 

`let self = this`

or 

```js
foo() {
    [1, 2, 3].forEach(function(number) {
      console.log(String(number) + ' ' + this.a + ' ' + this.b);
    }.bind(this));
```

or

```js
foo() {
    [1, 2, 3].forEach(function(number) {
      console.log(String(number) + ' ' + this.a + ' ' + this.b);
    }, this);
  },
```

Some methods that take function arguments allow an optional argument that defines the context to use when executing the function. `Array.prototype.forEach`, for instance, has an optional `thisArg` argument for the context. This argument makes it easy to work around this context loss problem.  `Array.prototype.map`, `Array.prototype.every`, and `Array.prototype.some` also takes an optional `thisArg` argument.

or

The `this` binding for arrow functions is not dependent on how the function is invoked. Instead, JavaScript resolves it by looking at the execution context of the surrounding environment at the time the arrow function was defined.

```js
foo() {
    [1, 2, 3].forEach((number) => {
      console.log(String(number) + ' ' + this.a + ' ' + this.b);
    });
  },
```

Note that this is not the same as saying that arrow functions get their context lexically. "Lexical" refers to the structure of the surrounding code without regard to its execution state; arrow functions rely on the execution state at the time of definition to determine their execution context.

Many students mistakenly think that arrow functions determine their context lexically. It certainly looks that way at first glance. However, **arrow functions do not get their context lexically.** Consider this code:

```js
let obj1 = {
  a: 'This is obj1',

  foo() {
    let bar = () => console.log(this.a);
    bar();
  },
};

let obj2 = {
  a: 'This is obj2',
};

obj1.foo();                   // This is obj1

obj2.foo = obj1.foo;
obj2.foo();                   // This is obj2
```

If arrow functions determined their context lexically, both `obj1.foo()` and `obj2.foo()` would print `This is obj1`. However, since the context is determined based on the context at the time the function is created, `obj2.foo()` prints `This is obj2`.

**Examples**

```js
let TESgames = {
  titles: ['Arena', 'Daggerfall', 'Morrowind', 'Oblivion', 'Skyrim'],
  seriesTitle: 'The Elder Scrolls',
  listGames() {
    this.titles.forEach(function(title) {
      console.log(this.seriesTitle + ' ' + title);
    });
  }
};

TESgames.listGames();
/*
undefined Arena
undefined Daggerfall
undefined Morrowind
undefined Oblivion
undefined Skyrim
*/
```

This happens because functions as arguments lose the surrounding context. In this case, the function expression invoked on each iteration of `forEach` inside of `listGames` loses `TESgames` as context. As a result, `this` on line 6 references the global object, and resolves to `undefined` rather than `"The Elder Scrolls"`.





**When is the execution context (`this`) set?**

At the time of function invocation 



**Using `call`**

  `calculate.call(this)` (an indirect invocation of a function,





**Examples**

1) What does `this` point to in the code below?

   ```js
   function whatIsMyContext() {
     return this;
   }
   ```

   We won't know the context of a function until execution time. Thus, we don't know what the context is here.

2. ```js
   function whatIsMyContext() {
     return this;
   }
   
   whatIsMyContext();
   ```

   Function calls set the execution context to the **implicit global object**, or global context for short. When we use the global object implicitly to call a function, we call it with the **global context**.

   Functions called inside a browser environment use the `window` object as the implicit global context, so `this` is the `window` object inside the function.

   In strict mode, however, the global context is the value `undefined`.

3. ```js
   function foo() {
     function bar() {
       function baz() {
         console.log(this);
       }
   
       baz();
     }
   
     bar();
   }
   
   foo();
   ```

   Since we call `baz` with the implicit global context, `this` is the `window` object.

5. ```js
   let obj = {
     count: 2,
     method() {
       return this.count;
     },
   };
   
   obj.method();
   ```

   Since we call `method` on object `obj`, `this` is the object `obj`. Thus, the return value will be `2`.

   

6. In strict mode, what does the following program log to the console?

   ```js
   function foo() {
     console.log(this.a);
   }
   
   let a = 2;
   foo();
   ```

   It raises an error. Since the function is called without an explicit object, `this` inside `foo`resolves to the global execution context, which is `undefined` in strict mode. The code raises an error because, in effect, we're trying to access the nonexistent `a` property of `undefined`.

7. ```js
   let a = 1;
   function bar() {
     console.log(this.a);
   }
   
   let obj = {
     a: 2,
     foo: bar,
   };
   
   obj.foo();
   ```

   2- Line 11 calls method `foo` with its context set to `obj` since the execution context for any method invoked without an explicit context provided is the calling object.

   

8. ```js
   let foo = {
     a: 1,
     bar() {
       console.log(this.baz());
     },
   
     baz() {
       return this;
     },
   };
   
   foo.bar();
   let qux = foo.bar;
   qux();
   ```

   Executing from line 12 sets `this` in `bar` to the `foo` object (via implicit method execution context). From the body of the `bar` method, it then calls `foo`'s `baz` method. `baz` returns `foo` since it has `foo`'s context (again via implicit method execution context; `this` === `foo`following the execution from line 12), which causes line 4 to log `foo`.

   Line 14 calls `bar` as a function in the global context, `window`; since `baz` doesn't exist in `window`, JavaScript raises an error.

   

9. ```js
   let myObject = {
     count: 1,
     myChildObject: {
       myMethod() {
         return this.count;
       },
     },
   };
   
   myObject.myChildObject.myMethod();
   ```

   `this` is `myChildObject`, which means `this.count` is `undefined`, so the return value is `undefined`.



10. ```js
    let person = {
      firstName: 'Peter',
      lastName: 'Parker',
      fullName() {
        console.log(this.firstName + ' ' + this.lastName +
                    ' is the Amazing Spiderman!');
      },
    };
    
    let whoIsSpiderman = person.fullName.bind(person);
    whoIsSpiderman();
    //Peter Parker is the Amazing Spiderman!
    ```

    The `bind` method returns a new function that permanently binds `this` in the function assigned to `fullName` to the `person` object itself.

11. ```js
    let computer = {
      price: 30000,
      shipping: 2000,
      total() {
        let tax = 3000;
        function specialDiscount() {
          if (this.price > 20000) {
            return 1000;
          } else {
            return 0;
          }
        }
    
        return this.price + this.shipping + tax - specialDiscount();
      }
    };
    
    console.log(computer.total());
    ```

    1. Use self = this 

    2. Bind specialDiscount using `.bind(this)`
    3. Use call or apply when invoking specialDiscount 
    4. Use Arrow function for specialDiscount

    # Summary

    - Function invocations (e.g., `parseInt(numberString)`) rely upon implicit execution context that resolves to the global object. Method invocations (e.g., `array.forEach(processElement)`) rely upon implicit context that resolves to the object that holds the method.
    - All JavaScript code executes within a context. The top-level context in a web browser is the `window`object. All global methods and Objects (such as `parseInt` or `Math`) are properties of this object. In Node, the top-level context is called `global`. However, be aware that Node has some peculiarities that cause it to behave differently from browsers.
    - You can't use `delete` to delete variables and functions declared at the global scope.
    - `this` is the current execution context of a function.
    - The value of `this` changes based on **how you invoke a function**, not **how you define it**.
    - In strict mode, `this` inside functions resolves to `undefined` when referring to the global execution context.
    - JavaScript has **first-class functions** which have the following characteristics:
      - You can add them to objects and execute them in the respective object's contexts.
      - You can remove them from their objects, pass them around, and execute them in entirely different contexts.
      - They're initially unbound, but are dynamically bound to a context object at execution time.
    - `call` and `apply` invoke a function with an explicit execution context.
    - `bind` permanently binds a function to a context and returns a new function.
    - Method invocations can operate on the data of the calling object.

    

**Higher order functions**

**Higher-order functions** can accept a function as an argument, return a function when invoked, or both. In other words, higher-order functions work with other functions.

To understand this concept, you must think of JavaScript functions as *values*; functions are objects. We know that they can take values as input and return a value as output. Thus, a higher-order function is one where either an input or output value is a function.

**Examples**

1) What are the characteristics that define higher-order functions?

   A higher-order function must either:

   1. Take a function as an argument,
   2. return a function,
   3. or Both.

2) ```js
   let numbers = [1, 2, 3, 4];
   function checkEven(number) {
     return number % 2 === 0;
   }
   
   numbers.filter(checkEven); // [2, 4]
   ```

   Of the two functions invoked (`checkEven` and `filter`), which is a higher-order function and why?

   `filter` is a higher-order function because it takes a function — `checkEven` — as an argument. `checkEven` is not a higher-order function since it doesn't take any function as an argument and doesn't return a function.

3. Implement `makeCheckEven` below, such that the last line of the code returns an array `[2, 4]`.

   ```js
   let numbers = [1, 2, 3, 4];
   function makeCheckEven() {
     // ... implement this function
   }
   
   let checkEven = makeCheckEven();
   
   numbers.filter(checkEven); // [2, 4]
   ```

   ```js
   function makeCheckEven() {
     return function(number) {
       return number % 2 === 0;
     };
   }
   ```

   

   4. 

   ```js
   
   function makeListTransformer(func) {
     // ... implement this function
   }
   
   let timesTwo = makeListTransformer(function(number) {
     return number * 2;
   });
   
   timesTwo([1, 2, 3, 4]); // [2, 4, 6, 8]
   ```

```js
function makeListTransformer(func) {
  return function(collection) {
    return collection.map(func);
  };
}
```





**Closures and Private Data**

Functions *close over* or *enclose* the lexical environment at their definition point, so we call them **closures**.  They always have access to what they close over, regardless of when and where the program invokes the function. You can think of closure as a function combined with any variables from its lexical scope that the function needs.

```js
function startup() {
  let status = 'ready';
  return function() {
    console.log('The system is ready.');
  };
}

let ready = startup();
let systemStatus = // ?
```

How can you set the value of `systemStatus` to the value of the inner variable `status` without changing `startup` in any way?

You can't do this. **There is no way to access the value of the variable from outside the function.** `status` is only defined in the body of the `startup` function.

As shown above, you have choices about how to organize your code and data. Using closures to restrict data access is a good way to force other developers to use the intended interface. By keeping the collection of items private, we enforce access via the provided methods. This restriction helps protect data integrity since developers must use the defined interface

so instead of being able to add a method and delete a property or access a property, use a closure and make sure that you can use a closure and dont include the list in the object

```js
let list = makeList();
list.clear = function() {
  this.items = [];
  console.log('all items deleted!');
};

instead 
let list = makeList();
list.clear = function() {
  // there is no way to access the items from within this method
  // because the closure that contains them is inaccessible

  items;                  // throws ReferenceError!
  this.items;             // undefined
};

//to add this method now we must update the original definition of makeList

function makeList() {
  let items = [];

  return {
    add(item) {
      let index = items.indexOf(item);
      if (index === -1) {
        items.push(item);
        console.log(item + ' added!');
      }
    },

    clear() {
      items = [];
      console.log('all items deleted!');
    },

    list() {
      items.forEach(function(item) {
        console.log(item);
      });
    },

    remove(item) {
      let index = items.indexOf(item);
      if (index !== -1) {
        items.splice(index, 1);
        console.log(item + ' removed!');
      }
    }
  };
}
```



**Garbage collector**

In JavaScript, values are allocated memory when they are created, and they are eventually, "automatically" freed up when they are not in use. This process of "automatically" freeing memory up is called **garbage-collection**. (GC)

Of course, JavaScript allocates memory for us and has GC, so this intricate dance of claim/test/copy/use/release isn't necessary. You can write the above code in a simpler, familiar way:

Notice that we don't need code to claim and release memory; the JavaScript runtime handles that for us. It gets memory from the system when we create new objects and primitive values and releases it when the program has no more references to them. That is, when there are no variables, objects, or closures that maintain a reference to the object or value, JavaScript marks the memory as eligible for GC. This concept is important. As long as the object or value remains accessible, JavaScript can't, and won't, garbage collect it.

```js
function logName() {
  let name = 'Sarah'; // Declare a variable and set its value. The JavaScript
                      // runtime automatically allocates the memory.
  console.log(name);  // Do something with name
}

logName();
// "Sarah" is now eligible for GC
// more code below this line
```

```js
function logName() {
  let name = 'Sarah'; // Declare a variable and set its value. The JavaScript
                      // runtime automatically allocates the memory.
  console.log(name);  // Do something with name
  return name;        // Returns "Sarah" to caller
}

let loggedName = logName(); // loggedName variable is assigned the value "Sarah"
                            // the "Sarah" assigned to `loggedName` is NOT eligible for GC
                            // the "Sarah" assigned to `name` IS eligible for GC
// more code below this line
```

```js
function logName() {
  let name = {
    firstName: 'Sarah'          // Declare a variable and set its value. The JavaScript
  };                            // runtime automatically allocates the memory.

  console.log(name.firstName);  // Do something with name
  return name;                  // Returns the `name` object to caller
}

let loggedName = logName();     // loggedName variable is assigned the value stored in name
                                // which is a reference to the object literal { firstName: "Sarah" }
                                // The value returned is NOT eligible for GC.
                                // This value is the same value that is assigned to name
// more code below this line
```

**Stack and the Heap**

JavaScript stores most primitive values as well as references on the stack, and everything else on the heap. You can think of references as pointers to the actual value of an object, array, or string that lives in the heap.

The stack doesn't participate in garbage collection. That means that **primitive values don't get involved in garbage collection when they are stored on the stack**. When a function or block begins executing in a JavaScript program, JavaScript allocates memory on the stack for the variables defined in that block or function. Since each item has fixed size, JavaScript can calculate the amount of memory it needs during the creation phase of execution without knowing the specific values. That means it can determine how much stack space it needs when hoisting occurs. When the block or function is done running, the allocated stack memory gets returned to the system automatically. This process is somewhat similar to garbage collection, but it is considered distinct.

Some primitive values can't be stored on the stack. Stack values typically must have a fixed size (say, 64 bits). Values that don't fit in that size must be stored elsewhere, typically the heap. Strings and BigInts, for instance, usually can't be stored in 64 bits, so they get placed in the heap or somewhere else. However, exactly where these values get stored is an implementation detail. As far as you, as a programmer, are concerned, they act like they are on the stack, so that's where they are. Since they act like they are on the stack, they probably don't participate in garbage collection, but, again, that is an implementation detail.

The heap is much trickier to deal with since each value has a different size that can't be determined ahead of time. Instead, new values must be added to the heap when they get created. Since the program can retain references to the values on the heap, it can't use the same allocate and release scheme used with the stack. Instead, it needs to rely on garbage collection to detect when a value's reference count reaches 0.

Garbage collection can occur at any time; it often occurs at periodic intervals during a program's lifetime. In particular, the programmer usually has no control over when GC occurs

All the garbage collector must do is look for and deallocate values that are eligible for garbage collection. If it uses a reference counting system, it needs to look for values with a reference count of 0.

Note that GC doesn't happen when a variable goes out of scope. That's a common misconception. A variable can go out of scope, but there can be many other references. Closures, arrays, and objects are a significant source of persistent references.



```js
function go() {
  let foo = {};
  let bar = { qux: foo };
  foo.xyz = bar;
}

go();
// Neither `foo` nor `bar` is eligible for GC
```

In this code, we create two objects in `go` that we assign to the `foo` and `bar` local variables. Furthermore, `bar` holds a reference to `foo`in its `qux` property, while `foo` holds a reference to `bar` in its `xyz` property. Both objects have reference counts of 2: one reference is to the variable to which each object is assigned, and the other reference is in a property of the other object. When we exit the `go` function, the reference counts of both objects get decremented by 1 since both `foo` and `bar` have gone out of scope, so they no longer hold references to the objects. However, the two objects still exist and are not eligible for garbage collection since they still reference each other. These two objects will never go away until the program ends.

[Memory Management](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)

**How Closures Affect Garbabe Collection**

When you create a closure, it stores a reference to all variables it can access. Each of those variables references an object or primitive value. Theoretically, as long as the closure exists, those variables remain in existence, which means that the objects or values they reference must also endure. For this reason, JavaScript can't garbage collect any objects referenced by the variables that the closure keeps in its context.

```js
function makeArrayLogger(arr) {
  return function() {
    console.log(arr);
  };
}

let logger = makeArrayLogger([1, 2, 3]);
```

After the above code runs, `logger` references a function that closed over the local variable `arr`, which right now contains the array `[1, 2, 3]`. Since the closure must keep `arr` around, the reference to `[1, 2, 3]` must also stick around, which means that JavaScript can't garbage collect `[1, 2, 3]`. When we call `logger` some time later, it still has access to `arr` and can log its value.

Before JavaScript can garbage collect `[1, 2, 3]`, you must ensure that nothing else in the program references it; that includes every closure that has access to the `[1, 2, 3]` array. That's not typically a concern, but if you find that you must remove a closure or other reference explicitly, you can assign `null` to the item that references it. For instance:

`logger = null;`

That dereferences the closure shown above, which in turn dereferences `[1, 2, 3]` through the `arr` variable. If nothing else has a reference to `[1, 2, 3]`, JavaScript is free to garbage collect it.



Examples

In the following code, when can JavaScript garbage collect each of the following arrays? `[1]`, `[2]`, and `[1, 2]`.

1) ```js
   let a = [1];
   
   function add(b) {
     a = a.concat(b);
   }
   
   function run() {
     let c = [2];
     let d = add(c);
   }
   
   run();
   ```

   We can GC `[1]` after line 4 executes. Since this line reassigns the `a` variable, `a` no longer references `[1]`, nor do any other variables in this program.

   We can GC `[2]` after the `run` function returns. Since `[2]` is only assigned to the `c` variable, `[2]` is no longer needed after `run` returns.

   We can GC `[1, 2]` only after the program ends. Since `a` is a global variable, the reference to `[1, 2]` doesn't go away until JS discards the `a` variable, and that only occurs when the program terminates.

   

2) In the following code, when can JavaScript garbage collect the value `["Steve", "Edie"]`?

```js
function makeHello(names) {
  return function() {
    console.log("Hello, " + names[0] + " and " + names[1] + "!");
  };
}

let helloSteveAndEdie = makeHello(["Steve", "Edie"]);
```

`["Steve", "Edie"]` is eligible for GC when the program finishes; specifically, after JavaScript garbage collects the function referenced by `helloSteveAndEdie`.



3. Is JavaScript a garbage-collected language, and if so, what does this entail?

   JavaScript is a garbage-collected language. This means that the JS runtime, rather than the developer, handles memory deallocation.

4. ```js
   let myNum = 1;
   
   function foo() {
     let myArr = ['this is an array'];
     // what is eligible for GC here?
   }
   
   foo();
   
   // what is eligible for GC here?
   
   // more code
   ```

   *Are either of the values `1` or `['this is an array']` eligible for garbage collection on line 5? What about on line 10?*

   Neither value is eligible for garbage collection on line 5. Since variables that contain numbers are stored on the stack, the value `1` is never eligible for GC. The array is also not eligible for GC since it is still referenced by the `myArr` variable.

   `['this is an array']` is eligible for garbage collection on line 10. Since `myArr` is function-scoped, the variable's reference to `['this is an array']` is broken after the function finishes running.

5. ```js
   function makeGreeting() {
     let foo = { greeting: 'hello' };
     return function(name) {
       foo.name = name;
       return foo;
     };
   }
   
   let greeting = makeGreeting();
   
   // is the object eligible for GC here?
   
   // more code
   ```

   No, it is not. The closure created on the function returned by `makeGreeting` on line 9 prevents the object from being garbage collected.

6. ```js
   let bash = {};
   //Will the object {} ever be eligible for garbage collection?
   ```

   Yes. After the script finishes running the object will be eligible for garbage collection.



**Partial Function Application**

Partial function application uses a function (we'll call it the *generator*) that creates a new function (the *applicator*) to call a third function (the *primary*). 

The generator receives some of the primary's arguments, while the applicator receives the rest when it gets invoked. The applicator then calls the primary and passes it all of its arguments, both those passed to the generator and those passed to the applicator.

```js
function primaryFunction(arg1, arg2) {
  console.log(arg1);
  console.log(arg2);
}

function generatorFunction(arg1) { //gernerator
  return function(arg2) { // applicator
    return primaryFunction(arg1, arg2);
  }
}

let applicatorFunction = generatorFunction('Hello');
applicatorFunction(37.2);   // Performs primaryFunction('Hello', 37.2);
// => Hello
// => 37.2
```

So the primary function is the one that does not utilize any other function 

The generator provides an argument the second function uses by use of a closure 

The applicator function provides the argument that will be passed to the generator 

```js
function makeAddN(addend) {                 // generator
  // Saves addend via closure; uses addend when function invoked.
  return function(number) {                 // applicator
    return add(addend, number);             // call primary
  }
}

let add1 = makeAddN(1);
add1(1);                           // 2
add1(41);                          // 42

let add9 = makeAddN(9);
add9(1);                           // 10
add9(9);                           // 18
```

Note how the closure lets us "carry around" a reference to the variable `addend`, which is a local variable in `makeAddN`. Closure allows the applicator to use the value of `addend` later, long after the local `makeAddN` finishes running. In effect, closure lets us define private data that persists for the function's lifetime. This technique is useful when we need to **package both data and functionality together**. We can pass around functions like this and not lose the private data.

Another approach produces a more flexible solution. We can create a general purpose generator function that partially applies a single argument to any two-argument primary:

```js
function add(first, second) {
  return first + second;
}

function repeat(count, string) {
  let result = '';
  let i;
  for (i = 0; i < count; i += 1) {
    result += string;
  }

  return result;
}

function partial(primary, arg1) {
  return function(arg2) {
    return primary(arg1, arg2);
  };
}

> let add1 = partial(add, 1);
> add1(2);
= 3
> add1(6);
= 7
> let threeTimes = partial(repeat, 3);
> threeTimes('Hello');
= HelloHelloHello
> threeTimes('!');
= !!!
```

Closures have a lot of similarities to objects. They both provide the means to organize code into data and a chunk of behavior that relies on that data. We'll discuss this relationship in later courses.

**Use Function.prototype.bind for Partial Function Application**

```js
// we use `null` since the function doesn't depend on `this`
> let add1 = add.bind(null, 1);
> add1(2);
= 3
> add1(6);
= 7
> let threeTimes = repeat.bind(null, 3);
> threeTimes('Hello');
= HelloHelloHello
> threeTimes('!');
= !!!
```



**This is a great way to use the partial function**

```js
function partial(primary, arg1) {
  return function(arg2) {
    return primary(arg1, arg2);
  };
}
```

So we have this primary function that takes two arguments. We then make a generator function that takes two arguments, one being a method and the other being the second argument we will pass to the primary. We return an anonymous function that takes one argument that will be the first argument we pass to the primary function. The anonymous function returns the primary function with the first argument passed in from the argument passed into the applicator and the second argument provided through a closure from the generator's second arguement.

ex/ *In our previous solution, `multiplyBy5` retains access to `func` and `b` long after `makePartialFunc` has finished execution. What makes this possible?*

This behavior is made possible by closures. When a new function is created, it retains access to any variables that are defined in the function's outer scope.



**Immediately Invoked Function Expressions (IIFE)**

Function that we define and invoke simultaneously

```js
(function() {
  console.log('hello');
})();                     // => hello

or 

(function() {
  console.log('hello');
}());

or

(function(a) {
  return a + 1;
})(2);                    // 3

or

let foo = function() {
  return function() {
    return 10;
  }();
}();

//We can omit the parentheses around an IIFE when the function definition is an expression that doesn't occur at the beginning of a line (recall the earlier invalid syntax example):
console.log(foo);       // => 10

//As with the original style, they let JavaScript distinguish between an ordinary function declaration and an IIFE
```

In JavaScript, surrounding a value with parentheses `( )` doesn't change the value. It is a grouping operator that controls the evaluation in expressions.

 With IIFEs, we use the parentheses to make it explicit that we want to parse the function definition as an expression. As an expression it means that there is a value returned — the function — that we can immediately "invoke."

So say we had a huge messy script and we needed to add a new object literal, well our variable name could have already been used (which would throw and error). So we would have to put it in a IIFE bc functions create their own scope. 

```js 
// thousands of lines of messy JavaScript code!
(function() {
  var myPet = {
    type: 'Dog',
    name: 'Spot',
  };

  console.log(`I have pet ${myPet.type} named ${myPet.name}`);
})();

// more messy JavaScript code

//or
// thousands of lines of messy JavaScript code!

(function(type, name) {
  var myPet = {
    type,
    name,
  };

  console.log(`I have pet ${myPet.type} named ${myPet.name}`);
})('Dog', 'Spot');

// more messy JavaScript code

```

It has a private scope for `myPet`, and it won't clash with any other functions.

With an IIFE we can make functions and objects that have access to private data.

So lets use this example to show that the studentId number is kept private and also self accumulating 

```js
let generateStudentId = (function() {
  let studentId = 0;

  return function() {
    studentId += 1;
    return studentId;
  };
})();
```

```js
let inventory = (function() {
  let stocks = [];

  return {
    stockCounts() {
      stocks.forEach(function(stock) {
        console.log(stock.name + ': ' + String(stock.count));
      });
    },
    addStock(newStock) {
      let isValid = stocks.every(function(stock) {
        return newStock.name !== stock.name;
      });

      if (isValid) { stocks.push(newStock) }
    },
  };
})();
//With this, the stocks list is private. Consequently, what is displayed by the stockCounts method will only be influenced by what is added through the addStock method.
```

As a final refactor, we can extract the function for validating that the new stock to be added has a unique name to a private function as well. This will clean up the `addStock` method and make the logic clearer to readers:

```js
let inventory = (function() {
  let stocks = [];
  function isValid(newStock) {
    return stocks.every(function(stock) {
      return newStock.name !== stock.name;
    });
  }

  return {
    stockCounts() {
      stocks.forEach(function(stock) {
        console.log(stock.name + ': ' + String(stock.count));
      });
    },
    addStock(newStock) {
      if (isValid(newStock)) { stocks.push(newStock) }
    },
  };
})();
```

**Summary**

- Closures let a function access any variable that was in scope when the function was defined.
- Objects and arrays that are no longer accessible from anywhere in the code are eligible for **garbage collection**, which frees up the memory that they use for reuse by other parts of the program.
- You can use closures to make variables "private" to a function, or set of functions, and inaccessible elsewhere.
- Closures allow functions to "carry around" values for later use.
- **Higher-order functions** are functions that take a function as an argument, return a function, or both.
- https://launchschool.com/exercise_sets/50a9820b





Examples: 

What is this?

1) ```js
   const person = {
     firstName: 'Rick ',
     lastName: 'Sanchez',
     fullName: this.firstName + this.lastName,
   };
   
   console.log(person.fullName);
   ```

   If you said it logs `NaN`, you're correct. It is tempting to say that the code will log "Rick Sanchez" to the console but that's not correct.

   Anywhere outside a function, the keyword `this` is bound to the global object. If the keyword is used inside a function, then its value depends on how the function was invoked.

   Since `window.firstName` and `window.lastName` (if you're using a browser) are not defined, the operation being performed here is `undefined + undefined` which results in fullName having the value `NaN`.

   ```js
   const person2 = {
     firstName: 'Rick ',
     lastName: 'Sanchez',
     fullName() { return this.firstName + this.lastName; },
   };
   
   console.log(person.fullName()); // 'Rick Sanchez'
   ```

   2. ```js
      const franchise = {
        name: 'How to Train Your Dragon',
        allMovies() {
          return [1, 2, 3].map(function(number) {
            return `${this.name} ${number}`;
          });
        },
      };
      ```

      The current implementation will not work because `this` will be bound to the wrong object (`window`) when the anonymous function passed to `map` is invoked. We want to access the object `franchise` from within that anonymous function.

      There are multiple ways to solve this problem. Both of the solutions below take advantage of JavaScript's lexical scoping rules, but in different ways. Use self = this or an arrow function. 

      or use hard binding 

      ```js
      const franchise = {
        name: 'How to Train Your Dragon',
        allMovies() {
          return [1, 2, 3].map(function(number) {
            return `${this.name} ${number}`;
          }.bind(this));
        },
      };
      
      ```

      

3. [Function.prototype.bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) is a method on all function objects that allows us to hard-bind a function to a particular object. The way this works is that you pass a context object to the `bind` method and it returns a new function that is essentially the same function but hard-bound to the context object supplied.

   Create a function `myBind`, that accepts two arguments: 1) The function to bind, 2) The context object, and returns a new function that's hard-bound to the passed in context object.

   ```js
   function myBind(func, ctx) {
     return function() {
       return func.apply(ctx);
     }
   }
   ```

   

The above solutions leverages `Function.prototype.apply` and the concept of closures to return a bound function. `myBind` receives a function and a context object as arguments. Then it returns a new function, which, when called, will call the original function using `apply`.

*Improving myBind()*

Our earlier implementation of the `Function.prototype.bind` was simplistic. `Function.prototype.bind` has another trick up its sleeve besides hard-binding functions to context objects. It's called partial function application. Read [this assignment](https://launchschool.com/lessons/0b371359/assignments/f2c6f687) and the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind#Partially_applied_functions) to learn more about partial function application.

Alter the `myBind` function written in the previous exercise to support partial function application of additional arguments to the original function.

4. 

   ```js
   function myBind(func, ctx, ...partialArgs) {
     return function(...args) {
       const fullArgs = partialArgs.concat(args);
   
       return func.apply(ctx, fullArgs);
     };
   }
   ```

   



**Object.prototype**

JS uses object prototype to share behavior, not class inheritance.  

https://www.youtube.com/watch?v=-N9tBvlO9Bo







**Constructor Pattern**

**new**

```js
// constructor function
function Person(firstName, lastName = '') {
  this.firstName = firstName;
  this.lastName = lastName;
  this.fullName = function() {
    return (this.firstName + ' ' + this.lastName).trim();
  };
}

let john = new Person('John', 'Doe');
let jane = new Person('Jane');

john.fullName();              // "John Doe"
jane.fullName();              // "Jane"

john.constructor;             // function Person(..)
jane.constructor;             // function Person(..)

john instanceof Person;       // true
jane instanceof Person;       // true
```

In this example, the `Person` function is a constructor function that we use to create objects. The reason that we say it's a constructor function is that **it's intended to be called with the `new`operator**; otherwise, it's just a regular JavaScript function. The fact that we capitalized the function's name is not a syntactical requirement, but a convention to reveal the intention that we should only use the function to construct objects.

In this case, the `this` in the function points to the global object, and we've defined properties and functions on the global object itself!

```js
function Person(firstName, lastName = '') {
  this.firstName = firstName;
  this.lastName = lastName;
  this.fullName = function() {
    return (this.firstName + ' ' + this.lastName).trim();
  };
}

Person('John', 'Doe');
window.fullName();          // "John Doe"
```

When we call a function with the `new` operator, the following happens:

1. A new object is created.
2. `this` in the function is set to point to the new object.
3. The code in the function is executed.
4. `this` is returned if the constructor doesn't explicitly return an object.



`this` *is the* **newly created object** *in a constructor invocation*

When a property accessor `myObject.myFunction` is preceded by `new`keyword, JavaScript will execute a **constructor invocation**, but **not** a **method invocation**. 

The context of a constructor invocation is the newly created object. It is used to initialize the object with data that comes from constructor function arguments, setup initial value for properties, attach event handlers, etc.

A constructor call creates an empty new object, which inherits properties from constructor's prototype. The role of constructor function is to initialize the object. 
As you might know already, the context in this type of call is the created instance.

**Object.create**

Every JavaScript Object has a special hidden property called `[[Prototype]]`. Yes, the square brackets are indeed part of its name. We can retrieve and set this property's value with the `Object.getPrototypeOf` and `Object.setPrototypeOf` methods. When we use `Object.create` to create an object, it sets the `[[Prototype]]` property of the created object to the passed-in object:

so when we create a new object a property called `[[Prototype]]` gets created and it is set to the object that is passed in 

so 

```js
let foo = {};
let qux = Object.create(foo);
console.log(Object.getPrototypeOf(qux) === foo); // true
```

In this case, we say that the object assigned to `foo` is the **prototype object** of the object returned by `Object.create` and assigned to `qux`. We could also say that we created the object `qux` with object `foo` as its prototype.

```js
let foo = {};
let qux = Object.create(foo);
console.log(Object.getPrototypeOf(qux) === foo); // true
console.log(foo.isPrototypeOf(qux));             // true

//you can set it manually but its slow 
Object.setPrototypeOf(qux, bar);
console.log(Object.getPrototypeOf(qux) === bar); // true
console.log(bar.isPrototypeOf(qux));             // true
```

**Chaining using `create`**

```js
let foo = {
  a: 1,
  b: 2,
};

let bar = Object.create(foo);
let baz = Object.create(bar);
let qux = Object.create(baz);

Object.getPrototypeOf(qux) === baz;       // true
Object.getPrototypeOf(baz) === bar;       // true
Object.getPrototypeOf(bar) === foo;       // true

foo.isPrototypeOf(qux);                   // true - because foo is on qux's prototype chain
```

The `Object.prototype` object is at the end of the prototype chain for all JavaScript objects. If you don't create an object from a prototype, its prototype is the `Object.prototype` object:

````js
let foo = {
  a: 1,
  b: 2,
};                                // created with object literal

Object.getPrototypeOf(foo) === Object.prototype;      // true
````

**Prototype Chain Lookup for Property Access**

JavaScript's prototype chain lookup for properties gives us the ability to store an object's data and behaviors not just in the object itself, but anywhere on its prototype chain. This is very powerful when we want to share data or behaviors:

- We can create dogs much more easily with the `dog` prototype, and don't have to duplicate `say` and `run` on every single dog object.
- If we need to add/remove/update behavior to apply to all dogs, we can just modify the prototype object, and all dogs will pick up the changed behavior automatically.

Some people call this pattern JavaScript's **Prototypal Inheritance**. The word "inheritance" comes from the classical object oriented programming languages 

From a top down / design time point of view, the objects on the bottom of the prototype chain "inherited" the properties and behaviors of all the upstream objects on the prototype chain; from a bottom up / run time point of view, objects on the bottom of the prototype chain can "delegate" requests to the upstream objects to be handled. Hence this design pattern is also called **Behavior Delegation**.

Objects created from prototypes can override shared behaviors by defining the same methods locally:

**Object.getOwnPropertyNames() and Object.prototype.hasOwnProperty()**

- The `hasOwnProperty` method on an object tests if a property is defined on the object itself.
- The `Object.getOwnPropertyNames` method returns an array of an object's own property names.





**Function Prototype**

In JavaScript, most **functions** have a special `prototype` property whose value is called the **function prototype**. If the function is used as a constructor (e.g., it is called with the `new`keyword), the newly created object is given a reference to the function prototype. This reference is called the **object prototype**. Note that most non-function objects do not have a `prototype`property.

Note that arrow functions are a special case of function. They do not have a `prototype`property. One consequence of this lack is that you cannot call an arrow function with the `new`keyword.

```js
let Foo = function() {};
let obj = Foo.prototype;

let bar = new Foo();
let baz = new Foo();

Object.getPrototypeOf(bar) === obj;  // true
Object.getPrototypeOf(baz) === obj;  // true

// delegation: constructor is a property of a function prototype
bar.constructor === Foo;             // true; bar is created from Foo
baz.constructor === Foo;             // true; baz is created from Foo

bar instanceof Foo;                  // true; bar is an instance of Foo
baz instanceof Foo;                  // true; baz is an instance of Foo
```

![Delegation in action](https://d3905n0khyu9wc.cloudfront.net/images/constructor_prototypes_1.png)

The key takeaway here is that every time we call a function as a constructor, JavaScript creates objects that are prototype linked to the object that is assigned to the `prototype` property (this happens via lines 10 and 20 above). With this understanding, we can use a constructor function and its `prototype` property to set up behavior delegation:

```js
let Dog = function() {};

Dog.prototype.say = function() {
  console.log(this.name + ' says Woof!');
}

Dog.prototype.run = function() {
  console.log(this.name + ' runs away.');
}

let fido = new Dog();
fido.name = 'Fido';
fido.say();             // => Fido says Woof!
fido.run();             // => Fido runs away.

let spot = new Dog();
spot.name = 'Spot';
spot.say();             // => Spot says Woof!
spot.run();             // => Spot runs away.
```

This approach of defining shared behaviors on the constructor's `prototype` property is called the **Prototype Pattern** of object creation.

```js
let DogPrototype = {
  bark() {
    console.log(this.weight > 20 ? 'Woof!' : 'Yip!');
  }
};

function Dog(name, breed, weight) {
  Object.setPrototypeOf(this, DogPrototype);
  this.name = name;
  this.breed = breed;
  this.weight = weight;
  // this.bark method removed.
}
```



When we use Object factories or add a method to a Constructor then a new copy of the method is given to each object.



```js
function Dog(name, breed, weight) {
  this.name = name;
  this.breed = breed;
  this.weight = weight;

  this.bark = function() {
    console.log(this.weight > 20 ? 'Woof!' : 'Yip!');
  };
}

let maxi = new Dog('Maxi', 'German Shepherd', 32);
let dexter = new Dog('Dexter', 'Rottweiler', 50);
let biggie = new Dog('Biggie', 'Whippet', 9);

maxi.bark(); // 'Woof!'

maxi.hasOwnProperty('bark');   // true
dexter.hasOwnProperty('bark'); // true
biggie.hasOwnProperty('bark'); // true
maxi.bark === dexter.bark;     // false
maxi.bark === biggie.bark;     // false
dexter.bark === biggie.bark;   // false
```



Earlier, we learned that we could use prototypes to share code between objects of the same type. Prototypes are especially useful for sharing methods as all objects of a particular type share the same prototype object.  Furthermore, delegation means that we can share methods by putting them in the prototype object; if an object doesn't contain a requested method, JavaScript searches the prototype chain to find the method.

Thus, we can define a method once in the prototype object, and let the inheriting objects delegate the method calls to the prototype. We can use prototypes in conjunction with constructors to achieve the same result:

*Using `Object.setPrototypeOf(this, prototype)` to set prototype*

Inneficient but at least here we are sharing the bark method instead of repeating it.  The `DogPrototype` object has the only copy of the method; all `Dog` objects delegate `bark` to the `DogPrototype` object.

```js
let DogPrototype = {
  bark() {
    console.log(this.weight > 20 ? 'Woof!' : 'Yip!');
  }
};

function Dog(name, breed, weight) {
  Object.setPrototypeOf(this, DogPrototype);
  this.name = name;
  this.breed = breed;
  this.weight = weight;
  // this.bark method removed.
}

maxi.hasOwnProperty('bark'); // false
dexter.hasOwnProperty('bark'); // false
biggie.hasOwnProperty('bark'); // false
Object.getPrototypeOf(maxi).bark === DogPrototype.bark; // true
Object.getPrototypeOf(dexter).bark === DogPrototype.bark; // true
Object.getPrototypeOf(biggie).bark === DogPrototype.bark; // true
```



What makes constructors special is a characteristic of most function objects in JavaScript: they have a `prototype` property that we call the **function prototype** to distinguish them from the prototypes used when creating ordinary objects. (Note that arrow functions **do not** have a `prototype`property.)

*What is this Prototype property and a constructor?*

A function's prototype property, by default, is **a plain object with one property: constructor , which is a reference to the function itself**. The constructor property is writable, non-enumerable, and configurable.

```js
//Create Constructor function 

> function Construct(name) {
... this.name = name
... }
undefined
  
//Create object from Construct using `new` keyword  
 
> let inherit = new Construct('inheriting')
undefined
  
  
//Make sure that new object has access to the property `name`
  
> inherit.name
'inheriting'
  
  
//Make sure that new object has access to the constructor property which points to the function that created it
  
> inherit.constructor
[Function: Construct]
  
  
//Log the new object and see the property inside and the Construct function which the constructor property points to  
  
> console.log(inherit)
Construct { name: 'inheriting' }
undefined
  

//Access the object created when a function is created. See that the prototype property points to an object that has a constructor property that points back to the function itself 
  
  
> Construct.prototype.constructor
[Function: Construct]
  
  
//Call the prototype method on the Function and it's empty. Is the constructor property hidden?
  
  
> Construct.prototype
{}
  
  
//See all the properties of the Function object  
  
> Object.getOwnPropertyNames(Construct)
[ 'length', 'name', 'arguments', 'caller', 'prototype' ]
  
//See all the properties of the prototype object and we can see constructor... so it's there   
 
> Object.getOwnPropertyNames(Construct.prototype)
[ 'constructor' ]
  

//Add a new method to the prototype 
 
> Construct.prototype.newMethod = function newMethod() {
... console.log('First delegated method from Construct')
... }
[Function: newMethod]
  
//Examine the prototype object and see the newMethod is present   
  
> Object.getOwnPropertyNames(Construct.prototype)
[ 'constructor', 'newMethod' ]
  
//Examine the new Object created from Constructor and see the new Method is not a method within inherit   
  
> inherit.hasOwnProperty('newMethod')
false
  
//See that the function itself does not contain the method, but the prototpe object that the new object essentially inherits from   
  
> Construct.hasOwnProperty('newMethod')
false
> Construct.prototype.hasOwnProperty('newMethod')
true
  
//Call the new Method on the new Object and see the new Object has access to it bc it was created from the Construct prototype   
  
> inherit.newMethod()
First delegated method from Construct
undefined
```

When you call a function `Foo` with the `new` keyword, JavaScript sets the new object's prototype to the current value of `Foo`'s `prototype` property. That is, the constructor creates an object that inherits from the constructor function's prototype. Since inheritance in JavaScript uses prototypes, we can also say that the constructor creates an object with a prototype that is the same as the constructor function's prototype.



**Prototype Wording**

The terminology of constructor prototypes and object prototypes is extremely confusing. Note in particular that we use the term "prototype" to refer to 2 distinct but related concepts:

- If `bar` is an object, then the object from which `bar` inherits is the **object prototype**. By default, constructor functions set the object prototype for the objects they create to the constructor's prototype object.
- The **constructor's prototype object**, also called the **function prototype**, is an object that the constructor uses as the object prototype for the objects it creates. In other words, each object that the constructor creates inherits from the constructor's prototype object. JavaScript stores the constructor's prototype object in the constructor's `prototype`property; that is, if the constructor's name is `Foo`, then `Foo.prototype` references the constructor's prototype object.

It's easy to get confused about the differences between these two kinds of prototypes. Be sure you understand the differences before moving on. In most cases, when we talk about a **prototype** without being more explicit, we mean an **object prototype**. We'll talk about the constructor's prototype, the function prototype, or the `prototype` property when talking about a constructor's prototype object.

Note that constructors don't inherit from the constructor's prototype object. Instead, the objects that the constructor creates inherit from it.



**When `new` is called**

We'll assume that the constructor function is named `Foo`:

1. It creates an entirely new object.
2. It sets `Foo.prototype` as the prototype for the new object. That is, the new object inherits from the object referenced by `Foo.prototype`.
3. It sets the execution context (`this`) for the function to point to the new object.
4. It invokes the function.
5. It returns the new object unless the function returns another **object**.



**Overriding Methods**

```js
let maxi = new Dog('Maxi', 'German Shepherd', 32);
let dexter = new Dog('Dexter', 'Rottweiler', 50);

dexter.bark = function() {
  console.log('WOOF!')
}

maxi.bark();   // Woof!
dexter.bark(); // WOOF!
```

The `dexter` object now has its own `bark` method that **overrides** the `bark` method from `Dog.prototype`. Each time we call `bark` on `dexter`, JavaScript looks for it first in the `dexter`object itself. Since it finds it there, it doesn't need to check the prototype.



```js
let ninja;
function Ninja() {
  this.swung = true;
}

ninja = new Ninja();

Ninja.prototype = {
  swingSword: function() {
    return this.swung;
  },
};

console.log(ninja.swingSword());
//Uncaught TypeError: ninja.swingSword is not a function
```

1. In this case, we didn't add a new method to the constructor function's prototype object by mutating it, but rather made it point to a different object. The `ninja` object, meanwhile, still inherited from the original prototype object, therefore it couldn't find a `swingSword` method anywhere on its prototype chain.

   

**Scope-Safe Constructors**

If not called with new it still works

```js 
function User(first, last){
  if (!(this instanceof User)) {
    return new User(first, last);
  }

  this.name = first + ' ' + last;
}

let name = 'Jane Doe';
let user = User('John', 'Doe');

console.log(name);        // => Jane Doe
console.log(user.name);   // => John Doe
```





**What is a Constructor**

Constructors are like regular functions, but we use them with the `new` keyword. There are two types of constructors: built-in constructors such as `Array` and `Object`, which are available automatically in the execution environment at runtime; and custom constructors, which define properties and methods for your own type of object.

A constructor is useful when you want to create multiple similar objects with the same properties and methods. It’s a convention to capitalize the name of constructors to distinguish them from regular functions.

*Notable Methods*

`instanceof` - ex/ `myBook instanceof Book` right operand must be a function 

You can use the constructor property ex/ `myBook.constructor === Book` bc the constructor will point to the function itself 

	- Every object in JavaScript inherits a `constructor` property from its prototype, which points to the constructor function that has created the object:
	- Note, however, that using the `constructor` property to check the type of an instance is generally considered bad practice because it can be overwritten.

**Instances**

In OOP, we often refer to individual objects of a specific data type as **instances** of that type

In that assignment, we used constructors to create instances of the `Dog` type. We can also think of objects created by factory functions as instances.

**Instance Properties**

It's convenient to describe the properties of an instance as **instance properties**. 

Since methods are also properties on an object, we can refer to methods stored directly in an object as instance properties too. More commonly, we call them **instance methods** just to distinguish them from ordinary data properties.

Any method defined in any prototype in the prototype chain of an object is considered to be an instance method of the object.

**Static Properties**

**Static properties** are defined and accessed directly on the constructor, not on an instance or a prototype. Typically, static properties belong to the type (e.g., `Dog`) rather than to the individual instances or the prototype object.

One common use of static properties is to keep track of all of the objects created by a constructor. For instance:

```js
function Dog(name, breed, weight) {
  this.name = name;
  this.breed = breed;
  this.weight = weight;
  Dog.allDogs.push(this);
}

Dog.allDogs = [];
```



**Static Methods**

Static properties don't have to be ordinary data properties. You can also define static methods:

```js
Dog.showSpecies = function() {
  console.log(`Dogs belong to the species ${Dog.species}`);
};

Dog.showSpecies();
```



**Psuedo-classical Pattern and the OLOO Pattern**





**Resetting prototype and constructor**

So every object is going to have a constructor function. 

If you reassign a prototype to another object, the constructor is going to point to some native code that is a prototype for that new object. But will it still be an nstance of Beget?

```js
let foo = {
  a: 1,
};


function Beget() {
  console.log(Beget.prototype.constructor)
  Beget.prototype = foo 
  console.log(Beget.prototype.constructor)
}



console.log(foo.constructor)
let b = new Beget 


console.log(b)
console.log(b.constructor)
console.log(Object.getPrototypeOf(b))

/*
ƒ Object() { [native code] }
example.js:7 ƒ Beget() {
  console.log(Beget.prototype.constructor)
  Beget.prototype = foo 
  console.log(Beget.prototype.constructor)
}
example.js:9 ƒ Object() { [native code] }
example.js:18 Beget {}
example.js:19 ƒ Beget() {
  console.log(Beget.prototype.constructor)
  Beget.prototype = foo 
  console.log(Beget.prototype.constructor)
}
example.js:20 {constructor: ƒ}
*/
```





Very simply said, `new X` is `Object.create(X.prototype)` with additionally running the `constructor` function. (And giving the `constructor` the chance to `return` the actual object that should be the result of the expression instead of `this`.)



use `console.dir(func)` to inspect object associate with function 

All objects, including functions, have a [[Prototype]] property.  This is something that points to the thing that made it..like Object.getPrototypeOf(obj). So yes the constructor has this property `prototype` which is an object with a `constructor` property that points to itself. It also has a hidden `[[prototype]]` property that references the prototype that made it. 

The object created from the constructor also has the `[[prototype]]` property that references the prototype that made IT..which is that object that guess what? has the constructor property that point to the constructor function 

```js
function Trial() {}

let newObj = new Trial

console.log(Trial.prototype.constructor) //[Function: Trial]
console.log(newObj.constructor) //[Function: Trial]
console.log(Trial.prototype.constructor === newObj.constructor) //true
```



```js
t.__proto__ === G.prototype === Object.getPrototypeOf(t)
```



**Prototypal Inheritance or Delegation or OLOO objects linking to other objects**

You can only use Object.getPrototypeOf not instances

```js
let myChair = {
  width: 50,
  minHeight: 45,
};
let myDeskChair = Object.create(myChair);
myDeskChair.maxHeight = 52;
myDeskChair.wheels = 5;

or 

let myChair = {
  init(width, height) {
    this.width = width;
    this.height = height;
    return this;
  }
};
let anotherChair = Object.create(myChair).init(40,60);

```

You can still access the constructor function in OLOO from the new object. How? We didn't create this from a function... Well bc the prototype is `myChair` and `myChair` is an object whose prototype was created by a function and therefor has access to a constructor function.. we will look up the chain of inheritance and actually call `myChair`'s constructor function 

```js 
let mychair = {hi:'hi'}


let newObj = Object.create(mychair)


console.log(newObj.__proto__)
//{hi: 'hi'} this ones __proto__ has a constructor function!
console.log(newObj.__proto__.__proto__)
//{constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
console.log(newObj.__proto__.__proto__.__proto__)
//null
```





**Psuedo-Classical Pattern**

The predecessor to OLOO is the **Pseudo-classical pattern**. It is known to be a more complex system, where constructor functions and prototypes work together to create objects.

 Under the hood however lies JavaScript’s true nature: a system of objects and prototypes secretly linking up to each other.

Hence why this pattern is called ***Pseudo\***-classical. **JavaScript \*is\* and always has been a \*prototype-based language.\*** It has no true classes of its own, and can only use objects and prototypes to perform prototype-based inheritance.!*

you can use instanceof here and Object.getPrototypeOf()

![img](https://miro.medium.com/v2/resize:fit:1400/1*TDgBz8ZoosDYPJrC8Eet0A.jpeg)

This image is the same for OLOO except for the Chair construcor function

- **JavaScript is an object-oriented programming language in the purest sense**: it has no classes of its own, and simple objects can become working prototypes for other objects
- **JavaScript supports prototype-based inheritance**: inheritance can only be performed using objects and prototypes. Anything else that claims to look like inheritance is just objects working under the surface
- **Objects delegate shared, common properties to their prototypes using a prototype chain**: this way, objects only contain the necessary properties that makes them unique, keeping them small and efficient



**Class Syntactic Sugar**

In this assignment, we'll look at the `class` keyword. ES6 has introduced `class` as another way to create objects as well as establish inheritance.  When we do this we automatically call the `constructor`method. We can create an instance of Point without calling the `constructor` by using `Object.create(Point.prototype)` and still use `instance of`

You define like this: 

```js
class Point {
  constructor(x = 0, y = 0) {
    this.x = x;
    this.y = y;
  }

  onXAxis() {
    return this.y === 0;
  }

  onYAxis() {
    return this.x === 0;
  }

  distanceToOrigin() {
    return Math.sqrt((this.x * this.x) + (this.y * this.y));
  }
  
  static.genericGreeting() { //define class method with static
    console.log('generic from Point')
  }
}
let pointA = new Point(30, 40);
let pointB = new Point(20);

pointA instanceof Point;                // true
pointB instanceof Point;                // true

pointA.distanceToOrigin();              // 50
pointB.onXAxis();                       // true
Point.genericGreeting //call static method on class name 
```

Here the functions are only part of the prototype even though they are defined within the class

**How to inherit from a class**

use `extends` and `super`

```js
class Square extends Rectangle {
  constructor(lengthOfSide) {
    super(lengthOfSide, lengthOfSide)
  }
}
```

We can call `super()` from the child class and it will call the constructor of the parent calss. If we want to call a method from the parent class on an instance of the child class then we can also use `super` but we call it with the method ex/ `super.introduce()`



**Object.defineProperties**

We want to have an object constructor that returns a new object with a log function that cannot be modified. In a normal constructor this is not possible. However, using the `defineProperties` method on `Object` we can provide properties and values and set whether each property can be changed or not. Here is an example of creating a property on an object that is read-only.

```js
let obj = {
  name: 'Obj',
};

Object.defineProperties(obj, {
  age: {
    value: 30,
    writable: false,
  },
});

console.log(obj.age); // => 30
obj.age = 32;         // throws an error in strict mode
console.log(obj.age); // => 30
```



**Object.freeze**

For property values that are objects, the references to the objects are frozen. This means that you can't point to other objects, but you can still use the frozen references to mutate the objects.

```js
let frozen = {
  integer: 4,
  string: 'String',
  array: [1, 2, 3],
  object: {
    foo: 'bar'
  },
  func() {
    console.log('I\'m frozen');
  },
};

Object.freeze(frozen);
frozen.integer = 8;
frozen.string = 'Number';
frozen.array.pop();
frozen.object.foo = 'baz';
frozen.func = function() {
  console.log('I\'m not really frozen');
};

console.log(frozen.integer);      // => 4
console.log(frozen.string);       // => String
console.log(frozen.array);        // => [1, 2]
console.log(frozen.object.foo);   // => baz
frozen.func();                    // => I'm frozen
```

**CommonJS modules, also known as Node modules and JS Modules or "ES modules" and "ECMAScript modules."**

The solution to all these problems is to split up a program into multiple files, commonly called **modules**. Each module can focus on a particular part of the problem, and a team of developers can work on separate parts without fear of conflicts

Modules can also make it easier to work with private data, which helps maintain encapsulation. You must explicitly export the items you want to make available; everything else is private to the module. Best of all, you can easily use a module elsewhere without first having to disentangle it from the program.

*Using CommonJS Modules*

**CommonJS module** syntax uses `require`- To use a CommonJS module, all you have to do is import it with the `require` function.

Not all flavors of JavaScript support CommonJS modules. In particular, browsers don't support them since they're loaded *synchronously*.   Synchronous loading makes CommonJS modules unsuitable for the browser environment -They're suitable for Node applications where everything resides on the same machine, but not the browser. Browsers don't natively support CommonJS modules, but you can use a transpiler like [Babel](https://babeljs.io/) to transpile code that uses CommonJS modules into a format that can be used by browsers.

*Creating CommonJS Modules*

Creating modules is reasonably straightforward. First, create a file with the code that you want to modularize, then add some additional code to export the items that you want other modules to use:

```js
//IN logit.js file
function logIt(string) {
  console.log(string);
}

module.exports = logIt;

//IN main.js file
const logIt = require("./logit");  //"./logit" is name of importing file
logIt("You rock!");
```

exporting multiple variables

```js
let prefix = ">> ";

function logIt(string) {
  console.log(`${prefix}${string}`);
}

function setPrefix(newPrefix) {
  prefix = newPrefix;
}

module.exports = {
  logIt,
  setPrefix,
};

const { logIt, setPrefix } = require("./logit");
logIt("You rock!"); // >> You rock!
setPrefix("++ ");
logIt("You rock!"); // ++ You rock!
```

In Node, all code is part of a CommonJS module, including your main program. Each module provides several variables that you can use in your code:

- `module`: an object that represents the current module

- `exports`: the name(s) exported by the module (same as `module.exports`)

- `require(moduleName)`: the function that loads a module

- `__dirname`: the absolute pathname of the directory that contains the module

- `__filename`: the absolute pathname of the file that contains the module

  With ES6, JavaScript now supports modules natively. It adds the `export` and `import` keywords to the language, and most modern browsers now support them. If you must support older browsers, you can use tools like Babel and Webpack. Babel *transpiles* (converts) ES6 code to ES5 code. Webpack consolidates all of the modules you need into a single file. We won't discuss Webpack in this course, but we'll talk about Babel in a later lesson.

**JS Modules**

```js
//foo.js
import { bar } from "./bar";

let xyz = 1; // not exported

export function foo() {
  console.log(xyz);
  xyz += 1;
  bar();
}

//bar.js
export let items = [];
export let counter = 0;

export function bar() {
  counter += 1;
  items.push(`item ${counter}`);
}

export function getCounter() {
  return counter;
}

//main.js
import { foo } from "./foo";
import { bar, getCounter, items, counter } from "./bar";

foo();
console.log(items);          // ["item 1"]
console.log(getCounter());   // 1
console.log(counter);        // 1

bar();
console.log(items);          // ["item 1", "item 2"]
console.log(getCounter());   // 2
console.log(counter);        // 2
```

Notice that exporting is as simple as preceding each declaration with the word `export`. Anything that you don't export explicitly is local to the module. Thus, the `xyz` variable in the `foo` modules is local to `foo`.

we can also rename upon importation

```js
//foo.js
export function foo() {
  console.log(1);
}

//main.js
import { foo as renamedFoo } from "./foo";

renamedFoo(); // 1
```

and even use the **Namespace import**

```js
//foo.js
export function foo() {
  console.log(1);
}

//main.js
import * as FooModule from "./foo";

FooModule.foo(); // 1
```

or **Default Imports/Exports**

```js
//bar.js
function bar() {
  console.log(2);
}

export default bar;

//main.js
import bar from "./bar";

bar(); // 2
```

Notice that curly braces are not required when importing a default export. In the example above, the default export from `bar.js` is imported and assigned the name `bar`.

rename by this 

```js
//bar.js
function bar() {
  console.log(2);
}

export default bar;

//main.js
import renamedBar from "./bar";

renamedBar(); // 2
```

even mix named and default in a JS module 

```js
//utils.js
export function foo() {
  console.log(1);
}

function bar() {
  console.log(2);
}

export default bar;

//main.js
import renamedBar, { foo } from "./utils.js";

foo(); // 1
renamedBar(); // 2
```

To summarize, mixing named and default exports allows you to provide a primary or fallback export through the default export, while also offering additional functionality or values through named exports.

**Module Summary**

The initial release of Node came equipped with the CommonJS module system. It's still in widespread use today. However, the CommonJS module's synchronous nature makes it unsuitable for use in a browser, which led to the development of JS modules. In recent years, Node has added support for JS modules, but older versions of Node don't have direct support for them. However, even if you're using an older version of Node, you can use a transpiler to emulate the JS modules.







Summary

- Factory functions (also known as the Factory Object Creation Pattern) instantiate and return a new object in the function body. They allow us to create new objects based on a pre-defined template and have two major downsides:

  - There is no way to tell based on a returned object itself whether the object was created by a factory function.
  - All objects created by factory functions that share behavior have an owned copy or copies of the methods, which can be redundant.

- Constructor functions are meant to be invoked with the

   

  ```
  new
  ```

   

  operator. They instantiate a new object behind the scenes and allow the developer to manipulate it using the

   

  ```
  this
  ```

   

  keyword. A typical constructor is used with the following pattern:

  1. The constructor is invoked with `new`
  2. A new object is created by the JS runtime
  3. The new object inherits from the constructor's prototype
  4. The new object is assigned to `this` inside the function body
  5. The code inside the function is executed
  6. `this` is returned unless there's an explicit object returned.

- Every object has a

   

  ```
  [[Prototype]]
  ```

   

  property that points to a special object, the object's prototype, which is used to lookup properties that don't exist on the object itself.

  - `Object.create` returns a new object with the argument object as its prototype.
  - `Object.getPrototypeOf` can be used to retrieve the prototype object for an object
  - `Object.setPrototypeOf` can be used to replace an object's prototype object with another object.
  - `obj.isPrototypeOf` can be used to check for prototype relationships between objects.

- The prototype chain property lookup is the basis for "prototypal inheritance", or property sharing through the prototype chain. Downstream objects "inherit" properties and behaviors from upstream objects, or, put differently, downstream objects can "delegate" properties or behaviors upstream.

  - A downstream object **overrides** an inherited property if it has a same-named property on itself. (Overriding is similar to shadowing, but it doesn't completely hide the overridden properties.)
  - `Object.getOwnPropertyNames` and `obj.hasOwnProperty` can be used to test whether a given property is owned by an object.

- Every non-arrow function has a `prototype` property that points to an object with a `constructor` property, that points back to the function itself. If the function is used as a constructor, then the returned object's `[[Prototype]]` will be set to the constructor's `prototype` property. This behavior allows us to set properties on the constructor's `prototype` object that will be shared by all objects returned by it. This is called the "Prototype Pattern" of object creation.

- The Pseudo Classical Pattern of object creation generates objects using a constructor function that defines state, and then defines shared behaviors on the constructor's `prototype`.

- The Objects Linking to Other Objects (OLOO) pattern of object creation uses a prototype object and

   

  ```
  Object.create
  ```

   

  to generate objects with shared behavior.

  - An optional `init` method on the prototype object is defined to set unique states on the returned objects.

- Properties can be either static or instance properties. Instance properties apply to a specific object instance of a type, while static properties apply to the type as a whole.









Vidoes: 

[JavaScript OOP video](https://www.youtube.com/watch?v=-N9tBvlO9Bo&feature=youtu.be)

Articles: 

[A shallow dive into the constructor property in Javascript](https://medium.com/@patel.aneeesh/a-shallow-dive-into-the-constructor-property-in-javascript-b0a89747058b) 

- [JavaScript Design Patterns: Building a Mental Model](https://medium.com/launch-school/javascript-design-patterns-building-a-mental-model-68c2d4356538)
- [JavaScript Weekly: Fundamental Object Design Patterns](https://medium.com/launch-school/javascript-weekly-fundamental-object-design-patterns-31453f68427f)
- [JavaScript Weekly: Making Sense of Closures](https://medium.com/launch-school/javascript-weekly-making-sense-of-closures-daa2e0b56f88)
- [JavaScript Weekly: Understanding Links on the Object Prototype Chain](https://medium.com/launch-school/javascript-weekly-understanding-links-on-the-object-prototype-chain-12962f05e149)
- [JavaScript Weekly: An Introduction to First-Class Functions](https://medium.com/launch-school/javascript-weekly-an-introduction-to-first-class-functions-9d069e6fb137)
- [JavaScript Weekly: What in the World is this?!](https://medium.com/launch-school/what-in-the-world-is-this-be803a85ed47)

Exercises 

https://launchschool.com/exercise_sets/95a27eff











WHY?

```js
function NewObj(name) {
  this.name = name 

  this.age = function() {
    return 1
  }
}

// let bob = new NewObj('Bob') //{name: , age: }
// console.log(bob.age())
// console.log(bob.name)
// console.log(bob.constructor)
// console.log(bob.hasOwnProperty('age'))
// console.log(bob.__proto__)//.constructor)

// console.log(NewObj.prototype === bob.__proto__)

let sue = Object.create(NewObj.prototype) //{}
// console.log(sue.age())
console.log(sue instanceof NewObj)
// console.log(sue.name)
// console.log(sue.constructor)
// console.log(sue.hasOwnProperty('age'))
// console.log(sue.__proto__)


class Cat {
  // constructor(name) {
  //   this.name = name;
  // }
  speaks() {
    return `Cat says meowwww.`;
  }
}

let fakeCat = Object.create(Cat.prototype);
console.log(fakeCat instanceof Cat); // logs true;
console.log(fakeCat.speaks());       // logs undefined says meowwww.
```

