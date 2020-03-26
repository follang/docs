---
title: "Docs"
description: "test post index"
date: 2020-01-11T14:09:21+09:00
draft: false

---

<h2 style="color: red !important;">FOL IS STILL IN VERY VERY EARLY DEVELOPMENT</h2>

## __*Everything*__ in **FOL** is declared like below:

```
declaration[options] name: type[options] = { implementation; };
```

## top-most declarations
```
use    // imports, includes ...
def    // preporcesr, macros, bocks, definitions ...
var    // all variables: ordinal, container, complex, special
pro    // subporgrams with side effects - procedures
fun    // subporgrams with no side effects - functions
ali    // new types: aiases, extensions
typ    // new types: records, enums, unions, classes
std    // standards: protocols, blueprints
```
## control flow
```
if(condition){} else(condition){} else{};
if(variable){ is (){}; is (){}; * {}; };
each(iterator){};
loop(condition){};
```
## keywords
```
continue; break; return; yeild; jump; result; report; panic; error; assert; check; test; or; and; ...
```
