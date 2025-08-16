---
sidebar_position: 12
title: for...in and for...of loop constructs
---

## `for...in` Loop

* **Iterates over enumerable properties** of an object
* Returns **property names/keys** (as strings)
* Works with objects, arrays, and other enumerable structures
* Includes inherited enumerable properties from the prototype chain

```js
const obj = { a: 1, b: 2, c: 3 };
for (let key in obj) {
    console.log(key); // "a", "b", "c"
    console.log(obj[key]); // 1, 2, 3
}

const arr = ['x', 'y', 'z'];
for (let index in arr) {
    console.log(index); // "0", "1", "2" (strings!)
    console.log(arr[index]); // "x", "y", "z"
}
```

## `for...of` Loop

* **Iterates over iterable objects** (arrays, strings, Maps, Sets, etc.)
* Returns **values** directly
* Only works with objects that implement the iterable protocol
* Does not iterate over object properties

```js
const arr = ['x', 'y', 'z'];
for (let value of arr) {
    console.log(value); // "x", "y", "z"
}

const str = "hello";
for (let char of str) {
    console.log(char); // "h", "e", "l", "l", "o"
}

// This would throw an error:
const obj = { a: 1, b: 2 };
// for (let value of obj) {} // TypeError: obj is not iterable
```