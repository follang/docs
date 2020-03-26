---
title: 'Control'
type: "docs"
weight: 25
---

At least two linguistic mechanisms are necessary to make the computations in programs flexible and powerful: some means of selecting among alternative control flow paths (of statement execution) and some means of causing the repeated execution of statements or sequences of statements. Statements that provide these kinds of capabilities are called control statements. A control structure is a control statement and the collection of statements whose execution it controls. This set of statements is in turn generally structured as a block, which in addition to grouping, also defines a lexical scope. 


## Choice type
### Condition
```
if(condition){} else(condition){} else{};
```
Checking equality 
```
if(x == 6) {
    // implementation
} else(x == 7) {
    // implementation
} else {
    // default implementation
}
```

### Selection
```
if(variable){ is (value){}; is (value){}; * {}; };
if(variable){ in (iterator){}; in (iterator){}; * {}; };
if(iterable){ has (member){}; has (member){}; * {}; };
if(variable){ of (type){}; of (type){}; * {}; };
if(type){ on (channel){}; on (channel){}; };
```

## Loop type

### Iteration
```
each(iterable){};
```

With start- and end-values 
```
each(x in .range(0..100)){
    //implementation
}

each(x in (.range(0..100)) if ( x % 2 = 0 )){
    //implementation
}
```
With enumeration of containers
```
each(x in array){
    // implementation
}
```
### Repetition
```
loop(condition){};
```

Condition befor block
```
loop(true){
    // implementation
};
```
Block before condition
```
loop{
    // implementation
}(true);
```

