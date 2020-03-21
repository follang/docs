---
title: "Comparison"
type: "docs"
weight: 300
---
Comparison operators are also defined both for primitive types and many type in the standard library. Parentheses are required when chaining comparison operators. For example, the expression `a == b == c` is invalid and may be written as `((a == b) == c)`.

Symbol  |	Meaning
---     | --- 
==	    | equal
!=	    | not equal
\>>	    | greater than
\<<	    | Less than
\>=	    | greater than or equal to
\<=	    | Less than or equal to


```
assert((123 == 123));
assert((23 != -12));
assert((12.5 >> 12.2));
assert(({1, 2, 3} << {1, 3, 4}));
assert(('A' <= 'B'));
assert(("World" >= "Hello"));
```
