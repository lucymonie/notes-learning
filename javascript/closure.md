## What is a closure?

I was asked this in an interview recently, and I would like to be able to give a better answer next time, so here are some notes.

[MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Closures) says
>A closure is the combination of a function and the lexical environment within which that function was declared. This environment consists of any local variables that were in-scope at the time that the closure was created.

An example of a closure in action from MDN
```
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```

From [StackOverflow](https://stackoverflow.com/questions/36636/what-is-a-closure)
- Functions containing no free variables are called pure functions.
- Functions containing one or more free variables are called closures.

This comes from [JavaScript Allonge](https://leanpub.com/javascriptallongesix/read#leanpub-auto-if-functions-without-free-variables-are-pure-are-closures-impure), which goes on to explain
>The function (y) => x is interesting. It contains a free variable (or non-local variable), x. A free variable is one that is not bound within the function. Up to now, we’ve only seen one way to 'bind' a variable, namely by passing in an argument with the same name. Since the function (y) => x doesn’t have an argument named x, the variable x isn’t bound in this function, which makes it 'free'.

Wikipedia says
>Free variable refers to variables used in a function that are neither local variables nor parameters of that function

In ['Javascript, understanding the weird parts'](https://www.udemy.com/understand-javascript), Anthony Alicea introduces a classic closure example
```
function buildFunctions () {

	let arr = [];

	for (i = 0; i < 3; i++) {
		arr.push(function() {
				console.log(i);
			});
	}
	return arr;

}

let fs = buildFunctions();
console.log(fs[0]()); // 3
console.log(fs[1]()); // 3
console.log(fs[2]()); // 3
```

The reason that the function logs '3' to the console in each instance is that the value of 'i' that is preserved in the lexical environment in which the function was created (but crucially, not run) is actually 3. This is because the for loop keeps on incrementing till it is no longer less than 3, so the value of 'i' when buildFunction returns the array is '3'. So when the functions that have been pushed into the array are finally invoked, the free variable (up the scope chain in the memory of the lexical environment) that they refer to is `i = 3`.

If you did want to bind the value of i for each increment, so you get
```
console.log(fs[0]()); // 0
console.log(fs[1]()); // 1
console.log(fs[2]()); // 2
```
You could use `let` to bind the variable 'i' to the body of the for loop.

```
for (let i = 0; i < 3; i++) {
  arr.push(function() {
      console.log(i);
    });
}
```

In addition, the `let` keyword re-binds it to each iteration, ensuring it is re-assigned the value from the previous loop iteration.
