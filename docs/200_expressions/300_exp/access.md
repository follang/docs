---
title: "Access"
type: "docs"
weight: 500
---

There are four access expresions:
- namespace member access
- subprogram member access
- container memeber access
- field member access


## Subprogram access

In most programming languages, it is called "method-call expresion". A method call consists of an expression (the receiver) followed by a single dot `.`, an expression path segment, and a parenthesized expression-list:
```
"3.14".cast(float).pow(2);                  // casting a numbered string to float, then rising it to power of 2
```


## Namespaces access

Accesing namespaces is done through double colon operator `::`:
```
use log mod[std] = { fmt::log };            // using the log namespace of fmt
io::console::write_out.echo();              // echoing out
```


## Container access
Containers can be indexed by writing a square-bracket-enclosed expression of type `int[arch]` (the index) after them.
```
var collection: int = { 5, 4, 8, 3, 9, 0, 1, 2, 7, 6 }

collection[5]                               // get the 5th element staring from front (this case is 0)
collection[-2]                              // get the 3th element starting from back (this case is 1)
```

Containers can be accessed with a specified range too, by using colon within a square-bracket-enclosed:

syntax | meaning
--- | ---
`:` | the whole container
elA`:`elB | from element `elA` to element `elB`
`:`elA | from beginning to element `elA`
elA`:` | from element `elA` to end

```
collection[-0]                              // last item in the array
{ 6 }
collection[-1:]                             // last two items in the array
{ 7, 6 }
collection[:-2]                             // everything except the last two items
{ 5, 4, 8, 3, 9, 0, 1, 2 }
```

If we use double colon within a square-bracket-enclosed then the collection is inversed:

syntax | meaning
--- | ---
`::` | the whole container in reverse
elA`::`elB | from element `elA` to element `elB` in reverse
`::`elA | from beginning to element `elA` in reverse
elA`::` | from element `elA` to end in reverse

```
collection[::]                              // all items in the array, reversed
{ 6, 7, 2, 1, 0, 9, 3, 8, 4, 5 }
collection[2::]                             // the first two items, reversed
{ 4, 5 }
collection[-2::]                            // the last two items, reversed
{ 6, 7 }
collection[::-3]                            // everything except the last three items, reversed
{ 2, 1, 0, 9, 3, 8, 4, 5 }
```


## Field access

Field access expressoin accesses fields inside constructs. Here is a recorcalled `user`:
```
var user1: user = {
    email = "someone@example.com",
    username = "someusername123",
    active = true,
    sign_in_count = 1
};

fun (user)getName(): str = { result = self.username; };
```
There are two types of fields that can be accesed within constructs:
- methods
- data

### Methods

Methods are accesed the same way like subprogram member access.
```
user1.getName()
```

### Data


There are multiple ways to acces data within the construct. The easiest one is by dot operator `.`:
```
user1.email                                 // accessing the field through dot-accesed-memeber
```

Another way is by using square bracket enclosed by name:
```
user1[email]                                // accessing the field through square-bracket-enclosed by name
```

And lastly, by square bracket enclosed by index:
```
user1[0]                                    // accessing the field through square-bracket-enclosed by index

```
