## `this` refers to the global object
* in browser: `this` refers to the `window` obj
* in nodejs: `this` refers to the `global` obj
## In strict mode
`this` refers to `undefined`
## Function invoked with `new` keyword
The function is known as constructor and returns a new instance, `this` refers to a newly created instance
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
## Object invoked
`this` refers to the object itself
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

## Fat arrow function
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