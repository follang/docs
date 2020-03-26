---
title: 'Aliases'
type: "docs"
weight: 55
---

An alias declaration binds an identifier to an existing type. All the properties of the existing type are bound to the alias too.

There are two type of aliasing:
- aliasing
- extending

## Aliasing

```
ali I5: arr[int, 5];
```

So now the in the code, instead of writing `arr[int, 5]` we could use `I5`:

```
~var[pub] fiveIntigers: I5 = { 0, 1, 2, 3, 4, 5 }
```
Another example is creating a `rgb` type that can have numbers only form 0 to 255:
```
ali rgb: int[8][.range(0 ... 255)] ;                  // we create a type that holds only number from 0 to 255
ali rgbSet: set[rgb, rgb, rgb];                       // then we create a type holding the `rgb` type
```

Alias declaration are created because they can simplify using them multiple times, their identifier (their name) may be expressive in other contexts, and–most importantly–so that you can define (attach) methods to it (you can't attach methods to built-in types, nor to anonymous types or types defined in other packages).

Attaching methods is of outmost importance, because even though instead of attaching methods you could just as easily create and use functions that accept the "original" type as parameter, only types with methods can implement standards `std[]` that list/enforce those methods, and you can't attach methods to certain types unless you create a new type derived from them.


## Extending

Extensions add new functionality to an existing constructs. This includes the ability to extend types for which you do not have access to the original source code (known as retroactive modeling). They are identified with `ext`:
```
ali type: type[];
```
For example, adding a `print` function to the default integer type `int`:
```
ali int: int[];

pro (int)print(): non {
    .echo(self)
}

pro main: int = {
    5.print()                   // method print on int
}
```

