## Notes from *You Don't Know JS: Scope & Closures* by Kyle Simpson

[Chapter 2: Lexical Scope](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch2.md)
There are two different types of scope used in programming languages, lexical scope and dynamic scope. JS uses lexical, which is vastly more common.  

Lexical scope is about how your code is structured. It's a taxonomy based on what you've written, and it defines the relationships between identifiers in your code.

In best practice, lexical scope is defined at write time, and you don't mess with it.  

```
function foo(a) {

  var b = a * 2;

  function bar (c) {
    console.log(a, b, c);
  }

}

foo(2); // 2 4 12
```
The code above has three nested scopes:
1. the global scope, which has a reference to foo
2. inside foo, which has access to a
3. inside bar, which has access to c

Since b is not defined in the scope of bar, the JS engine would look for it in the next outwardly nested scope, and it would find it there, and search no further. If, however, you wanted to access a variable by the same name but in the global scope, you could specifically identify it by referencing it as a property of the glocal object, as `window.b`.

There's a pattern called shadowing, where the same identifier name can be specified at multiple layers of nested scope (the inner identifier 'shadows' the outer identifier). References to `window` are the only way to override shadowed identifiers.

There are two ways to get around lexical scope, but neither is recommended or considered good practice. Both can be used at runtime to mess with the lexical scope
1. eval()
`eval()` takes a string with declarations in it and parses it as code

2. with
`with` takes an object and adds its properties as scoped identifiers

Strict mode makes it impossible to use `with` at all, and limits the use of `eval()` to its core functionality.
