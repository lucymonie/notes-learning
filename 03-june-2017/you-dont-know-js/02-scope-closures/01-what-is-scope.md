## Notes from *You Don't Know JS* by Kyle Simpson

[Book 2: Scope & Closures](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20&%20closures/README.md#you-dont-know-js-scope--closures)
[Chapter 1: What is Scope?](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch1.md)
Scope is a set of rules governing the storage of variables in some location, and finding those variables again at another time.  

Javascript is compiled before it's executed (usually just before it's executed). When the code is being compiled, variables are examined and sorted into an Abstract Syntax Tree (AST)   
>The tree for `var a = 2`; might start with a top-level node called VariableDeclaration, with a child node called Identifier (whose value is a), and another child called AssignmentExpression which itself has a child called NumericLiteral (whose value is 2).  

The Javascript engine then takes the AST and translates it into machine code to perform actions like saving the varaible 'a' in memory and so on.  

**Interaction between engine, compiler and scope**
Engine: runs the code, end to end
Compiler: sorts the code and make sense of it  
Scope: keeps a record of the relationships between variables

The compiler would take the code snippet `var a = 2;` and first check if there was already a variable called 'a' in the current scope. If there was, it would skip over this instruction and move on. If there was no existing variable, compiler would request for scope to add this new variable to the scope collection.  

The compiler would then create code for the engine to execute, handling the assignment `a = 2`. At runtime, the engine would request scope to look for a variable called 'a' that's accessible in the current scope collection. If there's nothing there, the engine looks in the next outer scope container for the variable. It goes on widening the scope until it finds the variable or logs an error.

**Assignment and reference**
When the engine runs the code produced by the compiler, there are two ways that it approaches variables. How it handles them depends on which side of the variable assignment they're on.  

A LHS lookup is about what's in scope under that name, and the taxonomy of variables, and ultimately about assignment. It occurs when there's an `=` or an argument is passed into a function and is thereby assigned to a parameter of that function.  

A RHS lookup is about retrieving the value that's been assigned to that variable, moving from the innermost scope to the outermost.  

**Additional notes**
If the engine is making a reference (RHS) lookup and it's not available in any scope, the engine will throw a `ReferenceError`. However, if it's a LHS lookup and the declaration is nowhere to be found, the scope will create a global variable and give that to the engine to use. Confusingly, the engine would likely then throw a `TypeError` rather than a `ReferenceError` - this is because there is something to match the reference, but its value is undefined. Use of Strict Mode can avoid this sort of issue.  

Function declarions don't work the same as LHS declarations as the compiler handles both value of the function and its name assignment.  
