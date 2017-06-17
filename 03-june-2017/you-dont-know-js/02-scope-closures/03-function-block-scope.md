## Notes from *You Don't Know JS* by Kyle Simpson

[Book 2: Scope & Closures](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20&%20closures/README.md#you-dont-know-js-scope--closures)
[Chapter 3: Function vs Block Scope](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md)
The most common and widely understood way to create scope in JS is with functions. However, it's not the only way.  

Function scope encourages the idea that all variables belong to the function, and can be used and reused throughout the entirety of the function (and indeed, accessible even to nested scopes).  

Usually we think about declaring a function and writing some code in it to do something. However, it's also possible take a bunch of code that would otherwise be exposed and wrap in in a function so things don't get messy. This adheres to the principle of least privilege (namely that in the design of software, you should expose only what is minimally necessary, and 'hide' everything else).  

```
for (var i=0; i<10; i++) {
		bar( i * 2 ); // oops, infinite loop ahead!
	}
```

By declaring the variable 'i' in the for loop above, it limits the availability of 'i' to the enclosing block of code - though not just to the for loop block.  

Using IIFEs is another way to limit the accessibility of a function in the broader scope.

Most function expressions used as callback parameters are not named, however this isn't a requirement of the pattern. It's perfectly fine to use a name and it helps in debugging (stack traces), allows recursion and makes the code more readable.

A variation on the conventional IIFE pattern is as follows
```
var a = 2;

(function IIFE( global ){

	var a = 3;
	console.log( a ); // 3
	console.log( global.a ); // 2

})( window );

console.log( a ); // 2
```
This allows us to explicitly differentiate between variables that are in the global scope and those that are local to the function.

There are three ways to scope a variable to a non-function block. There's `with` though this is frowned upon and downright disallowed in Strict Mode; there's `try/catch` and the catch block has block scope; and in ES6 there's the keyword `let`, which scopes to its local `{..}`.

>A particular case where let shines is in the for-loop case as we discussed previously.
```
for (let i=0; i<10; i++) {
	console.log( i );
}

console.log( i ); // ReferenceError
```
>Not only does `let` in the for-loop header bind the i to the for-loop body, but in fact, it re-binds it to each iteration of the loop, making sure to re-assign it the value from the end of the previous loop iteration.

The keyword `const` is also block scoped, but its use is more limited as its value can't be reassigned. That said, `var` still has its uses and shouldn't be abandoned entirely.
