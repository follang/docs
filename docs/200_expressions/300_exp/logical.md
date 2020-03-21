---
title: "Logical"
type: "docs"
weight: 500
---
A branch of algebra in which all operations are either true or false, thus operates only on booleans, and all relationships between the operations can be expressed with logical operators such as:

- `and` (conjunction), denoted `(x and y)`, satisfies `(x and y) = 1` if `x = y = 1`, and `(x and y) = 0` otherwise.
- `or` (disjunction), denoted `(x or y)`, satisfies `(x or y) = 0` if `x = y = 0`, and `(x or) = 1` otherwise.
- `not` (negation), denoted `(not x)`, satisfies `(not x) = 0` if `x = 1` and (not x) = 1` if `x = 0`.

```
assert((true and false), (false and true));
assert((true or false), true)
assert((not true), false)
```
