---
sidebar_position: 6
title: Map and Set
---

## Map
Map is a collection of keyed data items, just like an Object. But the main difference is that Map allows keys of any type.

Methods and properties are:
* `new Map()` – creates the map.
* `map.set(key, value)` – stores the value by the key.
* `map.get(key)` – returns the value by the key, undefined if key doesn’t exist in map.
* `map.has(key)` – returns true if the key exists, false otherwise.
* `map.delete(key)` – removes the element (the key/value pair) by the key.
* `map.clear()` – removes everything from the map.
* `map.size` – returns the current element count.
```js
let map = new Map();

map.set('1', 'str1');   // a string key
map.set(1, 'num1');     // a numeric key
map.set(true, 'bool1'); // a boolean key

// remember the regular Object? it would convert keys to string
// Map keeps the type, so these two are different:
alert( map.get(1)   ); // 'num1'
alert( map.get('1') ); // 'str1'

alert( map.size ); // 3
```
**Map can also use objects as keys.**
```js
let john = { name: "John" };

// for every user, let's store their visits count
let visitsCountMap = new Map();

// john is the key for the map
visitsCountMap.set(john, 123);

alert( visitsCountMap.get(john) ); // 123
```
Using objects as keys is one of the most notable and important `Map` features. The same does not count for `Object`. String as a key in `Object` is fine, but we can’t use another `Object` as a key in `Object`.
```js {9-10}
let john = { name: "John" };
let ben = { name: "Ben" };

let visitsCountObj = {}; // try to use an object

visitsCountObj[ben] = 234; // try to use ben object as the key
visitsCountObj[john] = 123; // try to use john object as the key, ben object will get replaced

// That's what got written!
alert( visitsCountObj["[object Object]"] ); // 123
```
As `visitsCountObj` is an object, it converts all `Object` keys, such as `john` and `ben` above, to same string `"[object Object]"`. Definitely not what we want.
## Iteration over Map
For looping over a `map`, there are 3 methods:
* `map.keys()` – returns an iterable for keys,
* `map.values()` – returns an iterable for values,
* `map.entries()` – returns an iterable for entries `[key, value]`, it’s used by default in `for..of`.
```js
let recipeMap = new Map([
  ['cucumber', 500],
  ['tomatoes', 350],
  ['onion',    50]
]);

// iterate over keys (vegetables)
for (let vegetable of recipeMap.keys()) {
  alert(vegetable); // cucumber, tomatoes, onion
}

// iterate over values (amounts)
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// iterate over [key, value] entries
for (let entry of recipeMap) { // the same as of recipeMap.entries()
  alert(entry); // cucumber,500 (and so on)
}
```
Besides that, Map has a built-in forEach method, similar to Array:
```js
// runs the function for each (key, value) pair
recipeMap.forEach( (value, key, map) => {
  alert(`${key}: ${value}`); // cucumber: 500 etc
});
```
## Object.entries: Map from Object
```js
// array of [key, value] pairs
let map = new Map([
  ['1',  'str1'],
  [1,    'num1'],
  [true, 'bool1']
]);

alert( map.get('1') ); // str1
```
If we have a plain object, and we’d like to create a Map from it, then we can use built-in method Object.entries(obj) that returns an array of key/value pairs for an object exactly in that format.
```js {6}
let obj = {
  name: "John",
  age: 30
};

let map = new Map(Object.entries(obj));

alert( map.get('name') ); // John
```

## Object.fromEntries: Object from Map
We’ve just seen how to create `Map` from a plain object with `Object.entries(obj)`.

There’s `Object.fromEntries` method that does the reverse: given an array of `[key, value]` pairs, it creates an object from them:
```js
let prices = Object.fromEntries([
  ['banana', 1],
  ['orange', 2],
  ['meat', 4]
]);

// now prices = { banana: 1, orange: 2, meat: 4 }

alert(prices.orange); // 2
```
We can use `Object.fromEntries` to get a plain object from `Map`.

E.g. we store the data in a `Map`, but we need to pass it to a 3rd-party code that expects a plain object.
```js {6}
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);

let obj = Object.fromEntries(map.entries()); // make a plain object (*)

// done!
// obj = { banana: 1, orange: 2, meat: 4 }

alert(obj.orange); // 2
```
A call to `map.entries()` returns an iterable of key/value pairs, exactly in the right format for `Object.fromEntries`.

We could also make line `(*)` shorter:
```js
let obj = Object.fromEntries(map); // omit .entries()
```

## Set
A `Set` is a special type collection – “set of values” (without keys), where each value may occur only once.
* `new Set([iterable])` – creates the set, and if an iterable object is provided (usually an array), copies values from it into the set.
* `set.add(value)` – adds a value, returns the set itself.
* `set.delete(value)` – removes the value, returns true if value existed at the moment of the call, otherwise false.
* `set.has(value)` – returns true if the value exists in the set, otherwise false.
* `set.clear()` – removes everything from the set.
* `set.size` – is the elements count.
The main feature is that repeated calls of `set.add(value)` with the same value don’t do anything. That’s the reason why each value appears in a `Set` only once.
```js
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

// visits, some users come multiple times
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// set keeps only unique values
alert( set.size ); // 3

for (let user of set) {
  alert(user.name); // John (then Pete and Mary)
}
```

## Iteration over Set
We can loop over a set either with `for..of` or using `forEach`:
```js
let set = new Set(["oranges", "apples", "bananas"]);

for (let value of set) alert(value);

// the same with forEach:
set.forEach((value, valueAgain, set) => {
  alert(value);
});
```
The callback function passed in `forEach` has 3 arguments: a `value`, then the same value `valueAgain`, and then the target object. Indeed, the same value appears in the arguments twice.

That’s for compatibility with `Map` where the callback passed `forEach` has three arguments. Looks a bit strange, for sure. But this may help to replace `Map` with `Set` in certain cases with ease, and vice versa.

The same methods `Map` has for iterators are also supported:

* `set.keys()` – returns an iterable object for values,
* `set.values()` – same as `set.keys()`, for compatibility with `Map`,
* `set.entries()` – returns an iterable object for entries `[value, value]`, exists for compatibility with Map.