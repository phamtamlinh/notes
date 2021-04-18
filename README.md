* [Functional programming](#functional-programming)


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