---
title: "Arithmetics"
type: "docs"
weight: 200
---
The behavior of arithmetic operators is only on intiger and floating point primitive types. For other types, there need to be operator overloading implemented.
 
 symbol | description
 --- | ---
\-   | substraction
\*   | multiplication
\+   | addition
/    | division
%    | reminder
^    | exponent

```
assert((3 + 6), 9);
assert((5.5 - 1.25), 4.25);
assert((-5 * 14), -70);
assert((14 / 3), 4);
assert((100 % 7), 2);
```
