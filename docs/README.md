
<h2 style="color: red !important;">FOL IS STILL IN VERY VERY EARLY DEVELOPMENT</h2>

## __*Everything*__ in **FOL** is declared like below:

```
declaration[options] name: type[options] = { implementation; };
```

## top-most declarations
```
use    // imports, includes ...
def    // preporcesr, macros, bocks, definitions ...
var    // all variables, ints, strings, bools, arrays, vecotrs ...
pro    // subporgrams with side effects - procedures
fun    // subporgrams with no side effects - functions
typ    // new types, structs, objects, interfaces, enums ...
```
## control flow
```
if(condition){} else(condition){} else{};
for(){};
each(){};
loop(condition){};
case(variable){like(){}; like(){}; else{}};
case(variable){type(){}; type(){}; else{}};
```
## keywords
```
continue; break; return; yeild; jump; result; default; report; error; assert; check; test; or; and;
```
