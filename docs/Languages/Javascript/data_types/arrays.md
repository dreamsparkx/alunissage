---
sidebar_position: 3
title: Arrays
---

## Declaration

```js
let arr = new Array();
let arr = [];
```

```js
let fruits = ["Apple", "Orange", "Plum"];
```

Array elements are numbered, starting with zero.

We can get an element by its number in square brackets:

```js
let fruits = ["Apple", "Orange", "Plum"];

alert( fruits[0] ); // Apple
alert( fruits[1] ); // Orange
alert( fruits[2] ); // Plum
```

We can replace an element:

```js
fruits[2] = 'Pear'; // now ["Apple", "Orange", "Pear"]
```

…Or add a new one to the array:

```js
fruits[3] = 'Lemon'; // now ["Apple", "Orange", "Pear", "Lemon"]
```

The total count of the elements in the array is its `length`:
```js
let fruits = ["Apple", "Orange", "Plum"];

alert( fruits.length ); // 3
```
We can also use `alert` to show the whole array.
```js
let fruits = ["Apple", "Orange", "Plum"];

alert( fruits ); // Apple,Orange,Plum
```
An array can store elements of any type.

For instance:
```js
// mix of values
let arr = [ 'Apple', { name: 'John' }, true, function() { alert('hello'); } ];

// get the object at index 1 and then show its name
alert( arr[1].name ); // John

// get the function at index 3 and run it
arr[3](); // hello
```

## Get last elements with “at”

Let’s say we want the last element of the array.

Some programming languages allow the use of negative indexes for the same purpose, like `fruits[-1]`.

Although, in JavaScript it won’t work. The result will be `undefined`, because the index in square brackets is treated literally.

We can explicitly calculate the last element index and then access it: `fruits[fruits.length - 1]`.

```js
let fruits = ["Apple", "Orange", "Plum"];

alert( fruits[fruits.length-1] ); // Plum
```
A bit cumbersome, isn’t it? We need to write the variable name twice.

Luckily, there’s a shorter syntax: `fruits.at(-1)`:
```js
let fruits = ["Apple", "Orange", "Plum"];

// same as fruits[fruits.length-1]
alert( fruits.at(-1) ); // Plum
```

## Methods pop/push, shift/unshift

A **queue** is one of the most common uses of an array. In computer science, this means an ordered collection of elements which supports two operations:

* `push` appends an element to the end.
* `shift` get an element from the beginning, advancing the queue, so that the 2nd element becomes the 1st.

Arrays support both operations.

In practice we need it very often. For example, a queue of messages that need to be shown on-screen.

There’s another use case for arrays – the data structure named **stack**.

It supports two operations:

* `push` adds an element to the end.
* `pop` takes an element from the end.

So new elements are added or taken always from the “end”.

A stack is usually illustrated as a pack of cards: new cards are added to the top or taken from the top:

For stacks, the latest pushed item is received first, that’s also called **LIFO (Last-In-First-Out)** principle. For queues, we have **FIFO (First-In-First-Out)**.

Arrays in JavaScript can work both as a queue and as a stack. They allow you to add/remove elements, both to/from the beginning or the end.

In computer science, the data structure that allows this, is called **deque**.

### Methods that work with the end of the array:

**`pop`**
```js
let fruits = ["Apple", "Orange", "Pear"];

alert( fruits.pop() ); // remove "Pear" and alert it

alert( fruits ); // Apple, Orange
```

Both `fruits.pop()` and `fruits.at(-1)` return the last element of the array, but `fruits.pop()` also modifies the array by removing it.

**`push`**
```js
let fruits = ["Apple", "Orange"];

fruits.push("Pear");

alert( fruits ); // Apple, Orange, Pear
```
The call `fruits.push(...)` is equal to `fruits[fruits.length] = ....`

### Methods that work with the beginning of the array:

**`shift`**

```js
let fruits = ["Apple", "Orange", "Pear"];

alert( fruits.shift() ); // remove Apple and alert it

alert( fruits ); // Orange, Pear
```
**`unshift`**

```js
let fruits = ["Orange", "Pear"];

fruits.unshift('Apple');

alert( fruits ); // Apple, Orange, Pear
```
Methods `push` and `unshift` can add multiple elements at once:
```js
let fruits = ["Apple"];

fruits.push("Orange", "Peach");
fruits.unshift("Pineapple", "Lemon");

// ["Pineapple", "Lemon", "Apple", "Orange", "Peach"]
alert( fruits );
```

## Internals

An array is a special kind of object. The square brackets used to access a property `arr[0]` actually come from the object syntax. That’s essentially the same as `obj[key]`, where `arr` is the object, while numbers are used as keys.

They extend objects providing special methods to work with ordered collections of data and also the `length` property. But at the core it’s still an object.

For instance, it is copied by reference:

```js
let fruits = ["Banana"]

let arr = fruits; // copy by reference (two variables reference the same array)

alert( arr === fruits ); // true

arr.push("Pear"); // modify the array by reference

alert( fruits ); // Banana, Pear - 2 items now
```

…But what makes arrays really special is their internal representation. The engine tries to store its elements in the contiguous memory area, one after another, and there are other optimizations as well, to make arrays work really fast.

But they all break if we quit working with an array as with an “ordered collection” and start working with it as if it were a regular object.

For instance, technically we can do this:

```js
let fruits = []; // make an array

fruits[99999] = 5; // assign a property with the index far greater than its length

fruits.age = 25; // create a property with an arbitrary name
```
That’s possible, because arrays are objects at their base. We can add any properties to them.

But the engine will see that we’re working with the array as with a regular object. Array-specific optimizations are not suited for such cases and will be turned off, their benefits disappear.

The ways to misuse an array:

* Add a non-numeric property like `arr.test = 5`.
* Make holes, like: add `arr[0]` and then `arr[1000]` (and nothing between them).
* Fill the array in the reverse order, like `arr[1000]`, `arr[999]` and so on.

Please think of arrays as special structures to work with the ordered data. They provide special methods for that. Arrays are carefully tuned inside JavaScript engines to work with contiguous ordered data, please use them this way. And if you need arbitrary keys, chances are high that you actually require a regular object {}.

## Performance

Methods `push/pop` run fast, while `shift/unshift` are slow.

Why is it faster to work with the end of an array than with its beginning? Let’s see what happens during the execution:
```js
fruits.shift(); // take 1 element from the start
```
It’s not enough to take and remove the element with the index 0. Other elements need to be renumbered as well.

The shift operation must do 3 things:

1. Remove the element with the index `0`.
2. Move all elements to the left, renumber them from the index `1` to `0`, from `2` to `1` and so on.
3. Update the `length` property.

### The more elements in the array, the more time to move them, more in-memory operations.

The similar thing happens with `unshift`: to add an element to the beginning of the array, we need first to move existing elements to the right, increasing their indexes.

And what’s with `push/pop`? They do not need to move anything. To extract an element from the end, the `pop` method cleans the index and shortens `length`.

The actions for the `pop` operation:

```js
fruits.pop(); // take 1 element from the end
```

**The `pop` method does not need to move anything, because other elements keep their indexes. That’s why it’s blazingly fast.**

The similar thing with the `push` method.

## Loops

```js {3}
let arr = ["Apple", "Orange", "Pear"];

for (let i = 0; i < arr.length; i++) {
  alert( arr[i] );
}
```

But for arrays there is another form of loop, `for..of`:

```js
let fruits = ["Apple", "Orange", "Plum"];

// iterates over array elements
for (let fruit of fruits) {
  alert( fruit );
}
```

Technically, because arrays are objects, it is also possible to use for..in:
```js {3}
let arr = ["Apple", "Orange", "Pear"];

for (let key in arr) {
  alert( arr[key] ); // Apple, Orange, Pear
}
```
But that’s actually a bad idea. There are potential problems with it:
1. The loop for..in iterates over all properties, not only the numeric ones.

There are so-called “array-like” objects in the browser and in other environments, that look like arrays. That is, they have `length` and indexes properties, but they may also have other non-numeric properties and methods, which we usually don’t need. The `for..in` loop will list them though. So if we need to work with array-like objects, then these “extra” properties can become a problem.

2. The `for..in` loop is optimized for generic objects, not arrays, and thus is 10-100 times slower. Of course, it’s still very fast. The speedup may only matter in bottlenecks. But still we should be aware of the difference.

Generally, we shouldn’t use `for..in` for arrays.

## A word about “length”

The `length` property automatically updates when we modify the array. To be precise, it is actually not the count of values in the array, but the greatest numeric index plus one.

For instance, a single element with a large index gives a big length:
```js
let fruits = [];
fruits[123] = "Apple";

alert( fruits.length ); // 124
```
Note that we usually don’t use arrays like that.

Another interesting thing about the `length` property is that it’s writable.

If we increase it manually, nothing interesting happens. But if we decrease it, the array is truncated. The process is irreversible, here’s the example:

```js
let arr = [1, 2, 3, 4, 5];

arr.length = 2; // truncate to 2 elements
alert( arr ); // [1, 2]

arr.length = 5; // return length back
alert( arr[3] ); // undefined: the values do not return
```
So, the simplest way to clear the array is: `arr.length = 0;`.

## new Array()

```js
let arr = new Array("Apple", "Pear", "etc");
```

It’s rarely used, because square brackets `[]` are shorter. Also, there’s a tricky feature with it.

If new Array is called with a single argument which is a number, then it creates an array without items, but with the given length.

Let’s see how one can shoot themselves in the foot:
```js
let arr = new Array(2); // will it create an array of [2] ?

alert( arr[0] ); // undefined! no elements.

alert( arr.length ); // length 2
```

## Multidimensional arrays

```js
let matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

alert( matrix[0][1] ); // 2, the second value of the first inner array
```

## toString

```js
let arr = [1, 2, 3];

alert( arr ); // 1,2,3
alert( String(arr) === '1,2,3' ); // true

alert( [] + 1 ); // "1"
alert( [1] + 1 ); // "11"
alert( [1,2] + 1 ); // "1,21"
```

Arrays do not have Symbol.toPrimitive, neither a viable valueOf, they implement only toString conversion, so here [] becomes an empty string, [1] becomes "1" and [1,2] becomes "1,2".

When the binary plus "+" operator adds something to a string, it converts it to a string as well, so the next step looks like this:
```js
alert( "" + 1 ); // "1"
alert( "1" + 1 ); // "11"
alert( "1,2" + 1 ); // "1,21"
```

## Don’t compare arrays with ==

Arrays in JavaScript, unlike some other programming languages, shouldn’t be compared with operator `==`.

This operator has no special treatment for arrays, it works with them as with any objects.

Let’s recall the rules:

* Two objects are equal == only if they’re references to the same object.
* If one of the arguments of == is an object, and the other one is a primitive, then the object gets converted to primitive.
* …With an exception of null and undefined that equal == each other and nothing else.

The strict comparison === is even simpler, as it doesn’t convert types.

So, if we compare arrays with ==, they are never the same, unless we compare two variables that reference exactly the same array.

```js
alert( [] == [] ); // false
alert( [0] == [0] ); // false
```

These arrays are technically different objects. So they aren’t equal. The == operator doesn’t do item-by-item comparison.

Comparison with primitives may give seemingly strange results as well:

```js
alert( 0 == [] ); // true

alert('0' == [] ); // false
```

Here, in both cases, we compare a primitive with an array object. So the array [] gets converted to primitive for the purpose of comparison and becomes an empty string ''.

```js
// after [] was converted to ''
alert( 0 == '' ); // true, as '' becomes converted to number 0

alert('0' == '' ); // false, no type conversion, different strings
```