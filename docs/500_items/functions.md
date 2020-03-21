---
title: 'Functions'
type: "docs"
weight: 50
---
A subprogram definition describes the interface to and the actions of the subprogram abstraction. A subprogram call is the explicit request that a specific subprogram be executed. A subprogram is said to be active if, after having been called, it has begun execution but has not yet completed that execution.A subprogram declaration consists of an identifier, zero or more argument parameters, a return value type and a block of code.
```
fun[] add(el1, el2: int[64]): int[64] = { result = el1 + el2 }
```

You’ve already seen one of the most important subprogram in the language: the main function, which is the entry point of many programs. You’ve also seen the `fun` or `pro` keyword, which allows you to declare new subprograms.

## Types
There are two main types of subprograms in fol:

- Procedurues

    A procedure is a piece of code that is called by name. It can be passed data to operate on (i.e. the parameters) and can optionally return data (the return value). All data that is passed to a procedure is explicitly passed.

- Functions

    A function is called pure function if it always returns the same result for same argument values and it has no side effects like modifying an argument (or global variable) or outputting to I/O. The only result of calling a pure function is the return value.

## Parameters
### Formal parameters

Subprogram typically describe computations. There are two ways that a subprogram can gain access to the data that it is to process: through direct access to nonlocal variables (declared elsewhere but visible in the subprogram) or through parameter passing. Data passed through parameters are accessed using names that are local to the subprogram. Subprogram create their own unnamed namespace. Every subprogram has its own Workspace. This means that every variable inside the subprogram is only usable during the execution of the subprogram (and then the variables go away). 

Parameter passing is more flexible than direct access to nonlocal variables. Prrameters are special variables that are part of a subprogram’s signature. When a subprogram has parameters, you can provide it with concrete values for those parameters. The parameters in the subprogram header are called formal parameters. They are sometimes thought of as dummy variables because they are not variables in the usual sense: In most cases, they are bound to storage only when the subprogram is called, and that binding is often through some other program variables.

Parameters are declared as a list of identifiers separated by semicolon (or by a colon, but for code cleanness, the semicolon is preferred). A parameter is given a type by : typename. If after the parameter the `:` is not declared, but `,` colon to identfy another paremeter, of which both parameters are of the same type if after the second one the `:` and the type is placed. Then the same type parameters continue to grow with `,` until `:` is reached.
```
fun[] calc(el1, el2, el3: int[64]; changed: bol = true): int[64] = { result = el1 + el2 - el3 }
```

In subprogram signatures, you must declare the type of each parameter. Requiring type annotations in subprogram definitions is obligatory, which means the compiler almost never needs you to use them elsewhere in the code to figure out what you mean. Subprogram can parameter overloaded too. It makes possible to create multiple subprogram of the same name with different implementations. Calls to an overloaded subprogram will run a specific implementation of that subprogram appropriate to the context of the call, allowing one subprogram call to perform different tasks depending on context:

```
fun retBigger(el2, el2: int): int = { return el1 | this > el2 | el2 }
fun retBigger(el2, el2: flt): flt = { return el1 | this > el2 | el2 }

pro main: int = {
    retBigger(4, 5);                                        // calling a subprogram with intigers
    retBigger(4.5, .3);                                     // calling another subprogram with same name but floats
}
```
The overloading resolution algorithm determines which subprogram is the best match for the arguments. Example:
```
pro toLower(c: char): char = {                              // toLower for characters
    if (c in {'A' ... 'Z'}){
        result = chr(ord(c) + (ord('a') - ord('A')))
    } else {
        result = c
    }
}

pro toLower(s: str): str = {                                // toLower for strings
    result = newString(.len(s))
    for i in {0 ... len(s) - 1}:
        result[i] = toLower(s[i])                           // calls toLower for characters; no recursion!
}
```

### Actual parameters
Subprogram call statements must include the name of the subprogram and a list of parameters to be bound to the formal parameters of the subprogram. These parameters are called actual parameters. They must be distinguished from formal parameters, because the two usually have different restrictions on their forms.

#### Positional parameters

The correspondence between actual and formal parameters, or the binding of actual parameters to formal parameters - is done by position: The first actual parameter is bound to the first formal parameter and so forth. Such parameters are called positional parameters. This is an effective and safe method of relating actual parameters to their corresponding formal parameters, as long as the parameter lists are relatively short. 

```
fun[] calc(el1, el2, el3: int): int = { result = el1 + el2 - el3 }

pro main: int = {
    calc(3,4,5);                                            // calling subprogram with positional arguments
}
```

#### Keyword parameters

When parameter lists are long, however, it is easy to make mistakes in the order of actual parameters in the list. One solution to this problem is with keyword parameters, in which the name of the formal parameter to which an actual parameter is to be bound is specified with the actual parameter in a call. The advantage of keyword parameters is that they can appear in any order in the actual parameter list. 

```
fun[] calc(el1, el2, el3: int): int = { result = el1 + el2 - el3 }

pro main: int = {
    calc(el3 = 5, el2 = 4, el1 = 3);                        // calling subprogram with keywords arguments
}
```
#### Mixed parameters
Keyword and positional arguments can be used at the same time too. The only restriction with this approach is that after a keyword parameter appears in the list, all remaining parameters must be keyworded. This restriction is necessary because a position may no longer be well defined after a keyword parameter has appeared.
```
fun[] calc(el1, el2, el3: int, el4, el5: flt): int = { result[0] = ((el1 + el2) * el4 ) - (el3 ** el5);  }

pro main: int = {
    calc(3, 4, el5 = 2, el4 = 5, el3 = 6);                  // element $el3 needs to be keyeorded at the end because 
                                                            // its positional place is taken by keyword argument $el5
}
```

### Default arguments
Formal parameters can have default values too. A default value is used if no actual parameter is passed to the formal parameter. The default parameter is assigned directly after the formal parameter declaration. The compiler converts the list of arguments to an array implicitly. The number of parameters needs to be known at compile time. 
```
fun[] calc(el1, el2, el3: rise: bool = true): int = { result[0] = el1 + el2 * el3 | this | el1 + el2;  }

pro main: int = {
    calc(3,3,2);                                            // this returns 6, last positional parameter is not passed but 
                                                            // the default `true` is used from the subprogram declaration
    calc(3,3,2,false)                                       // this returns 12
}
```

### Variadic subprograms
The use of `...` as the type of argument at the end of the argument list declares the subprogram as variadic. This must appear as the last argument of the subprogram. When variadic subprograms are used, the default arguments can not be used at the same time.
```
fun[] calc(rise: bool; ints: ... int): int = { result[0] = ints[0] + ints[1] + ints[2] * ints[3] | this | ints[0] + ints[1];  }

pro main: int = {
    calc(true,3,3,3,2);                                     // this returns 81, four parmeters are passed as variadic arguments
    calc(true,3,3,2)                                        // this returns 0, as the subprogram multiplies with the forth varadic parameter
                                                            // and we have given only three (thus the forth is initialized as zero)
}
```

`...` is called unpack operator - just like in Golang. In the subprogram above, you see `...`, which means pack all incoming arguments into `seq[int]` after the first argument. The sequence then is turned into a list at compile time.

{{% notice warn %}}

Nested procedures don't have access to the outer scope, while nested function have but can't change the state of it.

{{% /notice %}}

## Return

The return type of the subprogram has to always be defined, just after the formal parameter definition. Following the general rule of **FOL**: 
```
fun[] add(el1, el2: int[64]): int[64] = { result = el1 + el2 }
```

To make it shorter (so we don't have to type `int[64]` two times), we can use a *short form* by omitting the return type. The compiler then will assign the returntype the same as the functions return value.
```
fun[] add(el1, el2: int[64]) = { result = el1 + el2 }
```
{{% notice info %}}

Each function in FOL has two defined variables that are automatically returned at the end of the function.

{{% /notice %}}

Those variables are:
- a variable called `result`, which is the one that is returned and is same type as return type
- an error variable (called `error`), that can be reported from the funciton

{{% notice info %}}

Internally, those are a set of two variables, <em>set[result: any, eror: err]</em>. The result is of type any, and the any type shoud be known at compile time.

{{% /notice %}}

The implicitly declared variable `result` is of the same type of the return type. For it top be implicitly declared, the return type of the function shoud be always declared, and not use the short form. The variable is initialized with zero value, and if not changed during the body implementation, the same value will return (so zero).
```
pro main(): int = {
    fun[] add(el1, el2: int[64]): int[64] = { result = el1 + el2 }          // using the implicitly declared $result variable
    fun[] sub(el1, el2: int[64]) = { return el1 - el2 }                     // can't access the result variable, thus we use return
}
```
In addition, another implicitly decpared variable `error` of ype `err` is declared too. We talk for errors in [details here](/docs/spec/060_errors), but here is a short example:
```
pro main(): int = {
    fun[] add(el1, el2: int[64]): int[64] = { result = el1 + el2 }          // using the implicitly declared $result variable
    check(add(5,6))                                                         // this will check if the error is nil
}
```
The final expression in the function will be used as return value. For this to be used, the return type of the function needs to be defined (so the function cnat be in the short form)). ver this can be used only in one statement body.
```
pro main(): int = {
    fun[] add(el1, el2: int[64]): int[64] = { el1 + el2 }                   // This is tha last statement, this will serve as return
    fun[] someting(el1,el2: int): int = {
        if (condition) {

        } else {

        }
        el1 + el2                                                           // this will throw an error, cand be used in kulti statement body
    }
    fun[] add(el1, el2: int[64]) = { el1 + el2 }                            // this will throw an error, we can't use the short form of funciton in this way
```
Alternatively, the `return` and `report` statements can be used to return a value or error earlier from within the function, even from inside loops or other control flow mechanisms.
**The example below is just to show the `return` and `report` statements, there is a better way to handle errors as shown in [error section](/docs/spec/error)**
```
use file: mod[std] = { std::fs::File }

pro main(): int = {
    fun[] fileReader(path: str): str = {
        var aFile = file.readfile(path)
        if ( check(aFile) ) {
            report "File could not be opened" + file                        // report will not break the program, but will return the error here, and the funciton will stop
        } else {
            return file | stringify(this) | return $                        // this will be executed only if file was oopened without error
        }
    }
}
```
## Procedurues

Procedures are most common type of subprograms in Fol. When a procedure is "called" the program "leaves" the current section of code and begins to execute the first line inside the procedure. Thus the procedure "flow of control" is:

- The program comes to a line of code containing a "procedure call".
- The program enters the procedure (starts at the first line in the procedure code).
- All instructions inside of the procedure are executed from top to bottom.
- The program leaves the procedure and goes back to where it started from.
- Any data computed and RETURNED by the procedure is used in place of the procedure in the original line of code.

Procedures have side-effects, it can modifies some state variable value(s) outside its local environment, that is to say has an observable effect besides returning a value (the main effect) to the invoker of the operation. State data updated "outside" of the operation may be maintained "inside" a stateful object or a wider stateful system within which the operation is performed.

### Passing values

The semantics for passing a value to a procedure are similar to those for assigning a value to a variable. Passing a variable to a procedure will move or copy, just as assignment does. If the procedure is stack-based, it will automatically copy the value. If it is heap-based, it will move the value. 
```
pro[] modifyValue(someStr: str) = {
    someStr = someStr + " world!"
}

pro[] main: int =  {
                                        //case1
    var[mut] aString: str = "hello";                        // a string varibale $aString is declared (in stack as default)
    modifyValue(aString);                                   // the value is passed to a procedure, since $aVar is in stack, the value is copied
    .echo(aString)                                          // this prints: "hello", 
                                                            // value is not changed and still exists here, because was copied

                                        //case2
    @var[mut] aString: str = "hello";                       // a string varibale $bString is declared (in stack with '@')
    modifyValue(bString);                                   // the value is passed to a procedure, since $aVar is in heap, the value is moved
    .echo(bString)                                          // this throws ERROR, 
                                                            // value does not exists anymore since it moved and ownership wasn't return
}
```
As you can see from above, in both cases, the `.echo(varable)` does not reach the desired procedure, to print `hello world!`. In first case is not changed (because is coped), in second case is changed but never returned. To fix the second case, we can just use the `.give_back()` procedure to return the ownership:
```
pro[] modifyValue(someStr: str) = {
    someStr = someStr + " world!"
    .give_back(someStr)                                     // this returns the ownership (if there is an owner, if not just ignores it)
}

pro[] main: int =  {
                                        //case1
    var[mut] aString: str = "hello";                        // a string varibale $aString is declared (in stack as default)
    modifyValue(aString);                                   // the value is passed to a procedure, since $aVar is in stack, the value is copied
    .echo(aString)                                          // this still prints: "hello", 
                                                            // value is not changed and still exists here, because was copied

                                        //case2
    @var[mut] aString: str = "hello";                       // a string varibale $bString is declared (in stack with '@')
    modifyValue(bString);                                   // the value is passed to a procedure, since $aVar is in heap, the value is moved
    .echo(aString)                                          // this now prints: "hello world!", 
                                                            // value now exists since the ownership is return
}
```
### Lend parameters

But now, we were able to change just the variable that is defined in heap (case two), by moving back the ownership. In case one, since the value is copied, the owner of newly copied value is the procedure itself. So the `.give_back()` is ignored. To fix this, we use [borrowing](/docs/spec/050_pointers/#borrowing) to lend a value to the procedure
```
pro[] modifyValue((someStr): str) = {                         // we use `(someStr)` to mark it as borrowable
    someStr = someStr + " world!"
}

pro[] main: int =  {
                                        //case1
    var[mut] aString: str = "hello";                        // a string varibale $aString is declared (in stack as default)
    modifyValue(aString);                                   // the value is lended to the procedure
    .echo(aString)                                          // this now prints: "hello world!", 

                                        //case2
    @var[mut] aString: str = "hello";                       // a string varibale $bString is declared (in heap with '@')
    modifyValue(aString);                                   // the value is lended to the procedure
    .echo(aString)                                          // this now prints: "hello world!", 
}
```

So to make a procedure borrow a varibale it uses `(varName)`. 
```
pro[] borrowingProcedure(aVar: str; (bVar): bol; cVar, (dVar): int)
```

To call this procedure, the borrowed parameters always shoud be a variable name and not a direct value:

```
var aBool, anInt = true, 5
borrowingProcedure("get", true, 4, 5)                        // this will throw an error, cos it expects borrowable not direct value
borrowingProcedure("get", aBool, 4, anInt)                   // this is the proper way

```

If all parameters are going to be borrowable, then the procedure can encapsulate all the parameters in double brackets `(( //parameters ))`:
```
pro[] borrowingProcedure((aVar: str; bVar: bol; cVar, dVar: int))
```

When the value is passed as borrowable in procedure, by default it gives premission to change, so the same as `var[mut, bor]` as [disscussed here](/docs/spec/050_pointers/#borrowing).

### Return ownership

Return values can be though as return of ownership too. The ownership of a variable follows the same pattern every time: assigning a value to another variable moves or copies it. 
```
pro main(): int = {
    var s1 = givesOwnership();                              // the variable $s1 is given the ownership of the procedure's $givesOwnership return
    .echo(s1)                                               // prints "hi"
    var s2 = returnACopy();                                 // the variable $s2 is given the ownership of the procedure's $returnACopy return
    .echo(s2)                                               // prints: "there"
}
pro givesOwnership(): str = {                               // This procedure will move its return value into the procedure that calls it
    @var someString = "hi";                                 // $someString comes into scope
    return someString                                       // $someString is returned and MOVES out to the calling procedure
}
pro returnACopy(): int = {                                  // This procedure will move its return value into the procedure that calls it
    var anotherString = "there"                             // $anotherString comes into scope
    return anotherString                                    // $anotherString is returned and COPIES out to the calling procedure
}
```
When a variable that includes data on the heap goes out of scope, the value will be cleaned up automatically by `.de_alloc()` unless the data has been moved to be owned by another variable, in this case we give the ownership to return value. If the procedure with the retun value is not assigned to a variable, the memory will be freed again.

We can even do a transfer of ownership by using this logic:
```
pro main(): int = {
    @var s2 = "hi";                                         // $s2 comes into scope (allocatd in the heap)
    var s3 = transferOwnership(s2);                         // $s2 is moved into $transferOwnership procedure, which also gives its return ownership to $s3
    .echo(s3)                                               // prints: "hi"
    .echo(s2)                                               // this throws an error, $s2 is not the owner of anything anymore
}

pro transferOwnership(aString: str): str = {                // $aString comes into scope
    return aString                                          // $aString is returned and moves out to the calling procedure
}
```

This does not work with borrowing though. When a variable is lended to a procedure, it has permissions to change, but not lend to someone else. The only thing it can do is make a `.deep_copy()` of it:
```
pro main(): int = {
    @var s2 = "hi";                                         // $s2 comes into scope (allocatd in the heap)
    var s3 = transferOwnership(s2);                         // $s2 is moved into $transferOwnership procedure, which also gives its return ownership to $s3
    .echo(s3)                                               // prints: "hi"
    .echo(s2)                                               // prints: "hi" too
}

pro transferOwnership((aString: str)): str = {              // $aString comes into scope which is borrowed
    return aString                                          // $aString is borrowed, thus cant be lended to someone else
                                                            // thus, the return is a deep_copy() of $aString
}
```
## Functions

Functions compared to procedure are pure. A pure function is a function that has the following properties:

- Its return value is the same for the same arguments (no variation with local static variables, non-local variables, mutable reference arguments or input streams from I/O devices).
- Its evaluation has no side effects (no mutation of local static variables, non-local variables, mutable reference arguments or I/O streams).

Thus a pure function is a computational analogue of a mathematical function. Pure functions are declared with `fun[]`
```
fun[] add(el1, el2: int[64]): int[64] = { result = el1 + el2 }
```
{{% notice warn %}}

Functions in FOL are lazy-initialized. 

{{% /notice %}}

So it is an evaluation strategy which delays the evaluation of the function until its value is needed. You call a function passing it some arguments that were expensive to calculate and then the function don’t need all of them due to some other arguments.

Consider a function that logs a message:
```
log.debug("Called foo() passing it " + .to_string(argument_a) + " and " + .to_string(argument_b));
```
The log library has various log levels like “debug”, “warning”, “error” etc. This allows you to control how much is actually logged; the above message will only be visible if the log level is set to the “debug” level. However, even when it is not shown the string will still be constructed and then discarded, which is wasteful.


{{% notice tip %}}

Since Fol supports first class functions, it allows functions to be assigned to variables, passed as arguments to other functions and returned from other functions.

{{% /notice %}}

### Anonymous functoins

Anonymous function is a function definition that is not bound to an identifier. These are a form of nested function, in allowing access to variables in the scope of the containing function (non-local functions).

Staring by assigning a anonymous function to a vriable:
```
var[] f = fun[] (a, b: int): int = {                                    // assigning a variable to function
    return a + b
}
.echo(f(5,6))                                                           // prints 11
```

It is also possible to call a anonymous function without assigning it to a variable.
```
fun[] (a, b: int) = {                                                   //define anonymous function
    .echo(a + b)
}(5, 6)                                                                 //calling anonymous function
```


### Closures
Functions can appear at the top level in a module as well as inside other scopes, in which case they are called nested functions. A nested function can access local variables from its enclosing scope and if it does so it becomes a closure. Any captured variables are stored in a hidden additional argument to the closure (its environment) and they are accessed by reference by both the closure and its enclosing scope (i.e. any modifications made to them are visible in both places). The closure environment may be allocated on the heap or on the stack if the compiler determines that this would be safe. 

There are two types of closures:
- anonymous
- named

Anonymus closures automatically capture variables, while named closures need to be specified what to capture. For capture we use the `[]` just before the type declaration.
```
fun[] add(n: int): int = {
    fun added(x: int)[n]: int = {                                       // we make a named closure 
        return x + n                                                    // variable $n can be accesed because we have captured ti
    }    
    return adder()
}

var added = add(1)                                                      // assigning closure to variable
added(5)                                                                // this returns 6
```

```
fun[] add(n: int): int = {
    return fun(x: int): int = {                                         // we make a anonymous closure 
        return x + n                                                    // variable $n can be accesed from within the nested function
    }
}
```

### Currying
Currying is converting a single function of "n" arguments into "n" functions with a "single" argument each. Given the following function:
```
fun f(x,y,z) = { z(x(y));}
```
When curried, becomes:
```
fun f(x) = { fun(y) = { fun(z) = { z(x(y)); } } }
```
 And calling it woud be like:
 ```
f(x)(y)(z)
 ```
However, the more iportant thing is taht, currying is a way of constructing functions that allows partial application of a function’s arguments. What this means is that you can pass all of the arguments a function is expecting and get the result, or pass a subset of those arguments and get a function back that’s waiting for the rest of the arguments. 
 ```
fun calc(x): int = {
    return fun(y): int = {
        return fun (z): int = {
            return x + y + z
        } 
    }
}

var value: int = calc(5)(6)                                             // this is okay, the function is still finished
var another int = value(8)                                              // this completes the function

var allIn: int = calc(5)(6)(8)                                          // or this as alternative
 ```

### Higer-order functions
A higher-order function is a function that takes a function as an argument. This is commonly used to customize the behavior of a generically defined function, often a looping construct or recursion scheme.

They are functions which do at least one of the following:

- takes one or more functions as arguments
- returns a function as its result

```
//function as parameter
fun[] add1({fun adder(x: int): int}): int = {
    return adder(x + n)
}

//function as return
fun[] add2(): {fun (x: int): int} = {
    var f = fun (a, b: int): int = {
        return a + b
    }    
    return f
}
```
### Generators
A generator is very similar to a function that returns an array, in that a generator has parameters, can be called, and generates a sequence of values. However, instead of building an array containing all the values and returning them all at once, a generator yields the values one at a time, which requires less memory and allows the caller to get started processing the first few values immediately. In short, a generator looks like a function but behaves like an iterator.

For a function to be a generator (thus to make the keyword `yeild` accesable), it needs to return a type of container: `arr, vec, seq, mat` but not `set, any`.
```
fun someIter: vec[int] = {
    var curInt = 0;
    loop(){
        yeild curInt.inc(1)
    }
}
```

## Methods

There is another type of subprogram, called method, but it can be either a pure function either a procedure. A method is a piece of code that is called by a name that is associated with an object where it is implicitly passed the object on which it was called and is able to operate on data that is contained within the object.

They either are defined inside the object, or outside the object then the object in which they operate is passed like so (just like in [Golang]()):
```
pro (object)getDir(): str = { result = self.dir; };
```

