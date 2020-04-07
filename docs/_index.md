---
title: "Docs"
description: "test post index"
date: 2020-01-11T14:09:21+09:00
draft: false

---

<span style="font-size: 2em !important; color: red !important;">FOL IS STILL IN VERY VERY EARLY DEVELOPMENT</span>

<span style="font-size: 1.6em !important;">Everything in FOL follows the same structure:</span>

```
declaration[options] name: type[options] = { implementation; };
```

## declarations
```
use    // imports, includes ...
def    // preporcesr, macros, bocks, definitions ...

var    // all variables: ordinal, container, complex, special

pro    // subporgrams with side effects - procedures
fun    // subporgrams with no side effects - functions
log    // subporgrams with logic only - logicals

typ    // new types: records, entries, classes, aiases, extensions
std    // standards: protocols, blueprints
```
## control flow
```
when(condition){ case (){}; case (){}; * {}; };
loop(condition){  };
```

&nbsp;
<hr>

## example
```
use log: mod[std] = {fmt::log};

def argo: mod[init] = {
    
    -var const: str = "something here"

    +pro main: int = {
        log.warn("Last warning!...");
        .echo(add(3, 5));
    }

    fun add(a, b: int): int = { a + b }
}
```
