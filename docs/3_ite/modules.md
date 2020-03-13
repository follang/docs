---
title: 'Modules'
type: "docs"
weight: 10
---

## Imoprting modules

### System libraries
This is how including other libraries works, for example include `fmt` module from standard library:
```
use fmt: mod[std] = {fmt};

def main: mod[init] = {
    fmt::log.warn("Last warning!...")
}
```
To use only the `log` namespace of `fmt` module:
```
use log mod[std] = {fmt::log};

def main: mod[init] = {
    pro[] main: int = {
        log.warn("Last warning!...")
    }
}
```
But let's say you only wanna use ONLY the `warn` functionality of `log` namespace from `fmt` module:
```
use warn mod[std] = {fmt::log.warn};

def main: mod[init] = {
    pro[] main: int = {
        warn("Last warning!...")
    }
}
```
---
### Local libraries
To include a local package (example, package name `bender`), then we include the folder where it is, followed by the package name (folder is where files are located, package is the name defned with mod[])

```
use bend: mod[loc] = {../folder/bender};
```
Then to acces only a namespace:
```
use space: mod[loc] = {../folder/bender::space};
```

## Definig modules
### Main module
Every file in a folder is part of a package, which means everything in the folder that uses the same package name, share the same scope. Two pakages can't exist in same folder. 
```
def shko: mod[] = {
    // implementation
}
```
If a package is an executable, it should have `pro[] main: int` to tell the compiler where to start. 
```
use log mod[std] = {fmt::log};

def shko: mod[] = {
    pro[] main: int = {
        log.warn("Last warning!...")
    }
}
```

To save the fingers and some brackets, we use a short hand of package declaration at the very top of the page (like golang):
```
def shko: mod[]

use log mod[std] = {fmt::log};

pro[] main: int {
    log.warn("Last warning!...")
}
```

---
### Namespaces
A namespace can be defined in three ways: 
- in a subfolder
- same folder, but in a new file
- or the same file 

and is done by using slash when outside the main module
```
def shko/other: mod[] = {
    pro[] main: int = {
        // implementation
    }
    // implementation
}
```

or in the same module file:
```
def shko: mod[] = {
    pro[] main: int = {
        // implementation
    }
    def shko/other: mod[] = {
        // implementation
    }
}
```


### Scopes

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
---
### Blocks of code
Block statement is used for scopes where members get destroyed when scope is finished. And there are two ways to define a block: unnamed blocks and named blocks
#### Unnamed blocks
Are simply scopes, that may or may not return value, and are represented as: `{ //block }`
```
def shko: mod[] = {
    pro[] main: int = {
        {
            .echo("simple type block")
        }
        .echo({ return "return type block" })
    }
}
```

If the block is prepended with a dot in front like: `.{ //block }` then, when block is isolated on runtime too, in case of error, will not exit the program. **it is meant for testing only**
```
def shko: mod[] = {
    var const: int = 5
    pro[] main: int = {
        .{
            const = 6;                  // this throws an error, but the program doen't exit
        }
        .echo(const)                    // this still returns the value 5
    }
}
```
#### Named blocks
Blocks can be used as labels too, when we want to unconditionally jump to a specific part of the code.
```
def shko/other: mod[] = {
    pro[] main: int = {
        def block: blk[] = {            // $block A named block that can be referenced
            // implementation
        }
        def mark: blk[]                 // $mark A named block that can be referenced, usually for "jump" statements
    }
}
```

---
## Unit tests
Blocks defined with type `tst`, have access to the module (or namespace) defined in `tst["name", access]`.

```
def test1: tst["sometest", shko] = {}
def "some unit testing": tst[shko] = {}
```

