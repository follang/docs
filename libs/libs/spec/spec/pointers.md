---
title: 'Ownership'
type: "docs"
weight: 41
---

# Stack vs Heap

## The Stack

What is the stack? It's a special region of your computer's memory that stores temporary variables created by each function (including the main() function). The stack is a "LIFO" (last in, first out) data structure, that is managed and optimized by the CPU quite closely. Every time a function declares a new variable, it is "pushed" onto the stack. Then every time a function exits, all of the variables pushed onto the stack by that function, are freed (that is to say, they are deleted). Once a stack variable is freed, that region of memory becomes available for other stack variables.

The advantage of using the stack to store variables, is that memory is managed for you. You don't have to allocate memory by hand, or free it once you don't need it any more. What's more, because the CPU organizes stack memory so efficiently, reading from and writing to stack variables is very fast.

A key to understanding the stack is the notion that when a function exits, all of its variables are popped off of the stack (and hence lost forever). Thus stack variables are local in nature. This is related to a concept we saw earlier known as variable scope, or local vs global variables. A common bug is attempting to access a variable that was created on the stack inside some function, from a place in your program outside of that function (i.e. after that function has exited).

Another feature of the stack to keep in mind, is that there is a limit (varies with OS) on the size of variables that can be stored on the stack. This is not the case for variables allocated on the heap.

- very fast access
- don't have to explicitly deallocate variables
- space is managed efficiently by CPU (memory will not become fragmented)
- local variables only
- limit on stack size (OS-dependent)
- variables cannot be resized


## The Heap

The heap is a region of your computer's memory that is not managed automatically for you, and is not as tightly managed by the CPU. It is a more free-floating region of memory (and is larger). To allocate memory on the heap, you must use `var[new]`. Once you have allocated memory on the heap, you are responsible to deallocate that memory once you don't need it any more. If you fail to do this, your program will have what is known as a memory leak. That is, memory on the heap will still be set aside (and won't be available to other processes).

Unlike the stack, the heap does not have size restrictions on variable size (apart from the obvious physical limitations of your computer). Heap memory is slightly slower to be read from and written to, because one has to use pointers to access memory on the heap. Unlike the stack, variables created on the heap are accessible by any function, anywhere in your program. Heap variables are essentially global in scope.

Element of the heap have no dependencies with each other and can always be accessed randomly at any time. You can allocate a block at any time and free it at any time. This makes it much more complex to keep track of which parts of the heap are allocated or free at any given time.

- variables can be accessed globally
- no limit on memory size
- slower access
- no guaranteed efficient use of space, memory may become fragmented over time as blocks of memory are allocated, then freed
- you must manage memory (you're in charge of allocating and freeing variables)
- variables can be resized anytime

## Multithread
In a multi-threaded situation each thread will have its own completely independent stack but they will share the heap. Stack is thread specific and Heap is application specific. The stack is important to consider in exception handling and thread executions.

## Memory and Allocation

In the case of a normal variable, we know the contents at compile time, so the value is hardcoded directly into the final executable. This is why they are fast and efficient. But these properties only come from the variable immutability. Unfortunately, we can’t put a blob of memory into the binary for each piece of variable whose size is unknown at compile time and whose size might change while running the program.

Lets take an example, the user input as `str` (string), in order to support a mutable, growable piece of variable, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents. This means:

- The memory must be requested from the operating system at runtime.
- We need a way of returning this memory to the operating system when we’re done with our String.

That first part is done by us: when we call `var[new]`, its implementation requests the memory it needs. This is pretty much universal in any programming languages.

However, the second part is different. In languages with a garbage collector (GC), the GC keeps track and cleans up memory that isn’t being used anymore, and we don’t need to think about it. Without a GC, it’s our responsibility to identify when memory is no longer being used and call code to explicitly return it, just as we did to request it. Doing this correctly has historically been a difficult programming problem. If we forget, we’ll waste memory. If we do it too early, we’ll have an invalid variable. If we do it twice, that’s a bug too. We need to pair exactly one allocate with exactly one free.

Fol, copies [Rust](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html) in this aspect: the memory is automatically returned once the variable that owns it goes out of scope. 

<h5 style="color:green; !important;">
When a variable goes out of scope, Fol calls a special function for us to deallocate all the new memories we have allocated during this dunction call. 
</h5>

This function is called `.de_alloc()` - just like Rust's `drop()`, and it’s where the author of `var[new]` can put the code to return the memory. Fol calls `.de_alloc()` automatically at the closing curly bracket.


# Variables

Much like [C++]() and [Rust](), in Fol every variable declared, by default is created in stack unless explicitly specified othervise. Using option `[new]` or `[@]` in a variable, it allocates memory in the heap. The size of the allocation is defined by the type. Internally this creates a pointer to heap address, and dereferences it to the type you are having. Usually those behind the scene pointers here are unique pointers. This means that when the scope ends, the memory that the pointer used is freed.
```
var[new] intOnHeap: int[64];
@var intOnHeap: int[64];
```
## Assignments

[As discussed before](/docs/spec/040_variables/#assignments), declaring a new variable is like this:
```
var[pub] aVar: int[32] = 64
```

<h5 style="color:red; !important;">
However, when new variable is created and uses an old variable as value, the value is always cloned for "stack" declared values, but moved for "heap" declared values.
</h5>

```
@var aVar: int[32] = 64
{
    var bVar = aVar                                              // this moves the content from $aVar to $bVar
}
.echo(aVar)                                                      // this will throw n error, since the $aVar is not anymore owner of any value
```

When the variable is moved, the owner is changed. In example above, the value `64` (saved in stack) is owned my `aVar` and then the ownership is moved to `bVar`. Now `bVar` is the new owner of the variable, making the `aVar` useless and can't be refered anymore. Since the `bVar` now controls the value, it's lifetime lasts until the end of the scope. When the scope ends, the variable is destroyed with `.de_alloc()` function. This because when the ovnership is moved, the attributes are moved too, so the `@` of `aVar` is now part of the `bVar` even if not implicitly specified. To avoid destruction, the `bVar` needs to return the ownership back to `aVar` before the scope ends with `.give_back(bVar)` or `!bVar`.

```
@var aVar: int[32] = 64
{
    var bVar = aVar                                              // this moves the content from $aVar to $bVar
    !bvar                                                        // return ownership
}
.echo(aVar)                                                      // this now will print 64
```
This can be done automatically by using [borrowing](/docs/spec/040_variables//#borrowing). 

## Borrowing
Borrowing does as the name says, it borrows a value from another variable, and at the end of the scope it automatically returns to the owner.

```
pro[] main: int = {
    var[~] aVar: int = 55;
    {
        var[bor] newVar: int = aVar                              // represents borrowing
        .echo(newVar)                                            // this return 55
    }
    .echo(aVar)                                                  // here $aVar it not accesible, as the ownership returns at the end of the scope
    .echo(newVar)                                                // we cant access the variable because the scope has ended
}
```
Borrowing uses a predefined option `[bor]`, which is not conventional like other languages that use `&` or `*`. This because you can get away just with "borrowing" without using pointers (so, symbols like `*` and `&` are strictly related to pointers)

However, while the value is being borrowed, we can't use the old variable while is being borrowed but we still can lend to another variable:
```
pro[] main: int = {
    var[~] aVar: int = 55;
    {
        var[bor] newVar = aVar                                   // represents borrowing
        .echo(newVar)                                            // this prints 55
        .echo(aVar)                                              // this throws an error, cos we already have borrowd the value from $aVar
        var[bor] anotherVar = aVar                               // $anotherVar again borrows from a $aVar
    }
}
```
<h6 style="color:red; !important;"> 
When borrowed, a the value is read-only (it's immutable). To make it muttable, <em>firtsly</em>, the owner needs to be muttable, <em>secondly</em> the borrower needs to declarare that it intends to change. 
</h6> 

To do so, the borrower uses `var[mut, bor]`. However, when the value is declared mutable by owner, only one borrower within one scope can declare to modify it:
```
pro[] main: int = {
    var[~] aVar: int = 55;
    {
        var[mut, bor] newVar = aVar                              // [mut, bor] represents a mutable borrowing
        var[mut, bor] anotherVar = aVar                          // this throws an error, cos we already have borrowed the muttable value before
    }
    {
        var[mut, bor] anotherVar = aVar                          // this is okay, s it is in another scope
    }
}
```

# Pointers

The only way to access the same memory with different variable is by using pointers. In example below, we create a pointer, and when we want to dereference it to modify the content of the address that the pointer is pointing to, we use `*ptrname` or `.pointer_value(ptrname)`.
```
@var aContainer: arr[int, 5];                                    //allocating memory on the heap
var contPoint: ptr[] = aContainer;
    
*contPoint = { zero, one, two, three, four };                    //dereferencing and then assigning values
```
Bare in mind, that the pointer (so, the address itself) can't be changes, unless when created is marked as `var[mut]`. To see tha address of a pointer we use `&ptrname` or `.address_of(ptrname)`
```
@var aContainer: arr[int, 5];                                    //allocating memory on the heap
var contPoint: ptr[] = aContainer;
    
var anotherPoint: ptr[] = &contPoint;                            //assigning the same adress to another pointer
```
### Unique pointer

Ponter of a pointer is very simimilar to RUST move pointer, it actually, deletes the first pointer and references the new one to the location of deleted one. However this works only when the pointer is unique (all pointers by default all unique). This is like borrowing, but does not invalidate the source variable:
```
var aContainer: arr[int, 5] = { zero, one, two, three, four };

var contPoint: ptr[] = aContainer;
var anotherPoint: ptr[] = &contPoint;
```

with borrowing, we use `#varname` or `.borrow_from(varname)`
```
var aContainer: arr[int, 5] = { zero, one, two, three, four };
{
    var borrowVar = #aContainer;                                    //this makes a new var form the old var, but makes the old invalid (until out of scope)
}
```

### Shred pointer
Ponter can be shared too. They can get referenced by another pointer, and they don't get destroyed until the last reference's scope is finished. This is exacly like smart shared_ptr in C++. Pointer to this pointer makes a reference not a copy as unique pointers. Dereferencing is a bit complicated here, as when you dereference a pointer pointer you get a pointer, so you need to dereference it too to get the value.
```
@var aContainer: arr[int, 5] = { zero, one, two, three, four };

var contPoint: ptr[] = aContainer;
var pointerPoint: ptr[shared] = &contPoint;
```
Dereferencing (step-by-step):
```
var apointerValue = *pointerPoint
var lastpointerValue = *apointer
```
Dereferencing (all-in-one):
```
var lastpointer = *(*pointerPoint)
```
### Raw pointer
Lastly, pointers can be raw too. This is the base of ALL POINTERS AND VARIABLES. Pointers of this type need to MANUALLY GET DELETED. If a pointer gets deleted before the new pointer that points at it, we get can get memory corruptions:
```
var aContainer: arr[int, 5] = { zero, one, two, three, four };

var contPoint: ptr[raw] = aContainer;
var pointerPoint: ptr[raw] = &contPoint;
```
Deleting:
```
!(pointerPoint)
!(contPoint)
```

