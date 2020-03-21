---
title: 'Control'
type: "docs"
weight: 25
---

At least two linguistic mechanisms are necessary to make the computations in programs flexible and powerful: some means of selecting among alternative control flow paths (of statement execution) and some means of causing the repeated execution of statements or sequences of statements. Statements that provide these kinds of capabilities are called control statements. A control structure is a control statement and the collection of statements whose execution it controls. This set of statements is in turn generally structured as a block, which in addition to grouping, also defines a lexical scope. 


## Choice type
```
if(condition){} else(condition){} else{};
case(variable){like(){}; like(){}; none{}};
case(variable){type(){}; type(){}; none{}};
```

## Loop type
```
for(iterator){};
loop(condition){};
```

