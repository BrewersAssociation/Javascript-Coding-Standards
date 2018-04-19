
# Brewers Association Coding Standards: Javascript

This document is based on Clean Code Javascript by [Ryan McDermott](https://github.com/ryanmcdermott/clean-code-javascript), Eloqeunt Javascript by Addy Osomani , How to Write Clean Javascript by Noah Stokes

This document will outline the best practices for organizing and styling JS documents for Brewers Association Projects.

## Table of Contents

 - Introduction
 - Write Clean Code (Coding Styles)
 - Variables


## Write Clean Code (Coding Styles)
We try and share coding styles between JS, CSS, and PHP. 
> - [Use tabs over spaces for indents](http://lea.verou.me/2012/01/why-tabs-are-clearly-superior/)
> - 80 character wide columns
> - meaningful whitespace
> - comment your code _link to comment styles_
### Comment Styles

## Variables
### Use meaningful and pronounceable variable names

**Bad:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Good:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```

### Avoid Mental Mapping
Explicit is better than implicit. Related directly to above, using meaningful variable names

**Bad:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**Good:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

### Don't add unneeded context
If your class/object name tells you something, don't repeat that in your
variable name.

**Bad:**
```javascript
const Car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue'
};

function paintCar(car) {
  car.carColor = 'Red';
}
```

**Good:**
```javascript
const Car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue'
};

function paintCar(car) {
  car.color = 'Red';
}
```

### Use default arguments instead of short circuiting or conditionals
Default arguments are often cleaner than short circuiting. Be aware that if you
use them, your function will only provide default values for `undefined`
arguments. Other "falsy" values such as `''`, `""`, `false`, `null`, `0`, and
`NaN`, will not be replaced by a default value.

**Bad:**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}

```

**Good:**
```javascript
function createMicrobrewery(name = 'Hipster Brew Co.') {
  // ...
}
```

## **Functions**

BA Web Sites are huge, and have been edited over time. Because of this the global scope has become polluted with variables sprinkled throughout the site.

As modern javascript developers, we are now refactoring our code with the following best practices in mind.

 1. Use Function Expressions over Declarative Functions
 2. For semi-complex functionality use Self-Executing Anonymous Functions
 3. For complex functionality use the Revealing Module JS Pattern

### Name our Anonymous Functions (Expression Functions)
... as opposed to Function Declaration

Realize when we name use Function Expressions, the function can not be called ABOVE the function definition.

[Named function expressions demystified](http://kangax.github.io/nfe/)

[Intro to Self-executing anonymous functions] (Noah Stokes) (http://esbueno.noahstokes.com/post/77292606977/self-executing-anonymous-functions-or-how-to-write), good lead in to the Revealing Module Pattern

**Old (Function Declaration): **
If the function you are writing could be called ANYWHERE in code, is simple, doesn't create global variables, Declarative Functions are ok. (Most functions in plugins.js and script.js probably fall into this category).
```javascript
getPerson() // works
function getPerson() {
	// do something
	
	return person;
}
getPerson(); // works
```

**Good (Named Function Expression):**
Let's be strict about when a function can be called (probably only used on one page). Note expression is closed with a semicolon.
```javascript
getPerson(); // doesn't work
let person = function getPerson() {
	// do something
	
	return person;
};
getPerson(); // works
person(); //works
```

**Also Good (Self Executing Function Expression):**
Note expression is closed with ```();```
```javascript
// thousand line of codes
// written a year ago

// now you want to add some peice of code
// and you don't know what you have done in the past
// just put the new code in the self executing function
// and don't worry about your variable names

(function () {
    var i = 'I';
    var can = 'CAN';
    var define = 'DEFINE';
    var variables = 'VARIABLES';
    var without = 'WITHOUT';
    var worries = 'WORRIES';

    var statement = [i, can, define, variables, without, worries];

    alert(statement.join(' '));
    // I CAN DEFINE VARIABLES WITHOUT WORRIES
}());
```
### Function arguments (2 or fewer ideally)
Limiting the amount of function parameters is incredibly important because it
makes testing your function easier. Having more than three leads to a
combinatorial explosion where you have to test tons of different cases with
each separate argument.

One or two arguments is the ideal case, and three should be avoided if possible.
Anything more than that should be consolidated. Usually, if you have
more than two arguments then your function is trying to do too much. In cases
where it's not, most of the time a higher-level object will suffice as an
argument.

Since JavaScript allows you to make objects on the fly, without a lot of class
boilerplate, you can use an object if you are finding yourself needing a
lot of arguments.

To make it obvious what properties the function expects, you can use the ES2015/ES6
destructuring syntax. This has a few advantages:

1. When someone looks at the function signature, it's immediately clear what
properties are being used.
2. Destructuring also clones the specified primitive values of the argument
object passed into the function. This can help prevent side effects. Note:
objects and arrays that are destructured from the argument object are NOT
cloned.
3. Linters can warn you about unused properties, which would be impossible
without destructuring.

**Bad:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Good:**
```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```
**[⬆ back to top](#table-of-contents)**


### Functions should do one thing
This is by far the most important rule in software engineering. When functions
do more than one thing, they are harder to compose, test, and reason about.
When you can isolate a function to just one action, they can be refactored
easily and your code will read much cleaner. If you take nothing else away from
this guide other than this, you'll be ahead of many developers.

**Bad:**
```javascript
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**
```javascript
function emailActiveClients(clients) {
  clients
    .filter(isActiveClient)
    .forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**[⬆ back to top](#table-of-contents)**

### Function names should say what they do

**Bad:**
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Good:**
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

> Written with [StackEdit](https://stackedit.io/).
