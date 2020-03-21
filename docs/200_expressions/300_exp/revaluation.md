---
title: "Compounds"
type: "docs"
weight: 600
---

There are further assignment operators that can be used to modify the value of an existing variable. These are the compounds or aka compound assignments. A compound assignment operator is used to simplify the coding of some expressions. For example, using the operators described earlier we can increase a variable's value by ten using the following code:
```
value = value + 10;
```
This statement has an equivalent using the compound assignment operator for addition (+=).

```
value += 10;
```

There are compound assignment operators for each of the six binary arithmetic operators: `+`, `-`, `*`, `/`, `%` and `^`. Each is constructed using the arithmetic operator followed by the assignment operator. The following code gives examples for addition `+=`, subtraction `-=`, multiplication `*=`, division `/=` and modulus `%=`:
```

var value: int = 10;
(value += 10);        // value = 20
(value -= 5);         // value = 15
(value *= 10);        // value = 150
(value /= 3);         // value = 50
(value %= 8);         // value = 2
```

Compound assignment operators provide two benefits. Firstly, they produce more compact code; they are often called shorthand operators for this reason. Secondly, the variable being operated upon, or operand, will only be evaluated once in the compiled application. This can make the code more efficient.
