## Notes from *You Don't Know JS: Scope & Closures* by Kyle Simpson

[Chapter 4: Hoisting](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch4.md)

**Declaration hoisting**
The main thing to know about hoisting is that function declarations and variable declarations are compiled before being run by the JS engine. The result of this is that any function or variable declaration is treated as though it's at the top of its scope. This is a process known as 'hoisting'.

```
a = 2;
var a;
console.log(a); // 2
```
You might expect that the code above would result in 'undefined', but because of hoisting the code above is treated as though it looks like this
```
var a;
a = 2;
console.log(a); // 2
```

The JS engine handles assignment (eg `a = 2`) at runtime, so this is not hoisted and so the assignment of the value '2' to the variable 'a' doesn't occur until after the console.log of 'a'
```
var a;
console.log(a); // undefined
a = 2;
```

Function declarations are hoisted in full, so the whole of the function is effectively hauled to the top of the current scope. This is not the case for function expressions
```
foo(2); // TypeError: foo is not a function
var foo = function (a) {
  console.log(a);
}
```
Whereas if it was a function declaration, it would have been hoisted
```
foo(2); // 2
function foo (a) {
  console.log(a);
}
```
This is because the JS engine sees the code above like this
```
function foo (a) {
  console.log(a);
}

foo(2); // 2
```

**Functions before variables**  
Though both function declations and variables are hoisted, it's worth noting that function declarations are hoisted first.
```
foo(); // 1

function foo() {
	console.log( 1 );
}
var foo;

foo = function() {
	console.log( 2 );
};
```
The code above is treated like this
```
function foo() {
	console.log( 1 );
}
var foo;  // [ignored by JS engine]

foo(); // 1

foo = function() {
	console.log( 2 );
};
```
The variable 'foo' was ignored by the engine at runtime, and the call to foo was treated as a RHS reference to the function declaration of that name.

A duplicate function declaration will override a previous function declaration, so it's best to be careful about duplicate declarations in the same scope.
