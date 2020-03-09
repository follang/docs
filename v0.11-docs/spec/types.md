---
title: 'Types'
type: "docs"
weight: 30
---

Every value in Fol is of a certain data type, which tells Fol what kind of data is being specified so it knows how to work with that data. There are four main subsets: simple, container, complex and special.

## Simple

Simple types are the most primitive type of data:
```
int[options], flt[options], chr[options], bol
```

### Intiger type
An integer is a number without a fractional component. We used one integer of the u32 type, the type declaration indicates that the value it’s associated with should be an unsigned integer (signed integer types start with i, instead of u) that takes up 32 bits of space: 
```
var aVar: int[u32] = 45;
```
Each variant can be either signed or unsigned and has an explicit size. Signed and unsigned refer to whether it’s possible for the number to be negative or positive—in other words, whether the number needs to have a sign with it (signed) or whether it will only ever be positive and can therefore be represented without a sign (unsigned). It’s like writing numbers on paper: when the sign matters, a number is shown with a plus sign or a minus sign; however, when it’s safe to assume the number is positive, it’s shown with no sign.
```
Length  |   Signed  | Unsigned  |
-----------------------------------
8-bit   |   8       |   u8      |
16-bit  |   16      |	u16     |
32-bit	|   32      |	u32     |
64-bit	|   64	    |   u64     |
128-bit	|   128     |	u128    |
arch	|   arch    |	uarch   |
```

### Float type
Fol also has two primitive types for floating-point numbers, which are numbers with decimal points. Fol’s floating-point types are `flt[32]` and `flt[64]`, which are 32 bits and 64 bits in size, respectively. The default type is `flt[64]` because on modern CPUs it’s roughly the same speed as `flt[32]` but is capable of more precision. 
```
Length  |    Type  |
--------------------
32-bit	|   32     |
64-bit	|   64     |
arch	|   arch   |
```
Floating-point numbers are represented according to the IEEE-754 standard. The `flt[32]` type is a single-precision float, and `flt[f64]` has double precision.

```
pro[] main: int = {
    var aVar: flt = 2.;                         // float 64 bit
    var bVar: flt[64] = .3;                     // float 64 bit
    .assert(.sizeof(aVar) == .sizeof(bVar))     // this will true

    var bVar: flt[32] = .54;                    // float 32 bit
}
```
### Character type
In The Unicode Standard 8.0, Section 4.5 "General Category" defines a set of character categories. Fol treats all characters in any of the letter as Unicode letters, and those in the Number category as Unicode digits. 
```
chr[utf8,utf16,utf32]
```

```
def testChars: tst["some testing on chars"] = {
    var bytes = "hello";
    .assert(.typeof(bytes) == *var [5:0]u8);
    .assert(bytes.len == 5);
    .assert(bytes[1] == 'e');
    .assert(bytes[5] == 0);
    .assert('e' == '\x65');
    .assert('\u{1f4a9}' == 128169);
    .assert('💯' == 128175);
    .assert(.mem.eql(u8, "hello", "h\x65llo"));
}
```
### Boolean type
The boolean type is named `bol` in Fol and can be one of the two pre-defined values `true` and `false`. 
```
bol
```
## Containers
Containers re compound types. They contain other primitive types. To access the types in container those brackets are used: `[]`, so:
```
var container: type = { element, element, element }             // declaring a container
var varable: type = container[2]                                // accessing the last element
```
<h5 style="color:red; !important;">Containers are always zero indexed</h5>

### Arrays
```
arr[type,size]
```
Arrays are the most simple type of container. They contain homogeneous type, meaning that each element in the array has the same type. Arrays always have a fixed length specified as a constant expression `arr[type, size]`. They can be indexed by any ordinal type to acces its members.

```
pro[] main: int = {
    var anArray: arr[int, 5] = { 0, 1, 2, 3, 4 };             // declare an array of intigers of five elements
    var element = anArray[3];                                 // accessing the element
    .echo(element)                                            // prints: 3
}
```

### Sets
```
set[type,type,type..]
```
A set is a general way of grouping together a number of values with a variety of types into one compound type. Sets have a fixed length: once declared, they cannot grow or shrink in size. In other programming languages they usually are referenced as tuples.
```
pro[] main: int = {
    var aSet: set[str, flt, arr[int, 2]] = { "go", .3, { 0, 5, 3 } };
    var element = aSet[2][1];                                 // accessing the [1] element of the `arr` in the set
    .echo(element)                                            // prints: 5
}
```

### Sequences
```
seq[type]
```

Sequences are similar to arrays but of dynamic length which may change during runtime (like strings). Sequences are implemented as growable arrays, allocating pieces of memory as items are added. A sequence `s` is always indexed by integers from `0` to `.len(s)-1` and its bounds are checked.
```
pro[] main: int = {
    var aSequence: arr[str] = { "get", "over", "it" };        // declare an array of intigers of five elements
    aSequence.add("pal!")
    var element = aSequence[3];                               // accessing the element
    .echo(element)                                            // prints: pal!
}
```
The lower bound of an array or sequence may be received by the built-in `.low()`, the higher bound by `.high()`. The length may be received by `.len()`.
<h5 style="color:red; !important;">Sequences are a growable arrays, thus if not allocated in heap but in stack, the size will be defined automatically in compile time and will be changet to static containers (array)</h5>

To allocate memory on heap, the `var[new]` is used [ more about memory, ownreship and pointer ](/std/spec/050_pointers/):

```
pro[] main: int = {
    var[new] aSequence: arr[str] = { "get", "over", "it" };   // this sequence is stored in stack
}
```
### Maps
```
map[key,value]
```
A map is an unordered group of elements of one type, called the element type, indexed by a set of unique keys of another type, called the key type.
```
pro[] main: int = {
    var aMap: map[str, int] = { { "US", 45 }, { "DE", 82 }, { "AL", 54 } };
    var element = aMap["US"];                                 // accessing the "US" key
    .echo(element)                                            // prints: 45
}
```
The number of map elements is called its length. For a map `aMap`, it can be discovered using the built-in function `.len` and may change during execution To add a new element, we use `add` function:

```
.echo(.len(aMap))           // prints: 3
aMap.add({ "IT", 55 })
.echo(.len(aMap))           // prints: 4
```
The comparison operators `==` and `!=` must be fully defined for operands of the key type; thus the key type must not be a function, map, or sequence.
<h5 style="color:red; !important;">Maps are a growable containers too, thus if not allocated in heap but in stack, the size will be defined automatically in compile time and will be changet to static containers</h5>

### Any
```
any[]
```
### Matrix
Matrixes are of type SIMD (single instruction, multiple data )
```
mat[sizex]
mat[sizex,sizey]
mat[sizex,sizey,sizez]
```

## Complex
there are two complex types:
```
num[arch,64,128]
str[8,16,32]
```
### Numbers
Numbers are complex too and are made of intiger part, sign part, and float part, for example a number -23.44 is made of negative=true, int=24 and flt=0,44. By default has is made by default "architecture size" types, on 64bit:

- `num` is similar to `set[bol,int[arch],flt[arch]]`

### Strings
Strings are a complex type that are made of array of chars with null terminator '\0', and by default is utf8 encoded:

- `str` is similar to `set[seq[char[utf8]],'\0']`

## Special
```
ptr[basetype]
err[basetype]
non
nod
```
