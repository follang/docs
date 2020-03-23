---
title: 'Rolling'
type: "docs"
weight: 500
---

Rolling or list comprehension is a syntactic construct available FOL for creating a list based on existing lists. It follows the form of the mathematical set-builder notation - set comprehension.


Rolling has the same syntactic components to represent generation of a list in order from an input list or iterator:

- A variable representing members of an input list.
- An input list (or iterator).
- An optional predicate expression.
- And an output expression producing members of the output list from members of the input iterable that satisfy the predicate.

The order of generation of members of the output list is based on the order of items in the input. 

```
var aList: vec[] = { .range(..12) };

var another: vec[] = { ( x * x ) for ( x in aList ) if ( x % 3 == 0 ) }
```


