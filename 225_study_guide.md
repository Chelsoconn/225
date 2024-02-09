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

`this` is the current execution context of a function.  When a function is invoked in JS, it sets the execution context and can access the execution context object. When/how the function is invoked can change the context.  We can access the execution context object through the keyword `this`.

A function's implicit execution context is the global object. If you are in a browser, this is the `window` object.

```js
this
//Window {window: Window, self: Window, document: document, name: '', location: Location, …}

function whatIsThis() {
  console.log(this)
}

whatIsThis() ////Window {window: Window, self: Window, document: document, name: '', location: Location, …}
```

A method's execution context, or value of `this`,  is the calling object.  How does a method differ from a function? A method is defined as a property on an object.  It is a behavior of that object, or a function that can be called on a specific calling object (a function with a receiver). You can differentiate a method from a function by the syntax: `callingObject.someMethod()` which is a method invocation of `someMethod`. 

```js
let anObject = {
  name: 'Hi its me, anObject',
  text: 'this allows you to access me!',
  
  explainThis() {
    console.log(`${this.name}, and ${this.text}`)
  }
}

obj1.explainThis() //Hi its me, anObject, and this allows you to access me!
```

As you can see below, `this` from inside a method references the object the method is defined within. An arrow function behaves differently in regards to `this` (it does not bind its' own `this`).  Lastly, when we don't use `this` within the method, JS will resolve it as a variable and not a property.  

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



4. **What is strict mode, and how does it change how a program runs?**

The purpose of strict mode in JS in to catch mistakes and bad syntax and have them to raise errors. JS allows developers to silently fail often, which can lead to buggy code and difficulties in identifying problems.  Strict mode can also fix mistakes that make code less efficient and ensures that JS engines are able to be optimized. Strict mode can also make your code more compatible with future EMAScript versions.  When "use strict" is used within a script, all child scopes will be in strict mode. 

In regards to `this`, strict mode changes the implicit execution context from the global object to `undefined`.  This is why undeclared variables are inaccessible (will raise an error), and mispellings are prevented from defining new global properties.



5. **What are the benefits to using strict mode? When should you use it?**

Due to the stricter parsing and error- handling when using strict mode, problems that arise in your code are more likely to be identified before becoming issues down the road.  The introduction of strict mode was a way to aid developers in writing more reliable code that could be easily maintained.  *See above for benefits* 

In general, it is always a good idea to use strict mode. 



6. **What is implicit function execution context?**

When a program starts to run, JS creates a global object. If we are running in the browser, this global object is the `window` object. If we are running in Node, the  global object is `global`.  The area in which we run a program is called the execution environment.  When we use the term 'implicit', we are referring to the fact that JS implicitly sets the execution context if the developer does not explicity set one.

```js
//In Node
console.log(this)  //Object [global]
function tryThis() {
  console.log(this)
}
tryThis() //Object [global]

//In the Browser
console.log(this)  //Window
function tryThis() {
  console.log(this)
}
tryThis() //Window
```

Let's say we are not in strict mode in the browser, and we have an undeclared variable `x = 1`.  Because the implicit execution context is `window`, JS will actually interpret this as `window.x = 1` which is assigning a property `x` to the global object.  

A very similar behavior is seen when we declare global variables with `var`, and when we declare functions.  They are actually added as properties to the global object (except you can't delete them using the `delete` operator).  

Again, when we invoke a function without an explicit context, the implicit function execuction context (or the implicit binding) is the global object in Node. But let's now assign an object's method to a variable and invoke it as a function.  As we know, a method's implicit execution context is the calling object. Because we chose to assign it to a variable and invoke it as a function, the execution context is  now not the original object the method was defined within, but the global object. This shows that the context is set when you execute the function, not when you define it

```js
let obj = {
  whatIsThis() {
    console.log(this)
  }
}

obj.whatIsThis() //{ whatIsThis: [Function: whatIsThis] }
let invokeAsAFunc = obj.whatIsThis
invokeAsAFunc() //Window
```

Remember that when we use strict mode, that the implicit function execution context is set to `undefined`. Let's run this code again in strict mode and see what happens:

```js
"use strict"

let obj = {
  whatIsThis() {
    console.log(this)
  }
}

obj.whatIsThis() //{ whatIsThis: [Function: whatIsThis] }
let invokeAsAFunc = obj.whatIsThis
invokeAsAFunc() //undefined

console.log(this) //Window 
```

The implicit method execution context is the calling object, while the implicit function execution context is `undefined`.  Why then does logging `this` in the global scope log `Window`? Strict mode does not change what `this` points to in the global scope. 



7. **What is implicit method execution context?**

When a function is defined as a property on an object, it is called a method.  In order to invoke a method, we must use this syntax: `someObj.someMeth()`.  If the method is not invoked with an explicit context, then the implicit execution context is the object in which is was defined.  That is, JS implicitly binds the method's execution context to the calling object. 

```js 
let obj = {
  someMethod() {
    console.log(this) 
  }
}

obj.someMethod() //obj
```

That looks easy enough! But wait... 

```js
let obj = {
  arr : [1], 
  
  justSomeNormalMethod() {
    console.log(this)
  },
  
  anArrowInsideAMeth() {
    this.arr.forEach(num => {
      console.log(this)
    })
  }, 
  
  anArrowFunction: () => {
    console.log(this)
  }, 

}

obj.anArrowInsideAMeth()  //obj
obj.anArrowFunction()  //Window
obj.justSomeNormalMethod()  //obj

let invokeAsFunc = obj.justSomeNormalMethod
invokeAsFunc()  //Window
```

Here we can see that the implicit method execution context can differ depending on if an arrow function is used.  An arrow function doesn't bind their own `this` and will inherit it from the parent scope.  So for `anArrowInsideAMeth`, because the arrow function was defined within a parent function that bound `this` to the calling object, the execution context was inherited and is bound to the object. On the other hand, `anArrowFunction` 's  will bind `this` to the `Window` object in the browser. Finally `justSomeNormalMethod` will have an implicit execution context set as the object. One caveat is when we assign the method to a variable and invoke it as a function.  As we can see when we invoke `invokeAsFunc` the execution context is set to `Window`, concreting the concept that execution context is set upon invocation, not definition. 





8. **What is explicit function execution context?**

There are different ways that we can explicitly bind a function's execution context upon invocation.  If we do not explicity set the context using these methods, then JS will implicitly set the execution context. 

We can use the `call` and `apply` methods to set the execution contexts as we invoke the method/ function. This is called indirect invocation.  We can also hard bind the method/ function to an object using the `bind` method.  When we use `bind` we are not actually invoking the function, but rather, returning a new function that is hard bound to an object. 

The `call` method takes a first argument that will be set to the execution context, and subsequent comma- separated arguments that will be passed into the method/function as arguments. 

The `apply` method does the same, except the subsequent arguments are provided as an array or array-like object. 

```js
let launch = {
  name : 'Launch School',
  description: 'killer'
}

let someOtherSchool = {
  name : 'Some bootcamp',
  description : 'less killer',

  info(anotherDescriber) {
    console.log(`${this.name} is ${this.description}, and       ${anotherDescriber}.`)
  }

}


someOtherSchool.info("I didn't learn as much")  //Some bootcamp is less killer, and I didn't learn as much.
someOtherSchool.info.call(launch, "I'm learning a ton")  //Launch School is killer, and I'm learning a ton.

let launchSchool = someOtherSchool.info
launchSchool(`the implicit execution context for the function is ${this}`) //undefined is undefined, and the implicit execution context for the function is [Window Object].
launchSchool.call(launch, "I'm learning a ton") //Launch School is killer, and I'm learning a ton.
launchSchool.apply(launch, ["I'm learning a ton"]) //Launch School is killer, and I'm learning a ton. (Simulates a method call on object launch)

let boundToLaunch = someOtherSchool.info.bind(launch)
boundToLaunch("I'm learning a ton")  //Launch School is killer, and I'm learning a ton.

boundToLaunch.call(someOtherSchool, "I didn't learn as much")  //Launch School is killer, and I didn't learn as much.
```

In the code above, I've written the different ways to set an explicit execution context.  When we invoke `info` as a method on `someOtherSchool`, JS implicity sets the execution context to the calling object.  And yes, it is still implicit even though we used the `someOtherSchool` object to invoke the method. 

Compare this to `someOtherSchool.info.call(launch, "I'm learning a ton")`.  `someOtherSchool` is still the object used to invoke the `info` method, but by using `call` we explicitly were able to set the execution context to the object`launch`. 

I then assigned the return value of `someOtherSchool.info` to the global variable `launchSchool`.  Because the implicit execution context for a function is the Window object in the browser, when we invoke `launchSchool` without an explicit execution context we get "undefined is undefined, and the implicit execution context for the function is [Window Object].".  JS does not find the properties `name` and `description` on the global object, so `undefined` is returned.  We can see here why using indirect invocation is useful! This function requires a specific context to be useful, so when the context is lost, we must provide one to the function explicitly.

Now let's see what happens when we invoke the function assigned to `launchSchool` with an explicit execution context.  Yes, it works! We get "Launch School is killer, and I'm learning a ton."

Lastly, I used `bind` to permanently bind a function to the given execution context.  Invoking `bind` does not invoke the reciever function/method, but instead returns a new function that cannot be unbound from the object passed in as an argument. We can see this when we attempt to use `call` on `boundToLaunch` and it returns "Launch School is killer, and I didn't learn as much." Uh-oh. That's obviously not what we're trying to say! We can still access the original method and use any one of the above methods to set an explicit execution context. That's what is great abound `bind`.. It does not alter the original function/method, and we no longer have to worry about whether the context will change when invoked elsewhere.

*See 225 Notes under Bind/call/apply for more examples*



9. **What do call() and apply() do, and how are they different? Give an example of using each. Also unpack null as execution context and spread/ rest sytax**

*See question 8* 

Null as execution context: 

We can pass `null` into the `call` or `apply` methods when we don't have a specific value we want `this` to point to.

First, what will `null` do if passed as the initial argument to `call` or `apply`? If we are not in strict mode then `null` or `undefined` will be set to the global object and primitives will be converted to objects (`this` must always reference an object in non-strict mode). 

```js
let sanityCheck = {
  name : "just to be sure",

  whatIsNull() {
    console.log(this)
  }
}

sanityCheck.whatIsNull.call(null) //global (undefined as an argument/ using apply method has same result)
sanityCheck.whatIsNull() //{ name: 'just to be sure', whatIsNull: [Function: whatIsNull] }
```

If we are in strict mode and pass in `null` then the context will be set to `null`. Note that we can still perform operations that do not involve `this`. 

```js
"use strict"

let a = 5
let b = 6

let sanityCheck = {
  name : "just to be sure",

  whatIsNull() {
    console.log(a + b)
    console.log(this)
  }
}

sanityCheck.whatIsNull.call(null) //11 null
sanityCheck.whatIsNull() //11 { name: 'just to be sure', whatIsNull: [Function: whatIsNull] }
```

*Null with apply and spread syntax*

```js
let a = [1,2,3,4]

console.log(Math.max.apply(null, a)) //4 (works as Spread Syntax)
console.log(Math.max(...a))  //4
```





10. **Example of Lost Context and how to fix it**

    ```js
    function helpINeedContext(someFunc) {
      console.log(`Is there something here?: ${someFunc()}`); 
    }
    
    function starterFunc() {
      let aHiddenObj = {
        name: "I'm inside a hidden object, ",
        description: 'and you can only access me if you provide me as context',
        info() {
          return this.name + this.description;
        },
      };
    
      helpINeedContext(aHiddenObj.info); //line 14
    }
    
    starterFunc(); //Is there something here?: NaN
    ```

    Two ways to fix it: 

```js
line 14 : helpINeedContext(aHiddenObj.info.bind(aHiddenObj)); //Is there something here?: I'm inside a hidden object, and you can only access me if you provide me as context  
```

or

```js
function helpINeedContext(someFunc, context) { //add context param
  console.log(`Is there something here?: ${someFunc.call(context)}`); 
}

function starterFunc() {
  let aHiddenObj = {
    name: "I'm inside a hidden object, ",
    description: 'and you can only access me if you provide me as context',
    info() {
      return this.name + this.description;
    },
  };

  helpINeedContext(aHiddenObj.info, aHiddenObj); //provide context arg
}

starterFunc();  //Is there something here?: I'm inside a hidden object, and you can only access me if you provide me as context  
```



*Function inside a method in an object*

Here is an example of a function that is defined internally within another function that is a property of an object. Because a function's implicit execution context is the global object, if we don't provide an explicit context, we will not be able to access the object's properties from the internal function.

```js
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    function internalFunc() {
      console.log(this.someProp);
    };
    
    internalFunc()
  },
};

nestedFuncExample.funcProp(); //undefined
```

Just a quick side note: If we were to use an arrow function internally, this would work. This is because arrow functions don't have their own bindings to `this`, therefor `this` internally will be the same `this` from `funcProp`, which is the object `nestedFuncExample`. 

```js
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    internalFunc = () => {
      console.log(this.someProp);
    };
    
    internalFunc()
  },
};

nestedFuncExample.funcProp(); //You accessed me!
```

*Ways to fix it*

1. Use `self = this`

```js
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    self = this

    function internalFunc() {
      console.log(self.someProp);
    };
    
    internalFunc()
  },
};

nestedFuncExample.funcProp(); //You accessed me!
```

2. Use `bind`

```js
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    let internalFunc = (function() {
      console.log(this.someProp);
    }).bind(this);
    
    internalFunc()
  },
};

nestedFuncExample.funcProp(); //You accessed me!
```

3. Use `call`

```js
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    function internalFunc() {
      console.log(this.someProp);
    };
    
    internalFunc.call(this)
  },
};

nestedFuncExample.funcProp(); //You accessed me!
```



**Iterative Function inside a method**

```js
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    [1,2].forEach(function(num) {
      console.log(this.someProp)
    })
  },
};

nestedFuncExample.funcProp(); 
/*
undefined
undefined
*/
```

Because an anonymous function is being passed to forEach, the context for that function is the global object. Functions passed as arguments lose their surrounding context.

Ways to fix it:

1. Use `self = this`

```js
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    self = this;

    [1,2].forEach(function(num) {
      console.log(self.someProp)
    })
  },
};

nestedFuncExample.funcProp(); 
/*
You accessed me!
You accessed me!
*/
```

or 

2. Use `bind`

```
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    [1,2].forEach(function(num) {
      console.log(this.someProp)
    }.bind(this))
  },
};

nestedFuncExample.funcProp(); 
/*
You accessed me!
You accessed me!
*/
```

or 

3. Use the optional context `thisArg` argument on `map`, `every`, or `some`

```
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    [1,2].forEach(function(num) {
      console.log(this.someProp)
    }, this)
  },
};

nestedFuncExample.funcProp(); 
/*
You accessed me!
You accessed me!
*/
```

or 

4. Use an arrow function which looks to the surrounding environment to resolve its' execution context.

```js
let nestedFuncExample = {
  someProp : 'You accessed me!',
  funcProp() {
    [1,2].forEach(num => {
      console.log(this.someProp)
    })
  },
};

nestedFuncExample.funcProp(); 
/*
You accessed me!
You accessed me!
*/
```









What is the global object, and how can we access it?

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