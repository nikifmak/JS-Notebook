# Hoisting

> Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.

Inevitably, this means that no matter where functions and variables are declared, they are moved to the top of their scope regardless of whether their scope is global or local.

Of note however, is the fact that the hoisting mechanism only moves the declaration. __<u>The assignments are left in place.</u>__



## Undefined vs ReferenceError

```javascript
console.log(typeof variable); // Output: undefined
```

> In JavaScript, an undeclared variable is assigned the value undefined at execution and is also of type undefined.

```javascript
console.log(variable); // Output: ReferenceError: variable is not defined
```

> In JavaScript, a ReferenceError is thrown when trying to access a previously undeclared variable.



JavaScript allows us to both declare and initialise our variables simultaneously, this is the most used pattern: 

```javascript
var a = 100;
```

It is however important to remember that in the background, JavaScript is religiously declaring then initialising our variables.

As we mentioned before, all variable and function declarations are hoisted to the **top** of their scope. I should also add that variable *declarations* are processed before any code is executed.

However, in contrast, *undeclared* variables do not exist until code assigning them is executed. Therefore, assigning a value to an undeclared variable implicitly creates it as a global variable when the assignment is executed. This means that, **all undeclared variables are global variables.**

To demonstrate this behaviour, have a look at the following:

``` javascript
function hoist() {
  a = 20;
  var b = 100;
}

hoist();

console.log(a); 
/* 
Accessible as a global variable outside hoist() function
Output: 20
*/

console.log(b); 
/*
Since it was declared, it is confined to the hoist() function scope.
We can't print it out outside the confines of the hoist() function.
Output: ReferenceError: b is not defined
*/
```

Since this is one of the eccentricities of how JavaScript handles variables, it is recommended to **always declare variables regardless of whether they are in a function or global scope**. This clearly delineates how the interpreter should handle them at run time.
