# JS225 Study



1. **What is an object factory? Give an example of one.**

An object factory is a function that creates and returns a new object.  By using a function, we are able to produce multiple objects with the same properties and methods without replicating code.  In essence, we are able to create a template for objects. This ensures that we adhere to the DRY principle, which can simplify code refactoring and efficiency in the future. Let's give an example of code that does not use a factory function: 

Let's say we want to create an object `firstObject`.  We want to define it with a name and a priority (properties/ state). We also want to define a way to log this information in a user- friendly way (function/ behavior).

```js
let firstObject = {
  name: 'firstObject', 
  priority: 'Urgent',
  info() {
    console.log(`The item ${this.name} is ${this.priority}`)
  }
}

firstObject.info() //The item firstObject is Urgent
```

Great! Now we want another object with the same properties and behavior.  We have to replicate this code as follows:

```js
let secondObject = {
  name: 'secondObject', 
  priority: 'low-priority',
  info() {
    console.log(`The item ${this.name} is ${this.priority}`)
  }
}

secondObject.info() //The item secondObject is low-priority
```

While this works, as our program increases in size and we require more objects of this type, refactoring and maintaining our code will become tedious and error- prone.

We can remedy this by creating a factory function. This function can be called and the returned object can be assigned to a variable for later use.

```js
function tasks(name, priority) {
  return {
    name, 
    priority, 
    info() {
      console.log(`The item ${this.name} is ${this.priority}`)
    }
  }
}

let firstObject = tasks('firstObject', 'Urgent')
let secondObject = tasks('secondObject', 'low-priority')

firstObject.info()  //The item firstObject is Urgent
secondObject.info() //The item secondObject is low-priority
```



2. **What are some of the disadvantages of an object factory?**

While object factories remedy the problem of code duplication by providing a function that defines a template for future objects, there are some downsides with this approach. 

Each time you create a new object, JS allocates memory for that object. Because a factory function replicates the methods and properties defined in the object factory for each object, it is not an efficient use of memory.  

Let's explore this below:

```js
function tasks(name, priority) {
  return {
    name, 
    priority, 
    info() {
      console.log(`The item ${this.name} is ${this.priority}`)
    }
  }
}

let firstObject = tasks('firstObject', 'Urgent')
let secondObject = tasks('secondObject', 'low-priority')

console.log(firstObject.hasOwnProperty('info')) //true
console.log(firstObject.hasOwnProperty('name')) //true
console.log(secondObject.hasOwnProperty('info')) //true
console.log(firstObject.hasOwnProperty('priority')) //true

console.log(firstObject)  //{name: 'firstObject', priority: 'Urgent', info: [Function: info] }
console.log(secondObject) //{name: 'secondObject', priority: 'low-priority', info: [Function: info]}
```

We can see here that both objects returned by the factory function `tasks` have their own copy of the `info` method, as well as the `name` and `priority` properties. 

Another downside is that we have no way of figuring out if that object was created by a factory function. 

```js
console.log(Object.getPrototypeOf(firstObject) === Object.prototype) //true
console.log(firstObject instanceof tasks)  //false
console.log(firstObject instanceof Object) //true
```

As you can see, the prototype of `firstObject` is `Object.prototype` and when we try to determine if `firstObject` is an instanceof `tasks`, the result is `false`.  This is expected, as `tasks` is not used as a constructor function, but as a factory function. It illistrates how we cannot determine the origins of the returned objects. 



3. **What is `this`?**

`this` is the current execution context of a function.  When a function is invoked in JS, it can access the execution context  Where the function is invoked can change the context.

A function's execution context is the global object. If you are in a browser, this is the `window` object.

```js
this
//Window {window: Window, self: Window, document: document, name: '', location: Location, …}

function whatIsThis() {
  console.log(this)
}

whatIsThis() ////Window {window: Window, self: Window, document: document, name: '', location: Location, …}
```

A method's execution context, or value of `this`,  is the calling object.  How does a method differ from a functIon? A method is defined as a property on an object.  It is a behavior of that object, or a function that can be called on a specific calling object. You can differentiate a method from a function by the syntax: `callingObject.someMethod()`. 

```js
let anObject = {
  name: 'Hi its me, anObject',
  text: 'this allows you to access me!',
  
  explainThis() {
    console.log(`${this.name}, and ${this.text}`)
  }
}

obj1.explainThis() //
```

EXPLAIN THIS

```js
this.name = 'Hi its me, a property on the global object'
this.text = "and you shouldn't do this. 'this' inside a arrow function "

let name = 'Hi its me, a declared global variable'
let text = 'without "this" you are accessing me from the outer scope'

let anObject = {
  name: 'Hi its me, the "name" property on anObject',
  text: '"this" inside this obects methods allows you to access me!',
  
  explainThis() {
    console.log(`${this.name}, and ${this.text}`)
  },

  arrowFunctionsContext: () => {
    console.log(`${this.name}, and ${this.text}`)
  },

  notUsingThis() {
    console.log(`${name}, and ${text}`)
  }
}
anObject.explainThis() //Hi its me, anObject, and "this" allows you to access my properties!
anObject.arrowFunctionsContext()  //Hi its me, a property on the global object, and and you shouldn't do this. 'this' inside a arrow function 
anObject.notUsingThis()  //Hi its me, a declared global variable, and without "this" you are accessing me from the outer scope
```







What is strict mode, and how does it change how a program runs? 

What are the benefits to using strict mode? When should you use it?

What is implicit function execution context?

What is explicit function execution context?

How can we change a function's execution context at execution time?

What do call() and apply() do, and how are they different? Give an example of using each.

What is the global object, and how can we access it?

What is the bind() method, and how does it differ from call() an apply()?

Bind pp

What do we mean when we say a function can "lose it's context"? What are two ways a function can experience context loss? 

Context loss pp

Context loss pp

What is a closure? What are the benefits of closures, and how can we create one?

How can we use closures to create private data? Demonstrate how we can make a variable, secretNumber private, using a closure. Why should we use closures to make data private?

What is garbage collection? Which values in JS participate in GC? Why do we need to be aware of garbage collection, as software engineers?

In the following code, how can we retain access to the value "Steve"? When can JS garbage collect "Steve"? 

JavaScript

```js
function makeHello(name) {  return function() {    console.log("Hello, " + name + "!");  }; } let helloSteve = makeHello("Steve");
```

Notes

- Notes
    - After the code runs, `helloSteve` references a function that closed over the local variable `name`, which right now contains the string `"Steve"`. Since the closure must keep `name` around, the reference to `"Steve"` must also stick around, which means JS can't GC `"Steve"`. When we call `helloSteve` sometime later, it still has access to `name` and can log its value.
    
    ```jsx
    helloSteve();  // => Hello, Steve!
    ```
    
    - Before JS can garbage collect `"Steve"`, you must ensure nothing else in the program references `"Steve"`; that includes every closure that has access to the `"Steve"` string. That's not typically a concern, but if you find that you must remove a closure or other reference explicitly, you can assign `null` to the item that references it. For instance:
    
    ```jsx
    helloSteve = null;
    ```
    
    - That dereferences the closure shown above, which in turn dereferences `"Steve"` through the `name` variable. If nothing else now has a reference to `"Steve"`, JS is free to garbage collect it.
    - If we modify the code above by introducing a variable prior to returning the anonymous function, the code returns an anonymous function that closes over both the `name` *and* `message` variables. Those variables, in turn, reference the strings `"Steve"` and `"Hello, Steve!"` respectively.
    
    ```jsx
    function makeHello(name) {
      let message = "Hello, " + name + "!";
      return function() {
        console.log(message);
      };
    }
    
    let helloSteve = makeHello("Steve");
    ```
    
    - Theoretically, this will prevent garbage collection on both strings. However, since `name` isn't referenced within the `helloSteve` function, depending on the browser, `"Steve"` might or might not be garbage collected.

21. What is an IIFE? Give an example. Why would we use an IIFE in code?
22. How can you call an IIFE with an argument? 
23. How can you use an IIFE to create a private scope? What problems does this solve?
24. What is a first-class function? Give an example.
25. What concept(s) does the following code demonstrate? How does this work?

JavaScript

Copy

function multiply(first, second) {  return first * second; } function makeMultiplyN(multiplicand) {  return function(number) {    return multiply(multiplicand, number);  } }



26. What is partial function application, and what are the benefits of using it?
27. Create a reusable function using partial function application.
28. What is the difference between a constructor function and a regular function?
29. What does the new operator do?
30. What does the following code log to the console? Note: Remember we're running it in coderpad.

JavaScript

Copy

let a = 1; let foo; let obj; function Foo() {  this.a = 2;  this.bar = function() {    console.log(this.a);  };  this.bar(); } foo = new Foo(); foo.bar(); Foo(); obj = {}; Foo.call(obj); obj.bar(); console.log(this.a);



31. What will the following code log and why?

JavaScript

Copy

let ninja; function Ninja() {  this.swung = true; } ninja = new Ninja(); Ninja.prototype.swingSword = function() {  return this.swung; }; console.log(ninja.swingSword());





Answer

32. Implement the method described in the comments:

JavaScript

Copy

let ninjaA; let ninjaB; function Ninja() {  this.swung = false; } ninjaA = new Ninja(); ninjaB = new Ninja(); // Add a swing method to the Ninja prototype which // returns the calling object and modifies swung console.log(ninjaA.swing().swung);      // must log true console.log(ninjaB.swing().swung);      // must log true





Answer

33. What does Object.create do, and how is it used?
34. What is the function.prototype? 
35. What is behavior delegation? How does JS implement inheritance differently than Ruby?
36. What is OLOO? Give an example. What are the benefits to organizing your code this way?
37. What is the Pseudo-Classical Pattern? Give an example. What are the benefits to organizing your code this way?
38. What are some things we need to consider when designing our code? 
39. What's the difference between using OLOO/ the Pseudo-Classical Pattern and using factory functions? 
40. How does ES6 class syntax work? Give an example.
41. How does inheritance work with class syntax?
42. Write a constructor function. 
43. What is the prototype chain?
44. What is the .constructor property?