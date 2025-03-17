---
sidebar_position: 3
---

# Variables

A variable is a “named storage” for data. We can use variables to store goodies, visitors, and other data.

To create a variable in JavaScript, use the `let` keyword.

The statement below creates (in other words: declares) a variable with the name “message”:

```js
let me;
let message = 'Hello!';
let user = 'John', age = 25, messageMe = 'Hello';
```

:::info[`var` instead of `let`]
In older scripts, you may also find another keyword: `var` instead of `let`:
```js
var message = 'Hello';
```
The `var` keyword is almost the same as `let`. It also declares a variable but in a slightly different, “old-school” way.

There are subtle differences between `let` and `var`, but they do not matter to us yet. We’ll cover them in detail in the chapter [The old "var"](../advanced_workings/old-var).
:::

:::info[Functional languages]
It’s interesting to note that there exist so-called pure functional programming languages, such as Haskell, that forbid changing variable values.

In such languages, once the value is stored “in the box”, it’s there forever. If we need to store something else, the language forces us to create a new box (declare a new variable). We can’t reuse the old one.

Though it may seem a little odd at first sight, these languages are quite capable of serious development. More than that, there are areas like parallel computations where this limitation confers certain benefits.
:::

:::danger[An assignment without `use strict`]
Normally, we need to define a variable before using it. But in the old times, it was technically possible to create a variable by a mere assignment of the value without using let. This still works now if we don’t put use strict in our scripts to maintain compatibility with old scripts.
```js
// note: no "use strict" in this example

num = 5; // the variable "num" is created if it didn't exist

alert(num); // 5
```
This is a bad practice and would cause an error in strict mode:
```js {3}
"use strict";

num = 5; // error: num is not defined
```
:::

## Constants

To declare a constant (unchanging) variable, use `const` instead of `let`:

```js
const myBirthday = '18.04.1982';
```

Variables declared using const are called “constants”. They cannot be reassigned. An attempt to do so would cause an error:

```js {3}
const myBirthday = '18.04.1982';

myBirthday = '01.01.2001'; // error, can't reassign the constant!
```

### Uppercase Constants

There is a widespread practice to use constants as aliases for difficult-to-remember values that are known before execution.

Such constants are named using capital letters and underscores.

For instance, let’s make constants for colors in so-called “web” (hexadecimal) format:

```js
const COLOR_RED = "#F00";
const COLOR_GREEN = "#0F0";
const COLOR_BLUE = "#00F";
const COLOR_ORANGE = "#FF7F00";

// ...when we need to pick a color
let color = COLOR_ORANGE;
alert(color); // #FF7F00
```

### Summary

We can declare variables to store data by using the `var`, `let`, or `const` keywords.

* `let` – is a modern variable declaration.
* `var` – is an old-school variable declaration. Normally we don’t use it at all, but we’ll cover subtle differences from `let` in the chapter [The old "var"](../advanced_workings/old-var), just in case you need them.
* `const` – is like `let`, but the value of the variable can’t be changed.