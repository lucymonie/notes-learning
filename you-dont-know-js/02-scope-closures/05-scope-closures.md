## Notes from *You Don't Know JS: Scope & Closures* by Kyle Simpson

[Chapter 5: Scope Closures](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md)

A closure is a legitimate and smiled upon way to 'cheat' lexical scope and it is constructed at author-time.

>Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

I have written some [previous notes on closures](../../closure.md) and so I am going to cover this briefly and then move on to the module pattern.

Using the module pattern looks a bit like this
```
function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
}

var foo = CoolModule();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```
The neat thing about this function is that it returns an object with some methods, and the methods have the closure of `CoolModule` and thus retain access to the variables 'something' and 'another' even when they are invoked outside their lexical scope.

The return value is basically a public api for the module `CoolModule`.

There are two things that need to be present for it to be a module  
1. An enclosing function that's called at least once
2. The enclosing function has to return at least one function that retains access to its lexical scope
