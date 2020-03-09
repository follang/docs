---
title: "Specifications"
date: 2020-02-28T10:08:56+09:00
description: 
draft: false
collapsible: true
weight: 2
---

##    TYPES
```
primitive:
int, flt, chr, bol

container:
arr, seq, map, mat, set, any

complex:
num, str

special:
any, ptr, err
```

##    DEFINITIONS

```
use name: type = { modulename }
def name: type = { body }
```


## 	VARIABLES

```
var[opt] name: type = value	
var[opt] name: type = value, othername: type = value
var name: type = value
var[opt] name = value
var[opt] name: (type, type,...) = {value[0], value[1],...}
var[opt] name = value | {bool} | othervalue
```


##    FUNCTIONS

```
fun name(parameters): type = return
fun[opt] name(input output relation): type = {return}
fun[opt] name(parameters): type = {yeild}
fun[opt] name(a: list; lambda): list = {return}
fun 'operator'(two parameters max): type = {return}
fun (structname)name(parameters): type = return
```


##    STRUCTURES

```
typ[opt] name: type[opt] = {description}
typ[opt] name(inheritance): type[opt] = {description}
```


##    GENERICS

```
fun[opt] name(T: gen)(a: T, b: T): var[opt] = { body }
typ[opt] name(T: gen)(): obj[opt] = { body }
```
