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



11. **What is the global object, and how can we access it?**

The global object is always defined in JS. It is an object that is available in the global scope. The interface of the global object can change depending on execution environment. Within a browser, the global object is `Window`.  If you are running in Node, the global object is `global`.  

We can access the global object through the keyword `this`.  When we log `this` in the global scope or from a function defined in the global scope, `this` will reference the global object. 





12. **What is a closure? What are the benefits of closures, and how can we create one?**

In JS a closure is created every time a function is created. That is, at the function creation time.  The closure gives access to the lexical environment of the function at time of definition.  All variables/ functions available in the outer scope will still be accessible to the function even if they are inacessible (out of scope) at time of function invocation. A closure will only retain the information relevant to the function. A simple example would be something as follows:

```js
function letsClose() {
  someVar = 'The function has access to me through a closure!';

  return function() {
    console.log(someVar)
  }
}

let returnedFunc = letsClose()
returnedFunc() //The function has access to me through a closure!
```

By allowing functions to retain access to their lexical environment at definition, they provide developers a way to restrict data and enforce purposeful interfaces.  



13. **How can we use closures to create private data? Demonstrate how we can make a variable, secretNumber private, using a closure. Why should we use closures to make data private?**

In this simple example, we return an anonymous function when we invoke the `safe` function.  This anonymous function created a closure with all relevant variables, including `secretNumber`.  Therefor, even though the returned function is invoked in the global scope, it still retains access to the variable declared within the function due to the closure that was formed at the time of function creation. By doing this, we are able to keep `secretNumber` private and only accessible from the returned function. The appropriate interface (the returned function) must be utilized.  This ensures that the private data will not be altered unintentionally, and that is can only be accessed through the function invocation. In this case the returned function uses the secretNumber as an operand and returns a new number.

```js
function safe() {
  let secretNumber = 1234

  return function() {
    return secretNumber + 17
  }
}

let newNum = safe() 
console.log(newNum())
```

In this more complex example, an object is returned upon invocation of the `classroom` function.  A lot is going on here, and closures play a large role in the efficacy of this program.  All of the declared variables/ `generateID` function outside of the returned object are only accessible to the methods through closures. The local variable `id` is private even to the rest of the code within `classroom`. Because these variables and function were in scope at the time of function creation, they remain accessible when the function/ method is invoked elsewhere.  Since we can't access any of the locally scoped variables in the global scope, these are private to the function.  We can only access them the way the developer intended- through the methods defined in the returned object. 

```js
function classroom(teacher, subject) {
  let students = {};
  let generateId = function() {
    let id = 167456523;
    return () => id += 1;
  }();
  let newId = generateId;

  return {
    addStudent(name) {
      students[name] = newId();
      console.log(`${name} is added to the class!`);
    },

    removeStudent(name) {
      let otherStudents = Object.keys(students).filter(el => el !== name);
      let removed = otherStudents.reduce((newObj, key) => {
        newObj[key] = students[key];
        return newObj;
      },{});
      students = removed
      console.log(`${name} has been removed from the class!`);
    },
    
    classInfo() {
      console.log(`\nThe teacher of this ${subject} class is ${teacher}.\n`);
      console.log('Students and last 3 of Student IDs:\n');
      for (student in students) {
        lastDigits = String(students[student]).slice(-3);
        console.log(`${student} : ${lastDigits}`);
      }
    }
  }
}

let newClass = classroom('Mrs. Allen', 'Math');

newClass.addStudent('Chelsea'); //Chelsea is added to the class!
newClass.addStudent('Brandon'); //Brandon is added to the class!
newClass.addStudent('Maka');  //Maka is added to the class!
newClass.addStudent('Jack');  //Jack is added to the class!

newClass.removeStudent('Chelsea')  //Chelsea has been removed from the class!

newClass.classInfo();
/*
The teacher of this Math class is Mrs. Allen.

Students and last 3 of Student IDs:

Brandon : 525
Maka : 526
Jack : 527
*/
```



14. **What is garbage collection? Which values in JS participate in GC? Why do we need to be aware of garbage collection, as software engineers?**

JS automatically allocates memory for objects and frees that memory when they are no longer in use. This freeing of memory is called garbage collection, and happens periodically over the lifetime of the program.  But what makes something eligible for garbage- collection(GC)? 

To better understand, let's discuss where certain things are stored.  JS allocates memory on the stack for primitive values and references/pointers to objects.  The stack is not involved in GC, but has a separate system for returning memory back to the system.  This is a less complex system as primitives are of a fixed size, and the necessary memory space is more easily determined.

Objects, on the other hand, are stored on the heap. These can be of varying sizes that require different memory spaces, and the program can reference these values throughout the program.  This makes it more difficult to determine whether that memory space should be returned to the system.  

The way that JS does this is by evaluating the reference count to objects at certain intervals to see if it is elegible for GC.  When the reference count reaches 0, GC may return that memory back to the system.  Keep in mind that closures and object references may be present, even if not initially obvious.

It's important for developers to be aware of GC because it saves them from manual memory management that was present in some lower level languages. If programmers in these languages didn't free the memory space of unused values, a memory leak would occur.

*see garbage collection in 225 notes for examples*



15. **In the following code, how can we retain access to the value "Steve"? When can JS garbage collect "Steve"?**

```js
function makeHello(name) {  
  return function() {    
    console.log("Hello, " + name + "!");  
  };
}
                         } 
let helloSteve = makeHello("Steve");
```

After the code runs, `helloSteve` references a function that closed over the local variable `name`, which right now contains the string `"Steve"`. Since the closure must keep `name` around, the reference to `"Steve"` must also stick around, which means JS can't GC `"Steve"`. When we call `helloSteve` sometime later, it still has access to `name` and can log its value.

```jsx
helloSteve();  // => Hello, Steve!
```

Before JS can garbage collect `"Steve"`, you must ensure nothing else in the program references `"Steve"`; that includes every closure that has access to the `"Steve"` string. That's not typically a concern, but if you find that you must remove a closure or other reference explicitly, you can assign `null` to the item that references it. For instance:

```jsx
helloSteve = null;
```

That dereferences the closure shown above, which in turn dereferences `"Steve"` through the `name` variable. If nothing else now has a reference to `"Steve"`, JS is free to garbage collect it.

If we modify the code above by introducing a variable prior to returning the anonymous function, the code returns an anonymous function that closes over both the `name` *and* `message` variables. Those variables, in turn, reference the strings `"Steve"` and `"Hello, Steve!"` respectively.

```jsx
function makeHello(name) {
  let message = "Hello, " + name + "!";
  return function() {
    console.log(message);
  };
}

let helloSteve = makeHello("Steve");
```

Theoretically, this will prevent garbage collection on both strings. However, since `name` isn't referenced within the `helloSteve` function, depending on the browser, `"Steve"` might or might not be garbage collected.



16. **What is an IIFE? Give an example. Why would we use an IIFE in code?**

An IIFE, or immediately invoked function expression, is one that we define and invoke concurrently.  Note that the function must be in the form of an expression so there is a returned value that can be immediately invoked.  IIFE's encapsulte code by creating their own scopes, and can help to avoid variable name clashing in large programs.  They also can create private data that only child functions and objects can access.

```js
let generatePassword = function() {
  let starterPassword = 'AE172FS';
  let firstNum = 136;
  let increment = 12;
  
  return function() {
    let final = starterPassword + firstNum;
    firstNum += increment;
    return final;
  }
}()

console.log(generatePassword());
console.log(generatePassword());
console.log(generatePassword());
```

Here we can see that the return value of the IIFE is assigned to the global variable `generatePassword`.  The return value of the IIFE is an anonymous function that returns a password.  The values `starterPassword`, `firstNum` and `increment` are private to the IIFE.  They cannot be accessed outside of the function.  The only way to return a password is through the intended interface.



17. **How can you call an IIFE with an argument?**

```js
let generatePassword = function(increment) {
  let starterPassword = 'AE172FS';
  let firstNum = 136;
  
  return function() {
    let final = starterPassword + firstNum;
    firstNum += increment;
    return final;
  }
}(12)

console.log(generatePassword());
console.log(generatePassword());
console.log(generatePassword());
```



18. **What is a first-class function? A higher order function? Give an example.**

JS employs first- class functions, which aids in this language's versatile nature and allows developers to write expressive and modular code. First-class functions can be assignes to variables, passed as arguments, and returned from functions.

```js
//Function expression assigned to variable 
let someFunc = function() {
  console.log('I can be assigned to other variables too!')
}
someFunc() //'I can be assigned to other variables too!'
let someVar = someFunc //returned function assigned to another variable
console.log(someVar) //[Function: someFunc]
someVar() //'I can be assigned to other variables too!'
```

```js
//function passed as argument 
function addTogether(a, b) {
  return a + b;
}

function addThree(func, b) {
  return func(3, b);
}

console.log(addThree(addTogether, 4));
```

```js
//function returned from function 
function iCanReturnAFunc() {
  let aVar = 'Here I am!';

  return function() {
    console.log(aVar);
  }
}

let returnedFunc = iCanReturnAFunc();
returnedFunc();
```

A higher-order function either takes a function as an argument or returns a function.  

The benefits of utilizing first-class functions are code reusability, abstraction/modularity, and flexibility. 



19. **What concept(s) does the following code demonstrate? How does this work?**

```js
function multiply(first, second) {
  return first * second;
}

function makeMultiply(multiplicand) {
  return function(number) {
    return multiply(multiplicand, number);
  }
}
```

This code demonstrates partial function application.  In this code, we have a function `makeMultiplyN` (generator function) that returns a new anonymous function (applicator) that calls a third function, `multiply`(primary).  The generator function supplies one or more of the arguments that will be passed to the primary function.  The applicator function is able to access these arguments through a closure.  The applicator function supplies the rest of the argument to the primary function. 

Let's see how we can use this to make customizable functions:

```js  
let multiplyByNine = makeMultiply(9)
console.log(multiplyByNine(8)) //72
console.log(multiplyByNine(5)) //45

let multiplyByFive = makeMultiply(5)
console.log(multiplyByFive(8)) //40
console.log(multiplyByFive(5)) ///25
```

We assigned the return value of the generator to a variable. This returned function retains access to the argument passed into the generator via a closure.  Now this returned function can be invoked with a single argument, and the returned primary function is passed all necessary arguments. 

Partial function application allows us to package data and functionality together.  We can even increase the flexibility  of this by altering the code of the generator:

```js
function multiply(first, second) {
  return first * second;
}

function add(first, second) {
  return first + second;
}

function makeCalc(func, multiplicand) {
  return function(number) {
    return func(multiplicand, number);
  }
}

let multiplyByNine = makeCalc(multiply, 9);
console.log(multiplyByNine(8)); //72
console.log(multiplyByNine(5)); //45

let addNine = makeCalc(add, 9);
console.log(addNine(8)); //72
console.log(addNine(5)); //45
```



20. **What is the difference between a constructor function and a regular function?** 

Let's examine how `new` works with a contrcutor function using the code below. 

On lines 1-5 we define a constructor function `NewConst` with two arguments. We can see that the intention of this function is to be a constructor by the capitalized name.  On lines 7-9 we access the `prototype` property available for all functions and define  the method `newMeth`.  All objects created from this constuctor will have access to this method, but this function will only be defined within the constructor's prototype.  On line 11 we use the `new` operator to invoke the `NewConst` constructor function with two arguments. 

The `new` operator will create a new object and set the execution context to this object.  The code within the function will execute, so `this.name` and `this.type` will set new object properties `name` and `type` to the values of the arguments. The new object will then return.  We set this return value to `newObj`.  

When we log `newObj` on line 12 we get what we expect:  `newConst { name: 'Created from a constructor', type: 'object' }`, which is the new object. 

Logging the `prototype` property on the constructor function returns the object that `newObj` will inherit from (JS has prototypal inheritance).

Line 14 checks to make sure the dunder proto property (`__proto__`) on `newObj` is the same as the constructor `prototype` property. This allows for prototypal inheritance, as the objects created from the constructor function now have access to the properties defined up the prototypal chain.

 Line 15 ensures this, by calling the method `newMeth` defined on the constructor's `prototype` property on the new object `newObj`.

Line 16-17 shows that the method `newMeth` is not a property on the new object, but on `NewConst` and is still accessible to the object via prototypal inheritance. 

Line 18 logs `true` as the objects created by `new` from a constructor function are instances of the constructor.

Lines 19-21 access the properties defined on the new objects.  These properties belong the the new object. 

Finally line 22 accesses the `constructor` property defined within the `prototype` property on the constructor. This property points to the constructor function itself. 

While I have used the term prototypal inheritance often, a more appropriate description of this is behavior delegation. Behaviors that are accessible from the objects created from constructors can be delegated up the prototypal chain, so objects can be smaller and only contain unique behaviors. 

```js
function NewConst(name, type) {
  this.name = name; 
  this.type = type;
  console.log('using new keyword will invoke the constructor method');
}

NewConst.prototype.newMeth = function() {
  console.log("I am a method defined on the constructor's prototype object");
}

let newObj = new NewConst('Created from a constructor', 'object'); //using new keyword will invoke the constructor method
console.log(newObj); //newConst { name: 'Created from a constructor', type: 'object' }
console.log(NewConst.prototype); //{ newMeth: [Function (anonymous)] }
console.log(Object.getPrototypeOf(newObj) === NewConst.prototype); //true
newObj.newMeth(); //I am a method defined on the constructor's prototype object
console.log(newObj.hasOwnProperty('newMeth')); //false
console.log(NewConst.prototype.hasOwnProperty('newMeth'));  //true
console.log(newObj instanceof NewConst); //true
console.log(newObj.name); //Created from a constructor
console.log(newObj.type); //object
console.log(Object.getOwnPropertyNames(newObj))  //['name', 'type']
console.log(NewConst.prototype.constructor) //[Function: NewConst]
```

A constructor function is a function generally defined with a capital first letter, which shows that it is intended to be used with the `new` operator.  The `new` operator will create a new object, set the execution context to the new object (`this` inside the constructor function will reference the new object), set the `__proto__` property on the new object to the constructor's `prototype` property, the code within the function will execute, and the new object (`this` ) will be returned if a new object isn't explicitly returned. 

Keep in mind, a constructor function is a function, thus can be invoked without the `new` operator. The execution context within `Job` when not used with `new` will be set to the normal function execution context, in this case the global object.  This code was run in the browser, so the `window` object is the global object. When the function is invoked we are setting properties on the global object, thus when we invoke `info` on the `window` object, "Software Engineer: Back-end" is returned. 

When we invoke the constructor function using the `new` operator, `this` is set to the new object created. Therefore, the properties defined in the method are set to the new object.  The new object is returned and assigned to the global variable `usingNew`. `usingNew` can now invoke the functions defined within itself and its' prototypal chain. 

```js
function Job(title, description) {
  this.title = title;
  this.description = description;
  this.info = function() {
    return (`${this.title}: ${this.description}`);
  };
}

Job('Software Engineer', 'Back-end');
console.log(window.info());  // Software Engineer: Back-end


 let usingNew = new Job('Developer', 'Front-end');
 console.log(usingNew.info()); //Developer: Front-end
```



21. **What does the following code log to the console? **

```js
let a = 1;
let foo;
let obj;

function Foo() {
  this.a = 2;
  this.bar = function() {
    console.log(this.a);
  };
  this.bar();
}

foo = new Foo();

foo.bar();
Foo();

obj = {};
Foo.call(obj);
obj.bar();

console.log(this.a);
```

This logs `2` 6 times. 

Let's start with line 13: The `new` operator will create a new object, set the execution context to the new object, set the `__proto__` property on the new object to the `prototype` property of the constructor function, execute the code in the function, and return the new object unless an object is explitly returned. Because line 10 invoked the `bar1` method defined on the new object and this method logs the `a` property of the new object, `2` is returned (`this.a` on line 6 set a new property `a` to `2`).

Line 13 also set the return value of the constructor function to `foo`. Line 15 invokes the `bar` method on the object `foo`.   Again this logs `2` because `bar` logs the value of the object's `a` property.

Line 16 invokes the `Foo` function as a regular function. Because the implicit execution context of a function is the global object, `this` within the function when run on the browser is `window`.  Within the function then, `a` and `bar` are defined as properties on the window object. Line 10 is then `window.bar()` which will log `2` .

Line 18 creates a new object literal and assigns it to the global variable `obj`.  We then indirectly call the function `Foo` with that object as the explicit execution context using the method `call` .  This will set the properties `a` and `bar` on the new object and call the `bar` method from within the `Foo` function. `2` is returned.  At this point, if we log the new object `obj` we will get `{ a: 2, bar: [Function (anonymous)] }`.  

Calling `bar` on object then will give us `2` again on line 20.

Finally line 21, `this.a` in the global scope will evaluate to `window.a` in the browser as the implicit execution context is the global object.  Because we set the property `a` on the window object to `2` when we invoked the `Foo` function as a regular function, `2` will be returned again. 



23. **What will the following code log and why?** 

```js
let ninja;
function Ninja() {
  this.swung = true;
}

ninja = new Ninja();

Ninja.prototype.swingSword = function() {
  return this.swung;
};

console.log(ninja.swingSword());
```

True: Even though the swingSword method is defined on the prototype after the ninja object is created, the prototype chain lookup happens when the swingSword method is called on the object, and it can be found.



24. **Implement the method described in the comments:**

```js
let ninjaA;
let ninjaB;
function Ninja() {
  this.swung = false;
}

ninjaA = new Ninja();
ninjaB = new Ninja();

// Add a swing method to the Ninja prototype which
// returns the calling object and modifies swung

console.log(ninjaA.swing().swung);      // must log true
console.log(ninjaB.swing().swung);      // must log true
```

```js
Ninja.prototype.swing = function() {
  this.swung = true 
  return this
}
```



25. **What does Object.create do, and how is it used?**

`Object.create` is a static method that allows you to create a new object from an existing object.  The argument passed to the method will be set as the new object's prototype. This style of object creation is called OLOO, or objects linking to other objects. 

```js
let obj1 = {
  name : 'Original Object',
}

let obj2 = Object.create(obj1); 
console.log(obj2); //{}
console.log(obj2.__proto__); //{ name: 'Original Object' }
console.log(Object.getOwnPropertyNames(obj1)); //[ 'name' ]
console.log(Object.getOwnPropertyNames(obj2)); //[]
console.log(obj2.name) //Original Object

obj1.newName = function(name) {
  this.name = name;
  return this.name;
}

console.log(obj2); //{}
console.log(obj2.__proto__); //{ name: 'Original Object', newName: [Function (anonymous)] }
console.log(Object.getOwnPropertyNames(obj1)); //[ 'name', 'newName' ]
console.log(Object.getOwnPropertyNames(obj2)); //[]
console.log(obj2.newName('Objects new name')); //Objects new name
```

```js
let myChair = {
  init(width, height) {
    this.width = width;
    this.height = height;
    return this;
  }
};
let anotherChair = Object.create(myChair).init(40,60);

console.log(myChair)  //{ init: [Function: init] }
console.log(Object.getOwnPropertyNames(anotherChair)) //[ 'width', 'height' ]
console.log(Object.getPrototypeOf(anotherChair) === myChair) //true
console.log(anotherChair)  //{ width: 40, height: 60 }
```

We can also define an object with an `init` method that can be called to initialize properties on object's that use it as a prototype (works like a constructor).  Because execution context is set at function/ method invocation, `this` within the `init` method will reference the calling object.  In this example, `anotherChair` will have access to the `init` method defined within `myChair` because it was created using `Object.create`.  This static method set `myChair` as the prototype to `anotherChair`.  The `init` behavior is not copied within the new object, instead the behavior is delegated to the parent object. `this` will resolve to `anotherChair` and the porperties `width` and `height` will be set to the values passed in as arguments and belong to the object `anotherChair`. 



26. **What is the function.prototype?**

```js
function Const(name) {
  this.name = name 
  this.copiedMeth = function() {
    console.log('This method is copied in each new object.')
  }
}

Const.prototype.delegatedMeth = function() {
  console.log('There is only one copy of this delegated method in Const.')
}

let newObj = new Const('First Object')
console.log(newObj) //Const { name: 'First Object', copiedMeth: [Function (anonymous)] }
console.log(Const.prototype) //{ delegatedMeth: [Function (anonymous)] }
console.log(Const.prototype === Object.getPrototypeOf(newObj)) //true
console.log(newObj.hasOwnProperty('delegatedMeth')) //false
console.log(Const.prototype.hasOwnProperty('delegatedMeth')) //true
console.log(Const.prototype.constructor) //[Function: Const]
console.log(newObj.constructor) //[Function: Const]
console.log(newObj instanceof Const) //true
```

Because function are objects, they can have properties that are accessible and alterable.  One such property is the `prototype` property, which is useful when the function is used as a constructor. The `prototype` property of a function has a built- in property, `constructor`, that points to the function itself. When we use the `new` operator with a constructor function, the newly created object is assigned to the `prototype` property of the function.  In this way, we can define methods on the function's prototype property as a way to delegate behavior to the objects created from the function. This is called the Prototype Pattern of object creation.

Let's examine the code above..

Within the constructor function `Const` we see a property and a method defined on `this`.  What is `this`? If this function was invoked without the `new` operator, the implicit execution of the function would be the global object. Because we invoked the function as a constructor using `new`, a new object was created and `this` was set to this new object. Therefore, these properties are not defined on the function's prototype object, but instead belong to the newly created object. 

We then define a method on the function's `prototype` property.  This method is not copied in each object created from the constructor, but instead this is a delegated behavior.  When we invoke this method on an instance of `Const`, JS will traverse the prototypal chain until it finds this method in the object assigned to  `Const`'s `prototype` property. 



27. **What is behavior delegation? How does JS implement inheritance differently than Ruby?**

Objects in JS are able to delegate behavior to object's up the prototypal chain.  JS will traverse the prototypal chain when trying to resolve a property/method. Once it is resolved the search will stop. In this way, methods can be overridden by defining them within object's created from another.  This way of delegating behavior to object's upstream is called prototypal inheritance. While it is not true class inheritance that is seen in other. languages such as Ruby, the result is similar. 

The term inheritance in code refers to the ability to offer reusability and reduce redundancy. While a class in Ruby is more like a blueprint for future objects, a prototype in JS is a working object instance. Instead of inheriting from a class, objects inherit directly from eachother in JS.  The prototypal chain is traversing literal objects that have behavior delegated to them.  This way, objects downstream can be much smaller.

We can access an object's prototype via a hidden property `[[Prototype]]`.  We can use the static method `Object.getPrototypeOf` or the deprecated dunder prototype method `__proto__` to access this property.

If we attempt to access a property of an object that is not present, JS will look in the prototype of that object.  It will continue looking at the prototypes until the property is resolved, and then will stop. If it is not found, `undefined` is returned. 



28. **What is OLOO? Give an example. What are the benefits to organizing your code this way?**

    ```js
    var firstObj = { x: 1 };
    
    var secondObj = Object.create(firstObj);
    secondObj.y = 2;
    
    var thirdObj = Object.create(secondObj);
    
    console.log(thirdObj.x + thirdObj.y);  // 3
    console.log(Object.getPrototypeOf(thirdObj) === secondObj) //true
    console.log(Object.getPrototypeOf(secondObj) === firstObj) //true
    console.log(Object.getOwnPropertyNames(thirdObj)) //[]
    
    let objectCreatedFromFunc = Object.getPrototypeOf(firstObj) //created from function Object
    
    console.log(objectCreatedFromFunc) //{constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
    console.log(Object.getPrototypeOf(objectCreatedFromFunc)) //null
    ```

    OLOO is a style of object creation in JS that exposes the true prototypal nature of the language without extra semantics. JS utilizes this chain to delegate behavior upstream which reduces redundancy and links objects together in a modular, sensicle way. Object are linked through  accessibility of the `[[Prototype]]` property defined within every object.  In the above code, we use the static method `Obect.getPrototypeOf` method to access this property.  The object `firstObj` is defined as an object literal with one property `x`.  The object `secondObj` is then created using `Object.create`, which sets the new object's prototype to the argument passed.  The property `y` is defined within this object. Finally a third object is created with the second object as a prototype.  When we attempt to access the two properties defined upstream on the third object we see we are successful! This is because JS will traverse the prototypal chain until the property is resolved (and will return `undefined` if it is not found).  

    We then perform a few sanity checks that show that, yes, the prototypes of the objects are as we defined them. What then is happening here?

    ```js
    let objectCreatedFromFunc = Object.getPrototypeOf(firstObj) //created from function Object
    
    console.log(objectCreatedFromFunc) //{constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
    ```

    Well we already determined that every object has a `[[Prototype]]` property- even those created as object literals. We see that the prototype of this object is a function's prototype property.  Yes!.. the constructor function that creates objects has a `prototype` property that is set as the `[[Prototype]]` of an object literal.  That is why objects have access to all these methods defined here! The top of the chain is `null`, which is what is seen when we access the `[[Prototype]]` property of the constructor function's prototype property.

    The benefits of using the OLOO style of object creation is that we are embracing the prototypal nature of JS without utilizing any "middlemen".  OLOO stands for objects linked to other objects, which is exactly what we are doing.. we are using objects as prototypes that are useful in their own rights. We are delegating behavior and able to dynamically alter these objects as we go.  OLOO dramtically increases code reusability and readability, and allows developers to maintain programs with much more ease. We can modularize and create appropriate interfaces with private data and show a higher level of intention in our code. 

    While there are other syntaxes for object creation in JS, OLOO or Prototypal Inheritance, truly embraces the prototypal nature of JS.

    

29. **What is the Pseudo-Classical Pattern? Give an example. What are the benefits to organizing your code this way?**

The Pseudo-Classical Pattern of object creation utilizes a constructor function and the `new` operator to link object's in the prototypal chain.  The constructor function has a `prototype` property assigned to an object that will be assigned to the newly created object's `[[Prototype]]` property.  This object has a `constructor` property that points to the constructor function itself.  

The `new` operator does a list of things when invoked on a constructor function.  First it will create a new object. It then changes the execution context of the function to that of the newly created object (`this` within the function now points to the new object).  It then sets the object assigned to the function's `prototype` property to the `[[Prototype]]` property of the new object. The code within the function is executed and the new object is returned and can be assigned to a variable (unless a different object is explicitly returned).

```js
function Const(name) {
  this.name = name 
  this.copiedMeth = function() {
    console.log('This method is copied in each new object.')
  }
}

Const.prototype.delegatedMeth = function() {
  console.log('There is only one copy of this delegated method in Const.')
}

let newObj = new Const('First Object')
console.log(newObj) //Const { name: 'First Object', copiedMeth: [Function (anonymous)] }
console.log(Const.prototype) //{ delegatedMeth: [Function (anonymous)] }
console.log(Const.prototype === Object.getPrototypeOf(newObj)) //true
console.log(newObj.hasOwnProperty('delegatedMeth')) //false
console.log(Const.prototype.hasOwnProperty('delegatedMeth')) //true
console.log(Const.prototype.constructor) //[Function: Const]
console.log(newObj.constructor) //[Function: Const]
console.log(newObj instanceof Const) //true
```

*See problem 26 for code explanation.*

Some benefits of the Pseudoclassical Pattern is that it provides a way to instantly initialize properties on the new objects without having to run a seperate `init` method. 

Look up the OLOO benefits as well. 



30. **How does ES6 class syntax work? Give an example.** 

ES6 introduced a new class syntax was to emulate classes in other languages.  Under the hood, these classes are actually built upon prototypes.  No matter which syntax is used for object creation, JS is always prototypally-based in terms of inheritance. Objects inherit directly from other objects and behavior is delegated up the prototypal chain. 

```js
class Shop {
  constructor(name, type) {
    this.name = name;
    this.type = type;
    this.inventory = []
  }

  info() {
   console.log(`${this.name} is a ${this.type} shop.`);
  }
  
  addInventory(name, amount) {
    this.inventory.push({name: name, amount: amount});
  }
  
  logInventory() {
    this.inventory.forEach(item => {
      console.log(`${item.name} : ${item.amount}`)
    })
  }
}

class CoffeeShop extends Shop {
  constructor(name, type, sittingArea) {
    super(name, type) 
    this.sittingArea = sittingArea
  }

  static whatAmI() {
    console.log("I'm a Coffee Shop!")
  }

  openSeating(accessibility) {
    if (this.sittingArea && accessibility) {
      console.log("We have open seating!")
      return 
    } else {
      console.log("Sorry, we don't have open seating.")
    }
  }
}


let starbucks = new CoffeeShop('Starbucks', 'coffee', true)
CoffeeShop.whatAmI()  //I'm a Coffee Shop!
starbucks.info() //Starbucks is a coffee shop.
starbucks.addInventory('French Roast', 150)
starbucks.logInventory() //French Roast : 150

console.log(starbucks instanceof Shop) //true 
console.log(starbucks instanceof CoffeeShop) //true
console.log(Object.getPrototypeOf(starbucks)) //Shop {}
console.log(starbucks.constructor) //[class CoffeeShop extends Shop]
console.log(typeof starbucks.constructor) //function
console.log(Object.getOwnPropertyNames(Shop.prototype)) //[ 'constructor', 'info', 'addInventory', 'logInventory' ]
console.log(Object.getOwnPropertyNames(CoffeeShop.prototype)) //[ 'constructor', 'openSeating' ]
console.log(Object.getOwnPropertyNames(CoffeeShop)) //[ 'length', 'name', 'prototype', 'whatAmI' ]
console.log(starbucks) 
/*
CoffeeShop {
  name: 'Starbucks',
  type: 'coffee',
  inventory: [ { name: 'French Roast', amount: 150 } ],
  sittingArea: true
}
*/
```



31. **What is a module and why use them?**

Modules in JS are used to break code up into separate files. This allows for easier code maintenance in larger programs, and provides better manageability and structure when working with a team.  Having similar code in contained modules leads to better encapsulation and data protection because anything that is to be exported and imported must be done so explicitly.

CommonJS modules load synchronously, so are not suitable in the browser. Transpilers, such as Babel, enable CommonJS to be used in the browser.  This syntax uses `require` and `exports` :

```js
//In file with modularized code mod.js
function imInModule() {
  console.log("I'm from the module.");
}

module.exports = imInModule;

//In main file home.js
let logItHere = require("./mod.js")
logItHere() //"I'm from the module."
```



The second type of JS modules are called ES or JSModules, which are asynchronous and use `import` and `export` statements to make values accessible from the modules.  Asynchronous loading allows for concurrent loading of multiple modules, which is much more efficient.  ESModules were created as a way to standardize the module system, but because Node.js has supported CommonJS, you will still see plenty of modules written this way. Also it's important to keep in mind that older versions of Node.js don't support ESModules.  There are ways to ensure that ESModules are backward- compatible, but keeping compatability issues in mind is critical.

```js
//mod.js

let someVar = "I'm inside the module!"; 

export function fromTheMod() {
  console.log(someVar);


//home.js
import { fromTheMod } from "./mod.js";


fromTheMod(); //I'm inside the module!
```









Questions- 

15. So are strings/primitives up for GC?