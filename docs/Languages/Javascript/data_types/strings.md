---
sidebar_position: 2
title: Strings
---

## Quotes

Strings can be enclosed within either single quotes, double quotes or backticks:

```js
let single = 'single-quoted';
let double = "double-quoted";

let backticks = `backticks`;
```

Single and double quotes are essentially the same. Backticks, however, allow us to embed any expression into the string, by wrapping it in `${…}`:

```js
function sum(a, b) {
  return a + b;
}

alert(`1 + 2 = ${sum(1, 2)}.`); // 1 + 2 = 3.
```

Another advantage of using backticks is that they allow a string to span multiple lines:

```js
let guestList = `Guests:
 * John
 * Pete
 * Mary
`;

alert(guestList); // a list of guests, multiple lines
```

## Special Characters

| Operator     	| Usage                                                                                                                                                                                                       	|
|--------------	|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| `\n`         	| New line                                                                                                                                                                                                    	|
| `\r`         	| In Windows text files a combination of two characters   \r\n   represents a new break, while on non-Windows OS it’s just   \n . That’s for historical reasons, most Windows software also understands   \n. 	|
| `\', \", \'` 	| Quotes                                                                                                                                                                                                      	|
| `\\`         	| Backslash                                                                                                                                                                                                   	|
| `\t`         	| Tab                                                                                                                                                                                                         	|
| \b, \f, \v   	| Backspace, Form Feed, Vertical Tab – mentioned for completeness, coming from old times, not used nowadays (you can forget them right now).                                                                  	|

## String Length
```js
alert( `My\n`.length ); // 3
```
Note that \n is a single “special” character, so the length is indeed 3.
:::danger[`length` is a property]
People with a background in some other languages sometimes mistype by calling `str.length()` instead of just `str.length`. That doesn’t work.

Please note that `str.length` is a numeric property, not a function. There is no need to add parenthesis after it. Not `.length()`, but `.length`.
:::

## Accessing characters

To get a character at position `pos`, use square brackets `[pos]` or call the method `str.at(pos)`. The first character starts from the zero position:

```js
let str = `Hello`;

// the first character
alert( str[0] ); // H
alert( str.at(0) ); // H

// the last character
alert( str[str.length - 1] ); // o
alert( str.at(-1) );
```

As you can see, the `.at(pos)` method has a benefit of allowing negative position. If `pos` is negative, then it’s counted from the end of the string.

So `.at(-1)` means the last character, and `.at(-2)` is the one before it, etc.

The square brackets always return `undefined` for negative indexes, for instance:

```js
let str = `Hello`;

alert( str[-2] ); // undefined
alert( str.at(-2) ); // l
```

We can also iterate over characters using `for..of`:

```js
for (let char of "Hello") {
  alert(char); // H,e,l,l,o (char becomes "H", then "e", then "l" etc)
}
```

## Strings are immutable

Strings can’t be changed in JavaScript. It is impossible to change a character.

```js
let str = 'Hi';

str[0] = 'h'; // error
alert( str[0] ); // doesn't work
```

The usual workaround is to create a whole new string and assign it to str instead of the old one.

For instance:

```js
let str = 'Hi';

str = 'h' + str[1]; // replace the string

alert( str ); // hi
```

## Changing the case

Methods toLowerCase() and toUpperCase() change the case:

```js
alert( 'Interface'.toUpperCase() ); // INTERFACE
alert( 'Interface'.toLowerCase() ); // interface
```

Or, if we want a single character lowercased:

```js
alert( 'Interface'[0].toLowerCase() ); // 'i'
```

## Searching for a substring

There are multiple ways to look for a substring within a string.

### str.indexOf

The first method is str.indexOf(substr, pos).

It looks for the `substr` in `str`, starting from the given position `pos`, and returns the position where the match was found or `-1` if nothing can be found.

```js
let str = 'Widget with id';

alert( str.indexOf('Widget') ); // 0, because 'Widget' is found at the beginning
alert( str.indexOf('widget') ); // -1, not found, the search is case-sensitive

alert( str.indexOf("id") ); // 1, "id" is found at the position 1 (..idget with id)
```

The optional second parameter allows us to start searching from a given position.

For instance, the first occurrence of `"id"` is at position `1`. To look for the next occurrence, let’s start the search from position `2`:

```js
let str = 'Widget with id';

alert( str.indexOf('id', 2) ) // 12
```

If we’re interested in all occurrences, we can run indexOf in a loop. Every new call is made with the position after the previous match:

```js
let str = 'As sly as a fox, as strong as an ox';

let target = 'as'; // let's look for it

let pos = 0;
while (true) {
  let foundPos = str.indexOf(target, pos);
  if (foundPos == -1) break;

  alert( `Found at ${foundPos}` );
  pos = foundPos + 1; // continue the search from the next position
}
```

The same algorithm can be layed out shorter:

```js {4-7}
let str = "As sly as a fox, as strong as an ox";
let target = "as";

let pos = -1;
while ((pos = str.indexOf(target, pos + 1)) != -1) {
  alert( pos );
}
```

:::info [`str.lastIndexOf(substr, position)`]
There is also a similar method str.lastIndexOf(substr, position) that searches from the end of a string to its beginning.

It would list the occurrences in the reverse order.
:::

There is a slight inconvenience with `indexOf` in the `if` test. We can’t put it in the `if` like this:

```js
let str = "Widget with id";

if (str.indexOf("Widget")) {
    alert("We found it"); // doesn't work!
}
```

The `alert` in the example above doesn’t show because `str.indexOf("Widget")` returns `0` (meaning that it found the match at the starting position). Right, but `if` considers `0` to be `false`.

So, we should actually check for `-1`, like this:

```js {3}
let str = "Widget with id";

if (str.indexOf("Widget") != -1) {
    alert("We found it"); // works now!
}
```

### includes, startsWith, endsWith

The more modern method str.includes(substr, pos) returns `true/false` depending on whether `str` contains `substr` within.

It’s the right choice if we need to test for the match, but don’t need its position:

```js
alert( "Widget with id".includes("Widget") ); // true

alert( "Hello".includes("Bye") ); // false
```

The optional second argument of `str.includes` is the position to start searching from:

```js
alert( "Widget".includes("id") ); // true
alert( "Widget".includes("id", 3) ); // false, from position 3 there is no "id"
```

The methods str.startsWith and str.endsWith do exactly what they say:

```js
alert( "Widget".startsWith("Wid") ); // true, "Widget" starts with "Wid"
alert( "Widget".endsWith("get") ); // true, "Widget" ends with "get"
```

## Getting a substring

There are 3 methods in JavaScript to get a substring: `substring`, `substr` and `slice`.

`str.slice(start [, end])`
Returns the part of the string from `start` to (but not including) `end`.

```js
let str = "stringify";
alert( str.slice(0, 5) ); // 'strin', the substring from 0 to 5 (not including 5)
alert( str.slice(0, 1) ); // 's', from 0 to 1, but not including 1, so only character at 0
```

If there is no second argument, then `slice` goes till the end of the string:

```js
let str = "stringify";
alert( str.slice(2) ); // 'ringify', from the 2nd position till the end
```

Negative values for `start/end` are also possible. They mean the position is counted from the string end:

```js
let str = "stringify";

// start at the 4th position from the right, end at the 1st from the right
alert( str.slice(-4, -1) ); // 'gif'
```
`str.substring(start [, end])`

Returns the part of the string between `start` and `end` (not including `end`).

This is almost the same as `slice`, but it allows `start` to be greater than `end` (in this case it simply swaps `start` and `end` values).

```js
let str = "stringify";

// these are same for substring
alert( str.substring(2, 6) ); // "ring"
alert( str.substring(6, 2) ); // "ring"

// ...but not for slice:
alert( str.slice(2, 6) ); // "ring" (the same)
alert( str.slice(6, 2) ); // "" (an empty string)
```
Negative arguments are (unlike slice) not supported, they are treated as `0`.

`str.substr(start [, length])`
Returns the part of the string from `start`, with the given `length`.

In contrast with the previous methods, this one allows us to specify the `length` instead of the ending position:
```js
let str = "stringify";
alert( str.substr(2, 4) ); // 'ring', from the 2nd position get 4 characters
```
The first argument may be negative, to count from the end:

```js
let str = "stringify";
alert( str.substr(-4, 2) ); // 'gi', from the 4th position get 2 characters
```

| method                  	| selects…                                        	| negatives                	|
|-------------------------	|-------------------------------------------------	|--------------------------	|
| `slice(start, end)`     	| from `start` to `end` (not including `end`)     	| allows negatives         	|
| `substring(start, end)` 	| between `start` and `end` (not including `end`) 	| negative values mean `0` 	|
| `substr(start, length)` 	| from `start` get `length` characters            	| allows negative `start`  	|

## Comparing strings

1. A lowercase letter is always greater than the uppercase:

```js
alert( 'a' > 'Z' ); // true
```

2. Letters with diacritical marks are “out of order”:

```js
alert( 'Österreich' > 'Zealand' ); // true
```

This may lead to strange results if we sort these country names. Usually people would expect Zealand to come after Österreich in the list.

To understand what happens, we should be aware that strings in Javascript are encoded using UTF-16. That is: each character has a corresponding numeric code.

There are special methods that allow to get the character for the code and back:

`str.codePointAt(pos)`
Returns a decimal number representing the code for the character at position pos:

```js
// different case letters have different codes
alert( "Z".codePointAt(0) ); // 90
alert( "z".codePointAt(0) ); // 122
alert( "z".codePointAt(0).toString(16) ); // 7a (if we need a hexadecimal value)
```

`String.fromCodePoint(code)`

Creates a character by its numeric `code`
```js
alert( String.fromCodePoint(90) ); // Z
alert( String.fromCodePoint(0x5a) ); // Z (we can also use a hex value as an 
```

Now let’s see the characters with codes `65..220` (the latin alphabet and a little bit extra) by making a string of them:

```js
let str = '';

for (let i = 65; i <= 220; i++) {
  str += String.fromCodePoint(i);
}
alert( str );
// Output:
// ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~
// ¡¢£¤¥¦§¨©ª«¬­®¯°±²³´µ¶·¸¹º»¼½¾¿ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ×ØÙÚÛÜ
```

Capital characters go first, then a few special ones, then lowercase characters, and `Ö` near the end of the output.

Now it becomes obvious why `a > Z`.

The characters are compared by their numeric code. The greater code means that the character is greater. The code for `a` (97) is greater than the code for `Z` (90).

* All lowercase letters go after uppercase letters because their codes are greater.
* Some letters like `Ö` stand apart from the main alphabet. Here, its code is greater than anything from `a` to `z`.