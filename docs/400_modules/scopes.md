---
title: 'Scope'
type: "docs"
weight: 20
---

Every file in a folder is part of one package, thus they share the same scope. Thus two varibles with same name at global scope cant be defined. However, this is kind of special for functions. Functions of the same name and same amount of parameters (so identical functions, not function overloading) can be defined in two different files. When called, the one on the same file is used.

In case a function exists in two files and is called from thrid one, then this can be resolved in two ways. First when the package is defined, the priority shoud be defined too like: `def shko: mod[pri=5]`, this a function with same name, in a different file, with `def shko: mod[pri=4]` would be called before. If no proorities are defined, then the function in files with name of the files as priority. So the function `add()` in "file1.shko" would be called before "file2.shko". 

*file1.fol*
```
use log mod[std] = {fmt::log};

def shko: mod[] = {

    var globalVar = 5                   // first instance of a constant $globalVar variable

    pro[] main: int {
        log.info( add(5,6) )            // calling function in different file but same package (shko)
        log.info( sub(8,3) )            // this function is from the same file and not form file2
    }
    fun sub(a, b: int): int = {
        return .sub(a, b)
    }
}
```
*file2.fol*
```
use log mod[std] = {fmt::log};

def shko: mod[] = {

    var globalVar = 5                   // this throws an error, as it was declared before

    fun add(a, b: int): int = {
        return .add(a, b)
    }
    fun sub(a, b: int): int = {         // this does not throw error, even another function has the name
        return .sub(a, b)
    }
}
```
*file3.fol*
```
use log mod[std] = {fmt::log};

def shko: mod[] = {
    fun add(a, b: int): int = {
        return .add(a, b)
    }
}
```
