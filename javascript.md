* [Closures](#closures)
* [Currying](#curry)
* [Functional programming](#functional-programming)
* [`this` in javascript](#this-in-javascript)

### Closures

* A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**).
* A closure gives you access to an outer function's scope from an inner function
* Created every time a function is created at function creation time
* Lexical scoping: in the example below, `displayName` has access to variable `name`
  ```javascript
  function init() {
    var name = 'Mozilla'; // name is a local variable created by init
    function displayName() { // displayName() is the inner function, a closure
      alert(name); // use variable declared in the parent function
    }
    displayName();
  }
  init();
  ```
* Closure
  ```javascript
  function makeFunc() {
    var name = 'Mozilla';
    function displayName() {
      alert(name);
    }
    return displayName;
  }

  var myFunc = makeFunc();
  myFunc();
  ```
  * In some programming languages, `name` should only exist during `makeFunc`. Once `makeFunc()` finishes executing, you might expect that the name variable would no longer be accessible
  * It works in JS because of **closure** (function + lexical environment which consists any local variables)

### Currying

* Currying is a transformation of functions that translates a function from callable as `f(a, b, c)` to callable as `f(a)(b)(c)`

```javascript
function curry(f) { // curry(f) does the currying transform
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}

// usage
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

console.log( curriedSum(1)(2) ); // 3
```

* Partial Application

```javascript
const partialApply = (fn, ...fixedArgs) => {
  return function (...remainingArgs) {
    return fn.apply(this, fixedArgs.concat(remainingArgs));
  };
};

const add = (a, b) => a + b;
const add10 = partialApply(add, 10);
console.log(add10(5)) // 15
```

### Functional programming

* Pure functions instead of shared state & side effects
  * Pure function: Given the same inputs, always returns the same output, and has no side-effects
  * Shared state: often used in OOP, not favored in FP
  * Side effects: any application state change outside the called function other than its return value. E.g. modifying external var, logging, writing to screen, writing to file, triggering any external process, calling functions with side-effects
* Immutability over mutable data
  * An immutable object is an object that can’t be modified after it’s created.
  * Special immutable data structures called **trie data structures** (pronounced “tree”) which are effectively deep frozen. E.g. `Immutable.js`
* Function composition over imperative flow control
* Lots of generic, reusable utilities that use higher order functions to act on many data types instead of methods that only operate on their colocated data
* Declarative rather than imperative code (what to do, rather than how to do it)
  * **Imperative** programs: how to do things. Utilize statements (`for`, `if`, `switch`, `throw`)
    ```
      const doubleMap = numbers => {
      const doubled = [];
      for (let i = 0; i < numbers.length; i++) {
        doubled.push(numbers[i] * 2);
      }
      return doubled;
    };
    ```
  * **Declarative** programs: what to do. Use expressions (`map`, `filter`, `reduce`)
    ```
    const doubleMap = numbers => numbers.map(n => n * 2);
    ```
* Expressions over statements
* Containers & higher order functions over ad-hoc polymorphism

**Sources and further reading**
* [Master the JavaScript Interview: What is Functional Programming?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)

### `this` in javascript

* `this` refers to the global object
  * in browser: `this` refers to the `window` obj
  * in nodejs: `this` refers to the `global` obj
* In strict mode: `this` refers to `undefined`
* Function invoked with `new` keyword  
  * The function is known as constructor and returns a new instance, `this` refers to a newly created instance
  ```javascript
  function Person(fn, ln) {
    this.first_name = fn;
    this.last_name = ln;

    this.displayName = function() {
      console.log(`Name: ${this.first_name} ${this.last_name}`);
    }
  }

  let person = new Person("John", "Reed");
  person.displayName();  // Prints Name: John Reed
  ```
* Object invoked
  * `this` refers to the object itself
  ```javascript
  let user = {
    count: 10,
    foo: function() {
      console.log(this === window);
    }
  }

  user.foo()  // Prints false because now “this” refers to user object instead of global object.
  let fun = user.foo;
  fun() // Prints true as this method is invoked as a simple function.
  ```
  In the example above `fun()` is called in `window` context -> `this` refers to `window` while `user.foo()` iscalled in `user` context -> `this` refers to `user`

* Fat arrow function
  `this` inside fat arrow points to `this` inside upper function
  * without fat arrow
    * anonymous function passed as argument gets its `this` value from the function who is executing it.
    * in the example below, `this` is passed from `map` function which is `window`
    ```javascript
    var object = {
      provider: 'gmail.com',
      usernames: [ 'mike', 'john', 'nina' ],
      getEmails: function() {
        return this.usernames.map( function( username ){
          return username + '@' + this.provider;
        });
      }
    };
    var emails = object.getEmails();
    console.log( emails ); // ["mike@undefined", "john@undefined", "nina@undefined"]
    ```
  * with fat arrow
    * `this` is passed from upper function which is `getEmails` hence `this` refers to `object`
    ```javascript
    var provider = 'yahoo.com';
    var object = {
      provider: 'gmail.com',
      usernames: [ 'mike', 'john', 'nina' ],
      getEmails: function() {
        return this.usernames.map( ( username ) => {
          return username + '@' + this.provider;
        });
      }
    };
    var emails = object.getEmails();
    console.log( emails ); // ["mike@gmail.com", "john@gmail.com", "nina@gmail.com"]
    ```