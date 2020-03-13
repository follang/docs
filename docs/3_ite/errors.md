---
title: 'Errors'
type: "docs"
weight: 54
---
Unlike other programming languages, **FOL** does not have exceptions ([Rust](https://doc.rust-lang.org/book/ch09-00-error-handling.html) neither). It has only two types of errors:
- braking errors
- recoverable errors

Breaking errors cause a program to fail abruptly. A program cannot revert to its normal state if an unrecoverable error occurs. It cannot retry the failed operation or undo the error. An example of an unrecoverable error is trying to access a location beyond the end of an array.

Recoverable error are errors that can be corrected. A program can retry the failed operation or specify an alternate course of action when it encounters a recoverable error. Recoverable errors do not cause a program to fail abruptly. An example of a recoverable error is when a file is not found.


There are two keywords reserved and associated to two types of errors: `report` for a recoverable error and `panic` for the braking error. 

## Breaking error

`panic` keyword allows a program to terminate immediately and provide feedback to the caller of the program. It should be used when a program reaches an unrecoverable state. This most commonly occurs when a bug of some kind has been detected and it’s not clear to the programmer how to handle the error.

```
pro main(): int = {
    panic "Hello";
    .echo("End of main");                                                       //unreachable statement
}
```

In the above example, the program will terminate immediately when it encounters the `panic` keyword.
Output:
```
main.fol:3
routine 'main' panicked at 'Hello'
-------
```

Trying to acces an out of bound element of array:
```
pro main(): int = {
    var a: arr[int, 3] = [10,20,30];
    a[10];                                                                      //invokes a panic since index 10 cannot be reached
}
```
Output:
```
main.fol:4
routine 'main' panicked at 'index out of bounds: the len is 3 but the index is 10'
-------
a[10];
   ^-------- index out of bounds: the len is 3 but the index is 10

```
A program can invoke `panic` if business rules are violated, for example: if the value assigned to the variable is odd it throws an error:
```
pro main(): int = {
   var no = 13; 
   //try with odd and even
   if (no % 2 == 0) {
      .echo("Thank you , number is even");
   } else {
      panic "NOT_AN_EVEN"; 
   }
   .echo("End of main");
}
```

Output:
```
main.fol:9
routine 'main' panicked at 'NOT_AN_EVEN'
-------
```


## Recoverable error

`report` can be used to handle recoverable errors. As [discussed here](/docs/spec/functions/#return), FOL uses two variables `result` nd `error` in return of each subprogram. As name implies, `result` represents the type of the value that will be returned in a success case, and `error` represents the type of the error `err[]` that will be returned in a failure case.

When we use the keyword `report`, the error is returned to the subprogram's error variable and the subprogram qutis executing (the subprogram, not the program).
```
use file: mod[std] = { std::fs::File }

pro main(): int = {
    pro[] fileReader(path: str): str = {
        var aFile = file.readfile(path)
        if ( check(aFile) ) {
            report "File could not be opened" + file                        // report will not break the program, but will return the error here, and the subprogram will stop
        } else {
            return file.to_string()                                         // this will be executed only if file was oopened without error
        }
    }
}
```

Form this point on, the error is concatinated up to the main function. This is known as propagating the error and gives more control to the calling code, where there might be more information or logic that dictates how the error should be handled than what you have available in the context of your code.

```
use file: mod[std] = { std::fs::File }

pro main(): int = {
    var f = file.open("main.jpg");                                           // main.jpg doesn't exist
    if (check(f)) {
        report "File could not be opened" + file                             // report will not break the program
    } else {
        .echo("File was open sucessfulley")                                  // this will be executed only if file was oopened without error
    }
}
```

<h5 style="color:green; !important;">
By default, all errors are either conctinated up with report, or exited with panic.
</h5>

A simplier way to hande errors is through [pipes](/docs/spec/pipes)
