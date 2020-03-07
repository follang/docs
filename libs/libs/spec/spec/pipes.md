---
title: 'Pipes'
type: "docs"
weight: 56
---

Piping is a process that connects the output of the expression to the left to the input of the expression of the right. You can think of it as a dedicated program that takes care of copying everything that one expressionm prints, and feeding it to the next expression. The idea is the same as `bash pipes`. For example, an subprogram output is piped to a conditional through pipe symbol `|` then the conditional takes the input and returns true or false. If returned false, then the second part of pipe is returned. To access the piped variable, `result` keyword is used:
```
pro[] main: int = {
    fun addFunc(x, y: int): int = {
        return x + y;
    }
    var aVar: int = addFunc(4, 5) | result > 8 | return 6;
}
```
However, when we assign an output of a function to a variable, we shoud expect that errors within funciton can happen. So the pipe's other function is to do the error checking. If the function returns an erros, the pipe does not continue further.

When a pipe flow is part of the main function, the pipe reports the error with `panic` and exitin the program, if it's part of nested function, it reports the error with `report`, thus sending this report the the function it belongs.

```
pro[] main: int = {
    fun addFunc(x, y: int): int = {
        var aVar: int = someFunction(x, y) | result > 8 | return 6;         // if someFunction throws an error, the pipe blocks the function here and reports the error to function
        return aVar;
    }

    var bVar: int = addFunc(4, 5) | result > 8 | return 6;                  // if addFunc throws an error, the pipe panics (because it is in the main function), thus exiting the program
}
```

To handle better the error, that we can use the `error` keyword to check: The code below basically says: **if the function internally had an error, don't exit the program, but assign another value (or default value) to the variable**:
```
var aVar: int = addFunc(4, 5) | error != nil | return 5;
```
If we want capture the error in case there is one (not relaying on the internal function handling - because it might not have at all) we use the double symbol `||`. This is one of the simplest way to handle errors.
```
var aVar: int = addFunc(4, 5) || "something bad inside function has happened";
```
More on error handling can be [found here](/docs/spec/errors) 


