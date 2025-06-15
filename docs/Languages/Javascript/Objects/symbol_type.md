---
sidebar_position: 6
---

# Symbol Type

By specification, only two primitive types may serve as object property keys:

* string type, or
* symbol type.

Otherwise, if one uses another type, such as number, it’s autoconverted to string. So that `obj[1]` is the same as `obj["1"]`, and `obj[true]` is the same as `obj["true"]`.

Until now we’ve been using only strings.

## Symbols

A “symbol” represents a unique identifier.

A value of this type can be created using `Symbol()`:

```js
let id = Symbol();
```
Upon creation, we can give symbols a description (also called a symbol name), mostly useful for debugging purposes:
```js
// id is a symbol with the description "id"
let id = Symbol("id");
```
Symbols are guaranteed to be unique. Even if we create many symbols with exactly the same description, they are different values. The description is just a label that doesn’t affect anything.

For instance, here are two symbols with the same description – they are not equal:
```js {4}
let id1 = Symbol("id");
let id2 = Symbol("id");

alert(id1 == id2); // false
```
So, to summarize, a symbol is a “primitive unique value” with an optional description. Let’s see where we can use them.
:::info[Symbols don’t auto-convert to a string]
Most values in JavaScript support implicit conversion to a string. For instance, we can `alert` almost any value, and it will work. Symbols are special. They don’t auto-convert.

For instance, this `alert` will show an error:
```js {2}
let id = Symbol("id");
alert(id); // TypeError: Cannot convert a Symbol value to a string
```
That’s a “language guard” against messing up, because strings and symbols are fundamentally different and should not accidentally convert one into another.

If we really want to show a symbol, we need to explicitly call `.toString()` on it, like here:
```js {2}
let id = Symbol("id");
alert(id.toString()); // Symbol(id), now it works
```
Or get `symbol.description` property to show the description only:
```js {2}
let id = Symbol("id");
alert(id.description); // id
```
:::

## “Hidden” properties

Symbols allow us to create “hidden” properties of an object, that no other part of code can accidentally access or overwrite.

For instance, if we’re working with `user` objects, that belong to a third-party code. We’d like to add identifiers to them.

Let’s use a symbol key for it:

```js
let user = { // belongs to another code
  name: "John"
};

let id = Symbol("id");

user[id] = 1;

alert( user[id] ); // we can access the data using the symbol as the key
```

What’s the benefit of using `Symbol("id")` over a string `"id"`?

As `user` objects belong to another codebase, it’s unsafe to add fields to them, since we might affect pre-defined behavior in that other codebase. However, symbols cannot be accessed accidentally. The third-party code won’t be aware of newly defined symbols, so it’s safe to add symbols to the `user` objects.

Also, imagine that another script wants to have its own identifier inside `user`, for its own purposes.

Then that script can create its own `Symbol("id")`, like this:

```js
// ...
let id = Symbol("id");

user[id] = "Their id value";
```
There will be no conflict between our and their identifiers, because symbols are always different, even if they have the same name.

…But if we used a string `"id"` instead of a symbol for the same purpose, then there would be a conflict:
```js
let user = { name: "John" };

// Our script uses "id" property
user.id = "Our id value";

// ...Another script also wants "id" for its purposes...

user.id = "Their id value"
// Boom! overwritten by another script!
```

## Symbols in an object literal

If we want to use a symbol in an object literal `{...}`, we need square brackets around it.

Like this:
```js {5}
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123 // not "id": 123
};
```
That’s because we need the value from the variable id as the key, not the string “id”.

## Symbols are skipped by for…in

Symbolic properties do not participate in `for..in` loop.

```js {8}
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) alert(key); // name, age (no symbols)

// the direct access by the symbol works
alert( "Direct: " + user[id] ); // Direct: 123
```

Object.keys(user) also ignores them. That’s a part of the general “hiding symbolic properties” principle. If another script or a library loops over our object, it won’t unexpectedly access a symbolic property.

In contrast, Object.assign copies both string and symbol properties:
```js
let id = Symbol("id");
let user = {
  [id]: 123
};

let clone = Object.assign({}, user);

alert( clone[id] ); // 123
```

## Global Symbols

As we’ve seen, usually all symbols are different, even if they have the same name. But sometimes we want same-named symbols to be same entities. For instance, different parts of our application want to access symbol `"id"` meaning exactly the same property.

To achieve that, there exists a global symbol registry. We can create symbols in it and access them later, and it guarantees that repeated accesses by the same name return exactly the same symbol.

In order to read (create if absent) a symbol from the registry, use `Symbol.for(key)`.

That call checks the global registry, and if there’s a symbol described as key, then returns it, otherwise creates a new symbol `Symbol(key)` and stores it in the registry by the given `key`.

```js
// read from the global registry
let id = Symbol.for("id"); // if the symbol did not exist, it is created

// read it again (maybe from another part of the code)
let idAgain = Symbol.for("id");

// the same symbol
alert( id === idAgain ); // true
```
Symbols inside the registry are called global symbols. If we want an application-wide symbol, accessible everywhere in the code – that’s what they are for.

## Symbol.keyFor

We have seen that for global symbols, `Symbol.for(key)` returns a symbol by name. To do the opposite – return a name by global symbol – we can use: `Symbol.keyFor(sym)`:

```js
// get symbol by name
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

// get name by symbol
alert( Symbol.keyFor(sym) ); // name
alert( Symbol.keyFor(sym2) ); // id
```

The `Symbol.keyFor` internally uses the global symbol registry to look up the key for the symbol. So it doesn’t work for non-global symbols. If the symbol is not global, it won’t be able to find it and returns `undefined`.

That said, all symbols have the `description` property.
```js
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert( Symbol.keyFor(globalSymbol) ); // name, global symbol
alert( Symbol.keyFor(localSymbol) ); // undefined, not global

alert( localSymbol.description ); // name
```

## System symbols

There exist many “system” symbols that JavaScript uses internally, and we can use them to fine-tune various aspects of our objects.

They are listed in the specification in the Well-known symbols table:
* `Symbol.hasInstance`
* `Symbol.isConcatSpreadable`
* `Symbol.iterator`
* `Symbol.toPrimitive`
* …and so on.

For instance, `Symbol.toPrimitive` allows us to describe object to primitive conversion.

```js
obj[Symbol.toPrimitive] = function(hint) {
  // here goes the code to convert this object to a primitive
  // it must return a primitive value
  // hint = one of "string", "number", "default"
};
```

If the method `Symbol.toPrimitive` exists, it’s used for all hints, and no more methods are needed.

For instance, here `user` object implements it:

```js
let user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};

// conversions demo:
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```

As we can see from the code, user becomes a self-descriptive string or a money amount, depending on the conversion. The single method user[Symbol.toPrimitive] handles all conversion cases.