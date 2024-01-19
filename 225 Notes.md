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

