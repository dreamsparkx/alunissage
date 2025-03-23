---
sidebar_position: 3
---

# "this"

Objects are usually created to represent entities of the real world, like users, orders and so on:

```js
let user = {
  name: "John",
  age: 30
};
```

And, in the real world, a user can act: select something from the shopping cart, login, logout etc.

Actions are represented in JavaScript by functions in properties.

## Method examples

```js {6-8}
let user = {
  name: "John",
  age: 30
};

user.sayHi = function() {
  alert("Hello!");
};

user.sayHi(); // Hello!
```

Here we’ve just used a Function Expression to create a function and assign it to the property `user.sayHi` of the object.

Then we can call it as `user.sayHi()`. The user can now speak!

A function that is a property of an object is called its *method*.

So, here we’ve got a method `sayHi` of the object `user`.

Of course, we could use a pre-declared function as a method, like this:

```js {5-11}
let user = {
  // ...
};

// first, declare
function sayHi() {
  alert("Hello!");
}

// then add as a method
user.sayHi = sayHi;

user.sayHi(); // Hello!
```

## Method shorthand

There exists a shorter syntax for methods in an object literal:

```js {11}
// these objects do the same

user = {
  sayHi: function() {
    alert("Hello");
  }
};

// method shorthand looks better, right?
user = {
  sayHi() { // same as "sayHi: function(){...}"
    alert("Hello");
  }
};
```

## “this” in methods

It’s common that an object method needs to access the information stored in the object to do its job.

For instance, the code inside `user.sayHi()` may need the name of the `user`.

**To access the object, a method can use the this keyword.**

The value of `this` is the object “before dot”, the one used to call the method.

```js {6-7}
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // "this" is the "current object"
    alert(this.name);
  }

};

user.sayHi(); // John
```

Here during the execution of `user.sayHi()`, the value of `this` will be `user`.

Technically, it’s also possible to access the object without this, by referencing it via the outer variable:

```js {6}
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert(user.name); // "user" instead of "this"
  }

};
```
…But such code is unreliable. If we decide to copy `user` to another variable, e.g. `admin = user` and overwrite `user` with something else, then it will access the wrong object.

```js {6,15}
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert( user.name ); // leads to an error
  }

};


let admin = user;
user = null; // overwrite to make things obvious

admin.sayHi(); // TypeError: Cannot read property 'name' of null
```
If we used `this.name` instead of `user.name` inside the `alert`, then the code would work.

## “this” is not bound

Keyword `this` behaves unlike most other programming languages. It can be used in any function, even if it’s not a method of an object.

There’s no syntax error in the following example:
```js
function sayHi() {
  alert( this.name );
}
```

The value of `this` is evaluated during the run-time, depending on the context.

For instance, here the same function is assigned to two different objects and has different “this” in the calls:

```js {8-10}
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

The rule is simple: if `obj.f()` is called, then `this` is `obj` during the call of `f`. So it’s either user or `admin` in the example above.

:::info[Calling without an object: `this == undefined`]
We can even call the function without an object at all:
```js
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```
In this case `this` is `undefined` in strict mode. If we try to access `this.name`, there will be an error.

In non-strict mode the value of `this` in such case will be the global object (`window` in a browser). This is a historical behavior that `"use strict"` fixes.

Usually such call is a programming error. If there’s `this` inside a function, it expects to be called in an object context.
:::

:::info[The consequences of unbound `this`]
If you come from another programming language, then you are probably used to the idea of a “bound this”, where methods defined in an object always have this referencing that object.

In JavaScript this is “free”, its value is evaluated at call-time and does not depend on where the method was declared, but rather on what object is “before the dot”.

The concept of run-time evaluated this has both pluses and minuses. On the one hand, a function can be reused for different objects. On the other hand, the greater flexibility creates more possibilities for mistakes.

Here our position is not to judge whether this language design decision is good or bad. We’ll understand how to work with it, how to get benefits and avoid problems.
:::

## Arrow functions have no “this”

Arrow functions are special: they don’t have their “own” `this`. If we reference `this` from such a function, it’s taken from the outer “normal” function.

For instance, here `arrow()` uses `this` from the outer `user.sayHi()` method:

```js
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```

That’s a special feature of arrow functions, it’s useful when we actually do not want to have a separate `this`, but rather to take it from the outer context.

Arrow functions do **not** have their own `this`. Instead, they inherit `this` from the surrounding lexical context where they are defined.

## Example:

With Arrow Function (`() => {}`)

```js
let user = {
  name: 'my',
  ss: () => {
    console.log(this.name);
  }
};
user.ss(); // Output: undefined
```
* In this case, `this` refers to the outer lexical scope, which is likely the global object (`window` in browsers or `global` in Node.js).
* In the global scope, `this.name` is `undefined` unless you explicitly define it there.

With regular function (`function() {}`):

Regular functions have their own `this` context, which is dynamic and depends on how the function is called.

```js
let user = {
  name: 'my',
  ss: function() {
    console.log(this.name);
  }
};
user.ss(); // Output: 'my'
```
* Here, `this` refers to the object that called the function, which is `user`.
* `user.ss()` sets `this` to the `user` object, so `this.name` logs `'my'`.

## Summary

* Functions that are stored in object properties are called “methods”.
* Methods allow objects to “act” like `object.doSomething()`.
* Methods can reference the object as `this`.

The value of `this` is defined at run-time.

* When a function is declared, it may use `this`, but that `this` has no value until the function is called.
* A function can be copied between objects.
* When a function is called in the “method” syntax: `object.method()`, the value of `this` during the call is `object`.
* **Arrow functions**: `this` is **lexically** inherited — it does not depend on how the function is called.
* **Regular functions**: `this` is **dynamically** determined — it depends on the caller.