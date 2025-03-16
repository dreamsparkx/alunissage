---
sidebar_position: 1
---

# An Introduction to Javascript

## What is Javascript?

JavaScript is an **interpreted** language, **NOT** a compiled language. JS is a programming language that lets you create interactive web pages. It is a core technology of the World Wide Web. The programs in this language are called scripts. They can be written right in a web page’s HTML and run automatically as the page loads.

Scripts are provided and executed as plain text. They don’t need special preparation or compilation to run.

**ECMAScript** is a standard for scripting languages, including JavaScript, JScript, and ActionScript. It is best known as a JavaScript standard intended to ensure the interoperability of web pages across different web browsers.It is standardized by Ecma International in the document ECMA-262.

**ECMAScript** is commonly used for client-side scripting on the World Wide Web, and it is increasingly being used for server-side applications and services using runtime environments such as Node.js, Deno and Bun.

JavaScript can execute not only in the browser, but also on the server, or actually on any device that has a special program called the [JavaScript engine](https://en.wikipedia.org/wiki/JavaScript_engine).

The browser has an embedded engine sometimes called a “JavaScript virtual machine”.

Different engines have different “codenames”. For example:

* V8 – in Chrome, Opera and Edge.
* SpiderMonkey – in Firefox.
* …There are other codenames like “Chakra” for IE, “JavaScriptCore”, “Nitro” and “SquirrelFish” for Safari, etc.

:::info[How do engines work?]
Engines are complicated. But the basics are easy.
1. The engine (embedded if it’s a browser) reads (“parses”) the script.
2. Then it converts (“compiles”) the script to machine code.
3. And then the machine code runs, pretty fast.

The engine applies optimizations at each step of the process. It even watches the compiled script as it runs, analyzes the data that flows through it, and further optimizes the machine code based on that knowledge.
:::


The syntax of JavaScript does not suit everyone’s needs. Different people want different features.

That’s to be expected, because projects and requirements are different for everyone.

So, recently a plethora of new languages appeared, which are transpiled (converted) to JavaScript before they run in the browser.

Modern tools make the transpilation very fast and transparent, actually allowing developers to code in another language and auto-converting it “under the hood”.

Examples of such languages:

* **CoffeeScript** is “syntactic sugar” for JavaScript. It introduces shorter syntax, allowing us to write clearer and more precise code. Usually, Ruby devs like it.
* **TypeScript** is concentrated on adding “strict data typing” to simplify the development and support of complex systems. It is developed by Microsoft.
* **Flow** also adds data typing, but in a different way. Developed by Facebook.
* **Dart** is a standalone language that has its own engine that runs in non-browser environments (like mobile apps), but also can be transpiled to JavaScript. Developed by Google.
* **Brython** is a Python transpiler to JavaScript that enables the writing of applications in pure Python without JavaScript.
* **Kotlin** is a modern, concise and safe programming language that can target the browser or Node.

There are more. Of course, even if we use one of these transpiled languages, we should also know JavaScript to really understand what we’re doing.