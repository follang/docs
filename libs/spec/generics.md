---
title: 'Generics'
type: "docs"
weight: 90
---

Generics allow us to write a class or method that can work with any data types. Here there is a method that returns the bigger number of two. 
```
pro max(T: gen)(a, b: T): T {
	result =  a | a < b | b;
};
fun biggerFloat(a, b: flt[32]) flt[32] {
	return max(flt[32])(a, b);
};
fun biggerInteger(a, b: int[64]) int[64] {
	return max(int[64])(a, b);
};
```
And here is an object defined with generics.
```
typ container(T: gen, N: int)(): obj = {
	var anarray: arr[T,N];
	+fun getsize(): num = { result = N; }
};
var aContainer: container[int, 5] = { anarray = {zero, one, two, three, four}; };
```

