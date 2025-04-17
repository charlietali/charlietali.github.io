---
title: "this in JavaScript"
date: 2024-04-17 12:00:00 +0900
categories: [javascript]
layout: single
author_profile: true
---

## **`this` in JavaScript** 

The `this` keyword in JavaScript behaves differently depending on how it's used.
Here's a quick summary of how it works in the global scope and inside regular functions.



#### `this` in the global scope

When running this in a browser:

```javascript
console.log(this); // window
```

It logs the `window` object.
In the global scope, `this` refers to the global object, which is `window` in browsers.



#### `this` in regular functions

Same behavior inside a normal function:

```javascript
function logThis() {
	console.log(this);
}
logThis(); // window
```

When a function is called without any specific context, `this` still points to `window`.



**- What is `window` ?**🤔

`window` is the global object in the browser.

It includes things like:

- `alert()`
- `console.log()`
- `document.getElementById()`

It's basically a container for these functions. Nothing special — just a big object.
When you declare a global variable with `var`, that value gets stored there too:

```javascript
var count = 1;
console.log(window.count); // 1
```

This doesn't happen with `let` or `const`.



#### `this` in strict mode

In non-strict mode, calling a regular function gives you `window` as the value of `this`.
But if you're using `use strict`, things change.

```html
<script>
	'use strict';
    
	function showThis() {
        console.log(this);
    }
    
    showThis(); // undefined
</script>
```

Since `this` becomes explicitly `undefined`, you're forced to think about **what context** your function is being called in. Basically, It's a reminder to use `this` only when it's clearly bound.



**- Why strict mode?** 🤔

Strict mode does a few things:

- Prevents accidental global variables
- Throws errors for certain silent mistakes
- Disallows reserved words like `arguments` as variable names
- Makes `this` behave more predictably



#### `this` inside object methods

When a function is defined inside an object, it's called a **method**.

```javascript
const obj = {
    data: 'Park',
    showThis: function () {
        console.log(this);
    }
};

obj.showThis(); // obj
```

In this case, `this` refers to the object the method belongs to. In other words, its **owner**.
So inside a method, `this` usually means "the object that owns this function."



#### `this` in nested objects

The same rule applies when the method is inside a nested object.

```javascript
const obj = {
    data: {
        showThis: function () {
            console.log(this);
        }
    }
};

obj.data.showThis(); // obj.data
```

In this case, `this` refers to `obj.data`, because that's the object directly calling the method.



#### `this` in constructor functions

If you want to create multiple similar objects in JavaScript, 
you can use a **constructor function** instead of copying objects manually.
You can think of a constructor as a machine that builds new objects.

Here's how it works:

```javascript
function Machine() {
    this.name = 'Park';
}

const obj = new Machine();
```

- `Machine()` is a constructor function.
- When you call `new Machine()`, a new object is created.
- Inside the constructor, `this` refers to the newly created object.

So when you write `this.name = 'Park'`, you're assigning the `name` property to that freshly created object.



#### `this` in event listeners

When using `this` inside a regular function in an event listener,
it refers to the **DOM element** that the event is attached to.

```javascript
document.getElementById('button').addEventListener('click', function (e) {
    console.log(this);				// The element itself
	console.log(e.currentTarget);	// Same as `this`
});
```

Both `this` and `e.currentTarget` point to the element the event was attached to.



#### ⚠️ Inside nested functions or callbacks

A nested function inside an event listener won’t keep the same `this`; it defaults to `window`.

```javascript
document.getElementById('button').addEventListener('click', function () {
    [1, 2, 3].forEach(function () {
   		console.log(this); // window (because it's a regular function)     
    });
});
```

This happens because `this` in a regular function depends on **how** it's called,
 and here it’s called without any binding.



#### `this` in object methods with callbacks

The same thing happens in object methods with callbacks:

```javascript
const obj = {
	names: ['Kim', 'Park', 'Lee'],
	print: function () {
		this.names.forEach(function () {
			console.log(this); // window
		});
	}
};
```

The `forEach` callback is just a regular function.
So `this` inside it doesn't refer to `obj`. It falls back to the global object.



#### ⚙️ The fix: use arrow functions

Arrow functions don't have their own `this`.
They inherit `this` from the surrounding scope.

```javascript
const obj = {
	names: ['Kim', 'Park', 'Lee'],
	print: function () {
		this.names.forEach(() => {
			console.log(this); // refers to obj
		});
	}
};
```

This is one of the most common and useful reasons to use arrow functions inside methods.

------



#### 🧠 Quick Recap : What `this` Means in JavaScript

Here’s a quick recap of how `this` behaves in different contexts:

- **Global scope**:
   `this` refers to the global object (`window` in browsers).
- **Regular functions**:
   When called directly, `this` still points to `window`.
- **Constructor functions**:
   With `new`, `this` refers to the newly created object.
- **Event listeners**:
   In a regular function, `this` points to the element the event is attached to.