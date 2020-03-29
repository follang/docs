---
title: 'Matching'
type: "docs"
weight: 450
---

## Variable
As variable assignment:
```
var checker: str = when(variable) { 
    in .range(1..10) => "in range of 1-10"; 
    in .range(11..20) => "in range of 11-20";
    * => "out of range";
}

var is_it: int = when(variable) { 
    is "one" => 1; 
    is "two" => 2; 
    * => 0;
}

var has_it: bol = when(variable) { 
    has "o", "k" => true; 
    * => false;
}
```


## Function
As function return:
```
fun someValue(variable: int): str = when(variable) { 
    in .range(1..10) => "1-10"; 
    in .range(11..20) => "11-20"; 
    * => "0";
}
```

