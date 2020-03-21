---
title: 'Blocks'
type: "docs"
weight: 40
---
## Namespaces

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
## Blocks

Block statement is used for scopes where members get destroyed when scope is finished. And there are two ways to define a block: 
- unnamed blocks and 
- named blocks

### Unnamed blocks
Are simply scopes, that may or may not return value, and are represented as: `{ //block }`, with `.` before the brackets for return types and `_` for non return types:
```
def shko: mod[] = {
    pro[] main: int = {
        _{
            .echo("simple type block")
        }
        .echo(.{ return "return type block" })
    }
}
```

### Named blocks
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

