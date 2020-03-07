---
title: 'Objects'
type: "docs"
weight: 60
---

A type declaration binds an identifier, the type name, to a type. Type declarations come in two forms: 
- alias declarations and 
- type definitions (objects): records, enums, units and classes. 


## Alias declaration
An alias declaration binds an identifier to the given type. All the properties of the type are bound to the alias too: [credit to golang](https://golang.org/ref/spec#Type_declarations):
```
typ[pub] I5: arr[int, 5];
```

So now the in the code, instead of writing `arr[int, 5]` we could use `I5`:

```
~var[pub] fiveIntigers: I5 = { 0, 1, 2, 3, 4, 5 }
```
Alias declaration are created because they can simplify using them multiple times, their identifier (their name) may be expressive in other contexts, and–most importantly–so that you can define (attach) methods to it (you can't attach methods to built-in types, nor to anonymous types or types defined in other packages).

Attaching methods is of outmost importance, because even though instead of attaching methods you could just as easily create and use functions that accept the "original" type as parameter, only types with methods can implement standards `std[]` that list/enforce those methods, and you can't attach methods to certain types unless you create a new type derived from them.

## Type definition

### Records

A type definition creates a new, distinct type - a record. A record is an aggregate of data elements in which the individual elements are identified by names and types and accessed through offsets from the beginning of the structure. There is frequently a need in programs to model a collection of data in which the individual elements are not of the same type or size. For example, information about a college student might include name, student number, grade point average, and so forth. A data type for such a collection might use a character string for the name, an integer for the student number, a  floating- point for the grade point average, and so forth. Records are designed for this kind of need.

It may appear that records and heterogeneous [set](/docs/spec/types/#sets) are the same, but that is not the case. The elements of a heterogeneous `set[]` are all references to data objects that reside in scattered locations, often on the heap. The elements of a record are of potentially different sizes and reside in adjacent memory locations. Records are normally used as encapsulation structures, rather than data structures. 
```
typ user: rec = {
    username: str,
    email: str,
    sign_in_count: int[64],
    active: bol,
};
```

To use a record after we’ve defined it, we create an instance of that record by specifying concrete values for each of the fields. We create an instance by stating the name of the record and then add curly brackets containing key: value pairs, where the keys are the names of the fields and the values are the data we want to store in those fields. We don’t have to specify the fields in the same order in which we declared them in the record. In other words, the record definition is like a general template for the type, and instances fill in that template with particular data to create values of the type.
```
@var user1: user = {
    email = "someone@example.com",
    username = "someusername123",
    active = true,
    sign_in_count = 1,
};
```

Initializaion can be inline too:
```
@var[mut] user1: user = { email = "someone@example.com", username = "someusername123", active = true, sign_in_count = 1 }
```

To get a specific value from a record, we can use dot notation or the access brackets. If we wanted just this user’s email address, we could use `user1.email` or `user1[email]` wherever we wanted to use this value. If the instance is mutable, we can change a value by assigning into a particular field. Note that the entire instance must be mutable; FOL doesn’t allow us to mark only certain fields as mutable. 
```
@var[mut] user1: user = {
    email = "someone@example.com",
    username = "someusername123",
    active = true,
    sign_in_count = 1,
};

user1.email = "new.mail@example.com"
user1[username] = "anotherusername"
```


As with any expression, we can construct a new instance of the record as the last expression in the function body to implicitly return that new instance. As specified [in function return](/docs/spec/functions/#return), the final expression in the function will be used as return value. For this to be used, the return type of the function needs to be defined (here is defined as `user`) and this can be used only in one statement body. Here we have declared only one variable `user1` and that itslef spanc into multi rows:
```
pro buildUser(email, username: str): user = { user1: user = {
    email = "someone@example.com",
    username = "someusername123",
    active = true,
    sign_in_count = 1,
} }
```
Records can be nested by creating a record type using other record types as the type for the fields of record. Nesting one record within another can be a useful way to model more complex structures: 
```
var empl1: employee = {
    FirstName = "Mark",
    LastName =  "Jones",
    Email =     "mark@gmail.com",
    Age =       25,
    MonthlySalary = {
        Basic = 15000.00,
        Bonus = {
            HTA =    2100.00,
            RA =   5000.00,
        },
    },
}
```

A record may have methods associated with it. It does not inherit any methods bound to the given type, but the method set of an standard type remains unchanged.To create a method for a record, it needs to be declared as the reciever of that method, in FOL's. Making a getter fucntion:
```
fun (recieverRecord)someFunction(): str = { self.somestring; };
```

After declaring the record receiver, we then we have access to the record with the keyword `self`. A receiver is essentially just a type that can directly call the method. 
```
typ user: rec = {
    username: str,
    email: str,
    sign_in_count: int[64],
    active: bol,
};

fun (user)getName(): str = { result = self.username; };
```

Methods have some benefits over regular subprograms. In the same package subprograms with the same name are not allowed but the same is not true for a method. One can have multiple methods with the same name given that the receivers they have are different. 

Then each instantiation of the record can access the method. Receivers allow us to write method calls in an OOP manner. That means whenever an object of some type is created that type can call the method from itself.
```
var[mut] user1: user = { email = "someone@example.com", username = "someusername123", active = true, sign_in_count = 1 }

.echo(user1.getName());
```

Records can have default values in their fields too. 
```
typ user: rec = {
    username: str,
    email: str,
    sign_in_count: int[64] = 1,
    active: bol = true,
};
```

This makes possible to enforce some fields (empty ones), and leave the defaults untouched: 
```
@var[mut] user1: user = { email = "someone@example.com", username = "someusername123" }

```

### Structs

<h5 style="color: red; !important;">
Compared to records, structs cant have neither named values, neither default values
</h5>

Structs are simplified version of records. The are identified with `stc[]` keyword. Structs have the added meaning the record name provides but don’t have names associated with their fields; rather, they just have the types of the fields. Structs are useful when you want to give a `set[]` a name and make the tuple be a different type from other sets, and naming each field as in a regular record would be verbose or redundant.

```
typ regStc: stc = {
    int[8],
    str,
}
```
And the difference between a struct and aliased type to set is that, in a struct you can restrict the values (with ranges) assigned to each field:
```
typ rgbSet: set[int[8], int[8], int[8]];

typ regStc: stc = {
    int[8][.range(0 ... 255)];
    int[8][.range(0 ... 255)];
    int[8][.range(0 ... 255)];
}
```
This of course can be achieve just with variable types and aliased types too, but we would need to create two types:
```
typ rgb: int[8][.range(0 ... 255)] ;                  // we create a type that holds only number from 0 to 255
typ rgbSet: set[rgb, rgb, rgb];                       // then we create a type holding the `rgb` type
```


### Enums

Is an enumerated type (also called enumeration, `enm`) is a data type consisting of a set of named values called elements. It can have only one type of data and no methods, and new members can be added:
```
typ color: enm = {
    BLUE, RED, BLACK, WHITE: str = "#0037cd", "#ff0000", "#000000", "#FFFFFF";
};

color.add(GREEN) = "#ff0000";

if( something == color.BLUE ) { dosomething } else { donothing }
```


### Classes
Calsses are the way that FOL can apply OOP. They basically are a glorified record. Instead of methods to be used fom outside the body, they have the method declaration within the body.
Here are some ways that class can be defined. For example, creating an class `filer` that inherits from class called `system` and can be modified if inherited by another classes.
```
typ[pub,~] computer(system): cls = {
    var[pub] dir: str;
    var[pub] size: int[16];

    +fun aMethod(var1: int, var2: str): str = { return; };
}

var laptop: computer = { member1 = value, member2 = value; };
laptop.aMethod(5, "dell");
```


## Standards and Contracts
### Satndard

A standard is an established norm or requirement for a repeatable technical task. It is usually a formal declaration that establishes uniform technical criteria, methods, processes, and practices. 

S, what is a to be considered a standard:

- A standard specification is an explicit set of requirements for an item, object or service. It is often used to formalize the technical aspects of a procurement agreement or contract. 
- A standard test method describes a definitive procedure that produces a test result. It may involve making a careful personal observation or conducting a highly technical measurement. 
- A standard procedure gives a set of instructions for performing operations or functions.
- A standard guide is general information or options that do not require a specific course of action.
- A standard definition is formally established terminology.


In FOL, standards are named collection of method signatures and are created by using `std` keyword:
```
typ geometry: std = {
    fun area(): flt[64];
    fun perim(): flt[64];
};
```

There are two types of standards, the normal ones that enforce just function implementation and extended, that enforce inclusion too:
```
typ geometry: std = {
    fun area(): flt[64];
    fun perim(): flt[64];
};

typ geometry: std[ext] = {                                           // extended standard that enforces inclusion 
    fun area(): flt[64];
    fun perim(): flt[64];
    color: rgb;                                                      // every type that uses tis standard mus have a color variable member as `rgb` type
};
```
### Contract
A contract is a legally binding agreement that recognises and governs the rights and duties of the parties to the agreement. A contract is enforceable because it meets the requirements and approval of an higher authority. An agreement typically involves a written declaration given in exchange for something of value that binds the maker to do. Its an specific act which gives to the person to whom the declaration is made the right to expect and enforce performance. In the event of breach of contract, the higher authority will refrain the contract from acting.

In fol contracts are used to bind a type to a standard. If a type declares to use a standard, it is the job of the contract (compiler internally) to see the standard full-filled.

```
typ geo: std = {
    fun area(): flt[64];
    fun perim(): flt[64];
};

typ rect: rec[][geo] = {                                             // this type makes a contract to use the geometry standard
    width: int[64];
    heigh: int[64];
}

```
Now we can make `rect` records or classes, we have to respect the contract. If we don't implement the `geo` methods, when we instantiate a new object of type `rect` it will throw an error.
```
var aRectangle: rect = { width = 5; heigh = 6; }                     // this throws an error, we haven't fullfill the ocntract
```

To do so, we need first to create the default `rect` methods from `geo` standard, then instantiate a new object:

```
fun (rect)area(): flt[64] = { result = self.width + self.heigh }
fun (rect)perim(): flt[64] = { result = 2 * self.width + 2 * self.heigh }

var aRectangle: rect = { width = 5; heigh = 6; }                     // this from here on will work
```

The benifit of standard is that, we can create a subprogram that as parameter takes a standard, thus all objects with the standard can use afterwards that subprogram:

```
typ geo: std = {
    fun area(): flt[64];
    fun perim(): flt[64];
};

typ rect: rec[][geo] = {                                            // this type makes a contract to use the geometry standard
    width: int[64]; 
    heigh: int[64]; 
}
fun (rect)area(): flt[64] = { result = self.width + self.heigh }
fun (rect)perim(): flt[64] = { result = 2 * self.width + 2 * self.heigh }

typ circle: rec[][geo] = {                                          // another type makes a contract to use the geometry standard
    radius: int[64]; 
}
fun (circle)area(): flt[64] = { result = math::const.pi * self.radius ** 2 }
fun (circle)perim(): flt[64] = { result = 2 * math::const.pi * self.radius}

typ square: rec[] = {                                               // this type does not make contract with `geo`
    heigh: int[64] 
}

pro measure( standard: std) { .echo(standard.area() + "m2") }        // a siple method to print the standard's area

// instantiate two objects
var aRectangle: rect = { width = 5; heigh = 6; }                     // creating a new rectangle
var aCircle: circle = { radius = 5; }                                // creating a new rectangle
var aSquare: square = { heigh = 6; }                                 // creating a new square


// to call the measure function that rpints the surface
measure(aRectangle)                                                  // this prints: 30m2
measure(aSquare)                                                     // this throws error, square cant use measure method
measure(aCircle)                                                     // this prints: 78m2

```
