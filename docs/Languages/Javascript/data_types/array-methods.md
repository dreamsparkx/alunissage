---
sidebar_position: 4
title: Array Methods
---

## Add/remove items

* `arr.push(...items)` – adds items to the end,
* `arr.pop()` – extracts an item from the end,
* `arr.shift()` – extracts an item from the beginning,
* `arr.unshift(...items)` – adds items to the beginning.

## splice

How to delete an element from the array?

The arrays are objects, so we can try to use `delete`:

```js
let arr = ["I", "go", "home"];

delete arr[1]; // remove "go"

alert( arr[1] ); // undefined

// now arr = ["I",  , "home"];
alert( arr.length ); // 3
```
The element was removed, but the array still has 3 elements, we can see that arr.length == 3.

That’s natural, because delete obj.key removes a value by the key. It’s all it does. Fine for objects. But for arrays we usually want the rest of the elements to shift and occupy the freed place. We expect to have a shorter array now.

So, special methods should be used.

**The arr.splice method is a Swiss army knife for arrays. It can do everything: insert, remove and replace elements.**

```js
arr.splice(start[, deleteCount, elem1, ..., elemN])
```
It modifies `arr` starting from the index `start`: removes `deleteCount` elements and then inserts `elem1, ..., elemN` at their place. Returns the array of removed elements.

```js {3}
let arr = ["I", "study", "JavaScript"];

arr.splice(1, 1); // from index 1 remove 1 element

alert( arr ); // ["I", "JavaScript"]
```
Starting from the index 1 it removed 1 element.

In the next example, we remove 3 elements and replace them with the other two:
```js
let arr = ["I", "study", "JavaScript", "right", "now"];

// remove 3 first elements and replace them with another
arr.splice(0, 3, "Let's", "dance");

alert( arr ) // now ["Let's", "dance", "right", "now"]
```

Here we can see that `splice` returns the array of removed elements:
```js
let arr = ["I", "study", "JavaScript", "right", "now"];

// remove 2 first elements
let removed = arr.splice(0, 2);

alert( removed ); // "I", "study" <-- array of removed elements
```
The `splice` method is also able to insert the elements without any removals. For that, we need to set `deleteCount` to `0`:
```js
let arr = ["I", "study", "JavaScript"];

// from index 2
// delete 0
// then insert "complex" and "language"
arr.splice(2, 0, "complex", "language");

alert( arr ); // "I", "study", "complex", "language", "JavaScript"
```
:::info[Negative indexes allowed]
Here and in other array methods, negative indexes are allowed. They specify the position from the end of the array, like here:
```js
let arr = [1, 2, 5];

// from index -1 (one step from the end)
// delete 0 elements,
// then insert 3 and 4
arr.splice(-1, 0, 3, 4);

alert( arr ); // 1,2,3,4,5
```
:::
## slice
```js
arr.slice([start], [end])
```
It returns a new array copying to it all items from index `start` to `end` (not including `end`). Both `start` and `end` can be negative, in that case position from array end is assumed.

It’s similar to a string method `str.slice`, but instead of substrings, it makes subarrays.
```js
let arr = ["t", "e", "s", "t"];

alert( arr.slice(1, 3) ); // e,s (copy from 1 to 3)

alert( arr.slice(-2) ); // s,t (copy from -2 till the end)
```
We can also call it without arguments: `arr.slice()` creates a copy of `arr`. That’s often used to obtain a copy for further transformations that should not affect the original array.
## concat
The method arr.concat creates a new array that includes values from other arrays and additional items.
```js
arr.concat(arg1, arg2...)
```
It accepts any number of arguments – either arrays or values.

The result is a new array containing items from `arr`, then `arg1`, `arg2` etc.

If an argument `argN` is an array, then all its elements are copied. Otherwise, the argument itself is copied.
```js
let arr = [1, 2];

// create an array from: arr and [3,4]
alert( arr.concat([3, 4]) ); // 1,2,3,4

// create an array from: arr and [3,4] and [5,6]
alert( arr.concat([3, 4], [5, 6]) ); // 1,2,3,4,5,6

// create an array from: arr and [3,4], then add values 5 and 6
alert( arr.concat([3, 4], 5, 6) ); // 1,2,3,4,5,6
```
Normally, it only copies elements from arrays. Other objects, even if they look like arrays, are added as a whole:
```js
let arr = [1, 2];

let arrayLike = {
  0: "something",
  length: 1
};

alert( arr.concat(arrayLike) ); // 1,2,[object Object]
```
## Iterate: forEach
The arr.forEach method allows to run a function for every element of the array.
```js
arr.forEach(function(item, index, array) {
  // ... do something with an item
});
```
```js
// for each element call alert
["Bilbo", "Gandalf", "Nazgul"].forEach(alert);
```
```js
["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
  alert(`${item} is at index ${index} in ${array}`);
});
```
## Searching in array
### indexOf/lastIndexOf and includes
The methods arr.indexOf and arr.includes have the similar syntax and do essentially the same as their string counterparts, but operate on items instead of characters:
* `arr.indexOf(item, from)` – looks for `item` starting from index `from`, and returns the index where it was found, otherwise `-1`.
* `arr.includes(item, from)` – looks for `item` starting from index `from`, returns `true` if found.
```js
let arr = [1, 0, false];

alert( arr.indexOf(0) ); // 1
alert( arr.indexOf(false) ); // 2
alert( arr.indexOf(null) ); // -1

alert( arr.includes(1) ); // true
```
Please note that indexOf uses the strict equality === for comparison. So, if we look for false, it finds exactly false and not the zero.

If we want to check if item exists in the array and don’t need the index, then arr.includes is preferred.
The method arr.lastIndexOf is the same as indexOf, but looks for from right to left.
```js
let fruits = ['Apple', 'Orange', 'Apple']

alert( fruits.indexOf('Apple') ); // 0 (first Apple)
alert( fruits.lastIndexOf('Apple') ); // 2 (last Apple)
```
:::info[The `includes` method handles `NaN` correctly]
A minor, but noteworthy feature of includes is that it correctly handles NaN, unlike indexOf:
```js
const arr = [NaN];
alert( arr.indexOf(NaN) ); // -1 (wrong, should be 0)
alert( arr.includes(NaN) );// true (correct)
```
:::
### find and findIndex/findLastIndex
```js
let result = arr.find(function(item, index, array) {
  // if true is returned, item is returned and iteration is stopped
  // for falsy scenario returns undefined
});
```
If it returns `true`, the search is stopped, the `item` is returned. If nothing is found, `undefined` is returned.
```js
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

let user = users.find(item => item.id == 1);

alert(user.name); // John
```
The arr.findIndex method has the same syntax but returns the index where the element was found instead of the element itself. The value of `-1` is returned if nothing is found.

The arr.findLastIndex method is like `findIndex`, but searches from right to left, similar to `lastIndexOf`.
```js
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"},
  {id: 4, name: "John"}
];

// Find the index of the first John
alert(users.findIndex(user => user.name == 'John')); // 0

// Find the index of the last John
alert(users.findLastIndex(user => user.name == 'John')); // 3
```
### filter
The `find` method looks for a single (first) element that makes the function return `true`.

If there may be many, we can use arr.filter(fn).

The syntax is similar to `find`, but `filter` returns an array of all matching elements:
```js
let results = arr.filter(function(item, index, array) {
  // if true item is pushed to results and the iteration continues
  // returns empty array if nothing found
});
```
```js
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

// returns array of the first two users
let someUsers = users.filter(item => item.id < 3);

alert(someUsers.length); // 2
```
## Transform an array
### map
The arr.map method is one of the most useful and often used.

It calls the function for each element of the array and returns the array of results.
```js
let result = arr.map(function(item, index, array) {
  // returns the new value instead of item
});
```
```js
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
alert(lengths); // 5,7,6
```
### sort(fn)
The call to arr.sort() sorts the array in place, changing its element order.

It also returns the sorted array, but the returned value is usually ignored, as arr itself is modified.
```js
let arr = [ 1, 2, 15 ];

// the method reorders the content of arr
arr.sort();

alert( arr );  // 1, 15, 2
```
The order became 1, 15, 2. Incorrect. But why?

**The items are sorted as strings by default.**

Literally, all elements are converted to strings for comparisons. For strings, lexicographic ordering is applied and indeed `"2" > "15"`.

To use our own sorting order, we need to supply a function as the argument of `arr.sort()`.
```js
function compare(a, b) {
  if (a > b) return 1; // if the first value is greater than the second
  if (a == b) return 0; // if values are equal
  if (a < b) return -1; // if the first value is less than the second
}
```
```js
function compareNumeric(a, b) {
  if (a > b) return 1;
  if (a == b) return 0;
  if (a < b) return -1;
}

let arr = [ 1, 2, 15 ];

arr.sort(compareNumeric);

alert(arr);  // 1, 2, 15
```
### reverse
```js
let arr = [1, 2, 3, 4, 5];
arr.reverse();

alert( arr ); // 5,4,3,2,1
```
### split and join
```js
let names = 'Bilbo, Gandalf, Nazgul';

let arr = names.split(', ');

for (let name of arr) {
  alert( `A message to ${name}.` ); // A message to Bilbo  (and other names)
}
```
The split method has an optional second numeric argument – a limit on the array length. If it is provided, then the extra elements are ignored. In practice it is rarely used though:
```js
let arr = 'Bilbo, Gandalf, Nazgul, Saruman'.split(', ', 2);

alert(arr); // Bilbo, Gandalf
```
The call arr.join(glue) does the reverse to split. It creates a string of arr items joined by glue between them.
```js
let arr = ['Bilbo', 'Gandalf', 'Nazgul'];

let str = arr.join(';'); // glue the array into a string using ;

alert( str ); // Bilbo;Gandalf;Nazgul
```
### reduce/reduceRight
When we need to iterate over an array – we can use forEach, for or for..of.

When we need to iterate and return the data for each element – we can use map.

The methods arr.reduce and arr.reduceRight also belong to that breed, but are a little bit more intricate. They are used to calculate a single value based on the array.
```js
let value = arr.reduce(function(accumulator, item, index, array) {
  // ...
}, [initial]);
```
The function is applied to all array elements one after another and “carries on” its result to the next call.
* `accumulator` – is the result of the previous function call, equals initial the first time (if initial is provided).
* `item` – is the current array item.
* `index` – is its position.
* `array` – is the array.

As the function is applied, the result of the previous function call is passed to the next one as the first argument.

So, the first argument is essentially the accumulator that stores the combined result of all previous executions. And at the end, it becomes the result of reduce.

Sounds complicated?

The easiest way to grasp that is by example.

Here we get a sum of an array in one line:
```js
let arr = [1, 2, 3, 4, 5];

let result = arr.reduce((sum, current) => sum + current, 0);

alert(result); // 15
```
The function passed to reduce uses only 2 arguments, that’s typically enough.

1. On the first run, `sum` is the `initial` value (the last argument of `reduce`), equals `0`, and `current` is the first array element, equals `1`. So the function result is `1`.
2. On the second run, `sum = 1`, we add the second array element (`2`) to it and return.
3. On the 3rd run, `sum = 3` and we add one more element to it, and so on…

## Array.isArray
Arrays do not form a separate language type. They are based on objects.

So `typeof` does not help to distinguish a plain object from an array:
```js
alert(typeof {}); // object
alert(typeof []); // object (same)
```
…But arrays are used so often that there’s a special method for that: Array.isArray(value). It returns true if the value is an array, and false otherwise.
```js
alert(Array.isArray({})); // false

alert(Array.isArray([])); // true
```
## Most methods support “thisArg”
Almost all array methods that call functions – like `find`, `filter`, `map`, with a notable exception of `sort`, accept an optional additional parameter `thisArg`.

That parameter is not explained in the sections above, because it’s rarely used. But for completeness, we have to cover it.

Here’s the full syntax of these methods:
```js
arr.find(func, thisArg);
arr.filter(func, thisArg);
arr.map(func, thisArg);
// ...
// thisArg is the optional last argument
```
The value of `thisArg` parameter becomes `this` for `func`.

For example, here we use a method of `army` object as a filter, and `thisArg` passes the context:
```js {16-17}
let army = {
  minAge: 18,
  maxAge: 27,
  canJoin(user) {
    return user.age >= this.minAge && user.age < this.maxAge;
  }
};

let users = [
  {age: 16},
  {age: 20},
  {age: 23},
  {age: 30}
];

// find users, for who army.canJoin returns true
let soldiers = users.filter(army.canJoin, army);

alert(soldiers.length); // 2
alert(soldiers[0].age); // 20
alert(soldiers[1].age); // 23
```
If in the example above we used `users.filter(army.canJoin)`, then `army.canJoin` would be called as a standalone function, with `this=undefined`, thus leading to an instant error.

A call to `users.filter(army.canJoin, army)` can be replaced with `users.filter(user => army.canJoin(user))`, that does the same. The latter is used more often, as it’s a bit easier to understand for most people.