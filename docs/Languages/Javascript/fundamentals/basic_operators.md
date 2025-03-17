---
sidebar_position: 6
---

# Basic operators, maths

## Operator Precedence

If an expression has more than one operator, the execution order is defined by their precedence, or, in other words, the default priority order of operators.

From school, we all know that the multiplication in the expression `1 + 2 * 2` should be calculated before the addition. That’s exactly the precedence thing. The multiplication is said to have a higher precedence than the addition.

Parentheses override any precedence, so if we’re not satisfied with the default order, we can use them to change it. For example, write `(1 + 2) * 2`.

There are many operators in JavaScript. Every operator has a corresponding precedence number. The one with the larger number executes first. If the precedence is the same, the execution order is from left to right.

Here’s an extract from the precedence table (you don’t need to remember this, but note that unary operators are higher than corresponding binary ones):

| Precedence 	| Name           	| Sign 	|
|------------	|----------------	|------	|
| 14         	| unary plus     	| +    	|
| 14         	| unary negation 	| -    	|
| 13         	| exponentiation 	| **   	|
| 12         	| multiplication 	| *    	|
| 12         	| division       	| /    	|
| 11         	| addition       	| +    	|
| 11         	| subtraction    	| -    	|
| ...        	| ...            	| ...  	|
| 2          	| assignment     	| =    	|
| ...        	| ...            	| ...  	|

## Increment/Decrement

Increasing or decreasing a number by one is among the most common numerical operations.

So, there are special operators for it:

* Increment `++` increases a variable by 1:

```js
let counter = 2;
counter++;        // works the same as counter = counter + 1, but is shorter
alert( counter ); // 3
```

* Decrement `--` decreases a variable by 1:

```js
let counter = 2;
counter--;        // works the same as counter = counter - 1, but is shorter
alert( counter ); // 1
```

:::danger[Important]
Increment/decrement can only be applied to variables. Trying to use it on a value like 5++ will give an error.
:::

The operators `++` and `--` can be placed either before or after a variable.
* When the operator goes after the variable, it is in “postfix form”: `counter++`.
* The “prefix form” is when the operator goes before the variable: `++counter`.

Both of these statements do the same thing: increase `counter` by `1`.

Is there any difference? Yes, but we can only see it if we use the returned value of `++/--`.

Let’s clarify. As we know, all operators return a value. Increment/decrement is no exception. The prefix form returns the new value while the postfix form returns the old value (prior to increment/decrement).

To see the difference, here’s an example:

```js
let counter = 1;
let a = ++counter; // (*)

alert(a); // 2
```

In the line `(*)`, the postfix form `counter++` also increments `counter` but returns the old value (prior to increment). So, the `alert` shows `1`.

To summarize:

* If the result of increment/decrement is not used, there is no difference in which form to use:

```js
let counter = 0;
counter++;
++counter;
alert( counter ); // 2, the lines above did the same
```

* If we’d like to increase a value and immediately use the result of the operator, we need the prefix form:
```js
let counter = 0;
alert( ++counter ); // 1
```

* If we’d like to increment a value but use its previous value, we need the postfix form:
```js
let counter = 0;
alert( counter++ ); // 0
```

:::info[Increment/decrement among other operators]
The operators `++/--` can be used inside expressions as well. Their precedence is higher than most other arithmetical operations.

For instance:
```js
let counter = 1;
alert( 2 * ++counter ); // 4
```
Compare with:
```js
let counter = 1;
alert( 2 * counter++ ); // 2, because counter++ returns the "old" value
```
Though technically okay, such notation usually makes code less readable. One line does multiple things – not good.

While reading code, a fast “vertical” eye-scan can easily miss something like `counter++` and it won’t be obvious that the variable increased.

We advise a style of “one line – one action”:
```js
let counter = 1;
alert( 2 * counter );
counter++;
```
:::

## Bitwise operators

Bitwise operators treat arguments as 32-bit integer numbers and work on the level of their binary representation.

These operators are not JavaScript-specific. They are supported in most programming languages.

The list of operators:

* AND ( `&` )
* OR ( `|` )
* XOR ( `^` )
* NOT ( `~` )
* LEFT SHIFT ( `<<` )
* RIGHT SHIFT ( `>>` )
* ZERO-FILL RIGHT SHIFT ( `>>>` )

These operators are used very rarely, when we need to fiddle with numbers on the very lowest (bitwise) level. We won’t need these operators any time soon, as web development has little use of them, but in some special areas, such as cryptography, they are useful.

A bitwise operator treats their operands as a set of 32 bits (zeros and ones), rather than as decimal, hexadecimal, or octal numbers. For example, the decimal number nine has a binary representation of 1001. Bitwise operators perform their operations on such binary representations, but they return standard JavaScript numerical values.

The following table summarizes JavaScript's bitwise operators.

| Operator                     	| Usage   	| Description                                                                                                                                                             	|
|------------------------------	|---------	|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| Bitwise AND                  	| a & b   	| Returns a one in each bit position for which the corresponding bits of both operands are ones.                                                                          	|
| Bitwise OR                   	| a \| b  	| Returns a zero in each bit position for which the corresponding bits of both operands are zeros.                                                                        	|
| Bitwise XOR                  	| a ^ b   	| Returns a zero in each bit position for which the corresponding bits are the same. [Returns a one in each bit position for which the corresponding bits are different.] 	|
| Bitwise NOT                  	| ~ a     	| Inverts the bits of its operand.                                                                                                                                        	|
| Left shift                   	| `a << b`  	| Shifts a in binary representation b bits to the left, shifting in zeros from the right.                                                                                 	|
| Sign-propagating right shift 	| `a >> b`  	| Shifts a in binary representation b bits to the right, discarding bits shifted off.                                                                                     	|
| Zero-fill right shift        	| `a >>> b` 	| Shifts a in binary representation b bits to the right, discarding bits shifted off, and shifting in zeros from the left.                                                	|

## Comma

The comma operator `,` is one of the rarest and most unusual operators. Sometimes, it’s used to write shorter code, so we need to know it in order to understand what’s going on.

The comma operator allows us to evaluate several expressions, dividing them with a comma `,`. Each of them is evaluated but only the result of the last one is returned.

For example:

```js {1}
let a = (1 + 2, 3 + 4);

alert( a ); // 7 (the result of 3 + 4)
```

Here, the first expression `1 + 2` is evaluated and its result is thrown away. Then, `3 + 4` is evaluated and returned as the result.

:::info[Comma has a very low precedence]
Please note that the comma operator has very low precedence, lower than `=`, so parentheses are important in the example above.

Without them: `a = 1 + 2, 3 + 4` evaluates `+` first, summing the numbers into `a = 3, 7`, then the assignment operator `=` assigns `a = 3`, and the rest is ignored. It’s like `(a = 1 + 2), 3 + 4`.
:::