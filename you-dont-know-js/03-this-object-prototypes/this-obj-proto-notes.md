## Chapter 1: This or That 

In complex usage patterns, reference to `this` object properties are often simpler and more elegant than explicitly passing around context objects. When you start using JS, lots of people try to use `this` within a function to refer to the function object's self. However, in that context, `this` is actually bound to something else (the global object).

## Chapter 2: `this` all makes sense now!

The only thing that's important when it comes to figuring out `this` binding is knowing the scope in which the function was invoked. The location in the call stack is key to revealing how `this` has been bound.

#### There are four rules that determine how `this` is bound
- 1. Default
If none of the other rules apply, the binding will default to the scope in which the function was invoked. For example:
```
function foo () {
  console.log( this.a );
}

var a = 2;

foo() // 2
```

In the code above, `this` defaults to the global scope, because that's where the function foo was invoked.

NB: in strict mode, the global object is not available for `this` binding so default binding would not occur.

- 2. Implicit
If the call site has a containing or owning object, the binding will implicitly be connected with this object. For example:
```
function foo () {
  console.log( this.a )
}

var obj = {
  a: 2,
  foo: foo
}

obj.foo() // 2
```

As the object property foo points to the function foo, and it is the object property foo that is called, `this` is implicitly bound to obj. Thus `this.a` is 2.

The most local reference is the one that matters. For example:
```
function foo () {
  console.log( this.a) 
}

var obj2 = {
  a: 42,
  foo: foo
}

var obj1 = {
  a: 2,
  obj2: obj2
}

obj1.obj2.foo() // 42
```

- 3. Lost Implicit
This is when a function reference is passed in with an implicit location (say you passed in an object property (method) that points to a function. Instead of pointing to the object, `this` then simply points to the function as that's what the object property name is pointing to. So the intended (implicit) location of `this` gets lost. Another way this can happen is that if a variable is created that is a reference to an object property, and this is then called from the global scope, `this` gets lost and is bound to the global object (as long as you're not using strict mode).
``` 
function foo () {
  console.log( this.a );
}

var obj = {
  a: 2,
  foo: foo
}

var bar = obj.foo;

var a = 'Oh noes!';

bar(); // Oh noes!
```

- 4. Explicit
Instead of leaving the binding to its default or implicit state, we can control the binding by explicitly passing an object to which `this` will be bound when a function is invoked.

So the code above could be handled as follows

``` 
function foo () {
  console.log( this.a );
}

var obj = {
  a: 2,
  foo: foo
}

var bar = obj.foo;

var a = 'Oh noes!';

bar.call(obj); // 2
```

It turns out, however, that explicit binding won't work in every circumstance, and isn't a silver bullet. Sometimes we're going to want to use hard binding, which involves wrapping the call() or apply() method in a function wrapper.

```
function foo () {
 console.log( this.a );
}

var obj = {
  a: 2,
  foo: foo
}

function bar () {
  foo.call(obj);
}

var baz = bar;
baz(); // 2
```

You can even try to overwrite this by calling it as `bar.call(window);` thus attempting to bind `this` to the global object, but the hard binding pattern in bar() has hard-bound `this` to `obj` when it's called, and can't be overwritten. 

Hard binding is such a common pattern that there is a bind() method built into Javascript. 

function foo (something) {
 console.log( this.a, something);
 return this.a + something;
}

var obj = {
 a: 2
}

var bar = foo.bind(obj);

var b = bar(3); // 2 3
console.log(b); // 5

